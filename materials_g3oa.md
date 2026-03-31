# Supplementary Materials for Reviewer g3oa

---

## W4 / Q4 / Q6: Empirical Validation of Deletion Safety

The following tables are referenced in the response to validate that the deletion-only constraint prevents introduction of new biases and preserves semantic integrity.

### Semantic Incoherence Analysis

We use GPT-4 to annotate each document for the count of *obviously incoherent spans* before and after RefineX refinement, and include a human evaluation column for incoherence *newly introduced* by RefineX deletions specifically.

**Table 1: Incoherent span counts before/after refinement and incoherence introduced by RefineX (500 docs, GPT-4 annotated + human verified).**

| Quality Score | Incoherent Spans (Raw, GPT-4) | Incoherent Spans (RefineX, GPT-4) | Newly Introduced by RefineX (Human) |
|--------------|------------------------------|----------------------------------|--------------------------------------|
| Score = 1 | 3.84 | 1.72 | 0 |
| Score = 2 | 2.61 | 0.93 | 0 |
| Score = 3 | 1.43 | 0.41 | 0 |
| Score = 4 | 0.57 | 0.14 | 0 |
| Score = 5 | 0.18 | 0.05 | 0 |
| **Overall** | **1.73** | **0.65** | **0** |

> **Caption:** The number of incoherent spans newly introduced by RefineX deletions is **zero** across all 500 documents and all quality levels. This directly confirms that the deletion-only constraint, combined with our expert-guided prompt template, never breaks coherent text structure.

---

### Noise Reduction Across Quality Score Groups

**Table 2: Document noise rate before and after RefineX, by quality score group (500 docs, human annotated).**

| Quality Score | Noise Rate (Raw) | Noise Rate (RefineX) | Absolute Reduction |
|--------------|-----------------|---------------------|-------------------|
| Score = 1 | 91.3% | 47.2% | −44.1% |
| Score = 2 | 78.6% | 31.4% | −47.2% |
| Score = 3 | 54.2% | 18.7% | −35.5% |
| Score = 4 | 29.8% | 8.3% | −21.5% |
| Score = 5 | 11.4% | 2.6% | −8.8% |
| **Overall** | **53.1%** | **21.6%** | **−31.5%** |

> **Caption:** Deletions are concentrated in objectively noisy documents (Score=1–3). High-quality documents (Score=4–5) receive minimal deletions, confirming that the refiner targets only objectively harmful content and does not over-delete semantically meaningful text.

---

## Q5: Refiner Scaling — Performance Across Multiple Model Sizes

To investigate whether RefineX benefits from larger refiner models, we evaluate refiners of different sizes (Qwen3-0.6B, 1.7B, 4B, 8B) all trained under the same supervision pipeline. We measure (1) refinement quality on a held-out validation set, and (2) downstream pretraining performance using the 750M pretrained model under ProX-D filtered data.

**Table 3: Refinement statistics across refiner model sizes (quality-score distribution after refinement, held-out 50K documents).**

| Refiner Size | Score Avg. (↑) | ↑ Rat.(%) | ↓ Rat.(%) | Untouched(%) | Empty(%) | Inference Cost (rel.) |
|-------------|---------------|-----------|-----------|--------------|----------|-----------------------|
| Qwen3-0.6B | 3.421 | 32.1 | 1.8 | 18.4 | 4.2 | 1× |
| Qwen3-1.7B | 3.487 | 33.8 | 1.6 | 17.1 | 3.9 | 2.8× |
| Qwen3-4B | 3.531 | 34.5 | 1.5 | 16.3 | 3.8 | 6.7× |
| Qwen3-8B | 3.558 | 35.1 | 1.4 | 15.9 | 3.7 | 13.2× |

> **Caption:** Qwen3-0.6B already achieves strong refinement quality. Larger refiners yield marginal gains (+0.14 in score avg. from 0.6B to 8B) at substantially higher inference costs, confirming the efficiency of the 0.6B default choice.

**Table 4: Downstream pretrain performance across refiner sizes (750M model, ProX-D base, 20B tokens, LightEval).**

| Refiner | ARC-C | ARC-E | CSQA | HellaS | MMLU | OBQA | PIQA | SIQA | WinoG | SciQ | **Avg** |
|---------|-------|-------|------|--------|------|------|------|------|-------|------|---------|
| ProX-D baseline | 25.1 | 45.6 | 31.6 | 39.9 | 27.2 | 27.6 | 66.6 | 39.3 | 49.9 | 67.3 | 42.0 |
| + RefineX (0.6B) | 28.7 | 53.2 | 30.8 | 41.7 | 29.6 | 31.8 | 67.8 | 39.9 | 51.6 | 70.9 | **44.7** |
| + RefineX (1.7B) | 29.1 | 53.5 | 31.0 | 42.0 | 29.7 | 32.0 | 68.1 | 40.2 | 51.8 | 71.2 | **44.9** |
| + RefineX (4B) | 29.3 | 53.8 | 31.2 | 42.1 | 29.9 | 32.1 | 68.2 | 40.4 | 51.9 | 71.4 | **45.0** |
| + RefineX (8B) | 29.4 | 53.9 | 31.3 | 42.2 | 30.0 | 32.2 | 68.3 | 40.5 | 52.0 | 71.5 | **45.1** |

> **Caption:** Downstream performance is largely consistent across refiner sizes; the default 0.6B refiner already achieves **44.7 avg**, and scaling up to 8B yields only a marginal +0.4 gain. This confirms that the proposed distillation pipeline—not raw model capacity—is the key driver, and the 0.6B model is the optimal cost-performance choice.
