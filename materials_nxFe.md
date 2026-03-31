# Supplementary Materials for Reviewer nxFe

---

## Q: End-to-End Cost Comparison

Full cost breakdown for a realistic pretraining scenario of **30T tokens** (a standard scale for frontier model pretraining).

**Assumptions:** 72B expert on 8×H800 GPUs (160 tokens/s); 0.6B refiner on same hardware (~8,000 tokens/s); avg. 500 tokens/doc; RefineX expert applied once to 5M seed docs.

**Table 1: End-to-end GPU cost comparison across methods (30T token pretraining scenario).**

| Method | Expert Inference | Refiner Training | Refiner Inference | Total GPU-hrs | Fine-grained |
|---|---|---|---|---|---|
| Raw | 0 | 0 | 0 | 0 | Doc-level |
| FineWeb / ProX-D (heuristic) | 0 | 0 | ~0 | ~0 | Doc-level |
| E2E (Qwen2.5-72B) | ~147,600,000 | 0 | 0 | ~147,600,000 | Char-level |
| ProX-C (0.6B refiner) | ~12,480 (one-time) | ~640 | ~1,920,000 | ~1,933,120 | Char-level |
| **RefineX (ours)** | **~12,480 (one-time)** | **~640** | **~1,920,000** | **~1,933,120** | **Char-level** |

> **Caption:** RefineX achieves the same total cost as ProX-C (1.9M GPU-hrs) while delivering superior supervision quality via E2E-guided min-edit extraction. This is a 76× reduction vs. full E2E inference (~147.6M GPU-hrs), enabling char-level refinement at practical scale.
