---
name: xiaolin-style-talking-head-edit
description: >-
  Use this when the user wants a talking-head clip (口播) turned into a
  小Lin说-style knowledge video with dense packaging: real B-roll photos, pop-in
  cards, a circle avatar, animated subtitles, beat-synced cuts, and BGM plus SFX
  matched to the mood, built with the onetake skill (non-commercial only).
---
# 小Lin说-style talking-head edit (onetake)

Turns a raw talking-head clip into a polished short knowledge video. Built on [feitangyuan/onetake](https://github.com/feitangyuan/onetake). Read and follow the `onetake-motion-promo` skill for install, the comp protocol, the motion library, rendering and verification. This skill adds the talking-head and 小Lin说 layer on top of it.

## 0. Gates
- **License:** onetake is PolyForm Noncommercial. Ask whether the video is non-commercial (a personal post is fine). For paid client work or monetised accounts, say it needs a commercial licence or another tool (AE, DaVinci) and reuse only the method.
- **Get the raw clip:** chat attachment, a folder on the owner's computer, or a share link from the owner's preferred netdisk. For a share link, have the owner sign in on the agent browser if needed, then download it. Do not hard-code vendor paths or private credentials.

## 1. Style study (do before composing)
- Download 2 recent reference videos from the target channel at low res and run `scripts/analyze_ref.py` on 3-minute samples. Also measure average shot length with ffmpeg scene detection. Write `STYLE_NOTES.md` with the numbers and rules.
- Typical 小Lin说 rules:
  - A visual change every 2.5 to 3.5 s. Tight jump cuts with no breaths or ums.
  - Soft ease-out moves with a gentle spring. No motion blur.
  - When B-roll or graphics fill the frame, the host shrinks into a circle avatar with a thin champagne-gold ring. The avatar stays through many B-roll changes and carries the sequence.
  - Pop-in side cards, keyword slams, and data cards.
  - Dark or clean stage with one accent colour.
- For a 9:16 short under 60 s: aim for a change every 2 to 2.5 s, remove pauses, speed speech up 1.1 to 1.2x, and put a hook graphic in the first 1 to 2 s.

## 2. Speech and timing
- Transcribe with faster-whisper (word timestamps). Cut silences and filler, then build a time map from source time to output time (`work/timemap.json`).
- Extract talk frames as a JPEG sequence and track the face box so the circle avatar crop follows the head.
- Subtitles come from the transcript. Fix names and English terms by hand.

## 3. Visual material
- Ask the user's preference. Many want **real imagery** (real photos or footage, or photorealistic AI images), not drawn scenes, especially for real-world objects such as clothing.
- Source free-licensed photos from Wikimedia Commons, Flickr CC and rawpixel public domain. Prefer at least 1080 px on the short side. Log every file with its source URL, author and licence in `assets/real/CREDITS.md`. CC BY and BY-SA need on-screen or caption credit.
- Use image or video generation only when the user allows it, and label it in the credits.
- Draw images per frame in the comp (`<img>`/canvas) with pushes, parallax and carry transitions (`morphRect`, `iris`, `zoomThrough`, `gather`), not `<video>`.

## 4. Music and sound
- Split the script into emotional sections, for example a curious opening, an elegant history part, and a tight "rules" section. Pick one licensed BGM per section (Kevin MacLeod, CC BY 4.0, credit needed) and crossfade at section boundaries.
- Detect beats (librosa) and snap cuts and card entrances to the beat grid. Duck the music under the voice (sidechain, about -12 dB) and normalise the mix to -14 LUFS.
- SFX: never use plain synth beeps. Use real samples: Kenney CC0 UI and percussion packs, and University of Iowa MIS instrument samples (marimba, vibraphone, crotales), with a Voxengo impulse-response room reverb. Map comp `__events()` to voices through onetake `sfx_palette.py` and `score.py`, and match the instrument to the section mood.

## 5. Review loop
- Send a 10 to 15 s preview plus a contact sheet before the full render.
- If the user asks for progress updates during long work, create a short-lived routine and delete it on delivery.
- Run `verify_promo.py`, then check stills at several timestamps for ugly frames, low-res photos, and subtitles covering faces.

## 6. Deliver
Deliver the MP4, the contact sheet, and a short list of which onetake effects each section used, the instruments and SFX sources, and the image and music credits. Offer to copy the files to the owner's preferred local folder or netdisk location (do not hard-code a vendor or private path).
