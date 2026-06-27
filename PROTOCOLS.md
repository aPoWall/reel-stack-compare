# Reels Editing Protocols

Public-safe operating protocol for Reels Pipeline OS.

```text
brief -> source -> timing -> story -> EDL -> overlays -> render -> QA -> publish package
```

## Protocol Stack

| id | protocol | output |
|----|----------|--------|
| P0 | brief protocol | goal, audience, source type, mode, privacy boundary, expected artifacts |
| P1 | source protocol | `source_map.md`, ffprobe facts, orientation, fps, audio channels, HDR/SDR decision |
| P2 | timing protocol | transcript JSON, word timings, pause map, dead-air/stutter hints |
| P3 | story protocol | source blocks: hook, setup, tension, proof, turn, closer, tail |
| P4 | EDL protocol | `edit_decision_list.json` with `cuts[]`, `overlay_tracks[]`, `render.outputs`, branch status |
| P5 | overlay protocol | caption/card/rail/proof/stamp/cover lanes, safe-zone rules, viewer-facing copy |
| P6 | render protocol | draft, review MP4, text-free base, final 1080x1920 MP4 |
| P7 | QA protocol | ffprobe, listening pass, first-3s check, safe-zone pass, privacy pass, learning note |

## Soft Communication Layer

| layer | question | editing signal |
|-------|----------|----------------|
| hook | why watch | conflict, question, useful promise or visible proof in the first seconds |
| voice | how it speaks | tone, tempo, pauses, caption density, persona lane and visible process level |
| proof | why believe | screen, object, timeline, before/after, quote-safe source or action inside the frame |
| turn | why finish | reversal, conclusion, CTA, next branch and learning note |

## Community Borrow Map

| source | what to borrow | where it lands |
|--------|----------------|----------------|
| [Voronik1801/reel_pipline](https://github.com/Voronik1801/reel_pipline) | portable iPhone kit, `avconvert`, preflight, templates, stutter/dead-air checks | P1 source, P2 timing, P7 QA |
| [MeiGen-AI/X-Cut](https://github.com/MeiGen-AI/X-Cut) | chat intent, dynamic skill selection, asset analysis, real-time Remotion timeline | P0 brief, P3 story, future editor surface |
| [birchrust/openscript](https://github.com/birchrust/openscript) | MCP tools, EDL v2, multi-track timeline, verification layer | P4 EDL, P7 QA, agent control |
| [heygen-com/hyperframes](https://github.com/heygen-com/hyperframes) | HTML/CSS/media render loop, CLI lint/preview/render, agent skills | P5 overlays, P6 render |
| [jurczykpawel/reelstack](https://github.com/jurczykpawel/reelstack) | CLI/API production stages, provider registry, prompt templates, self-hosting | P0 brief, P6 render, package automation |
| [tsensei/OpenReels](https://github.com/tsensei/OpenReels) | topic-to-short flow: research, script, voiceover, visuals, music, captions, assembly | article-demo pack, idea-to-reel branch |
| [OpenCut-app/OpenCut](https://github.com/OpenCut-app/OpenCut) | open timeline editor direction, plugin/headless surface | future browser review board |
| [OpenTimelineIO](https://github.com/AcademySoftwareFoundation/OpenTimelineIO) | editorial timeline interchange format | EDL export and NLE bridge |

## Public Boundary

Public protocol files may include:

- generic timing ranges;
- source labels like `source_take_a`;
- `cuts[]`, `overlay_tracks[]`, render routes and QA status;
- branch intent and learning notes;
- source links and community attribution.

Public protocol files exclude:

- raw footage;
- private transcript text;
- faces, handles, names and private screenshots;
- local paths and source filenames;
- provider keys or internal agent/session IDs.

## Configurator Output

The configurator should return:

- selected `source`, `goal`, `mode`;
- selected `route`;
- recommended branch pack;
- full protocol stack P0-P7;
- artifacts to create;
- public-safe agent prompt;
- learning note instruction.

## Test Session Routes

| route | owner | expected output |
|-------|-------|-----------------|
| `reel_edit` | `reel-edit` | render-grade spine, review/final MP4, text-free base, QA and learning note |
| `block_timeline` | `reel-block-edit` | source blocks, cut map, overlay events, board state and manual review notes |
| `palmier` | `reel-palmier-board` | editable Palmier board, frame shells, native text clips and winner cutlane |
| `shaper` | `shaper-reels` | monochrome proof/process overlay, safe-zone contact sheet and style reuse note |
| `article_research` | `stack-compare` | public research sessions, source credits, route matrix and article-ready summary |

## Same Source Comparison Protocol

Use one source package for all routes:

- `source_map.md`;
- transcript JSON and pause map;
- baseline audio policy;
- privacy boundary;
- public-safe source labels.

Run each route into `test_runs/<route>/`. Compare:

- first 3 seconds;
- safe-zone readability;
- editability;
- render or board QA;
- public boundary;
- publish readiness;
- learning note quality.
