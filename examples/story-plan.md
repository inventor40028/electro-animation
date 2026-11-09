# EXAMPLE — Shot plan structure (story.js)

Working patterns lifted from `shorts-xqivo/web/story.js` (the finished "5 PM Time Trap").

## Shot table (pure functions of frame f)
```js
export const FPS = 30, TOTAL = 1950, DECK = 0;
export const SHOTS = [
  { id: 'hook',        a: 0,    b: 190,  light: 'office' },
  { id: 'clientroom',  a: 190,  b: 400,  light: 'client' },
  { id: 'city',        a: 400,  b: 560,  light: 'city' },
  { id: 'officerecv',  a: 560,  b: 680,  light: 'office' },
  { id: 'trap1',       a: 680,  b: 900,  light: 'office' },
  { id: 'trap2',       a: 900,  b: 1100, light: 'evening' },
  { id: 'saveA',       a: 1100, b: 1184, light: 'office' },
  { id: 'inv1',        a: 1185, b: 1198, light: 'office' },
  { id: 'inv2',        a: 1198, b: 1211, light: 'office' },
  { id: 'inv3',        a: 1211, b: 1224, light: 'office' },
  { id: 'saveB',       a: 1224, b: 1384, light: 'office' },
  { id: 'clientrecv',  a: 1385, b: 1449, light: 'client' },
  { id: 'payoff',      a: 1450, b: 1550, light: 'evening' },
  { id: 'endcard',     a: 1550, b: TOTAL, light: 'card' },
];
export const shotAt = (f) => SHOTS.find((s) => f >= s.a && f <= s.b) || SHOTS[SHOTS.length - 1];
```

## Camera keys (glide via smootherstep; creep on holds; kicks on impacts)
```js
const CAMS = {
  saveB: [
    // 360° orbit around the invoice, then track it as it zips away
    { f: 1249, pos: [2.6, 1.8, 0.6], look: [0.3, 1.8, 0.3], fov: 46 },
    { f: 1305, pos: [-2.2, 1.9, -1.0], look: [0.3, 1.8, 0.3], fov: 42 },
    { f: 1360, pos: [1.4, 1.8, 1.7], look: [0.3, 1.8, 0.3], fov: 38 },
    { f: 1384, pos: [0.5, 2.1, 2.4], look: [-5.5, 2.2, 0.8], fov: 42 },
  ],
};
export const camAt = (f) => camFromKeys(CAMS[shotAt(f).id], f, { creep: 0.0012, kicks: KICKS });
```

## DOF (rack to a subject, off after a threshold)
```js
export const dofAt = (f) => {
  const S = shotAt(f), C = camAt(f);
  const d = dist(C.pos, C.look);
  if (S.id === 'city') { if (f >= 547) return { focus: d, cinf: 0 }; if (f >= 530) return { focus: d, cinf: 5 }; }
  return { focus: d, cinf: (cf[S.id] || 0) };
};
```

## Actors: card redraw keyed by state; side view + payoff walk-out
```js
out.push(R(T, 'freelancer', { seed: 3, expr, look, L: armL, R: armR, lift,
  side: S.id === 'trap2' && f >= 980 && f < 1065,
  blink: blinkAt(f, 3) }, homePos, { tint, shadow: false, alpha }));
```

## FX: static cut-in cards + moving cards
```js
const FX = (key, draw, pos, s, extra = {}) => ({ key, draw, x: pos[0], y: pos[1], z: pos[2], s, order: 5, ...extra });
// hover, then zip + poof, keyed by rounded phase so cards only redraw on change
```
