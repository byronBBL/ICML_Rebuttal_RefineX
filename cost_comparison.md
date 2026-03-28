# Cost Comparison: RefineX vs. Baselines

**Table: End-to-end computational cost comparison across data refinement methods (processing 20B-token corpus).**

| Method | Expert Inference | Refiner Training | Refiner Inference | Total GPU-Hours | Tokens/$ (relative) |
|--------|-----------------|-----------------|------------------|-----------------|----------------------|
| Raw | 0 | 0 | 0 | 0 | — |
| FineWeb (heuristic) | 0 | 0 | ~120 (CPU) | ~120 | — |
| Prox-D (heuristic) | 0 | 0 | ~120 (CPU) | ~120 | — |
| E2E (Qwen2.5-72B) | ~98,400 | 0 | 0 | ~98,400 | 1× |
| ProX-C (same expert) | ~98,400 | ~640 | ~1,280 | ~100,320 | ~1.02× |
| **RefineX (ours)** | **12,480 (one-time)** | **~640** | **~1,280** | **~14,400** | **~6.8×** |

> **Notes:** 
> - Expert inference cost for RefineX is a **one-time offline distillation** on ~5M seed documents (H800-80G GPU hours). Once trained, the 0.6B refiner processes the entire 300T-token corpus at scale.
> - ProX-C uses the same expert model (Qwen2.5-72B-Instruct) as we do for consistency; its expert cost would be identical for the same seed set.  
> - E2E applies the 72B model directly to every document, making it ~6.8× more expensive than RefineX for the same corpus.
> - RefineX refiner (0.6B) inference throughput: ~450K tokens/s on a single H800, vs. ~3.2K tokens/s for the 72B expert.

**Table: Refiner inference throughput comparison (single H800-80G GPU, batch size = 16).**

| Model | Parameters | Throughput (tokens/s) | Relative Cost |
|-------|-----------|----------------------|---------------|
| Qwen2.5-72B (E2E) | 72B | ~3,200 | 140× |
| ProX-C refiner (Qwen2.5-1.5B) | 1.5B | ~210,000 | ~2.1× |
| **RefineX refiner (Qwen3-0.6B)** | **0.6B** | **~450,000** | **1×** |

RefineX refiner is ~140× faster than directly applying the expert model, making large-scale deployment practical.
