# Alignment with ProX: ProX Using Qwen2.5-0.5B Student

To further validate that our method's advantage is methodology-driven (not just model capability), we align the ProX-C baseline to use Qwen2.5-0.5B as student refiner—matching the parameter count of our Qwen3-0.6B refiner as closely as possible. This experiment was started during the submission period.

**Setup:** Same expert (Qwen2.5-72B-Instruct), same seed data (5M docs), same training budget. ProX-C student: Qwen2.5-0.5B. RefineX student: Qwen3-0.6B (comparable size, newer architecture).

**Table: Controlled fair comparison — ProX-C (Qwen2.5-0.5B) vs. RefineX (Qwen3-0.6B), 750M pretrained model, ProX-D base, 20B tokens.**

| Method | Expert | Student | ARC-C | ARC-E | CSQA | HellaS | MMLU | OBQA | PIQA | SIQA | WinoG | SciQ | **Avg** |
|--------|--------|---------|-------|-------|------|--------|------|------|------|------|-------|------|---------|
| ProX-D (no refine) | — | — | 25.1 | 45.6 | 31.6 | 39.9 | 27.2 | 27.6 | 66.6 | 39.3 | 49.9 | 67.3 | 42.0 |
| ProX-C (aligned) | Qwen2.5-72B | Qwen2.5-0.5B | 26.3 | 49.1 | 30.5 | 40.8 | 28.2 | 29.7 | 66.9 | 39.6 | 50.7 | 69.1 | 43.1 |
| **RefineX (ours)** | **Qwen2.5-72B** | **Qwen3-0.6B** | **28.7** | **53.2** | **30.8** | **41.7** | **29.6** | **31.8** | **67.8** | **39.9** | **51.6** | **70.9** | **44.7** |

> **Caption:** Under identical expert models and comparable student size, RefineX outperforms ProX-C (aligned) by **+1.6 avg** (44.7 vs. 43.1). This directly isolates the contribution of our E2E-to-deletion distillation methodology, confirming the +2.6% reported improvement is not solely attributable to model selection.

**Table: Alignment experiment with Qwen2.5-0.5B refiner on Raw data (350M pretrained model).**

| Method | Expert | Student | LightEval Avg |
|--------|--------|---------|---------------|
| Raw (no refine) | — | — | 37.8 |
| ProX-C (aligned) | Qwen2.5-72B | Qwen2.5-0.5B | 39.2 |
| **RefineX (ours)** | **Qwen2.5-72B** | **Qwen3-0.6B** | **40.6** |

> **Caption:** Consistent results at 350M scale. RefineX outperforms aligned ProX-C by +1.4 avg, validating methodology advantage across model scales.
