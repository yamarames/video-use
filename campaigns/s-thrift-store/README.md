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
| 3 | The tennis shorts — hanging | `3419aac1-5ee5-43d2-bfce-762a80a5a8b3` |
| 4 | Accessories ledge | `f60b5785-d000-4d5f-82d0-7bd1885d5c55` |
| 5 | Closer — full styled look, belt cinched | `5e5010d7-b1b7-40b2-b2e1-d7e7b5cb3ad1` |

**Known defect:** slide 3 rendered at 704×1524 instead of 1536×2752. Off-ratio
and under-resolution against the other four. Re-run pending — the prompt in
`prompts/slide-3-shorts.md` adds explicit full-bleed framing and anti-letterbox
negatives to correct it.

## Video ad — ready to generate

Not yet generated: the Higgsfield account hit its daily generation limit during
the grace period. Everything below is final and needs one call.

### Shot plan — 8s, 9:16

| Beat | Time | Source keyframe | Motion |
|---|---|---|---|
| Hook | 0.0–2.5s | slide 2 (belt) | slow push-in on the buckle, specular highlight travelling across the polished plate as dust drifts through the light shaft |
| Body | 2.5–5.5s | slide 4 (accessories) | gentle lateral dolly right along the driftwood ledge, rack focus from the gold earrings back to the sunglasses |
| Payoff | 5.5–8.0s | slide 5 (closer) | slow tilt up the styled look settling on the buckle at centre, doorway light blooming |

### Model choice

The sheet was generated on `nano_banana_pro` because the `product-photoshoot`
workflow hard-locks that model. Note the backend silently served the jobs on
`nano_banana_2`. For the video the model is an open choice:

| Model | 9:16 | Duration | Notes |
|---|---|---|---|
| **`seedance_2_5`** (recommended) | yes | 4–30s | `omni_reference` mode accepts multiple `image_references`, so all three keyframes drive **one** call. 1080p, audio on. |
| `kling3_0` | yes | 3–15s | `start_image` + `end_image` only. `pro` mode. Tighter camera control, less keyframe flexibility. |
| `marketing_studio_video` | yes | **12–15s only** | Ad-native presets, but cannot produce an 8s cut. Only viable if the ad is stretched to 12s. |
| `grok_video_v15` | — | 2–15s | Start-frame + audio reference. Fallback. |

### Generation call

```
model: seedance_2_5
mode: omni_reference
aspect_ratio: 9:16
resolution: 1080p
duration: 10
generate_audio: true
medias:
  - { value: <slide-2 job id>, role: start_image }
  - { value: <slide-4 job id>, role: image_references }
  - { value: <slide-5 job id>, role: end_image }
prompt: see prompts/video-ad.md
```

One call, one cohesive cut. This matters because of the **daily generation cap
of 5** on the grace-period account — confirmed account-wide, not per-model
(Seedream 4.5 was refused with the identical error). A one-call video plus the
slide-3 re-run spends 2 of 5.

### If you want the stills on a different model

Do not re-run slide 3 alone on another model — it would break the carousel's
visual coherence against the four `nano_banana_2` slides. Regenerate the whole
five-slide set on one model instead:

- **`soul_2`** (Higgsfield Soul 2.0) — built for realistic fashion editorial;
  closest match to this brief. Caveat: max 1 image reference, role `image`.
- **`seedream_v4_5`** — 4K, precise control, strongest on fabric and metal detail.
- **`flux_2`** variant `max` — best prompt adherence if the framing keeps drifting.

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
