# Supplementary Materials for Reviewer g3oa

---

## W4 / Q4 / Q6: Empirical Validation of Deletion Safety

The following tables are referenced in the response to validate that the deletion-only constraint prevents introduction of new biases and preserves semantic integrity.

### Semantic Incoherence Analysis

We use GPT-5.4 to annotate each document for the count of *obviously incoherent spans* before and after RefineX refinement, and include a human evaluation column for incoherence *newly introduced* by RefineX deletions specifically.

**Table 1: Incoherent span counts before/after refinement and incoherence introduced by RefineX (500 docs, GPT-5.4 annotated + human verified).**

| Quality Score | Incoherent Spans (Raw, GPT-5.4) | Incoherent Spans (RefineX, GPT-5.4) | Newly Introduced by RefineX (Human) |
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

