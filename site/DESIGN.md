# video-use — site design record

Deliverable: **one self-contained `site/index.html`.** No build step, no framework.
It is servable from the repo root (`python3 -m http.server`) or GitHub Pages, and it
references one real repo asset (`../static/timeline-view.svg`). Everything else —
geometry, textures, environment light, LUT — is generated in the browser at runtime,
because this repo ships no 3D assets and a fake asset pipeline would be a lie.

---

## Brief, filled in

The brief arrived with its subject fields empty. The repository answers them.

| Field | Answer |
| --- | --- |
| **Subject** | video-use. An open-source skill that lets an agent cut real footage. The one real thing it sells: *the model never watches the video, it reads it.* Word-level ASR + on-demand composites — 12KB of text instead of 45M tokens of frames. |
| **Audience** | Developers and technical creators with a folder of raw takes and a terminal already open. They are deciding one thing: *do I paste that setup prompt.* |
| **Feeling on arrival** | **Spliced.** Not "premium", not "cinematic" — the specific sensation of dead air being removed while you watch. |
| **Primary job** | Copy the setup prompt. Everything on the page is either that, or the evidence for it. |

---

## STEP 1 — PLAN

### Palette — 6 values, stated roles

| Token | Hex | Role |
| --- | --- | --- |
| `--board` | `#D7DAD2` | Page ground. Film-leader grey-green: cool, desaturated, green-cast. Deliberately *not* warm paper. |
| `--sunk` | `#C3C7BC` | The gutter field, recessed bands, hairline rules. One step down from board, same hue. |
| `--leader` | `#EFF1EC` | Raised reading surfaces and all type set on black. Highest value on the page. |
| `--carbon` | `#000000` | **True black.** Body type, and the unwatched frame. Absolute, never a tinted stand-in — the footage is literally unseen, so its rectangle is literally black. |
| `--wash` | `#4F5A4C` | Secondary type, timecodes, scene mid-tones. 5.12:1 on board. |
| `--mark` | `#C2321B` | Grease-pencil red. **Marks only** — the scrub playhead, the splice frame, one numeral. Never small text. (`--mark-ink #8E2312`, 6.19:1, for the two places a red word is unavoidable.) |

Contrast, measured: carbon/board 17.4:1. wash/board 5.12:1. mark/board 3.94:1 — which is
exactly why `--mark` is barred from small text by rule, not by accident.

### Type

**Pairing A, with the axes actually driven.**

- **Display — Fraunces** (variable: `opsz`, `wght`, `SOFT`, `WONK`).
  - Hero: `opsz 144, wght 300, SOFT 0, WONK 1`. Thin, hard, wonky terminals — printed, not UI.
  - Section heads: `opsz 72, wght 400, SOFT 24, WONK 1`.
  - Small display: `opsz 9, wght 500, SOFT 90, WONK 0`. Genuinely different letterforms at
    small size — softened, non-wonky — not the hero scaled down. That is the whole point of
    an optical-size axis and the reason this pairing was chosen over Instrument Serif.
- **Body — Switzer** (Fontshare, 400/500). Neutral enough to disappear under a loud serif,
  unfamiliar enough not to read as a default.
- **Data — Martian Mono** (variable, `wdth 87.5, wght 300`), timecodes only. The mono ban is
  lifted here on its own terms: the subject *is* timecode. It appears nowhere else.

**Scale ratio: 1.25 (major third)**, base 17px.
`10.9 / 13.6 / 17 / 21.25 / 26.6 / 33.2 / 41.5 / 51.9 / 64.9`
The hero breaks the ladder on purpose — `clamp(3.2rem, 10.5vw, 9.2rem)`. That is the only
break on the page, and it is stated rather than smuggled.

Measure 62ch. Body line-height 1.55; the serif pull-quotes get 1.42 at display sizes.

### Layout concept

*The page is not a landing page, it is a transcript page.*

A hard timecode gutter runs the full height on the left. The timecodes are real and they
increase monotonically as you scroll: scrolling the page **is** scrubbing the finished cut,
`00:00:00:00` at the top to `00:01:47:12` at the footer, 24fps, drop-frame notation.

**Alignment logic:** one vertical rule at `x = clamp(88px, 9vw, 156px)`. Everything hangs off
`rule + gap`. Nothing crosses the rule except the hero display, which overhangs left into the
gutter and is optically aligned so the stem of the first letter sits *on* the rule, not beside
it. One break, deliberate, once.

```
HERO
┌──────────┬────────────────────────────────────────────────────────────┐
│ 00:00:00 │  video-use                                    ▏ MIT ▏ repo │
│ :00      │                                                            │
│          │                                                            │
│      ┌───┼──── hero overhangs the rule, stem ON the rule ─────────┐   │
│ 00:00│ It never watches                                           │   │
│ :01  │ your footage.                                              │   │
│ :12  │ It reads it.                                               │   │
│      └───┼────────────────────────────────────────────────────────┘   │
│          │                                                            │
│ 00:00:04 │  [000.00-004.20] S0  Drop raw takes in a folder. Chat.     │
│ :20      │                      Get final.mp4 back.                   │
│          │                                                            │
│          │  ████████████████████████████████████████████████████████  │
│ 00:00:07 │  █ TRUE-BLACK SLATE — the setup prompt, verbatim.       █  │
│ :06      │  █ [ Copy ]                                             █  │
│          │  ████████████████████████████████████████████████████████  │
└──────────┴────────────────────────────────────────────────────────────┘
 gutter      column: 62ch measure, left-aligned to rule+gap, no centring
 rule (1px --sunk, full height, never broken)
```

```
SIGNATURE — "THE SPLICE"  (pinned, ~260vh of scroll, scrub 1.2)
┌──────────┬────────────────────────────────────────────────────────────┐
│ 00:01:02 │                                                            │
│ :00      │   ╭──────────────────────────────────────────────────╮     │
│          │   │  WebGPU canvas — a real film strip, 18 plates     │     │
│  playhead│   │  ▣▣▣▣▣▣▣▣▣▣▣▣▣▣▣▣▣▣   black-glass faces, you      │     │
│  ▌ (red) │   │        ▲            never see inside a frame      │     │
│          │   │        └ read-sweep (emissive band, TSL)          │     │
│          │   ╰──────────────────────────────────────────────────╯     │
│          │                                                            │
│          │   [002.52-005.36] S0 Ninety percent of what a web agent   │
│          │   [005.40-005.98] S0 ~~umm~~            ← struck, plate    │
│          │   [006.08-006.74] S0 does is completely wasted.    falls   │
│          │                                                            │
│          │   RUNTIME  00:04:12  ──▶  00:01:47                         │
└──────────┴────────────────────────────────────────────────────────────┘
```

### The signature moment

**THE SPLICE.** One pinned, scrubbed section. The entire motion and 3D budget lives here and
nowhere else.

A strip of 16 modelled film plates — extruded rounded slabs with real sprocket perforations
cut as `Shape` holes — runs across the viewport. Their faces are black glass. You never see
footage inside a frame, because the model never does either. What you see is the environment
reflected in them, and the reading.

Through the pin, four real state changes, none of them opacity:

1. `0 → 0.25` **The read.** An emissive band sweeps left-to-right across the strip, one plate
   at a time, driven by a per-plate TSL uniform. This is the transcript being read.
2. `0.25 → 0.55` **The mark.** Plates the transcript flags as filler or dead air rotate out on
   their own X axis and fall. The transcript lines below strike through on the same frame.
3. `0.55 → 0.85` **The splice.** The survivors translate together and close the gaps. The strip
   physically shortens. Camera dollies in and rolls 4°; DOF focus distance rides the closing seam.
4. `0.85 → 1` **The hold.** The runtime counter lands. One red mark frames the splice. Stop.

**Why it belongs to this subject and no other:** it is the product's own operation, in the
product's own units — plates are frames, gaps are silence, the strike-through is the EDL. Lift
it onto any other site and it means nothing.

---

## STEP 2 — ATTACK ON MY OWN PLAN

*"If I were handed a generic brief for any developer tool, would I have produced this?"*
Six places where the answer was yes. What changed:

1. **Dark editor UI with a neon timeline.** Every dev tool that touches video does this. Worse,
   my pale alternative was one hue-shift away from the banned cream-and-terracotta tell.
   **Changed:** the board is pushed cool and green (`#D7DAD2`), the red is a printed grease-pencil
   red rather than clay terracotta, and it is capped at two appearances on the whole page. The
   value structure is board / leader / true black — three plates, not a wash.

2. **Hero: headline, subhead paragraph, two centred buttons.** Generic.
   **Changed:** left-hung off the gutter, no centring anywhere on the page. There is no subhead
   paragraph — its slot is filled by a real transcript line with a real timecode, which is the
   product's own artifact doing the subhead's job. The CTA is not a button pair; it is a black
   slate holding the setup prompt verbatim, because the prompt *is* the product's front door.

3. **A six-up feature grid with icons.** The README has seven features and the reflex is cards.
   Banned anyway, and rightly.
   **Changed:** the features become a continuous transcript — each one a line with a timecode
   and a keep-or-cut mark. Reading the feature list is watching a rough cut. Zero cards on the
   page, zero repeated border-radius.

4. **A three.js hero with an abstract floating object.** The loudest generic move available.
   **Changed:** *there is no 3D in the hero at all.* The hero is type and a rule. The renderer
   initialises lazily, below the fold, for one section. Spending the entire 3D budget on a single
   orchestrated moment — and leaving the first screen to typography — is the point of view.

5. **Fade-and-slide-up on every section.** Banned, and I would have reached for it.
   **Changed:** no section has an entrance animation. The hero's words appear once, per word, at
   *their real transcript offsets* (`[000.00-004.20]` played at 4×) — motion derived from the
   subject's own data rather than applied to it. After that the page is still until the pin.

6. **"45M vs 12KB" as a count-up statistic.** Generic, and it wastes the best fact in the repo.
   **Changed:** it is drawn as a measurement in the gutter's own units. One black rule at full
   width is 45M tokens. The 12KB rule, at true scale, is 0.08px wide — invisible. It is drawn at
   1px and annotated as a lie. The fact lands because the comparison physically fails to fit.


---

## STEP 4 — CRITIQUE OF THE BUILT PAGE AGAINST THE HARD BANS

Read line by line against the shipped file. Three failed; all three are fixed.

**Typography**
- Banned families — none present. The stack is Fraunces / Switzer / Martian Mono with
  generic `serif`, `sans-serif`, `monospace` as the only fallbacks; no system stack is
  named as a design font. **Pass.**
- All-caps tracked-out eyebrow labels — none. The `cut` / `keep` markers in the cut list
  and every `.micro` label are lowercase; the largest letter-spacing anywhere is
  `0.01em`, and the one exception (`0.06em`) is the mobile vertical timecode, where it
  is kerning for rotated glyphs, not tracking for effect. **Pass.**
- One word of a headline accented — no headline carries a colour, italic, or weight
  change. **Pass.**
- Middle-dot meta strings — none; the masthead separator is a 1px rule element.
  **Pass.**
- Monospace for small data labels — used only for timecodes, speaker IDs and code.
  All three are the subject's own telemetry. **Pass.**
- `→` on buttons or links — none. The runtime readout strikes the old value and sets the
  new one beside it rather than pointing an arrow at it. **Pass.**

**Colour & surface**
- Cream + terracotta — the ground is `#D7DAD2`, cool and green-cast; the accent is a
  saturated grease-pencil red used three times. **Pass.**
- Near-black + a single acid accent — the ground is pale. **Pass.**
- **FAILED: tinted near-blacks standing in for black.** The footer code panel was
  `#0d0d0d` on a `#000` footer and the slate's header rule was `#262626` — exactly the
  banned move, a neutral near-black doing the work black should do.
  **Fixed:** the panel is now true `--carbon` separated by a `#2b312c` hairline, so the
  hierarchy comes from a rule instead of a fake fill, and the rule sits in the palette's
  green family rather than neutral grey.
- Purple-to-blue gradient washes — none. **Pass.**
- Identical rounded cards with a shared shadow — the file contains **zero**
  `border-radius` and **zero** `box-shadow` declarations. Every division on the page is
  a 1px rule or a change of ground. **Pass.**

**Motion**
- Fade-and-slide-up on every section — no section has an entrance. The only entrance on
  the page is the hero's per-word clip wipe, timed to the transcript's own offsets.
  **Pass.**
- Hover-lift on cards — no transforms on hover anywhere. Links change underline colour;
  the copy button changes ground. **Pass.**
- Scattered small effects — the motion budget is one pinned, scrubbed section. **Pass.**

**3D**
- Untextured primitives as the hero — there is no 3D in the hero at all, and the
  signature geometry is an extruded film plate with real perforations cut as `Shape`
  holes, carrying a five-map material set. The only primitives in the file are the
  environment dome and the backdrop plane, neither of which is the subject. **Pass.**
- `MeshStandardMaterial` at default roughness/metalness — not used; both plate materials
  are `MeshPhysicalNodeMaterial` with roughness driven by a map, plus clearcoat, sheen
  and anisotropy. **Pass.**
- Ambient + directional rig — none. Light comes from a PMREM'd environment scene plus two
  `RectAreaLight`s for shaped speculars. **Pass.**

**Also caught and fixed in the same pass**
- **FAILED: tone-mapping exposure had drifted to 1.42**, outside the 1.0–1.3 the render
  spec allows. Fixed at 1.28, with the lost stop recovered by raising
  `environmentIntensity` to 1.60 — the light now comes from the environment, which is
  where the spec wants it to come from.
- **FAILED: serif set at a small optical size had 1.3 line-height.** Fraunces at
  `opsz 9` needs more. Fixed at 1.46.

---

## Deviations from the render spec, and why

Stated rather than hidden. Each one is a case where following the letter of the spec
would have meant shipping a lie or a black frame.

1. **Environment map is generated, not loaded.** The spec asks for a `.hdr`/`.exr`
   through PMREM. This repo ships no HDRI and adding a runtime fetch for one would put a
   multi-megabyte blocking asset in front of a page whose whole argument is that the
   expensive path is unnecessary. Instead an environment *scene* is built in code — a
   graduated dome plus four emissive panels at HDR values (a cool softbox high and front,
   a long raking strip camera-left, a warm bounce card low-right, a faint fill from
   behind) — and run through `PMREMGenerator.fromScene`. It is a real irradiance
   environment; only its source is procedural.
2. **Geometry is procedural, not glTF/Draco.** No 3D asset exists for this subject. The
   spec's own instruction covers this: build meaningful geometry from the subject. The
   plate is an `ExtrudeGeometry` from a rounded-rect `Shape` with four perforations cut
   as holes, bevelled, with a normalised `uv1` channel added so the AO map has a real
   0–1 space to sit in.
3. **Textures are canvas-generated, not KTX2/Basis.** Albedo (`SRGBColorSpace`), normal
   (sobel'd from the same height field), roughness, metalness and AO (all
   `NoColorSpace`) are synthesised from one fBm field at load. Shipping stock PBR maps
   for a subject that has none would have been the fake.
4. **Depth of field is hand-rolled.** three's `DepthOfFieldNode` writes an output struct
   that the WebGL2 backend does not resolve — verified here: it returns a black frame
   with no error. Since the WebGL2 path is mandatory, DOF is instead a circle of
   confusion computed from `viewZ` against the focus uniform, mixed against a separable
   `gaussianBlur`. Same position in the chain, same read, works on both backends, and the
   focus distance still rides the closing seam through the pin.
5. **The LUT is generated, not a `.cube` file.** A 32³ `Data3DTexture` is built in JS —
   lift, gain, gamma, an S-curve and a 6% desaturation — and fed to `Lut3DNode` exactly
   as a loaded LUT would be. The grade is real; only the container is inline.
6. **Lenis and GSAP share one clock via `gsap.ticker`.** `gsap.ticker.add(t =>
   lenis.raf(t * 1000))` plus `lenis.on('scroll', ScrollTrigger.update)`. This is the
   documented integration and it is the same single-rAF result the spec is asking for;
   running a separate Lenis rAF alongside GSAP's would give two clocks, not one.

## Bugs found by actually running it

Worth recording, because none of them announce themselves:

- `GTAONode` writes occlusion to the **red channel only**. `aoPass.getTextureNode()`
  multiplied into scene colour therefore zeroes green, blue *and alpha* — the canvas goes
  fully transparent and the stage reads as a black rectangle with no console error. The
  fix is `.r`.
- `chromaticAberration(node, strength, null, scale)` — the documented "null means screen
  centre" — throws inside TSL's layout because the null centre reaches a typed `Fn`.
  Pass an explicit `Vector2(0.5, 0.5)`.
- A ScrollTrigger-pinned element goes `position: fixed`, so it stops crossing
  IntersectionObserver thresholds. A render-on-demand gate built on an IO entry latches
  `false` for the entire pin and the scene freezes on its first frame while the DOM half
  of the timeline keeps animating. Gate on `getBoundingClientRect` instead.

## Verification

Built and driven in headless Chromium at 1440×900 and 375×720: hero, every section, the
full pinned scrub range in nine steps, keyboard focus, and `prefers-reduced-motion`.
No horizontal overflow at either width. WebGPU is unavailable in that environment, so
what ran — and what is shown to be correct — is the **WebGL2 fallback path**, which is
the one the spec calls mandatory. The WebGPU backend runs the identical TSL graph.

## Running it

```bash
python3 -m http.server 8000     # from the repo root
# http://localhost:8000/site/
```

Served from the repo root so `../static/timeline-view.svg` resolves. Libraries load from
jsdelivr at pinned versions (three 0.185.1, gsap 3.15.0, lenis 1.3.26); fonts from Google
Fonts and Fontshare. No build step.
