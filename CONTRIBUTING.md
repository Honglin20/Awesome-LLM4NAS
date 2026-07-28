# Contributing to Awesome LLM4NAS & NAS4LLM

Thanks for helping keep this list accurate and comprehensive! This file explains **what we accept**, **how to place a paper**, and **the per-entry format**.

## 1. Inclusion criteria

**In scope** — a paper is accepted if it has all of:

- A clear, verifiable role for **LLM × NAS** (see the two axes below).
- A reachable primary link (arXiv / OpenReview / ACL Anthology / DOI / publisher). ResearchGate-only or Research-Square-only preprints are **not** accepted unless an arXiv or venue version exists.
- Enough public detail to fill the columns (especially the *Algorithm & Search Space* / *What it does* and *Hardware-awareness* fields).

**Out of scope** — do not add:

- Pure prompt-engineering that never touches model *structure*.
- Pure quantization / distillation **unless** it is framed as an architecture/bit-width **search**.
- LLM training tricks, scaling-law studies, or fixed hand-designed architectures with **no search** component. (Note: `LLaMA Pro` and `SOLAR` are included only as *context rows* — they are growth recipes, not searches, and are marked as such. Prefer not to add more of these.)
- Papers that the contributor has not at least skimmed for accuracy — every claim (venue, code link, HW-awareness) must be checkable.

## 2. The single placement rule: LLM4NAS vs NAS4LLM

Ask one question:

> **Is the LLM the thing being searched, or the thing doing the search?**

- The LLM is the **target** (its depth / width / MoE / precision / attention structure is explored) → **Part II · NAS4LLM**.
- The LLM is the **agent / tool** (it generates, mutates, scores, or advises someone else's architecture) → **Part I · LLM4NAS**.

Borderline example: `LLaMA-NAS` searches sub-architectures *of* an LLM → **NAS4LLM**. `LLM-NAS` (Zhu et al.) uses an LLM *to* search HW-NAS-Bench architectures → **LLM4NAS**. When genuinely ambiguous, open an issue and tag the row `borderline` until resolved.

## 3. LLM4NAS — the 6 role tags

Tag a Part-I paper with the **primary** role (and optionally one secondary, `+②`):

| Tag | Role | Meaning |
|-----|------|---------|
| ① | Direct Code Generation | LLM emits architecture code/spec that is then trained. |
| ② | Iterative / Evolutionary | LLM performs mutation/crossover inside a feedback-driven search loop. |
| ③ | Predictor / Zero-Cost Proxy | LLM scores candidates without training them. |
| ④ | Knowledge / Advisor | LLM transfers design knowledge to build/prune/bias the search space. |
| ⑤ | End-to-End Agentic | LLM autonomously orchestrates the whole AutoML/NAS pipeline. |
| ⑥ | Constraint-Aware | Search under explicit hardware / fairness / resource budgets. |

There is also a **Cross-Domain / Modality-Specific** bucket for Part-I work outside standard image classification (GNN, quantum, drug discovery, time-series, …).

## 4. NAS4LLM — organization by *what is searched*

Part-II sections are defined by the **search space**, not the algorithm:

- `Structured Pruning as Search` — heads / neurons / layers / channels selected by gradient, importance, or EA.
- `Width / Depth / Layer / Block & Elastic Supernet` — macro-structure search, often via a once-trained supernet.
- `Mixture-of-Experts (MoE) Search` — expert topology / count / routing / subsetting.
- `Mixed-Precision / Quantization Search` — per-layer bit-widths / schedules.
- `Attention / Sublayer Structure Search` — attention patterns/types, sublayer order, activations.
- `Hardware-Aware (LLM-era)` — search whose objective embeds device latency/energy/memory.
- `Foundations (Pre-LLM Transformer NAS)` — canonical templates (HAT, HAQ, HAWQ, ZipLM, …). Add sparingly.
- `Benchmarks` — NAS benchmarks for LMs.

Put the paper in the section matching its **primary** search space. State both the **algorithm** (RL / EA / gradient / differentiable / predictor / random / one-shot weight-sharing / mask-mutation / …) and the **search space** in the *Algorithm & Space* column.

## 5. The Hardware-awareness column (the error-prone one)

This column is **the** differentiator of this list, so be precise. Allowed values:

- `—` — the paper is accuracy/quality-driven only; no latency/energy/memory/FLOPs modeling anywhere.
- `✓ <short note>` — hardware is modeled. You **must** state:
  1. **Metric** — latency / throughput / energy / peak memory / FLOPs / params / active-param budget.
  2. **Modeling** — *lookup table* (measured on devices) / *analytical* / *on-device measurement* / *end-to-end in the search objective* / *post-hoc reporting only*.
  3. Where possible, a one-line quantified result (e.g. "up to 54% lower latency").

If latency is only **reported after the search** (not inside the objective), say so explicitly (e.g. `✓ speedup reported post-hoc`). Do **not** write `✓` for a paper that merely mentions "efficient" or "fewer FLOPs" without modeling — use `—` or `✓ FLOPs proxy only`.

## 6. Per-entry template

Copy this row shape and fill it. Keep *What it does* to ≤ 2 sentences.

**Part I (LLM4NAS)**
```
| YEAR | VENUE | [Title](https://arxiv.org/abs/XXXX.XXXXX) | ① (+②) | One-sentence what the LLM does + how. | HW-aware cell | [✓](repo-url) or — |
```

**Part II (NAS4LLM)**
```
| YEAR | VENUE | [Title](https://arxiv.org/abs/XXXX.XXXXX) | Algorithm + Search space | One-sentence what is searched + how. | HW-aware cell | [✓](repo-url) or — |
```

Conventions:
- **Year** = publication year (use the venue year, not the arXiv v1 year, when they differ).
- **Venue** = short form (NeurIPS, ICML, ICLR, AAAI, ACL, EMNLP, CVPR, arXiv, …). Append `-W`/`Findings`/`D&B`/`Industry` where relevant.
- **Paper** = title links to the canonical primary source (prefer arXiv `abs` URL; OpenReview/DOI otherwise).
- **Code** = link to the **official** repo if one exists and is reachable; otherwise `—`. Do not link unrelated third-party forks.
- Order rows within a section by **year, newest first**.

## 7. Before you open a PR

- Verify the link resolves and the venue/year/code are correct.
- Confirm the HW-awareness cell against the paper (not the abstract alone).
- Make sure the paper isn't already listed under another section (search the README).
- One paper = one row (pick the primary section; note secondary tags in the Method/Algorithm cell).

Thanks for contributing!
