# Contributing to AI Mindset Reels Pipeline

This is an **open community stack**. Add your branch, your example, or your improvement to a layer — and send it back so it lives next to everyone else's.

## What you can contribute

1. **A branch** — a new editing recipe (a different overlay-density branch, a new style, a different audio policy). Ship it as a generic EDL + a one-paragraph description.
2. **A stack comparison** — compare your montage contour against ours (or against Max's). The dashboard is a forkable Shaper template; add your side.
3. **A layer improvement** — a better transcript step, a faster draft encoder, a new overlay lane type, an OTIO export, a browser-preview prototype.
4. **An example** — a worked recipe (source blocks → EDL → overlay lanes → QA) showing your style.

## The one hard rule: no personal data in public

This repo is **public**. Contributions must contain **mechanics only**, never personal content:

- ❌ no real footage (no `.mp4`/`.mov` of real people), no contact sheets / frame strips of faces
- ❌ no real names, handles, or nicknames (yours or anyone's)
- ❌ no real on-screen caption text, no personal monologue content in `cuts[].note` or overlay segments
- ❌ no local file paths, project names, or source filenames that identify a recording
- ✅ generic rhetorical labels only: `hook / tension / turn / setting / punch / theme / closer`
- ✅ generic overlay segment labels: `caption / highlight / card / term / rail / stamp`
- ✅ structure, timings, schema, contracts — yes; the actual words said in the reel — no

If your branch was built from a real recording, **strip it to structure** before opening a PR (rename labels to generic, remove footage, replace caption text with `caption`/`highlight`). Keep the real version in your own private repo.

## How to submit

```bash
# 1. fork github.com/aPoWall/reel-stack-compare
# 2. add your branch as a generic EDL
mkdir -p branches/<your-handle>
cp your_edit.json branches/<your-handle>/edit_decision_list.json   # sanitized!
# 3. add a row describing it
#    branches/<your-handle>/README.md — one paragraph: what the branch does, when to use it
# 4. (optional) add your side to the dashboard comparison
# 5. open a PR
```

### Branch EDL format

Use the generic schema from [STACK.md](STACK.md#edl-schema-generic-example). Minimum:

```json
{
  "schema": "reels.edl.v1",
  "strategy": { "mode": "<your-mode>", "style": "<your-style>", "audio_policy": "single_source" },
  "cuts": [ { "label": "hook", "sourceStart": 0.0, "sourceEnd": 3.7, "note": "cold open" } ],
  "overlay_tracks": [ { "type": "caption", "lane": "captions", "from": 0.8, "to": 44.0 } ],
  "render": { "outputs": [ { "label": "final", "res": "1080x1920", "encoder": "libx264", "crf": 18 } ] }
}
```

## Style

The dashboard is **Shaper** aesthetic: pure B&W, JetBrains Mono, 1px frames, lowercase, inversion = active. No color, no Tailwind, no emoji in headings. Keep contributions in register.

## Review

PRs are reviewed for: (1) the no-personal-data rule, (2) a clear one-paragraph description, (3) a valid generic EDL. That's it — low friction, high signal.

Born in **AI Mindset {space}**. Agent + human keep the stack together.
