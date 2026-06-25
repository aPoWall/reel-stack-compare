# portable iPhone kit

Public-safe branch pack for a transferable iPhone Reels workflow. It distills the useful mechanics from the public [`Voronik1801/reel_pipline`](https://github.com/Voronik1801/reel_pipline) repo into the broader Reels Stack Configurator.

## Use When

- source is iPhone `.MOV`;
- the creator needs a repeatable human/agent handoff;
- speed matters more than a full block-editor UI;
- color, subtitles, cover/finale and QA must be predictable.

## Pack Contract

| layer | contract |
|------|----------|
| preflight | check `ffmpeg`, `ffprobe`, `whisper`, `avconvert`, Python packages, fonts and folders |
| ingest | convert iPhone HLG/bt2020 to SDR bt709 through Apple `avconvert` |
| timing | create word-level transcript and verify weak windows through audio energy |
| plan | copy a template and fill source ranges, corrections and identity slots |
| QA | run stutter/dead-air review, contact-sheet check, audio pass and privacy pass |

## What We Take

- `avconvert -p Preset1920x1080` as the canonical iPhone color front-end;
- `pipeline_check.sh` before creative work;
- copy-fill-run build/render templates;
- RMS dead-air review for pauses hidden by background noise;
- stutter review from long words, near-repeats and intra-piece pauses;
- replaceable cover/finale/music/sticker slots;
- final re-check by frames and transcript.

## What This Pack Adds To The OS

- maps template plans into `source_blocks[]` and `cuts[]`;
- treats captions as an overlay lane;
- keeps account identity in private config;
- adds public-safe branch packaging;
- routes survivor findings back to skills.

## Public Boundary

Keep creator footage, real captions, account assets, handles, local paths, raw transcripts and private project names outside this branch. Contribute mechanics only.
