# s__thrift_store — "Stone Town Archive"

Instagram 9:16 ad package. Vintage secondhand shop, Stone Town, Zanzibar.
Handle: `@s__thrift_store` (also tags `@summer_thrift_store`).

Direction chosen by the client: **whole-store brand vibe**, **90s mail-order
catalogue** register. Generated on Higgsfield from five reference photos of the
real shop and its stock.

## Locked visual system

Every asset in this campaign uses these exact values. This is what makes the
carousel and the video read as one shoot.

| Slot | Value |
|---|---|
| Palette | sun-bleached bone white, warm ochre lime-washed plaster, faded terracotta, dusty rose, weathered teak brown |
| Surface | chalky lime-washed ochre plaster with worn patina and hairline cracks; weathered teak and driftwood shelving; woven sisal matting |
| Lighting | hard equatorial midday sun raking 45° camera-left through a shuttered wooden window, warm bounce off the plaster, deep shadow side with selective fill, 4200K |
| Camera | eye-level, locked; 35mm natural at f/4, medium depth of field |
| Composition | centred subject, asymmetric off-centre prop weight in the lower third |
| Register | 400-speed colour negative grain, softened contrast, lifted milky blacks, halation on speculars, warm blown highlights, faintly cyan shadows, un-retouched |

No typography is baked into any image. Text is overlaid in-app so it stays
editable and never warps.

## The inventory in frame

| Item | Detail | Price |
|---|---|---|
| GUESS 1981 belt | two-tone tooled leather, cream base with cognac Aztec chevron inlay, large polished silver oval buckle with rhinestone border | $18 |
| Lacoste tennis shorts | vintage late-70s/early-80s, cream cotton piqué, centre pleat, two pockets, side hem vents | $15 |
| Accessories | gold knotted-loop clip-on earrings, white cat-eye sunglasses with lilac lenses, leather-strap watches, shell and aqua-stone necklaces, navy caps, felt beret | — |

## Product sheet — 5 slides, 9:16, 2K

Shot at 9:16 so every slide doubles as a keyframe for the video ad and works as
a Story/Reels static.

| # | Slide | Job ID |
|---|---|---|
| 1 | Cover — shop interior, bamboo rail, styled dress form | `f0456f95-ab5d-4df8-a462-268a4e0b5578` |
| 2 | The belt — hero still on teak | `07d204b2-bb85-4c25-b748-417743d64fd6` |
| 3 | The tennis shorts — hanging | `00919cba-a651-45ea-a798-d8c9d69d6d8a` |
| 4 | Accessories ledge | `f60b5785-d000-4d5f-82d0-7bd1885d5c55` |
| 5 | Closer — full styled look, belt cinched | `5e5010d7-b1b7-40b2-b2e1-d7e7b5cb3ad1` |

Slide 3 was first rendered at 704×1524 — off-ratio and under-resolution against
the other four. Re-run on 2026-09-05 with the full-bleed framing block in
`prompts/slide-3-shorts.md`; it now matches the set at 1536×2752. The superseded
job was `3419aac1-5ee5-43d2-bfce-762a80a5a8b3`.

All five slides are 1536×2752, 9:16. None has been visually inspected — CDN
egress is blocked from the build environment, so dimensions come from job
metadata, not from the pixels.

## Video ad — delivered

Job `5037b87d-74a7-402d-84c7-396dfd6502aa` · 1080×1920 · 10s · silent ·
`kling3_0` `pro` · 17.5 credits.

### Shot plan — 8s, 9:16

Option A, 10s, two keyframes:

| Beat | Time | Keyframe | Motion |
|---|---|---|---|
| Hook | 0.0–4.0s | slide 2 (belt) — `start_image` | slow push-in on the buckle, specular highlight travelling across the polished plate as dust drifts through the light shaft |
| Transition | 4.0–6.5s | interpolated | the frame lifts and opens off the shelf toward the doorway |
| Payoff | 6.5–10.0s | slide 5 (closer) — `end_image` | settles on the styled look with the buckle at centre, doorway light blooming |

### Model choice

The sheet was generated on `nano_banana_pro` because the `product-photoshoot`
workflow hard-locks that model. Note the backend silently served the jobs on
`nano_banana_2`.

Video model: **`kling3_0`** (client's choice).

| Model | 9:16 | Duration | Keyframe slots | Notes |
|---|---|---|---|---|
| **`kling3_0`** (chosen) | yes | 3-15s | `start_image`, `end_image` **only** | `pro` mode. No `image_references` slot, so a middle keyframe cannot be passed. |
| `seedance_2_5` | yes | 4-30s | `start_image`, `end_image`, `image_references` | Would have carried all three keyframes in one call. |
| `marketing_studio_video` | yes | **12-15s only** | — | Cannot produce a sub-12s cut. |

### Consequence of the Kling switch

Kling interpolates between exactly two keyframes. The accessories ledge (slide 4)
cannot ride along as a middle reference. Two ways to run it:

**Option A — single call, two beats (default).** 10s, slide 2 to slide 5.
The belt opens, the styled look closes. The accessories beat is dropped from
the video and lives only in the carousel. Costs 1 generation.

**Option B — two calls, three beats.** 5s slide 2 to slide 4, then 5s slide 4
to slide 5, cut together in post. Preserves the original arc. Costs 2
generations plus an assembly step.

### Generation call (Option A)

```
model: kling3_0
aspect_ratio: 9:16
duration: 10
mode: pro
sound: off
medias:
  - { value: 07d204b2-bb85-4c25-b748-417743d64fd6, role: start_image }   # slide 2, belt
  - { value: 5e5010d7-b1b7-40b2-b2e1-d7e7b5cb3ad1, role: end_image }     # slide 5, closer
prompt: see prompts/video-ad.md
```

`sound: off` is deliberate — the model's own note says it lowers credit cost,
and the audio direction below calls for a licensed track added in post, so
generated audio would only be discarded.

Daily generation cap on this grace-period account is **5**, confirmed
account-wide rather than per-model (Seedream 4.5 was refused with the identical
error as Nano Banana Pro). Option A plus the slide-3 re-run spends 2 of 5.

### If you want the stills on a different model

Do not re-run slide 3 alone on another model — it would break the carousel's
visual coherence against the four `nano_banana_2` slides. Regenerate the whole
five-slide set on one model instead: `soul_2` (fashion editorial),
`seedream_v4_5` (4K, fabric and metal detail), or `flux_2` variant `max`.

## Copy

Overlay in-app. Nothing here is baked into the pixels.

**Hooks (pick one, 0–1.5s):**
- Everything in this shop already had a life.
- 1981. Still going.
- Thrifted in Stone Town.

**Body (3–5s):** Vintage GUESS belt — $18. Lacoste tennis shorts — $15.

**CTA (last 1.5s):** DM to claim · @s__thrift_store · Stone Town, Zanzibar

**Caption:**
> Everything here already had a life. Vintage GUESS 1981 belt, $18. Late-70s
> Lacoste tennis shorts, $15. Gold knots, cat-eye frames, shell strands — all
> one-of-one, all in store now. DM to claim before someone else does.
> 📍 Stone Town, Zanzibar · @summer_thrift_store

**Audio:** slow warm soul or a dusty 90s R&B loop. Cut the beat on the push-in
at 2.5s and again at 5.5s so the shot changes land on the downbeat.

## Run log

| Date (UTC) | Event |
|---|---|
| 2026-09-04 20:26 | Five sheet slides generated, 2 credits each. Slide 3 off-ratio. |
| 2026-09-04 20:27 | Daily generation cap hit at 5 generations. Slide-3 re-run refused. |
| 2026-09-04 21:19 | Cap confirmed still active 53 min later — not a short rolling window. |
| 2026-09-05 00:06 | Cap reset at **00:00 UTC**. Slide 3 re-run, correct at 1536×2752. |
| 2026-09-05 00:07 | Video ad generated on `kling3_0` `pro`, 17.5 credits. |

**Daily cap: 5 generations, resets at 00:00 UTC.** Confirmed account-wide, not
per-model — Seedream 4.5 was refused with the identical error as Nano Banana Pro.
Total campaign spend: 27.5 credits.
