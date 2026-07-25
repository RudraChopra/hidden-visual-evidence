# Related Work Starter Pack

This file is a research handoff for Xavier. It is intentionally organized as
notes and contrasts rather than final paper prose.

## Priority order

### Must discuss in the main related-work section

1. **Causal Attribution via Activation Patching (CAAP)**  
   Izadi et al., 2026.  
   https://arxiv.org/abs/2603.13652  
   Directly intervenes on contextualized internal patch representations in
   Vision Transformers and uses the target-class score as an attribution
   signal. This is the closest method-side paper and must be discussed
   carefully.  
   **Difference from ours:** CAAP studies ViT classification and patches
   internal activations into a neutral context. Our framework perturbs image
   regions in a multimodal generative model, ranks them by answer-position
   hidden-state change, validates them with segmentation masks plus necessity
   and sufficiency, and uses the evidence for hallucination diagnosis.

2. **Interpreting and Editing Vision-Language Representations to Mitigate
   Hallucinations**  
   Jiang et al., 2024.  
   https://arxiv.org/abs/2410.02762  
   Projects VLM image representations into vocabulary space, uses the scores
   to localize objects, and edits latent features to reduce hallucinations.  
   **Difference from ours:** their semantic projection is the closest prior
   work to our post-merger semantic probe. We first choose a region through an
   independently validated causal input intervention, then read out the same
   region and confirm its role through counterfactual masking. Our main
   application is diagnosis rather than latent feature editing.

3. **Towards Best Practices of Activation Patching in Language Models:
   Metrics and Methods**  
   Zhang and Nanda, 2023.  
   https://arxiv.org/abs/2309.16042  
   Shows that activation-patching conclusions depend strongly on corruption
   methods and recovery metrics, and gives methodological recommendations.  
   **Difference from ours:** our primary method is input-patch perturbation,
   not activation patching. This paper is important for explaining why the
   layer-by-region analysis is described as sensitivity evidence and why any
   future activation-patching extension needs matched controls and careful
   recovery metrics.

4. **RISE: Randomized Input Sampling for Explanation of Black-box Models**  
   Petsiuk, Das, and Saenko, 2018.  
   https://arxiv.org/abs/1806.07421  
   Estimates image saliency by applying random masks and measuring changes in
   output probability.  
   **Difference from ours:** RISE is black-box and output-based. We use
   single-region perturbations to measure changes in an internal
   answer-position representation, then separately evaluate output-level
   necessity and sufficiency.

5. **Patchscopes: A Unifying Framework for Inspecting Hidden Representations
   of Language Models**  
   Ghandeharioun et al., 2024.  
   https://arxiv.org/abs/2401.06102  
   Interprets hidden representations by transplanting activations into a new
   prompt or context and decoding them in natural language.  
   **Difference from ours:** Patchscopes targets language-model hidden states
   and representation explanation. We probe spatial visual tokens associated
   with a causally selected image region, and do not treat the decoded tokens
   as literal reasoning.

6. **HALP: Detecting Hallucinations in Vision-Language Models without
   Generating a Single Token**  
   Kogilathota et al., 2026.  
   https://arxiv.org/abs/2603.05465  
   Trains probes on visual, vision-token, and query-token states to predict
   hallucination risk before generation across several VLM families.  
   **Difference from ours:** HALP asks whether a hallucination is likely. We
   diagnose a flagged answer by identifying the responsible image cue,
   reading its semantic interpretation, and testing whether masking it changes
   the answer.

### Strong supporting citations

7. **Locating and Editing Factual Associations in GPT (ROME)**  
   Meng et al., 2022.  
   https://arxiv.org/abs/2202.05262  
   Introduces causal tracing to identify internal computations responsible for
   factual recall.  
   **Use in our paper:** establishes the broader intervention-based
   interpretability lineage. Our setting is spatial visual evidence in VLMs.

8. **Interpretable Explanations of Black Boxes by Meaningful Perturbation**  
   Fong and Vedaldi, 2017.  
   https://arxiv.org/abs/1704.03296  
   Learns image regions whose removal changes a classifier decision.  
   **Use in our paper:** foundational input-perturbation attribution method.

9. **Explaining by Removing: A Unified Framework for Model Explanation**  
   Covert, Lundberg, and Lee, 2020.  
   https://arxiv.org/abs/2011.14878  
   Organizes explanation methods around feature removal, model behavior, and
   attribution summaries.  
   **Use in our paper:** positions patch blur as a removal-based intervention
   and motivates matched random controls.

10. **Transformer Interpretability Beyond Attention Visualization**  
    Chefer, Gur, and Wolf, 2020.  
    https://arxiv.org/abs/2012.09838  
    Propagates relevance through transformer attention and skip connections.  
    **Difference from ours:** relevance propagation is a backward attribution
    method, whereas ours directly perturbs image regions and evaluates
    behavioral consequences.

11. **The Hidden Life of Tokens: Reducing Hallucination of Large
    Vision-Language Models via Visual Information Steering**  
    Li et al., 2025.  
    https://arxiv.org/abs/2502.03628  
    Studies layerwise visual-token logits and uses activation steering to
    reduce hallucinations.  
    **Difference from ours:** VISTA modifies inference to reduce
    hallucinations. Our layerwise analysis tracks where spatial causal
    evidence and target semantics emerge, while our application is post-hoc
    diagnosis.

## Recommended four-bucket structure

### 1. Intervention-based interpretability

Use ROME, activation-patching best practices, and Patchscopes. Explain that
internal interventions can localize or decode information, but methodology and
metrics matter.

### 2. Image perturbation and transformer attribution

Use Meaningful Perturbation, RISE, Explaining by Removing, Chefer et al., and
CAAP. Position our method between input perturbation and internal causal
attribution.

### 3. Internal visual representations in VLMs

Use Jiang et al. and VISTA. Explain that prior work reveals or edits internal
visual semantics, while our semantic readout is tied to an independently
selected causal region and a counterfactual answer flip.

### 4. Hallucination detection versus diagnosis

Use HALP and the standard POPE-related hallucination literature already in the
paper. State the distinction clearly:

> Detection predicts whether an answer is likely to be hallucinated. Diagnosis
> identifies the visual cue that drove a flagged answer, how the model
> represented that cue, and whether removing it changes the answer.

## Novelty risk to address explicitly

CAAP and Jiang et al. are close enough that reviewers may raise them. The
paper should not claim that it is the first work to perturb image evidence or
project VLM visual states into vocabulary space.

The defensible novelty is the combined framework and the scientific finding:

1. answer-position hidden-state change selects spatial evidence;
2. exact masks and matched necessity/sufficiency validate the evidence;
3. strong localization does not transfer into reliable correctness
   verification;
4. the same causal region can be semantically read and counterfactually
   removed to diagnose a hallucination;
5. the framework is demonstrated across Qwen 7B and 3B.
