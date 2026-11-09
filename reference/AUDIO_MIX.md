# AUDIO / MIX — SFX mapping & mastering rules (Agent 5)

## Voice verification (lesson learned)
- Generating a voice WAV is NOT enough. Verify the FINAL mix actually contains the intended
  voice BEFORE delivering. Compare the mix's voice band RMS to the source take, and confirm the
  voice model/alias used (e.g. `echo`) is truly the timbre/sex wanted.
- If a voice "didn't take", check: the mix was rebuilt AFTER the voice file was written, and
  no older voice file was silently reused. Always rebuild the mix from the confirmed voice.
- Provide the exact voice id + model used in the deliverable notes.

Source: merged from the old kit's `templates/audio/cues.md` + `sfx_gen.py` and the new
kit's `audio.py`/`audio_xqivo.py`.

## Voice (the master)
- Narration pace ≈ 3.7–4.0 words/s; keep the voice UNCHANGED in the mix.
- Every visual reaction lands **0–3 frames before** its word.
- Generate via Voice Studio (`http://127.0.0.1:3900/v1`): `POST /v1/audio/speech`
  with `{model:"tts-1", voice:"<voice_id>", input, response_format:"wav"}`.
  List voices via `GET /v1/audio/voices`.

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
