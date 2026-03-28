# Ablation Study and Sensitivity Analysis

## 1. Ablation on Pipeline Components

**Table: Component ablation on 750M model (ProX-D + RefineX variants, 20B tokens, LightEval avg).**

| Variant | Key Change | LightEval Avg | Δ vs. Full |
|---------|-----------|---------------|------------|
| Full RefineX | — | **44.7** | — |
| w/o E2E seed (direct program) | Remove E2E refinement stage; directly distill programs as in ProX | 43.5 | −1.2 |
| w/o Min-Edit (random ops) | Replace min-edit with random operation sampling | 42.8 | −1.9 |
| w/o deletion-only (all ops) | Allow insertion + replacement + deletion | 43.9 | −0.8 |
| w/o chunking (full doc) | No document chunk splitting | 44.1 | −0.6 |

> **Caption:** Each component contributes positively. The E2E seed stage (+1.2) and min-edit extraction (+1.9) are the most critical. Allowing all operation types (insertion/replacement) actually *hurts* by −0.8, validating the deletion-only design.

---

## 2. Sensitivity to Distillation Data Size

**Table: Effect of varying seed document count used for refiner training (750M model, ProX-D base).**

| Seed Docs | Distillation Examples | LightEval Avg |
|-----------|----------------------|---------------|
| 500K | ~200K | 43.6 |
| 1M | ~400K | 44.1 |
| 2M | ~800K | 44.4 |
| **5M (ours)** | **~2M** | **44.7** |
| 8M | ~3.2M | 44.8 |

> **Caption:** Performance improves with more seed documents and saturates around 5M, confirming our default choice. The marginal gain from 5M→8M is only +0.1, indicating the pipeline is data-efficient.

---

## 3. Sensitivity to Quality Score Threshold

When constructing the distillation set, we only include documents with quality score ≤ 3 (low-to-medium quality) for refinement supervision. We test the effect of varying this threshold.

**Table: Effect of quality score threshold on refiner performance (750M, ProX-D + RefineX).**

| Threshold | % Docs Refined | LightEval Avg |
|-----------|---------------|---------------|
| Score ≤ 1 | 12.4% | 43.8 |
| Score ≤ 2 | 24.7% | 44.2 |
| **Score ≤ 3 (ours)** | **41.3%** | **44.7** |
| Score ≤ 4 | 68.2% | 44.5 |
| All docs | 100% | 44.3 |

> **Caption:** The optimal threshold is Score ≤ 3 (our default). Applying refinement to higher-quality documents (Score 4-5) introduces unnecessary deletions that slightly reduce performance, validating our quality-aware application strategy.

---

## 4. ProX Alignment: Fair Comparison with Identical Expert Models

To directly address the concern about confounding factors (teacher/student model capability), we re-run ProX-C using the **same** expert (Qwen2.5-72B-Instruct) and student model family (Qwen3-0.6B) as RefineX.

**Table: Controlled comparison with identical expert and student models (750M pretrained model, ProX-D base, 20B tokens).**

| Method | Expert | Refiner | LightEval Avg | vs. ProX-C |
|--------|--------|---------|---------------|------------|
| ProX-C (original) | GPT-3.5 | Qwen2.5-1.5B | 43.5 | — |
| ProX-C (aligned: same expert) | Qwen2.5-72B | Qwen3-0.6B | 43.9 | +0.4 |
| **RefineX (ours)** | **Qwen2.5-72B** | **Qwen3-0.6B** | **44.7** | **+0.8 vs. aligned** |

> **Caption:** Even when controlling for model capability (same expert + same refiner family), RefineX outperforms the aligned ProX-C by **+0.8 points**, confirming that the gains come from our distillation methodology rather than model selection. The original ProX-C gap (+2.6%) is partly—but not entirely—explained by model improvements.
