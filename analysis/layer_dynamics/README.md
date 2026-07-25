# Layer-by-image-region analysis

## Purpose

This analysis asks whether the visual region emphasized by the hidden-state
perturbation framework changes across model depth.

It is an exploratory sensitivity analysis, not activation patching. It should
be used to explain how the main causal-patch framework develops across depth,
not presented as a separate standalone method.

## Verified Qwen 7B results

- 80 examples and 29 hidden-state indices.
- Grounded patch AP rises from **0.365** at index 15 to **0.781** at index 21.
- Grounded top-1 target hit rises from **32.5%** at index 15 to **92.5%** at
  index 21.
- Grounded top-3 target hit reaches **97.5%** at index 20 and remains there
  through the final state.
- From index 21 onward, **80.0%** of examples keep the same top patch through
  every remaining state.
- Mean adjacent-layer top-patch stability from index 21 onward is **95.9%**.
- Spatial concentration is strongest at index 24, where normalized heatmap
  entropy reaches **0.827**.
- The best average grounded patch AP occurs at index 21.

## Airport service-vehicle example

For selected example 25:

- The strongest patch changes across early and middle depths.
- Patch 30, which contains the airport ground-service vehicle, becomes the top
  patch at index 15.
- Patch 30 remains the top patch through the final hidden state.
- Its share of total positive sensitivity rises from **6.5%** at index 15 to
  **38.0%** at index 24.

## Paper framing

Use this result to support the main method:

> Target-aligned evidence is weak and spatially unstable at earlier depths, but
> becomes sharply localized and stable in later Qwen hidden states. This helps
> explain why later answer-position representations provide useful causal patch
> rankings.

Do not claim:

> Index 15 is the exact point where the model reads the service vehicle.

That would require activation patching or another direct intervention on
internal activations.

## Main versus appendix

Recommended appendix figures:

1. `localization_across_depth.png`
2. `layer_by_patch_airport.png`
3. `semantic_margin_across_depth.png`

Keep the headline paper focused on the causal-patch framework. Mention the
depth result briefly in the main text only if page space allows.
