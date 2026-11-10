# AUDIO / MIX — SFX mapping & mastering rules (Agent 5)

Source: merged from the old kit's `templates/audio/cues.md` + `sfx_gen.py` and the new
kit's `audio.py`/`audio_xqivo.py`. (Tool-agnostic — adapt to whatever TTS/mix stack is
available without renaming this doc.)

## Voice (the master)
- Narration pace ≈ 3.7–4.0 words/s; keep the voice UNCHANGED in the mix.
- Every visual reaction lands **0–3 frames before** its word.
- Use the TTS stack that is configured/available. If no voice server is reachable (or the
  requested voice/timbre cannot be confirmed), **STOP and ask the user** how to proceed —
  do not silently fall back to a different (e.g. wrong-gender) voice.

## Voice verification (required — a hard gate before delivery)
- Generating a voice file is NOT enough. The FINAL mix must be verified to contain the
  intended voice before you call it done.
- Confirm BOTH:
  1. the voice source (the exact voice/timbre you selected) — if you cannot audibly verify
     it, or the tool does not expose the timbre, ASK the user to confirm the voice matches
     before building the mix; and
  2. the mix was rebuilt AFTER that voice file was written (no stale/older voice file
     silently reused).
- Provide the voice id + model + build order in the deliverable notes so it is auditable.

## Mix levels (the contract)
- Music = **50% of voice gated-RMS** (−6 dB). Never louder.
- SFX = 50% or less; peaks capped so no hit pokes above narration.
- Only one linear gain + one peak limiter on the whole mix. No ducking/compression
  that changes the ratio. (One exception: a −3 dB speech-duck for a music bed is used
  in the old kit — keep sentence-final words on top.)
- Master: ~ −15 LUFS integrated, true peak ≤ −1.2 dBTP, LRA small.

## SFX must match the visible event
Every sound needs a seen source and a natural time. Common cues and their placement:
- **Ding / success** — a bright chime or expanding ring sound exactly at the payoff.
- **Door open/close + slam** — creak + thunk at the open, low thud + rattle at the
  slam. Match the visible door motion.
- **Whoosh** — camera glide / object sweep (fast attack, band centre rises then falls).
- **Impact/kick** — a punchy low hit at the shake moment.
- **Foley** — typing clicks (small), cup set-down, paper rustle, pen.
- **Ambience bed** — room tone that fits the environment (office hum, city traffic,
  forest breeze); integrated (not reduced), low under voice.

## Example cue table format (from old kit `cues.md`)
| # | time s | frame | cue | what | naive LU | applied LU | pan |
|---|---|---|---|---|---|---|---|
| 1 | 0.60 | 18 | whoosh | camera pull-back | −14 | −20 | 0 |
| 2 | 2.07 | 62 | leaves | leaves fall | −18 | −24 | L30 |
| 3 | 4.00 | 120 | thunk | spear stamp | −8 | −14 | 0 |
| 4 | 5.87 | 176 | rumble | masonry groan | −12 | −18 | 0 |
| 5 | 6.20 | 186 | crash | collapse | 0 | −6 | 0 |
| 6 | 13.15 | 394 | ting | glint/ding | −10 | −16 | 0 |
| 7 | 14.18 | 425 | sting | twist line | −6 | −12 | 0 |
| 8 | 18.27 | 548 | snap | brace breaks | 0 | −6 | 0 |

## Distance rule (for world sounds)
Apply distance offset: `dB = −14·log10(d/3.5)`; low-pass `17000·(3.5/d)^0.55 Hz`;
add more reverb with distance.

## Build order
1. Voice track (Voice Studio WAV).
2. Music bed script + SFX cue table (frames = seconds×30).
3. Mix to the level contract, write `mix_raw.wav`.
4. Mux with the silent video via ffmpeg (aac 192k, -shortest).
