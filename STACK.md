# Reels Stack Configurator – stack spec

> Public stack specification for agentic short-form editing: source audit → ingest → transcript → source blocks → EDL → overlay lanes → render → QA → publish loop.

- **Live dashboard:** https://apowall.github.io/reels-pipeline-os/
- **Repo:** https://github.com/aPoWall/reels-pipeline-os
- **Last rebuilt:** 2026-06-25

This document is the downloadable operating spec behind the dashboard. It is intentionally public-safe: examples are generic, branch names are generic, and raw media/transcripts stay outside the repo.

## 1. Core Thesis

A Reels pipeline should choose the **editing contract** before it renders.

The reusable state is:

```text
source audit -> ingest/color -> transcript/timing -> source blocks -> EDL state -> overlay lanes -> render route -> QA -> publish loop
```

The MP4 is an output. The durable artifact is the recipe: source blocks, cut map, overlay tracks, QA decisions and branch status.

## 2. Decision Order

### 2.1 Source

| source | default pack | notes |
|--------|--------------|-------|
| `iphone_single` | `portable-iphone-kit` | iPhone `.MOV`, HLG/bt2020, one creator |
| `multi_take` | `edl-block-editor` | repeated takes, good/bad doubles, reusable cut map |
| `collab` | `collab-spine` / `creator-collab-challenge` | interruptions and handoffs are story beats |
| `screen_system` | `proof-flow` | terminal/agent/proof overlays with redaction |
| `public_reference` | `reference-echo` | borrow mechanics, keep own footage and copy |
| `public_package` | `public-dashboard` | open-source branch recipe and docs |

### 2.2 Goal

| goal | output |
|------|--------|
| `publish_fast` | fastest safe MP4 + QA |
| `reusable_recipe` | EDL + branch matrix |
| `manual_review` | block/timeline state + contact sheet |
| `public_branch` | sanitized README + sample EDL |
| `challenge` | metrics schema + collab EDL |
| `skill_update` | survivor notes written back into skill memory |

### 2.3 Mode

Pick the editorial mode before the visual style:

| mode | use when | default surface |
|------|----------|-----------------|
| `dry_preserve` | raw take already works | clean trim, almost no overlay |
| `caption_only` | social default | sparse bottom or top captions |
| `proof_overlay` | evidence matters | one proof lane, cropped/redacted |
| `visible_flow` | process is the point | rail / graph / sequence |
| `social_short` | punch and CTA carry the reel | tight cut and minimal copy |
| `experimental` | deliberate exploration | one wild branch with clean baseline preserved |

## 3. Pack Catalog

### 3.1 `portable-iphone-kit`

Use for one-creator iPhone footage and a fast production handoff.

**Borrowed mechanics from `Voronik1801/reel_pipline`:**

- `pipeline_check.sh` before creative work;
- Apple `avconvert -p Preset1920x1080` for iPhone HLG/bt2020 → SDR bt709;
- copy-fill-run templates (`build_TEMPLATE.py`, `make_reel_TEMPLATE.py`);
- stutter review via long words, pauses and near-repeats;
- RMS dead-air detector;
- replaceable subtitle, cover, finale, music and sticker slots;
- final QA by frames and re-transcription.

**Artifacts:**

- `source_map.md`
- `raw_sdr/*.mov`
- `transcripts/*.json`
- `edit_decision_list.json`
- `QA.md`
- `renders/final.mp4`

**Canon:**

```bash
ffprobe -v error -select_streams v -show_entries stream=color_transfer,color_primaries,color_space -of default=nw=1 raw/SHOT.MOV
ffprobe -v error -select_streams v -show_entries side_data=rotation -of default=nw=1 raw/SHOT.MOV
avconvert -p Preset1920x1080 -s raw/SHOT.MOV -o raw_sdr/SHOT.mov --replace
```

Then grade/render from SDR and tag exports as bt709.

### 3.2 `edl-block-editor`

Use when the edit should be reusable and inspectable.

**Adds:**

- source blocks;
- `cuts[]` as single source of truth;
- independent `overlay_tracks[]`;
- branch surface: `clean`, `captions`, `micro`, `low_rail`, `soft_card`, `challenge`;
- preview state for `reel-block-edit` / Palmier-style UI;
- render adapters for ffmpeg, Remotion or browser.

### 3.3 `inside-insanity`

Use for Alex personal Instagram reels.

**Rules:**

- one audio spine by default;
- real physical footage first;
- terminal/protocol layer as controlled friction;
- lowercase captions and sparse overlays;
- Saved Messages preview before any public post;
- survivor patterns are written back into `inside-insanity` and `reel-edit`.

### 3.4 `collab-spine`

Use for creator exchanges where the story lives in the handoff.

**Contract:**

```json
{
  "collabBeats": [
    {
      "id": "beat_01",
      "speaker": "creator_a",
      "start": 0.0,
      "end": 2.8,
      "type": "handoff | interruption | acknowledgement | response",
      "preserve_reason": "sets the next turn"
    }
  ]
}
```

Public branch files can say `creator_a`, `creator_b`, `turn`, `hook`, `handoff`, `motif_insert`. They must not include real handles, private screenshots, raw transcript text or local paths.

### 3.5 `creator-collab-challenge`

Use when the collab is also a public growth experiment.

**Metrics:**

| metric | meaning |
|--------|---------|
| `followers_start` | baseline follower count |
| `followers_current` | current public count |
| `followers_delta` | current minus baseline |
| `content_count` | posts/reels inside the window |
| `prep_minutes` | preparation time per artifact |
| `paid_ads_allowed` | default `false` |
| `trial_results` | aggregated Trial Reels summary |

### 3.6 `proof-flow`

Use for terminal, product, agent, workflow and proof reels.

**Rules:**

- crop and redact before render;
- one proof lane per beat;
- no secrets, tokens, emails or private links;
- viewer-facing text only;
- proof must be readable on mobile.

### 3.7 `reference-echo`

Use when a public reference gives a useful visual grammar.

**Contract:**

- extract mechanics: frame, contrast, caption density, pacing, transition logic;
- use own footage and own words;
- one borrowed mechanic per branch;
- document source attribution in private notes or public-safe sources tab.

### 3.8 `public-dashboard`

Use for open-source packaging.

**Artifacts:**

- `index.html`
- `STACK.md`
- `CONTRIBUTING.md`
- `branches/*/README.md`
- `branches/*/edit_decision_list.json`
- `assets/og-cover.png`

## 4. EDL Schema

Generic public-safe shape:

```json
{
  "schema": "reel.edl.v3",
  "source": {
    "kind": "iphone_single | multi_take | collab | screen_system | public_reference",
    "media": "private/source.mov",
    "public_name": "source_take_a"
  },
  "strategy": {
    "pack": "portable-iphone-kit",
    "mode": "caption_only",
    "style": "minimal_caption",
    "audio_policy": "single_source"
  },
  "source_blocks": [
    {
      "id": "b01",
      "role": "hook",
      "summary": "public-safe summary",
      "sourceStart": 0.0,
      "sourceEnd": 3.8
    }
  ],
  "cuts": [
    {
      "id": "c01",
      "block": "b01",
      "sourceStart": 0.0,
      "sourceEnd": 3.8,
      "outStart": 0.0,
      "outEnd": 3.8,
      "risk": "baseline | insert | experimental | safe-zone-watch"
    }
  ],
  "overlay_tracks": [
    {
      "id": "captions",
      "type": "caption",
      "items": [
        {
          "start": 0.4,
          "end": 2.6,
          "text": "viewer-facing caption",
          "safeZone": "lower-third"
        }
      ]
    }
  ],
  "render": {
    "route": "ffmpeg | remotion | browser | mcp-video",
    "outputs": {
      "review": "renders/review.mp4",
      "final": "renders/final_1080x1920.mp4"
    },
    "video": {"width": 1080, "height": 1920, "fps": 24, "colorspace": "bt709"},
    "audio": {"codec": "aac", "channels": 2, "sampleRate": 48000, "targetLufs": -16}
  },
  "qa": {
    "ffprobe": "pending",
    "contact_sheet": "pending",
    "listening_pass": "pending",
    "privacy_pass": "pending"
  }
}
```

## 5. Overlay Lane Contract

| lane | rule |
|------|------|
| `caption` | one short phrase per beat; viewer-facing |
| `card` | one conceptual beat; no permanent dashboard wallpaper |
| `rail` | process sequence, thin lower/side lane |
| `stamp` | small context marker |
| `proof` | only for proof mode; redacted |
| `cover` | first-frame or first-2s hook surface |

On-screen copy must not include internal terms like `EDL`, `branch`, `render`, `segment`, file paths, source filenames or implementation notes.

## 6. Audio Policy

- Default: one continuous voice master.
- Cross-take speech splice: explicit experimental branch only.
- Preserve a clean baseline before experiments.
- Listen to every speech seam.
- Avoid per-segment filters on iPhone MOV.
- Reject silent or suspicious AAC output.

## 7. QA Gate

### Preflight

```bash
command -v ffmpeg
command -v ffprobe
command -v avconvert
python3 -c "import PIL, numpy"
```

### Render

- 1080×1920 vertical output;
- fps stable;
- `yuv420p`;
- SDR `bt709` tags;
- AAC stereo 48k;
- target loudness around −16 LUFS;
- contact sheet for hook, overlay, seam and ending;
- listening pass;
- privacy pass.

### Public Packaging

Public files must exclude:

- raw footage;
- private transcript text;
- real names and handles;
- screenshots with private UI;
- local paths and filenames;
- provider keys;
- chat IDs and internal CRM details.

## 8. External Research Synthesis

EXA MCP research and primary docs point to a converging pattern:

| source | useful role |
|--------|-------------|
| [OpenTimelineIO](https://opentimelineio.readthedocs.io/en/stable/) | editorial interchange: clips, timing, tracks, markers, metadata, external media refs |
| [Remotion timeline docs](https://www.remotion.dev/docs/building-a-timeline) | typed tracks/items and Player sync |
| [Remotion render docs](https://www.remotion.dev/docs/timeline/render) | render the same JSON-serializable timeline state through `inputProps` |
| [OpenReel Video](https://github.com/Augani/openreel-video) | browser-local WebCodecs/WebGPU editor direction |
| [Omniclip](https://github.com/omni-media/omniclip) | local browser editing, WebCodecs, no cloud upload |
| [X-Cut](https://github.com/MeiGen-AI/X-Cut) | chat-driven video agent with multi-track timeline |
| [mcp-video](https://github.com/KyaniteLabs/mcp-video) | MCP surface for ffmpeg edits and Remotion creation |
| [ReelForge](https://github.com/pedro199288/reelforge) | short-form automation: silence, captions, take selection, effects |
| [Premotion](https://github.com/arach/premotion) | review notes → agent plan → source rewrite → render |

Practical conclusion: keep the state model small and portable, then route rendering by use case.

## 9. Minimal Agent Prompt

```text
Read STACK.md and the selected branch README.
Choose source, goal and mode.
Run preflight.
Build a public-safe EDL.
Render a review MP4.
Run QA.
Keep private media, transcript text, names, handles, local paths and keys out of public artifacts.
Write the survivor back into the relevant skill.
```
