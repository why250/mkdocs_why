# Image and evidence guidelines

Use figures as engineering evidence, not decoration.

## What to preserve

Keep an image when it does at least one of these:

- defines the circuit topology or node/device naming used in the derivation;
- establishes an important simulation or measurement observation;
- supports a numerical claim that is difficult to reconstruct from prose alone;
- records a simulator setup, port/termination definition, sweep, or measurement condition needed for reproducibility;
- shows a comparison whose visual relationship matters.

Do not archive:

- duplicate screenshots of the same information;
- UI chrome that does not help reproduce the result;
- low-value progress screenshots;
- images whose only purpose is aesthetic.

## Storage and naming

For a new note with slug `<note-slug>`, store new images under:

```text
docs/blog/posts/image/<note-slug>/
```

Use descriptive English `kebab-case` names, for example:

```text
schematic-independent-emitter.png
noise-independent-emitter-1hz.png
schematic-shared-emitter.png
noise-shared-emitter-1hz.png
```

Avoid `image1.png`, timestamp-only names, or simulator-generated opaque names.

Keep legacy images in their existing locations unless the user explicitly asks for migration.

## Evidence before interpretation

For every important figure, distinguish:

1. **Observation** — what can be read directly from the image;
2. **Interpretation** — the physical explanation inferred from the observation.

Example:

> 仿真观察：共享 emitter 后，Q66/Q26 的输出噪声贡献由约 208 pV/√Hz 增至约 2.70 nV/√Hz。

Then separately:

> 物理解释：共享 emitter 节点在差模下接近 AC virtual ground，使原有 emitter local-feedback 对 cascode 自身噪声的抑制消失。

Do not present interpretation as if it were directly measured.

## Figure captions

A useful caption should answer at least two of:

- what topology/setup is shown;
- what quantity is plotted or tabulated;
- what comparison matters;
- what operating condition applies.

Prefer:

```markdown
*图 2：共享 cascode emitter 后的 ADS Noise Contribution，1 Hz。Q66/Q26 成为主导噪声源。*
```

Avoid captions like “仿真结果” or “电路图”.

## Cropping

Crop screenshots to preserve:

- circuit and relevant device/net labels;
- axis labels, units, legends, frequency/bias condition;
- the exact table rows or values cited in the note.

Remove irrelevant desktop chrome when possible, but do not crop away information needed to verify the claim.

## Simulation evidence

For ADS/Spectre/Virtuoso-derived figures, record nearby in text when known:

- simulator and analysis type;
- dataset/measurement name;
- frequency or integration range;
- temperature/corner;
- port/reference conditions;
- relevant bias point.

If these are unknown, do not invent them. Put them in `待验证` if they matter.

## Generated explanatory figures

A generated small-signal diagram or conceptual sketch is allowed when it makes the derivation clearer, but it must not replace original evidence.

Use generated figures for:

- half-circuit or virtual-ground explanation;
- simplified current/noise flow;
- feedback-loop intuition.

Label such figures as schematic/illustrative, not measured or simulated evidence.

## Minimal evidence set

For a topology-comparison note, a good default is:

1. one schematic per topology;
2. one key result image per topology;
3. optional one conceptual small-signal diagram if the mechanism is non-obvious.

Add more only when each extra figure supports a distinct claim.
