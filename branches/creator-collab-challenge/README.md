# creator-collab challenge

Public-safe branch pack for a two-creator Reels challenge.

## Use When

- a collab reel should keep the live exchange alive;
- a public creator-growth challenge needs a reusable metric schema;
- the private agreement, real handles and raw footage stay outside the open repo.

## Pack Contract

| layer | contract |
|------|----------|
| challenge | track public aggregate metrics and `paid_ads_allowed:false` |
| turn map | mark each handoff, interruption and acknowledgement |
| EDL | preserve live turns as story beats |
| overlay | use direct viewer-facing captions and a thin challenge rail |
| QA | check interruptions at 1x, privacy boundary and visual clarity |

## Metrics

- `followers_start`
- `followers_current`
- `followers_delta`
- `content_count`
- `prep_minutes`
- `publish_window`
- `trial_results`
- `paid_ads_allowed`

## Visual Recipe

- open with a real motif object or scene action;
- use direct typography over the image;
- keep one yellow semantic keyword when useful;
- keep faces, hands, objects and action clear;
- keep production labels, local paths, source filenames and private chat text out of frame.

## Public Boundary

Use `creator_a`, `creator_b`, `hook`, `turn`, `handoff`, `motif_insert` and aggregate public counts. Keep private partner agreement, account names, raw footage, source captions and baseline screenshots outside this repo.
