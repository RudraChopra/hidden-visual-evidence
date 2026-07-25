# Hidden Visual Evidence Results Repository

This folder is the canonical record for the paper currently titled:

**Self Verification Through Hidden Visual Evidence in Vision Language Models**

## Current paper framing

The framework identifies image patches that causally drive a vision-language
model's answer. The method transfers as a localization and intervention tool
across Qwen2.5-VL 7B and 3B. It does not provide a reliable cross-model
correctness certificate. Its strongest application is causal hallucination
diagnosis:

1. locate the visual cue that drove the answer,
2. read out what object concept is represented at that cue,
3. mask the cue and test whether the hallucination is corrected.

## Canonical headline results

- Controlled patch AP: **0.761** on Qwen 7B and **0.689** on Qwen 3B.
- Top-1 target hit: **88.5%** and **84.8%**.
- Top-3 necessity lift: **0.211** and **0.161**.
- Top-3 sufficiency lift: **0.508** and **0.339**.
- Verification AUROC: **0.747** in Qwen 7B discovery, **0.558** under frozen
  Qwen 3B transfer.
- Semantic diagnosis: the causal patch carries stronger queried-object signal
  than a matched random patch in **70.0%** of Qwen 7B and **75.0%** of Qwen 3B
  held-out examples.
- Airport/car case: clean p(yes) **0.936**, causal-patch blur **0.048**,
  random-patch blur **0.931**; the causal patch reads out truck/vehicle/car.

See `RESULTS_INDEX.csv` and `PAPER_NUMBERS.json` before copying any number into
the paper.

## Random-layer toy baseline

The selected Qwen 7B semantic depth and its frozen relative-depth transfer to
Qwen 3B are compared with all alternative hidden-state indices.

On hallucinated examples:

- Qwen 7B mean target-concept margin: **1.910** at the selected depth versus
  **0.408** across alternative layers, exact layer-randomization p = **0.034**.
- Qwen 3B mean target-concept margin: **1.506** at the transferred depth versus
  **0.467** across alternative layers, p = **0.027**.

This is a lightweight baseline for the application figure, not a new
model-selection claim.

## Folder structure

- `analysis/random_layer_baseline/`: layer-baseline CSVs, JSON, figures, and
  paper-ready text.
- `figures/`: canonical paper figures.
- `notebooks/`: final runnable notebooks.
- `paper_assets/`: LaTeX snippets and result text.
- `raw_results/`: original result ZIPs.
- `RESULTS_INDEX.csv`: source-of-truth table for reported numbers.
- `FILE_MANIFEST_SHA256.csv`: integrity manifest.

## Important caveats

1. The Qwen 7B verification result is a discovery result. Do not describe
   AUROC 0.747 as frozen cross-model confirmation.
2. The frozen Qwen 3B verification AUROC is 0.558 and remains the primary
   transfer result. The tuned 0.623 result is an appendix ablation.
3. The first exported semantic-probe summary accidentally included Qwen 3B
   validation rows. Use `qwen_semantic_corrected_results.zip` and the values in
   `RESULTS_INDEX.csv`.
4. The semantic vocabulary projection is an exploratory post-merger
   visual-token probe. It is not literal chain-of-thought and not a calibrated
   object classifier.
5. The hallucination taxonomy currently has one provisional reviewer. Do not
   report formal taxonomy percentages until an independent review is complete.
6. Both evaluated models are from the Qwen family.

## Suggested GitHub setup

Large binary outputs should use Git LFS:

```bash
git lfs install
git lfs track "*.zip"
git lfs track "*.png"
git add .gitattributes
```

Commit raw result ZIPs without modifying them. `FILE_MANIFEST_SHA256.csv`
allows later verification that files have not changed.


## Layer-dynamics extension

The repository now includes an exploratory all-layer spatial analysis under
`analysis/layer_dynamics/`.

The result supports the main framework rather than creating a separate method:
target-aligned patch rankings emerge sharply and become stable in later Qwen
7B hidden states. Use the appendix assets in `paper_assets/` and avoid claiming
that the analysis identifies the exact causal read layer.

## Related-work handoff

`related_work/RELATED_WORK_NOTES.md` contains a verified starter pack organized
around intervention-based interpretability, image perturbation, internal visual
representations, and hallucination diagnosis. The final prose remains assigned
to Xavier.
