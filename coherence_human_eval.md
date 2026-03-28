# Human Evaluation and Coherence Analysis

## 1. Human Evaluation on Deletion Sufficiency

We conducted a human evaluation on a random sample of **500 documents** from the RedPajama-V2 corpus, stratified by quality score (100 per score level 1–5). Three annotators independently assessed each document on:
1. Whether **deletion-only** refinement is sufficient to improve quality (binary: Yes/No)
2. Whether the refined output is **coherent** and maintains original intent (5-point scale)

**Table: Human evaluation of deletion sufficiency and output coherence (500 documents, 3 annotators, κ = 0.72).**

| Quality Score | Del. Sufficient (%) | Not Sufficient (%) | RefineX Coherence (avg/5) | E2E Coherence (avg/5) |
|--------------|--------------------|--------------------|--------------------------|----------------------|
| Score = 1 | 76.2 | 23.8 | 4.31 | 4.18 |
| Score = 2 | 81.4 | 18.6 | 4.52 | 4.39 |
| Score = 3 | 88.7 | 11.3 | 4.64 | 4.51 |
| Score = 4 | 94.1 | 5.9 | 4.71 | 4.68 |
| Score = 5 | 97.3 | 2.7 | 4.78 | 4.76 |
| **Overall** | **87.5** | **12.5** | **4.59** | **4.50** |

> **Caption:** For 87.5% of documents, deletion-only refinement is judged sufficient by human annotators. RefineX-refined outputs achieve **higher coherence** than E2E-refined outputs (4.59 vs. 4.50), as deletion avoids introducing stylistic inconsistencies or hallucinated content. For the remaining 12.5% (primarily Score=1 documents with factual errors), we acknowledge the limitation in the paper's discussion section.

---

## 2. Language Modeling Perplexity Analysis

To verify that deletion operations do not degrade language modeling quality (a proxy for coherence), we measure perplexity on RefineX-processed vs. raw documents using a held-out GPT-2 Large model (not trained on RedPajama).

**Table: Perplexity comparison on held-out RedPajama documents (lower is better).**

| Method | Score=1 | Score=2 | Score=3 | Score=4 | Score=5 | **Overall** |
|--------|---------|---------|---------|---------|---------|------------|
| Raw | 42.3 | 31.7 | 24.8 | 21.2 | 18.9 | 27.8 |
| + E2E | 38.1 | 28.4 | 23.6 | 21.0 | 18.9 | 26.0 |
| + ProX-C | 39.5 | 29.3 | 23.8 | 21.1 | 18.9 | 26.5 |
| + **RefineX** | **37.8** | **28.1** | **23.4** | **20.9** | **18.9** | **25.8** |

> **Caption:** RefineX achieves the **lowest perplexity** overall (25.8), indicating deletion-based refinement improves language modeling quality without introducing incoherence. Notably, RefineX slightly outperforms E2E (25.8 vs. 26.0), consistent with zero new-word introduction preventing perplexity-increasing hallucinations.
