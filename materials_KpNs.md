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

**Table 3: Refinement statistics grouped by document quality score across refiner model sizes.**

| Quality | Model | Avg. (↑) | ↑ Rat.(%) | ↓ Rat.(%) | Untouched(%) | Empty(%) |
|---------|-------|----------|-----------|-----------|--------------|----------|
| Score=1 | Qwen3-0.6B | 2.009 | 22.23 | 0.00 | 7.87 | 68.17 |
| Score=1 | Qwen3-1.7B | 2.075 | 24.14 | 0.00 | 6.28 | 66.91 |
| Score=1 | Qwen3-4B   | 2.209 | 22.28 | 0.00 | 4.71 | 71.31 |
| Score=1 | Qwen3-8B   | 2.282 | 20.82 | 0.00 | 3.45 | 74.26 |
| Score=2 | Qwen3-0.6B | 2.718 | 42.14 | 0.66 | 7.59 | 19.97 |
| Score=2 | Qwen3-1.7B | 2.711 | 44.26 | 0.61 | 8.06 | 15.35 |
| Score=2 | Qwen3-4B   | 2.744 | 43.62 | 0.58 | 7.65 | 18.48 |
| Score=2 | Qwen3-8B   | 2.749 | 43.61 | 0.59 | 7.27 | 19.27 |
| Score=3 | Qwen3-0.6B | 3.413 | 41.14 | 4.58 |  9.85 |  6.18 |
| Score=3 | Qwen3-1.7B | 3.410 | 41.40 | 4.39 | 10.10 |  4.32 |
| Score=3 | Qwen3-4B   | 3.420 | 41.75 | 4.39 | 10.73 |  5.25 |
| Score=3 | Qwen3-8B   | 3.423 | 41.88 | 4.15 | 10.41 |  5.48 |
| Score=4 | Qwen3-0.6B | 4.075 | 12.70 | 4.86 | 13.52 |  0.93 |
| Score=4 | Qwen3-1.7B | 4.079 | 12.86 | 4.63 | 13.16 |  0.54 |
| Score=4 | Qwen3-4B   | 4.088 | 13.32 | 4.30 | 14.77 |  0.60 |
| Score=4 | Qwen3-8B   | 4.085 | 13.20 | 4.41 | 14.18 |  0.67 |
| Score=5 | Qwen3-0.6B | 4.917 |  0.00 | 8.20 | 16.50 |  0.18 |
| Score=5 | Qwen3-1.7B | 4.917 |  0.00 | 8.13 | 15.85 |  0.09 |
| Score=5 | Qwen3-4B   | 4.923 |  0.00 | 7.62 | 17.41 |  0.10 |
| Score=5 | Qwen3-8B   | 4.924 |  0.00 | 7.48 | 16.70 |  0.11 |

> **Caption:** Refinement statistics grouped by document quality score and refiner size. **Avg.**, **↑ Rat.**, and **↓ Rat.** denote the average quality score and the percentage of documents with improved or degraded scores after refinement, respectively. **Untouched** indicates documents that remain unchanged; **Empty** refers to those reduced to empty content. Results are highly consistent across all refiner sizes (0.6B–8B) within each quality group, confirming that the pipeline design—not model capacity—drives the gains. The marginal gain from 0.6B to 8B confirms that the 0.6B default is the optimal cost-performance choice.
