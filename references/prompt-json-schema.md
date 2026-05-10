# Prompt JSON Schema For Codebase Infographics

Use this reference when creating `prompt.json` for GPT Image 2 from a codebase.

## Recommended Shape

```json
{
  "type": "technical_infographic",
  "title": "Exact visible title",
  "subtitle": "Optional short subtitle",
  "aspect_ratio": "9:16",
  "subject": {
    "codebase_name": "Project or component name",
    "purpose": "One-sentence source-grounded description",
    "audience": "Developers, AI builders, maintainers, or non-technical learners"
  },
  "visual_style": {
    "medium": "hand-drawn chalkboard, clean engineering whiteboard, scientific atlas, blueprint, dark handbook page, etc.",
    "background": "Specific surface/background",
    "line_quality": "How strokes, borders, arrows, and labels should look",
    "mood": "Educational, precise, dense but readable"
  },
  "color_system": {
    "primary": "Main structure/text",
    "accent_1": "Core flow or computation",
    "accent_2": "External systems or IO",
    "accent_3": "Warnings, constraints, or bottlenecks",
    "accent_4": "Optional categories"
  },
  "layout": {
    "composition": "How the page is divided",
    "main_region": "Primary diagram area",
    "sidebar": "none by default; do not render sidebars, source footers, or legend boxes unless explicitly requested",
    "numbering": "Numbered steps, lanes, layers, or panels",
    "hierarchy": "Readable title, section headings, labels, and arrows"
  },
  "layout_contract": {
    "type": "fixed_vertical_zones",
    "zones": [
      {
        "id": "zone_1",
        "label": "Visible zone label",
        "canvas_share": "10-15%",
        "must_be_larger_than": [],
        "must_contain": ["Mandatory visible item"],
        "must_not_contain": ["Forbidden visible item"]
      }
    ],
    "global_constraints": [
      "No visible source footer",
      "No legend box unless explicitly requested",
      "Do not render source_hint as visible text"
    ]
  },
  "focus_area": {
    "name": "The dominant core mechanism to make visually largest",
    "required_canvas_share": "45-65% of the main diagram",
    "must_show": ["Important internal subcomponent 1", "Important internal subcomponent 2"],
    "must_not_reduce_to": "A single labeled box"
  },
  "visual_budget": {
    "hero_regions": ["Core loop, model architecture, state update, or materialization path"],
    "compressed_regions": ["Setup, routing, input branches, defaults, simple formulas"],
    "budget_rule": "State how canvas is shifted from low-value facts to the mechanisms worth teaching"
  },
  "worked_example": {
    "scenario": "Concrete scenario or labeled assumption used for numeric derivation",
    "shape_trace": [
      {
        "stage": "Stage name",
        "formula": "Formula from code",
        "value": "Concrete shape/count/value",
        "attach_to_step": 3,
        "attach_to_visual": "Name of the panel object, tensor, state, or arrow where this value must appear",
        "visual_note": "Why this should be shown"
      }
    ],
    "schedule_trace": [
      {
        "stage": "Schedule name",
        "formula": "Formula from code",
        "curve": "How the value changes visually",
        "value": "Concrete count/range when derivable",
        "attach_to_step": 4,
        "attach_to_visual": "Name of the loop, timeline, scheduler arrow, or mixer where this schedule must appear"
      }
    ],
    "unknowns": ["Optional non-critical detail not verified in available code"]
  },
  "main_diagram": [
    {
      "step": 1,
      "label": "Short visible label",
      "content": "What to draw in this panel",
      "importance": "P0 core mechanism, P1 supporting architecture, or P2 runtime glue",
      "visual_weight": "hero, large, medium, small, or tiny",
      "expanded_details": ["Nested visual detail 1", "Nested visual detail 2"],
      "shape_badges": [
        {
          "label": "Visible short shape label",
          "value": "Concrete shape/count/value",
          "attach_to": "Visual object or arrow inside this panel"
        }
      ],
      "inline_callouts": ["Short note placed next to the relevant visual element, not in a separate sidebar"],
      "lineage_links": [
        {
          "from": "Producer shape/value/panel",
          "to": "Consumer shape/value/panel",
          "label": "Visible derivation such as y = mask 4 + latent 16"
        }
      ],
      "content_invariants": ["Mandatory visible node, label, or relationship that must not be omitted"],
      "source_hint": "Grounding metadata only; do not render source_hint as visible text in the image"
    }
  ],
  "legend": "omit by default; encode color/category meaning directly in panel labels",
  "text_requirements": {
    "language": "English",
    "readability": "All main labels must be crisp, readable, and accurately spelled",
    "priority": "Title, step labels, module names, shape badges, lineage labels, and inline callouts must be correct"
  },
  "negative_prompt": [
    "no fake code",
    "no random unreadable microtext",
    "no unrelated robot imagery",
    "no generic cyberpunk dashboard",
    "no README command workflow as the main story unless explicitly requested",
    "no right-side insight column unless explicitly requested",
    "no detached tensor trace table",
    "no visible source footer",
    "no source path captions",
    "no legend box",
    "no mux legend",
    "no cluttered layout"
  ]
}
```

## Story Selection Rule

Before choosing a diagram pattern, choose the **thing worth teaching**:

- What is the central transformation?
- What representation changes across the system? Examples: prompt -> embeddings -> latents -> pixels, request -> domain object -> database row, source files -> AST -> bundle, image -> features -> prediction.
- Which module owns the hard or distinctive work?
- Which details are merely wrappers around that work?

The final image should spend most of its visual area on the central transformation and the modules that make it happen. Choose one `focus_area` and make it visually dominant.

Use this budget:

- P0 core mechanism: 65-85% of main diagram.
- Dominant `focus_area`: 45-65% of main diagram.
- P1 supporting architecture: 10-30% of main diagram.
- P2 runtime glue: 0-5% of main diagram.
- For ML/video/diffusion systems, conditioning, input setup, and runtime setup together should usually use only 15-25% of the canvas.
- For ML/video/diffusion systems, denoising loop, model/DiT architecture, and decode/materialization should usually use at least 70% of the canvas.

If P2 appears, compress it into one small "Interface / Runtime" strip, footer, or tiny callout. Do not lead with it.

Fail the draft if:

- More than one P1 section appears before the first P0 section.
- The most important subsystem is a small inset or simple label.
- Input parsing, model loading, setup, or saving uses more space than the core mechanism.
- Important mechanics, constraints, or trace values are placed in a detached right-side notes column instead of inside the main flow.
- Input branches, conditioning lanes, formulas, or defaults consume space needed by the model architecture, repeated loop, state update, or decode path.

## Importance Judgment Rules

Before designing the final layout, judge what deserves visual space. Do not treat all true facts as equally drawable.

Give more space to mechanisms that:

- own the hard transformation or repeated loop
- mutate important state over time
- change representation, dimensionality, or compute scale
- hide non-obvious architecture, cache, chunking, scheduling, or materialization logic
- strongly affect quality, performance, correctness, or failure modes

Compress or omit details that:

- are one-step setup, routing, loading, or default selection
- are input branches whose only role is to feed the core
- can be explained by a short arrow, icon, or inline badge
- repeat information already visible elsewhere
- are formulas that a curve, mixer, or state-transition diagram can explain

For each proposed panel, ask: if this panel were deleted, would the viewer still understand the core mechanism? If yes, shrink it, merge it, or turn it into an inline callout.

Record the result in `visual_budget`, panel `visual_weight`, and compressed panel wording.

## Layout Contract Rules

Use `layout_contract` for dense or fragile technical diagrams where repeated image generations must stay structurally similar. Visual weights such as `hero` or `large` are not enough when one region must reliably dominate another.

The contract should specify:

- fixed top-to-bottom or left-to-right zones
- each zone's canvas share as a percent range
- relative constraints, such as "DiT architecture must be larger than denoising loop"
- mandatory visible content invariants for each zone
- forbidden visible elements, such as source footers, source path captions, large legends, or extra sidebars

For core model diagrams, the largest area should be the architecture or mechanism that is hardest to reconstruct mentally. A denoising loop can be important while still being smaller than the DiT/model architecture.

Do not ask GPT Image to draw source paths. Keep `source_hint` as grounding metadata for the prompt author; it must not appear as visible text.

## Source Priority Rules

Use README/docs to learn scope, terminology, supported modes, default examples, and user-facing entry points. Do not let README/docs become the visual story unless the user explicitly asks for a README or documentation diagram.

For P0 core mechanisms, prefer implementation evidence:

- implementation files, functions, classes, model modules, engine loops, protocol handlers, or data transformation code
- tests/examples only when they reveal behavior not obvious from implementation
- README/docs only for defaults, scenario assumptions, or public names

Fail the draft if:

- `subject.codebase_name`, title, or subtitle says `README`, `docs`, `examples`, or `run workflow` when a deeper codebase was provided.
- P0 `source_hint` values mainly cite README/docs rather than implementation files.
- The largest panels are downloads, checkpoint choices, CLI commands, prompt-extension APIs, examples, frontends, or save paths while the actual algorithm/model/engine is available in source.

If only README/docs are available, label the output honestly as a documentation workflow and keep core claims modest.

## Incremental Evidence Rules

Before writing the final JSON, build an internal evidence ledger in small batches. The ledger is not a separate output file.

Use this sequence:

1. `source_map`: scan docs, configs, entry points, and quick searches to identify P0/P1/P2 candidates and the next implementation files to read.
2. `mechanism_cards`: read selected P0 implementation files in small batches, recording claim, source, priority, `importance_score`, `why_it_matters`, `visual_budget`, `compression_strategy`, visual role, panel candidate, and unresolved questions.
3. `shape_ledger`: for numeric systems, record scenario, formula, value, source, and `attach_to_panel` for shapes, counts, schedules, caches, chunks, and loop counts that explain representation changes or compute scale.
4. `lineage_links`: record producer panel -> derived value -> consumer panel for tensors, states, objects, caches, or tokens.
5. `budget_pass`: before writing the storyboard, decide which mechanisms get hero/large/medium/small/tiny space and which facts are merged, drawn as arrows, or omitted.
6. `layout_contract`: when stability matters, record fixed zones, percent ranges, relative size constraints, forbidden visible extras, and content invariants.

After the ledger is sufficient, generate the prompt JSON. Do not stop with analysis, and do not output a separate knowledge-base file unless the user asks.

## Quantitative Trace Rules

If the code exposes formulas, dimensions, counts, schedules, cache sizes, chunk sizes, token lengths, or loop counts, add a `worked_example`. Use the user's scenario when provided; otherwise use a documented default and label it as an assumption.

For ML/video/array systems, include concrete values where possible:

- input artifact shape
- encoded/intermediate representation shape
- latent/cache/chunk/tile shape
- token/sequence length
- timestep/sigma schedule count and curve direction
- output artifact shape

Do not leave these as only symbolic labels when the code can produce numbers.

Do not show every derivable number. Prefer numbers that explain representation changes, compute scale, state flow, cache/chunk behavior, or output shape. Demote incidental dimensions to source hints or omit them.

Every item in `worked_example.shape_trace` and `worked_example.schedule_trace` must include `attach_to_step` and `attach_to_visual`. The same value must also appear inside the matching `main_diagram` panel as a `shape_badge`, `inline_callout`, or `lineage_link`. A trace that appears only in `worked_example` is not enough.

Example for a video diffusion codebase, assuming landscape 480P, `size=832*480`, `frame_num=81`, `vae_stride=(4,8,8)`, `patch_size=(1,2,2)`, `z_dim=16`, `sp_size=1`:

- RGB target: `3 x 81 x 480 x 832`.
- VAE latent / seeded noise: `16 x 21 x 60 x 104`, from `(81-1)//4+1`, `480//8`, `832//8`.
- Patch grid: `21 x 30 x 52`, because patchify uses `(1,2,2)` over latent time/height/width.
- Transformer token count: `21 * 30 * 52 = 32760` tokens.
- I2V mask after temporal reshape: `4 x 21 x 60 x 104`; VAE image latent: `16 x 21 x 60 x 104`; image condition `y`: `20 x 21 x 60 x 104`.
- I2V model input before patchify: latent `x` plus condition `y` -> `36 x 21 x 60 x 104`.
- Scheduler curve: shifted sigmas use `sigma' = shift * sigma / (1 + (shift - 1) * sigma)`, then `t = sigma' * num_train_timesteps`; plot as a descending high-noise-to-low-noise curve across `sampling_steps`.

For this example, the diagram must visibly connect `mask 4 + image latent 16 = y 20`, then `x 16 + y 20 = I2V model input 36`. Do not place these derivations only in a separate worked trace table.

## Inline Trace And Lineage Rules

Prefer full-width main-flow layouts. Put supporting details directly next to the tensor, state, module, loop, or arrow they explain.

Use these fields in `main_diagram` panels:

- `shape_badges`: compact visible labels attached to specific objects, such as `x: 16 x 21 x 60 x 104`.
- `inline_callouts`: short notes placed inside the relevant panel, such as `VAE stride compresses time by 4, space by 8`.
- `lineage_links`: arrows or braces that connect derived values, such as `mask 4 + image latent 16 -> y 20`.

Avoid:

- a right-side "Core Mechanics" column
- a detached "Worked Tensor Trace" table
- facts that require the reader to look far away from the flow node being explained
- isolated variable names whose producer is not visible

## Diagram Pattern Guidance

### Pipeline

Use for one main execution path, but start at the first semantically meaningful transformation, not necessarily the first line of code.

Prefer stages like:

- user/domain input
- representation or encoding
- core transformation loop, expanded into inner steps
- branch-specific domain processing
- model/service/storage interaction
- decoding/materialization
- output artifact

Avoid making these first-class stages unless they are the actual product:

- validation/parsing
- CLI argument parsing
- logging setup
- generic environment setup
- default parameter assignment
- filename generation

### Architecture Map

Use for apps and services. Represent `main_diagram` as layers or zones:

- user/client layer
- API/UI layer
- application orchestration
- domain/services
- persistence/external systems
- background jobs/queues
- observability/config

### State Or Lifecycle

Use for agents, protocols, jobs, and media systems. Represent states and transitions:

- idle/initializing
- input accepted
- planning/preparing
- running loop
- intermediate artifacts
- completion/failure
- retry/cancel paths

### Module Atlas

Use for libraries and SDKs. Represent modules as grouped panels:

- public entry points
- core abstractions
- adapters/integrations
- utilities
- extension points
- examples/tests

## Visual Style Presets

- `chalkboard_lecture`: dark blackboard, chalk dust, hand-drawn arrows, colored chalk categories.
- `engineering_whiteboard`: white background, marker lines, boxes, arrows, concise annotations.
- `scientific_atlas`: light paper, precise thin lines, low-saturation colors, editorial labels.
- `dark_system_handbook`: dark neutral background, modular panels, glowing but restrained accents.
- `blueprint_cutaway`: blueprint grid, technical linework, orthographic panels, measurement tags.

## Text Density Rules

- Titles: 4-12 words.
- Step labels: 1-5 words.
- Panel content: draw instructions, not paragraphs.
- Inline callouts: 5-14 words each, attached to the relevant module, tensor, loop, arrow, or state.
- Avoid placing long file paths in the visible image. Use short filenames only when they help.
- Prefer exact function/class names only for key anchors.
- Do not use a right-side insight sidebar by default. If the user explicitly asks for one, it must be short and secondary.
- Keep formulas close to the visual object they explain.
- Do not render source paths, citations, source footers, or `source_hint` values as visible text by default.
- Do not render a legend box by default. Put category meaning in local labels such as "green T5 context" or "orange CLIP tokens."

## Inline Callout Quality Rules

Good inline callouts:

- Explain bottlenecks tied to the focus area: "Latent space reduces the tensor size the DiT must denoise."
- Explain tradeoffs tied to the focus area: "Higher guidance strengthens text alignment but can over-constrain motion."
- Explain hidden constraints tied to the focus area: "Temporal masks tell the model which frames are fixed image conditions."
- Explain mental models tied to the focus area: "The scheduler is the step planner for walking noise toward video."
- Explain failure modes tied to the focus area: "Bad conditioning can be amplified across every denoising step."

Weak callouts:

- "default sampling steps: 50"
- "rank 0 saves output"
- "uses argparse"
- "logs job args"
- "supports size 1280*720"
- "loads T5, VAE, WanModel"
- "example prompt is inserted"
- "optional prompt expansion"

Use weak items only when the user explicitly asks for an operations/runtime diagram.

## Core Expansion Rules

When a P0 module is an algorithm, model, engine, loop, protocol, planner, renderer, compiler pass, database/query layer, or state machine, include `expanded_details` with the internal mechanism.

Examples:

- Transformer / DiT: show a complete architecture, not a thumbnail: patchify purpose, patch/token grid, timestep embedding, timestep projection, AdaLN/time modulation or equivalent shift/scale/gate behavior, 2D/3D positional encoding such as RoPE, self-attention scope, T5/CLIP or other cross-attention/context injection, FFN, head/output projection, unpatchify purpose, predicted output shape.
- Diffusion / denoising: show the state flow, not only formulas: noisy latent `x_t`, timestep/sigma curve, model prediction, optional conditional/unconditional branches, visual CFG/mixer when applicable, scheduler update, `x_t -> x_{t-1}` repeat, final latent.
- Encoder / decoder: show the reconstruction/materialization path when it has real logic: input/output normalization, downsample/upsample stride, latent channel count, scaling/mean/std transforms, chunk/tile/cache behavior, causal context behavior, clamp/normalization before output. Use "overlap" only when code actually implements overlap; otherwise name the verified behavior such as cached causal convolution or chunked temporal decode.
- Compiler / build tool: parse, AST, transform passes, dependency graph, codegen, emit.
- Web request path: route match, auth/session, validation, domain service, database transaction, response serialization.
- Agent loop: observe, plan, tool call, result integration, memory update, stop condition.

Do not write one large panel that says "run model" or "process request"; show what happens inside.

For video diffusion or transformer models, explicitly ask GPT Image 2 to visualize why each transformation exists:

- Patchify converts a latent grid into tokens so attention can operate over time and space.
- Timestep embeddings condition every block on the current noise level.
- AdaLN/time modulation gates or shifts normalized activations using the timestep signal.
- RoPE or positional encoding preserves temporal/spatial order inside attention.
- Unpatchify reconstructs the grid-shaped latent prediction from tokens.
- VAE decode may contain the final important reconstruction logic. If code exposes mean/std inversion, causal convolution cache, per-time or chunked decode, clamp, tiling, or overlap, draw that mechanism instead of reducing decode to a save box. Inspect the code and name the actual behavior rather than guessing.

For DiT diagrams, these are content invariants unless the code proves otherwise:

- patchify / patch embedding
- token grid or token sequence
- timestep embedding and timestep projection
- AdaLN or equivalent time modulation with shift, scale, and gate signals
- positional encoding such as RoPE when present
- self-attention
- cross-attention or other condition injection
- FFN / MLP
- head / output projection
- unpatchify or grid reconstruction

If a mandatory invariant such as AdaLN/time modulation is omitted from a Wan-style DiT diagram, treat the draft as factually wrong.

## Example Reframe

If a repository has a `generate.py` CLI for a video diffusion model, do not make the main diagram "Parse Args -> Runtime Setup -> Prompt Extend -> Save MP4." Instead, frame it as:

1. Tiny runtime/request strip.
2. Compact conditioning mux: merge T2V/I2V inputs and keep only context tokens, image tokens, mask/image latent lineage, and the condition tensor that feeds the core.
3. Small latent setup: seeded noise, key latent shape, and patch/token count.
4. Hero denoising loop: timestep/sigma curve, model prediction branches, visual CFG/mixer if applicable, scheduler update, and `x_t -> x_{t-1}` loopback. Do not make the CFG formula a main panel when the mixer graphic explains it.
5. Hero DiT/model architecture: patch embedding, time embedding/projection, AdaLN shift/scale/gate, 3D positional encoding, self-attention, cross-attention, FFN, head, unpatchify.
6. Medium VAE decode: final latent becomes RGB frames through verified decode mechanics such as mean/std inversion, causal cache, per-time/chunk decode, clamp, and output shape.

CLI parsing, distributed rank setup, and file saving can be tiny captions or a compact outer shell. Do not use a right-side "Core Mechanics" or "Worked Tensor Trace" column; put those facts inside the matching panels.

For a Wan2.1-style 9:16 regression diagram, use a fixed zone contract:

- request + conditioning + latent setup: 15-20%
- denoising state loop: 20-25%
- DiT architecture: 35-45%, larger than denoising
- VAE decode reconstruction: 15-20%
- visible legend/source/footer: 0%

## Validation Checklist

- The JSON is syntactically valid.
- The diagram has one clear story, not every detail from the repository.
- The main diagram has 4-9 sections for dense vertical infographics, or 3-6 zones for architecture maps.
- Labels are readable and short.
- Every technical claim can be traced to code or docs, or is clearly a harmless visual simplification.
- The prompt tells GPT Image 2 what style, layout, labels, colors, and negative details to enforce.
- The first main section is not CLI parsing, setup, logging, or config defaults unless that is the requested topic.
- There is no independent right-side insight sidebar, notes column, or detached worked trace table unless explicitly requested.
- At least half of the visible area teaches the codebase's distinctive mechanism.
- The JSON has a `focus_area` for non-trivial codebases.
- The JSON has a `visual_budget` or equivalent layout instruction showing what is expanded and what is compressed.
- Dense or fragile diagrams have a `layout_contract` with fixed zones and area constraints.
- The JSON has a `worked_example` when concrete shapes, counts, schedules, cache sizes, or loop counts can be derived.
- Every `worked_example` trace item has `attach_to_step` and `attach_to_visual`.
- Every concrete trace value is mirrored in the matching `main_diagram` panel through `shape_badges`, `inline_callouts`, or `lineage_links`.
- Derived values used by later panels have visible producer-to-consumer lineage.
- The focus area is the largest visual region and is not merely an inset.
- P0 model/loop/engine/state-machine sections include `expanded_details`.
- P0 ML/video/array sections include shape trace, timestep/schedule trace, modulation/conditioning details, patchify/unpatchify purpose, and encode/decode chunk/cache/overlap behavior when applicable.
- Dominant model architecture panels include their content invariants; for DiT this includes AdaLN/time modulation when present.
- Conditioning/input/setup do not take over the diagram when denoising/model/decode is the real mechanism.
- Equations do not take over the diagram when a visual curve, mixer, arrow, or architecture cutaway would teach better.
- Decode/materialization is not an afterthought if the code contains meaningful reconstruction, cache, chunk, normalization, or rendering logic.
- `source_hint` is not rendered as visible text.
- There is no visible source footer, source path caption, legend box, or mux legend unless explicitly requested.
- "Implementation Insights" is not used as a generic sidebar title.

## Anti-Patterns To Avoid

- **Conditioning takeover**: input lanes, prompt encoders, or branch setup consume the page while the core model is underexplained.
- **Formula worship**: equations for CFG, schedules, scoring, or updates become large panels instead of compact labels on a curve, mixer, or loop.
- **Architecture thumbnail**: the main model/engine/compiler/renderer appears as a small box without internal structure.
- **Decode afterthought**: decode, materialization, rendering, or output reconstruction is collapsed into a save box despite meaningful implementation logic.
- **Layout drift**: vague visual weights let the image model move area away from the intended core across generations.
- **Visible source clutter**: source paths, citations, source footers, or legend boxes consume space without teaching the mechanism.
- **Missing invariant**: mandatory architecture facts such as AdaLN/time modulation disappear from the final prompt or image.
