# Evaluation on Math and Code Benchmarks

To address concerns about deletion-only refinement potentially harming structured data (code, math), we evaluate our 750M pretrained models on dedicated math and code benchmarks in addition to the LightEval suite.

**Math Benchmarks:** GSM8K (grade school math), MATH (competition math, 5-shot), MathBench-Arithmetic.

**Code Benchmarks:** HumanEval (pass@1), MBPP (pass@1).

**Table: Math benchmark performance (750M models, 20B tokens, ProX-D filtered base corpus).**

| Method | GSM8K | MATH (5-shot) | MathBench-Arith | **Math Avg** |
|--------|-------|---------------|-----------------|-------------|
| Raw | 2.1 | 1.3 | 12.4 | 5.3 |
| + ProX-C | 2.4 | 1.5 | 13.1 | 5.7 |
| + **RefineX** | **2.6** | **1.6** | **13.8** | **6.0** |
| ProX-D | 2.3 | 1.4 | 12.8 | 5.5 |
| + ProX-C | 3.1 | 1.8 | 14.6 | 6.5 |
| + **RefineX** | **3.4** | **2.0** | **15.2** | **6.9** |

> **Caption:** RefineX **improves** math performance compared to baselines, indicating that deletion-only refinement does not harm mathematical reasoning. The gains are consistent with overall LightEval trends.

**Table: Code benchmark performance (750M models, 20B tokens, ProX-D filtered base corpus).**

| Method | HumanEval (pass@1) | MBPP (pass@1) | **Code Avg** |
|--------|--------------------|---------------|-------------|
| Raw | 2.4 | 3.1 | 2.8 |
| + ProX-C | 2.6 | 3.4 | 3.0 |
| + **RefineX** | **2.9** | **3.7** | **3.3** |
| ProX-D | 2.5 | 3.2 | 2.9 |
| + ProX-C | 3.0 | 3.8 | 3.4 |
| + **RefineX** | **3.2** | **4.0** | **3.6** |

> **Caption:** RefineX consistently matches or outperforms ProX-C on code benchmarks. The 0% new-word introduction guarantee means no spurious code tokens are added. Importantly, our deletion operations on code documents target noisy metadata (e.g., license boilerplate, random sequences) rather than functional code, preserving AST integrity.

**Note on deletion safety for code/math:** Our minimum-edit-distance extraction is guided by expert-model refined outputs. For code and math documents, the expert model (Qwen2.5-72B-Instruct) largely outputs `keep_all()`, meaning the refiner learns to leave well-formed structured content untouched. The fraction of code/math documents receiving deletions is significantly lower than for web text (8.3% vs. 31.6%), further reducing the risk of structural corruption.
