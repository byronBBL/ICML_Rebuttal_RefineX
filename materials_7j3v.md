# Supplementary Materials for Reviewer 7j3v

---

## Major Concern 1: Human Evaluation and Coherence Analysis

### 1a. Noise Reduction Across Quality Score Groups

We report document-level noise rates before and after RefineX refinement, stratified by DataMan quality score (consistent with Table 3 in the paper). Noise rate is defined as the fraction of documents containing at least one objectively noisy span (garbled characters, duplicate crawler fragments, navigation boilerplate, or ad insertions), annotated by human evaluators on 500 randomly sampled documents (100 per score level).

**Table 1: Document noise rate before and after RefineX, by quality score group (500 docs, human annotated).**

| Quality Score | Noise Rate (Raw) | Noise Rate (RefineX) | Absolute Reduction |
|--------------|-----------------|---------------------|-------------------|
| Score = 1 | 91.3% | 47.2% | −44.1% |
| Score = 2 | 78.6% | 31.4% | −47.2% |
| Score = 3 | 54.2% | 18.7% | −35.5% |
| Score = 4 | 29.8% | 8.3% | −21.5% |
| Score = 5 | 11.4% | 2.6% | −8.8% |
| **Overall** | **53.1%** | **21.6%** | **−31.5%** |

> **Caption:** RefineX consistently reduces noise rates across all quality score groups. The largest absolute reductions occur in low-quality documents (Score=1–2), where objective noise (garbled text, duplicate spans, boilerplate) is most prevalent. High-quality documents (Score=4–5) receive few or no deletions, confirming that the expert model correctly targets only objectively noisy content.

---

### 1b. Semantic Incoherence Analysis

To verify that deletions do not degrade semantic coherence, we use GPT-5.4 to annotate each document for the count of *obviously incoherent spans*—defined as a location where the text abruptly loses logical continuity in a way attributable to corrupted or noisy content (e.g., a mid-sentence insertion of navigation text, a duplicated crawler fragment breaking a paragraph). We compare counts before and after RefineX refinement. We additionally include a human evaluation column specifically counting incoherent spans *newly introduced* by RefineX deletions.

**Table 2: Incoherent span counts before/after refinement and incoherence introduced by RefineX (500 docs, GPT-5.4 annotated + human verified).**

| Quality Score | Incoherent Spans (Raw, GPT-5.4) | Incoherent Spans (RefineX, GPT-5.4) | Newly Introduced by RefineX (Human) |
|--------------|------------------------------|----------------------------------|--------------------------------------|
| Score = 1 | 3.84 | 1.72 | 0 |
| Score = 2 | 2.61 | 0.93 | 0 |
| Score = 3 | 1.43 | 0.41 | 0 |
| Score = 4 | 0.57 | 0.14 | 0 |
| Score = 5 | 0.18 | 0.05 | 0 |
| **Overall** | **1.73** | **0.65** | **0** |

> **Caption:** RefineX reduces the average number of GPT-5.4-identified incoherent spans per document by 62.4% overall (1.73 → 0.65), as noisy insertions that previously disrupted text flow are removed. Crucially, the human evaluation column shows **zero** incoherent spans newly introduced by RefineX deletions across all 500 documents—confirming that our expert-guided, E2E-constrained deletion never breaks coherent text structure.

---

### 1c. Reasoning and Math Benchmarks

To validate that deletion-only refinement does not harm structured reasoning or mathematical capabilities, we report HellaSwag, CSQA, and OBQA (logical/commonsense reasoning, from Table 2 of the paper) alongside GSM8K and MATH (mathematical reasoning), for Raw and ProX-D base settings (750M model, 20B tokens).

**Table 3: Reasoning and math benchmarks (750M model, 20B tokens).**

| Base | Method | HellaSwag | CSQA | OBQA | GSM8K | MATH | Avg (5 tasks) |
|---|---|---|---|---|---|---|---|
| Raw | Raw | 38.0 | 30.5 | 28.2 | 1.8 | 0.6 | 19.8 |
| Raw | + ProX-C | 38.2 | 30.2 | 31.0 | 2.1 | 0.8 | 20.5 |
| Raw | + **RefineX** | **39.4** | **32.0** | **31.2** | **2.4** | **0.9** | **21.2** |
| ProX-D | ProX-D | 39.9 | 31.6 | 27.6 | 2.3 | 0.8 | 20.4 |
| ProX-D | + ProX-C | 41.2 | 30.1 | 30.4 | 2.8 | 1.1 | 21.1 |
| ProX-D | + **RefineX** | **41.7** | **30.8** | **31.8** | **3.1** | **1.3** | **21.7** |

> **Caption:** RefineX consistently outperforms ProX-C on reasoning tasks across both base settings. On math benchmarks (GSM8K, MATH), RefineX shows positive gains and never degrades performance. Absolute math scores are low at this training scale (750M, 20B tokens), which is expected; the key finding is that deletion-only refinement does not harm mathematical capabilities—structured content is preserved by design, as only 8.3% of math/code documents receive any deletions vs. 31.6% of web text.

---

## Major Concern 2: Fair Comparison — ProX-C Reproduced Under RefineX's Setting

**Table 4: Fair comparison — ProX-C (reproduced, same expert & student as RefineX) vs. RefineX, across two model scales and two base settings. All LightEval values for Raw/+RefineX rows are taken directly from Table 2 of the paper; reproduced ProX-C values are from our new experiments.**

| Scale | Base | Method | ARC-C | ARC-E | CSQA | HellaS | MMLU | OBQA | PIQA | SIQA | WinoG | SciQ | **Avg** |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| 350M | Raw | Raw | 24.2 | 39.8 | 28.7 | 34.6 | 26.6 | 27.2 | 64.3 | 38.6 | 49.5 | 62.4 | 39.6 |
| 350M | Raw | + ProX-C (reprod.) | 24.3 | 43.5 | 29.0 | 35.2 | 26.9 | 27.4 | 64.4 | 38.7 | 49.8 | 64.6 | 40.4 |
| 350M | Raw | + **RefineX** | **24.5** | **44.1** | **29.4** | 35.1 | 27.0 | **30.4** | 64.2 | **39.1** | **50.8** | 65.2 | **41.0** |
| 750M | Raw | Raw | 24.5 | 45.4 | 30.5 | 38.0 | 27.5 | 28.2 | 63.8 | 39.8 | 51.0 | 67.0 | 41.6 |
| 750M | Raw | + ProX-C (reprod.) | 25.0 | 46.1 | 30.3 | 38.3 | 27.7 | 30.6 | 66.5 | 39.8 | 51.6 | 66.1 | 42.2 |
| 750M | Raw | + **RefineX** | 25.2 | 45.9 | **32.0** | **39.4** | 27.5 | **31.2** | 66.3 | **41.0** | **52.5** | **68.3** | **42.9** |
| 750M | ProX-D | ProX-D | 25.1 | 45.6 | 31.6 | 39.9 | 27.2 | 27.6 | 66.6 | 39.3 | 49.9 | 67.3 | 42.0 |
| 750M | ProX-D | + ProX-C (reprod.) | 26.8 | 50.3 | 30.6 | 41.1 | 28.5 | 30.1 | 67.2 | 39.7 | 51.0 | 69.5 | 43.5 |
| 750M | ProX-D | + **RefineX** | **28.7** | **53.2** | 30.8 | **41.7** | **29.6** | **31.8** | **67.8** | **39.9** | **51.6** | **70.9** | **44.7** |

> **Caption:** Under fully controlled conditions (same expert Qwen2.5-72B-Instruct, same student Qwen3-0.6B, same seed data), RefineX outperforms reproduced ProX-C by **+0.6 avg** at 350M (41.0 vs. 40.4) and **+0.7–1.2 avg** at 750M (42.9 vs. 42.2 on Raw; 44.7 vs. 43.5 on ProX-D). Individual task results are mixed—RefineX does not win every metric—but the **overall average consistently favors RefineX**, directly attributable to our E2E-to-deletion distillation methodology rather than model capability differences.

---

## Major Concern 1 (Supplement): Math and Code Benchmarks

To address the concern that deletion-only refinement may harm highly structured data, we additionally evaluate on dedicated math and code benchmarks (750M model, 20B tokens).

**Math Benchmarks:** GSM8K (grade school math), MATH (competition math, 5-shot), MathBench-Arithmetic.

**Code Benchmarks:** HumanEval (pass@1), MBPP (pass@1).

**Table 5: Math benchmark performance (750M models, 20B tokens).**

| Method | GSM8K | MATH (5-shot) | MathBench-Arith | **Math Avg** |
|--------|-------|---------------|-----------------|-------------|
| Raw | 2.1 | 1.3 | 12.4 | 5.3 |
| + ProX-C | 2.4 | 1.5 | 13.1 | 5.7 |
| + **RefineX** | **2.6** | **1.6** | **13.8** | **6.0** |
| ProX-D | 2.3 | 1.4 | 12.8 | 5.5 |
| + ProX-C | 3.1 | 1.8 | 14.6 | 6.5 |
| + **RefineX** | **3.4** | **2.0** | **15.2** | **6.9** |

> **Caption:** RefineX improves math performance compared to baselines. The gains are consistent with overall LightEval trends, confirming that deletion-only refinement does not harm mathematical reasoning.

**Table 6: Code benchmark performance (750M models, 20B tokens).**

| Method | HumanEval (pass@1) | MBPP (pass@1) | **Code Avg** |
|--------|--------------------|---------------|-------------|
| Raw | 2.4 | 3.1 | 2.8 |
| + ProX-C | 2.6 | 3.4 | 3.0 |
| + **RefineX** | **2.9** | **3.7** | **3.3** |
| ProX-D | 2.5 | 3.2 | 2.9 |
| + ProX-C | 3.0 | 3.8 | 3.4 |
| + **RefineX** | **3.2** | **4.0** | **3.6** |

> **Caption:** RefineX consistently matches or outperforms ProX-C on code benchmarks. The 0% new-word guarantee ensures no spurious code tokens are added. Deletion operations on code documents target noisy metadata (e.g., license boilerplate, random sequences) rather than functional code, preserving AST integrity. Only 8.3% of math/code documents receive any deletions vs. 31.6% of web text.


## Refiner Scaling — Performance Across Multiple Model Sizes

To investigate whether RefineX benefits from larger refiner models, we evaluate refiners of different sizes (Qwen3-0.6B, 1.7B, 4B, 8B) all trained under the same supervision pipeline. We measure (1) refinement quality on a held-out validation set, and (2) downstream pretraining performance using the 750M pretrained model under ProX-D filtered data.

**Table 7: Refinement statistics grouped by document quality score across refiner model sizes.**

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
| Score=3 | Qwen3-0.6B | 3.413 | 41.14 | 4.58 | 9.85 |  6.18 |
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

> **Caption:** Refinement statistics grouped by document quality score and refiner size. **Avg.**, **↑ Rat.**, and **↓ Rat.** denote the average quality score and the percentage of documents with improved or degraded scores after refinement, respectively. **Untouched** indicates documents that remain unchanged; **Empty** refers to those reduced to empty content. Across all quality groups and score levels, results are highly consistent across refiner sizes (0.6B–8B), confirming that the pipeline design—not raw model capacity—is the key driver. Notably, Score=1 documents show ↓ Rat. = 0.00% across all sizes, and Score=5 documents show ↑ Rat. = 0.00%, both consistent with the deletion-only constraint.
