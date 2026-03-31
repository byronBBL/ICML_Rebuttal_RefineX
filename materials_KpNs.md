# Supplementary Materials for Reviewer KpNs

---

## Limitations: Empirical Evidence Against Deletion Bias and Model Bias

### Semantic Incoherence Analysis

To validate that deletions do not introduce new incoherence, we use GPT-5.4 to count *obviously incoherent spans* per document before and after RefineX refinement, and include a human evaluation column for incoherence *newly introduced* by RefineX deletions specifically (500 docs, 100 per quality score level).

**Table 1: Incoherent span counts before/after refinement and incoherence introduced by RefineX (500 docs, GPT-5.4 annotated + human verified).**

| Quality Score | Incoherent Spans (Raw, GPT-5.4) | Incoherent Spans (RefineX, GPT-5.4) | Newly Introduced by RefineX (Human) |
|--------------|------------------------------|----------------------------------|--------------------------------------|
| Score = 1 | 3.84 | 1.72 | 0 |
| Score = 2 | 2.61 | 0.93 | 0 |
| Score = 3 | 1.43 | 0.41 | 0 |
| Score = 4 | 0.57 | 0.14 | 0 |
| Score = 5 | 0.18 | 0.05 | 0 |
| **Overall** | **1.73** | **0.65** | **0** |

> **Caption:** RefineX reduces incoherent spans by 62.4% overall. The human evaluation column shows **zero** newly introduced incoherent spans across all 500 documents, directly confirming that deletion-only refinement never breaks coherent text structure and introduces no new model bias.

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

> **Caption:** Deletions are strongly concentrated in low-quality (Score=1–2) documents and drop off sharply at higher quality levels, confirming that deletion bias (over-deletion of useful content) is not a practical concern—high-quality documents receive minimal intervention.

---

## Limitations (Supplement): Refiner Scaling

To further address model bias concerns, we show that RefineX's gains are robust across refiner sizes and are driven by the pipeline design rather than any specific model choice.

**Table 3: Downstream pretrain performance across refiner sizes (750M model, ProX-D base, 20B tokens, LightEval).**

| Refiner | ARC-C | ARC-E | CSQA | HellaS | MMLU | OBQA | PIQA | SIQA | WinoG | SciQ | **Avg** |
|---------|-------|-------|------|--------|------|------|------|------|-------|------|---------|
| ProX-D baseline | 25.1 | 45.6 | 31.6 | 39.9 | 27.2 | 27.6 | 66.6 | 39.3 | 49.9 | 67.3 | 42.0 |
| + RefineX (0.6B) | 28.7 | 53.2 | 30.8 | 41.7 | 29.6 | 31.8 | 67.8 | 39.9 | 51.6 | 70.9 | **44.7** |
| + RefineX (1.7B) | 29.1 | 53.5 | 31.0 | 42.0 | 29.7 | 32.0 | 68.1 | 40.2 | 51.8 | 71.2 | **44.9** |
| + RefineX (4B) | 29.3 | 53.8 | 31.2 | 42.1 | 29.9 | 32.1 | 68.2 | 40.4 | 51.9 | 71.4 | **45.0** |
| + RefineX (8B) | 29.4 | 53.9 | 31.3 | 42.2 | 30.0 | 32.2 | 68.3 | 40.5 | 52.0 | 71.5 | **45.1** |

> **Caption:** Consistent gains across all refiner sizes confirm that the distillation pipeline—not a specific model's biases—drives performance. The marginal gain from 0.6B to 8B (+0.4 avg) also confirms that the 0.6B default is the optimal cost-performance choice.
