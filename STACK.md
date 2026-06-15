# AI Mindset Reels Pipeline — the full stack

> An open, community stack for **agentic short-form (Reels) editing**. Not one "correct" pipeline — a stack you fork, adapt, and send your branch back to.

This file is the downloadable export of the whole stack: every layer, the tools, the contracts between layers, the EDL schema, the skills, and how to run it. All examples are **generic** — no personal footage, names, or content (see [CONTRIBUTING](CONTRIBUTING.md) for the privacy rule).

- **Live dashboard:** https://apowall.github.io/reel-stack-compare/
- **Repo (fork me):** https://github.com/aPoWall/reel-stack-compare

---

## Where this came from

It started inside **AI Mindset {space}** as a format called **stack compare**: take your montage contour, set it next to someone else's, and steal the best parts. The first comparison was against **Max Postnikov's** stack (a deterministic long-form YouTube cut-chain). That surfaced what to borrow (cut-plan-as-SSOT, draft→final, ElevenLabs Scribe) and what stays our moat (visual block editor, mode/style library, audio-first lip-sync).

From there it grew into a reusable **pipeline package**: a worked example (the "answer-card" recipe), a family of overlay-density branches, and an open contribution model so other people's stacks and branches live next to ours.

**Thesis:** *a short-form editor should store the **recipe** — source blocks + EDL + overlay lanes + QA/audio — not just the final mp4. Then a style can be pinned, reproduced, and forked.*

---

## The stack, layer by layer

```
transcript → source blocks → EDL (SSOT) → overlay lanes → draft → QA → final → publish
```

Each layer has a **tool** and a **contract** (what it must guarantee to the next layer).

| # | layer | tool | contract |
|---|-------|------|----------|
| 0 | **transcript** | ElevenLabs Scribe (word-level) · Whisper/WhisperX local fallback | word-level timecodes; cuts only land in pauses, never mid-word |
| 1 | **source blocks** | transcript + manual editorial map | rhetorical blocks (hook/tension/turn/setting/punch/theme/closer); each block is a self-contained thought from one source |
| 2 | **EDL (SSOT)** | `edit_decision_list.json` | the single source of truth: every cut + overlay decision lives here, not in code; draft and final render from the same file |
| 3 | **overlay lanes** | overlay renderer (ffmpeg + OpenCV/Pillow) | lanes (`caption`/`card`/`rail`/`stamp`) are **independent of the cut map**; on-screen copy stays viewer-facing — no proof/render/EDL labels in frame |
| 4 | **draft** | ffmpeg (videotoolbox HW, low-res) | cheap fast preview from the EDL to check cut/timing before the hero render |
| 5 | **QA** | ffprobe + contact sheet | resolution/fps/audio checks; frame-strip sanity at hook/card/rail/end; safe-zone pass; gate against silent audio / tiny AAC bitrate |
| 6 | **final** | ffmpeg (libx264 CRF) | 1080×1920, one continuous voice master (AAC stereo 48kHz), loudnorm; audio-first lip-sync (stream-copy, no per-segment filters) |
| 7 | **publish** | Instagram Trial Reels (A/B) | publish two versions when goals differ; watch-time, not views; deterministic metadata |

**Audio-first rule (critical):** one reel = one continuous audio source. Visual variety comes from swapping *video* in speech pauses (visual swap), never from cutting speech across takes. A/B audio alternation is the #1 cause of bad reels.

---

## EDL schema (generic example)

The Edit Decision List is the heart of the stack. Generic shape:

```json
{
  "schema": "reels.edl.v1",
  "source": "source/take.mov",
  "strategy": {
    "mode": "caption_only_soft",
    "style": "answer_card_terminal_caption",
    "audio_policy": "single_source_multi_take_remix_soft_boundaries"
  },
  "cuts": [
    { "label": "hook",    "sourceStart": 0.00, "sourceEnd": 3.70, "note": "cold open" },
    { "label": "tension", "sourceStart": 4.10, "sourceEnd": 12.3, "note": "setup" },
    { "label": "punch",   "sourceStart": 22.0, "sourceEnd": 23.5, "note": "one-liner", "insert": true }
  ],
  "overlay_tracks": [
    { "type": "card",    "lane": "hook card",   "from": 0.25, "to": 5.35 },
    { "type": "caption", "lane": "captions",    "from": 0.80, "to": 44.0 },
    { "type": "rail",    "lane": "terminal",    "from": 21.0, "to": 30.0 }
  ],
  "render": {
    "outputs": [
      { "label": "draft", "res": "360p", "encoder": "videotoolbox" },
      { "label": "final", "res": "1080x1920", "encoder": "libx264", "crf": 18 }
    ]
  }
}
```

Notes:
- `cuts[]` carry `sourceStart`/`sourceEnd` so the same map renders by MoviePy, ffmpeg, or (later) Remotion/OTIO.
- `insert: true` marks a cross-take splice — only allowed in a branch explicitly tagged experimental, with a clean baseline preserved (`single_source_multi_take_remix` policy).
- `overlay_tracks[]` are independent lanes; they do not move when the cut map changes.

---

## Branch surface — one cut map, many overlay densities

A single soft cut map can carry a ladder of overlay densities — pick the branch by goal:

| branch | overlays |
|--------|----------|
| `clean`     | none — soft phrase-level cuts, stereo master (baseline) |
| `captions`  | captions only |
| `micro`     | tiny stamp + sparse captions |
| `low_rail`  | low-opacity timeline rail + sparse captions |
| `soft_card` | compact low-opacity card (most explicit) |

KPI-aware (from short-form research): **seamless cuts → likes (fluency)**, **overlapping cuts → completion/rewatch (prediction-error)**. Match the cut to the KPI; publish two versions when goals differ and A/B them.

---

## The skills (how the agent runs it)

| skill | owns | version |
|-------|------|---------|
| **`reel-edit`** | video/EDL state: transcription, smart cut, modes/styles, presets, audio-first render, draft→final | 3.8 |
| **`reel-block-edit`** | branch surface as a UI pattern: EDL strip, overlay lanes as first-class tracks, `realself.timeline.json` manifest, dashboard export | 1.6 |
| **`stack-compare`** | the Shaper comparison artifact: build a B&W dashboard comparing two stacks, with a live EDL layer and research matrix | 1.1 |

Division of labor: `reel-edit` owns the video state; `stack-compare` owns the Shaper artifact. The agent and human argue at the level of **blocks, branches, and overlay lanes** — a shared, reproducible editing state.

---

## What we borrowed from Max + roadmap

- **Borrowed:** montage plan as SSOT (→ OpenTimelineIO), draft→final two-tier render, ElevenLabs Scribe for word timestamps, source-range discipline.
- **Not ported:** long-form YouTube deterministic chain, heavy asset pipeline, YouTube API — Reels need branch surface and fast preview.
- **Roadmap:** `realself.timeline.v1` → OTIO `.otio`; WebCodecs + Mediabunny live preview in the block editor; ProRes-4444 alpha overlay lane; cloud render via Remotion Lambda or Hyperframes (HTML-native, agent-first).

Full technology survey (12 EXA queries: Remotion, OTIO, Hyperframes, X-Cut, CutAgent, libass vs HTML caption engines, multi-take selection, jump-cut engagement study, …) lives in the dashboard's **технологии** tab.

---

## Run it

This repo ships the **dashboard** (a single static `index.html`) and this stack doc. The editing skills (`reel-edit`, `reel-block-edit`, `stack-compare`) drive the actual rendering inside Claude Code / Codex. To reproduce the pipeline you need: ffmpeg (with libx264), Python (OpenCV, Pillow), a transcription provider (Scribe or local Whisper), and the skills.

To **fork the dashboard** as your own stack-compare artifact, copy `index.html`, edit the header/thesis and the two pipeline columns, keep the Shaper design tokens. See the dashboard README.

To **contribute your branch**, see [CONTRIBUTING.md](CONTRIBUTING.md).
