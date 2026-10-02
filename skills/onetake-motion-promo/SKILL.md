---
name: onetake-motion-promo
description: >-
  Use this when the user wants a short (10–30s) motion promo, teaser, launch or
  feature-demo video made in code (kinetic type, transitions, logo lockup), e.g.
  做个宣传短片 / 动效视频 / 发布视频, using the open-source onetake skill (HTML composition
  rendered frame by frame). Non-commercial use only.
---
# onetake motion promo

Make a short motion film with [feitangyuan/onetake](https://github.com/feitangyuan/onetake): one HTML page where every value is a pure function of time, rendered frame by frame by headless Chromium, with motion blur, synthesised SFX, and an automatic checker.

## 0. License gate (do first)
onetake is **PolyForm Noncommercial 1.0.0**. Using it for anything sold or monetised (paid game, ads, in-app purchases, crowdfunding, client work) is not allowed, and that covers the output video too, because the license restricts use of the software. Ask the user whether the project is non-commercial before rendering. If it's commercial, suggest borrowing only the method (beat sheet, carry rule) and making the film in UE Sequencer, AE, DaVinci and so on.

## 1. Workspace
- Keep a local **onetake kit** on the agent machine (see this pack's `INSTALL.md`). A typical layout:
  - `repo/` — full copy of feitangyuan/onetake (`SKILL.md`, `references/`, `lib/motion.js`, `scripts/`, `templates/`, `looks/`, `LICENSE`).
  - `fonts/NotoSansSC.ttf` (or another OFL CJK font). The upstream repo ships none; pass it to `look.py` with `--cjk`.
  - Optional stand-ins: `dump_events.py` and `score.py` for scripts the docs mention but the public repo may lack.
  - Optional `example/` — a short working comp (`comp.html`, `BEATS.md`, `look.json`) as reference.
- If the kit is missing or stale, fetch from `raw.githubusercontent.com/feitangyuan/onetake/main/...` (GitHub API / tree). Fonts and other binaries must be fetched separately.
- Copy `repo/{lib,looks,scripts,templates}` and the CJK font into a fresh project dir under the agent's workspace (e.g. `/workspace/<project>/`), and build in `<project>/film/`.
- Deps: `pip install playwright numpy scipy pillow matplotlib fonttools brotli opencv-python`, then `playwright install --with-deps chromium`, plus `ffmpeg`. Voice-over (optional) also needs faster-whisper and kokoro-onnx.

## 2. Protocol (from onetake SKILL.md; read `repo/SKILL.md` and `repo/references/*.md` fully before composing)
1. **Reference (optional):** run `scripts/analyze_ref.py ref.mp4 --out ana/` for an energy map, cut points, and curve params.
2. **Concept:** pick or offer 3 one-sentence visual concepts. The best ones have one element carrying through the whole film (for example "a crane hook carries everything"). Pick a look from `looks/looks.json` or build one with `look.py from-shot`.
3. **Beat sheet** (`BEATS.md`) with the columns `t · beat · what moves · what rests · what carries into the next beat`. Shot lengths must differ by at least 4x, and every section boundary needs something that survives into the next beat.
4. **Assets:** real UI can be rebuilt in HTML (`lib/ui_kit.js`), or web pages recorded with `record_footage.py`. For game or 3D footage, export PNG/JPG sequences from the engine (UE Movie Render Queue, Blender) and draw them per frame with `<img>` or canvas `drawImage` (`frames/NNNN.jpg`, index = floor(t*fps)). Don't rely on `<video>`, because frame-accurate seek is unverified. Placeholder sections should carry a visible label saying they are placeholders.
5. **Compose:** copy `templates/comp.html` and `lib/motion.js` into one HTML page. It must expose a deterministic `window.__seek(t)`, plus `__ready` and `__meta {dur,fps,cuts,inFrame}`, and optionally `__motion` (drives adaptive blur) and `__events()` (drives SFX). Run `look.py apply ... --cjk fonts/<CJK>.ttf` to produce `look.js` with fonts inlined. Preview with `comp.html?play`, and inspect a single frame with `?t=6.4&hud`. Run `scripts/probe.py comp.html` to check continuity.
6. **Motion library** (`window.OM`, see `references/motion-library.md`):
   - Enter: `wordRise`, `letterDrop`, `popIn`, `maskRise`, `typeOn`, `flyThrough`.
   - Carry: `morphRect`, `iris`, `zoomThrough`, `hop`, `gather`, `seal`, `staccato`.
   - Contact: `impact`, `impactSplit`, `press`, `cursor`, `squash`, `sim.*`.
   - Camera: `camera`, `view`, `dof`, `whip`, `shake`, `drift`.
   - Fluid: `lightField`, `ripple`, `silk`.
7. **SFX:** from the film dir, dump `__events()` to `events.json` (kit `dump_events.py` or equivalent), then map event kinds to `sfx_palette` voices with `score.py` (or equivalent) to get `sfx.wav`. Only add music if the user supplies a licensed track.
8. **Render:** run `scripts/stills.py` for a contact sheet first. Then run `scripts/render.py comp.html` for a draft at 1080p30 with a 180° shutter (about 100s for 16s on 6 workers). Add `--final` for 4K60.
9. **Verify:** run `scripts/verify_promo.py film.mp4 --comp`. The checks are cadence, rest (at least 25% static frames), burst, audio, continuity (at least 0.5), curves, and framing. Fix failures before delivering. For game-style cuts, rest may need held frames.

## 3. Known gaps and workarounds
- `score.py` and `dump_events.py` may not be in the public repo. Provide kit stand-ins if needed.
- The shipped `motion.js` is "lite". Operators mentioned in comments, such as a follow-cam, don't exist, so use `OM.camera` keyframes plus handheld drift.
- The anchor in `OM.sim.verlet` is fixed. For a cable hanging from a moving trolley, or a cable that changes length, write your own pendulum plus Verlet sim, precomputed at 240Hz so it stays deterministic.
- Kokoro TTS supports only en-us, en-gb, and ja. Chinese voice-over needs another TTS.
- There is no three.js/WebGL, only CSS 3D and 2.5D parallax. Real 3D gameplay must come in as rendered frame sequences.
- Watch-outs from early runs: small elements on a large flat background make the opening feel slow. Cap swing angles around 20°. Check rotation during `gather`.

## 4. Deliver
Attach the MP4 and the contact sheet in chat. If the user prefers file delivery, copy them to the owner's preferred local folder or netdisk location (do not hard-code a vendor or private path). List which parts are placeholders and what assets would replace them.
