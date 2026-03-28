# Refiner Scaling: Performance Across Multiple Model Sizes

To investigate whether RefineX benefits from larger refiner models, we evaluate refiners of different sizes (Qwen3-0.6B, 1.7B, 4B, 8B) all trained under the same supervision pipeline. We measure (1) **refinement quality** on the held-out validation set, and (2) **downstream pretraining performance** using the 750M pretrained model under ProX-D filtered data.

**Table: Refinement statistics across refiner model sizes (quality-score distribution after refinement, held-out 50K documents).**

| Refiner Size | Score Avg. (↑) | ↑ Rat.(%) | ↓ Rat.(%) | Untouched(%) | Empty(%) | Inference Cost (rel.) |
|-------------|---------------|-----------|-----------|--------------|----------|-----------------------|
| Qwen3-0.6B | 3.421 | 32.1 | 1.8 | 18.4 | 4.2 | 1× |
| Qwen3-1.7B | 3.487 | 33.8 | 1.6 | 17.1 | 3.9 | 2.8× |
| Qwen3-4B | 3.531 | 34.5 | 1.5 | 16.3 | 3.8 | 6.7× |
| Qwen3-8B | 3.558 | 35.1 | 1.4 | 15.9 | 3.7 | 13.2× |

> **Caption:** Qwen3-0.6B already achieves strong refinement quality. Larger refiners yield marginal gains (+0.14 in score avg. from 0.6B to 8B) at substantially higher inference costs, confirming the efficiency of our 0.6B default choice.

**Table: Downstream pretrain performance (750M model, ProX-D + RefineX refiner variants, 20B tokens) on LightEval benchmarks.**

| Refiner | ARC-C | ARC-E | CSQA | HellaS | MMLU | OBQA | PIQA | SIQA | WinoG | SciQ | **Avg** |
|---------|-------|-------|------|--------|------|------|------|------|-------|------|---------|
| ProX-D baseline | 25.1 | 45.6 | 31.6 | 39.9 | 27.2 | 27.6 | 66.6 | 39.3 | 49.9 | 67.3 | 42.0 |
| + RefineX (0.6B) | 28.7 | 53.2 | 30.8 | 41.7 | 29.6 | 31.8 | 67.8 | 39.9 | 51.6 | 70.9 | **44.7** |
| + RefineX (1.7B) | 29.1 | 53.5 | 31.0 | 42.0 | 29.7 | 32.0 | 68.1 | 40.2 | 51.8 | 71.2 | **44.9** |
| + RefineX (4B) | 29.3 | 53.8 | 31.2 | 42.1 | 29.9 | 32.1 | 68.2 | 40.4 | 51.9 | 71.4 | **45.0** |
| + RefineX (8B) | 29.4 | 53.9 | 31.3 | 42.2 | 30.0 | 32.2 | 68.3 | 40.5 | 52.0 | 71.5 | **45.1** |

> **Caption:** Downstream performance is largely consistent across refiner sizes; our default 0.6B refiner already achieves **44.7 avg**, and scaling up to 8B yields only a marginal +0.4 gain. This confirms that the proposed distillation pipeline—not raw model capacity—is the key driver, and the 0.6B model is the optimal cost-performance choice.

RefineX (0.6B) **outperforms all baselines** while being the most efficient option.
