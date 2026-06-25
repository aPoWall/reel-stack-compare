# Contributing to Reels Stack Configurator

This repo is a public configurator for montage stacks. Contribute **branch packs**, **EDL schemas**, **QA checks**, **render routes** or **sanitized worked recipes**.

## What To Contribute

1. **Branch pack** – a reusable source/goal/mode recipe with tools, artifacts and QA.
2. **Generic EDL** – a public-safe `edit_decision_list.json`.
3. **Layer improvement** – better ingest, transcript, EDL, overlay, render or QA step.
4. **Render route** – ffmpeg, Remotion, browser/WebCodecs, MCP or cloud render adapter.
5. **Challenge mechanic** – aggregate metrics + sanitized collab recipe.
6. **Portable production kit** – dependency check, templates, platform-specific normalization and QA scripts.

## Public Boundary

This repo is public. Contributions store **mechanics only**.

Do not include:

- real footage;
- contact sheets or frame strips with real faces;
- real names, handles, nicknames or account names;
- raw transcript text or real captions;
- local paths, project names or source filenames that identify a recording;
- private Instagram/Telegram screenshots;
- provider keys, chat IDs, emails or internal CRM details.

Use:

- `creator_a`, `creator_b`, `host_creator`, `guest_creator`;
- `hook`, `tension`, `turn`, `proof`, `punch`, `closer`;
- `caption`, `highlight`, `card`, `rail`, `stamp`, `proof`;
- aggregate metrics only.

If the branch came from a real recording, strip it to structure before opening a PR.

## Branch Folder

```bash
branches/<branch-pack>/
  README.md
  edit_decision_list.json
```

`README.md` should answer:

- when to use this pack;
- what source it expects;
- what mode it defaults to;
- what artifacts it creates;
- what QA must pass;
- what stays private.

## EDL Minimum

Use `reel.edl.v3` from [STACK.md](STACK.md#4-edl-schema). Minimum public-safe shape:

```json
{
  "schema": "reel.edl.v3",
  "strategy": {
    "pack": "portable-iphone-kit",
    "mode": "caption_only",
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
      "risk": "baseline"
    }
  ],
  "overlay_tracks": [
    {
      "id": "captions",
      "type": "caption",
      "items": [
        {"start": 0.4, "end": 2.6, "text": "caption", "safeZone": "lower-third"}
      ]
    }
  ],
  "qa": {
    "ffprobe": "required",
    "contact_sheet": "required",
    "listening_pass": "required",
    "privacy_pass": "required"
  }
}
```

## Challenge Branch Minimum

```json
{
  "challenge": {
    "goal": "first_to_10000_followers",
    "paid_ads_allowed": false,
    "publish_window": "public date range",
    "metrics": [
      "followers_start",
      "followers_current",
      "followers_delta",
      "content_count",
      "prep_minutes",
      "trial_results"
    ]
  },
  "collabBeats": [
    {
      "id": "beat_01",
      "type": "handoff",
      "speaker": "creator_a",
      "preserve_reason": "sets the next turn"
    }
  ]
}
```

Keep real agreements, screenshots and private baselines in a private CRM or repo.

## Portable Kit Minimum

```json
{
  "branch": "portable-kit-name",
  "platform": "macOS | browser | linux | cloud",
  "pipeline": [
    {"label": "preflight", "contract": "check tools and folders"},
    {"label": "ingest", "contract": "normalize media safely"},
    {"label": "timing", "contract": "word timing plus audio-energy check"},
    {"label": "qa", "contract": "state checks before final export"}
  ],
  "privacy": "mechanics_only"
}
```

## Dashboard Style

Keep the static dashboard close to Shaper:

- monochrome base;
- restrained accent colors only for data state;
- JetBrains Mono / system monospace;
- 1–2px frames;
- radius 8px or less;
- no decorative gradients;
- no private data in UI text.

## Review Criteria

PR review checks:

- privacy boundary;
- valid JSON;
- clear source/goal/mode;
- reproducible artifacts;
- QA gate;
- useful README.

Low friction, high signal.
