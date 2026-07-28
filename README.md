# 🧠 Awesome LLM4NAS & NAS4LLM

> A curated, actively-maintained list of papers on **Large Language Models × Neural Architecture Search** — the intersection of *"using LLMs to design model architectures"* and *"searching the architecture of LLMs themselves."*

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](./CONTRIBUTING.md)
[![Papers](https://img.shields.io/badge/Papers-85+-blue.svg)](#-table-of-contents)

The field sits on **two complementary axes** that are constantly confused. This repo keeps them deliberately separate:

| Axis | LLM plays the role of… | Target being optimized | Section |
|------|------------------------|------------------------|---------|
| **LLM4NAS** | the **searcher / designer** (it generates, mutates, scores, or advises) | usually a *non-LLM* model (CNN, GNN, RL policy, …) | [Part I](#part-i--llm4nas-llm-as-the-searcher) |
| **NAS4LLM** | the **search target** (its own structure is reshaped) | an LLM / Transformer itself | [Part II](#part-ii--nas4llm-llm-as-the-search-target) |

> **Scope rule (how to place a paper):** *Is the LLM the thing being searched, or the thing doing the search?* If the LLM is the **object** whose depth/width/MoE/precision is being explored → **NAS4LLM**. If the LLM is the **agent** that proposes or evaluates someone else's architecture → **LLM4NAS**. (This is exactly where the existing `zhichao-lu/Awesome-LLM-NAS` list goes wrong — it files `LLaMA-NAS` under "LLM-based NAS" even though there the LLM is the *target*, not the *searcher*.)

Each entry lists **Year · Venue · Paper · Method · What it does · Hardware-awareness · Code**. Hardware-awareness is called out explicitly everywhere it applies — *what* metric is modeled and *how* (lookup table, analytical, on-device measurement, end-to-end in the search objective).

---

## 📊 Taxonomy at a Glance

**Part I — LLM4NAS** (6 roles the LLM can play):

| # | Role | LLM does… |
|---|------|-----------|
| ① | **Direct Code Generation** | writes the architecture as code, then it gets trained |
| ② | **Iterative / Evolutionary** | mutates/crosses candidates inside a search loop, driven by feedback |
| ③ | **Predictor / Zero-Cost Proxy** | scores candidates *without* training |
| ④ | **Knowledge / Advisor** | transfers human design knowledge to prune or shape the search space |
| ⑤ | **End-to-End Agentic** | autonomously orchestrates the whole AutoML pipeline |
| ⑥ | **Constraint-Aware** | optimizes under hardware / fairness / resource budgets |

**Part II — NAS4LLM** (organized by *what is being searched*):

`Structured Pruning-as-Search` · `Width/Depth/Layer & Elastic Supernet` · `Mixture-of-Experts Search` · `Mixed-Precision / Quantization Search` · `Attention / Sublayer Structure` · `Hardware-Aware (dedicated)` · `Foundations (pre-LLM Transformer NAS)` · `Benchmarks`

---

## 📑 Table of Contents

- [Surveys & Position Papers](#surveys--position-papers)
- **Part I — LLM4NAS (LLM as the searcher)**
  - [① Direct Code Generation](#-direct-code-generation)
  - [② Iterative / Evolutionary Optimization](#-iterative--evolutionary-optimization)
  - [③ Performance Predictor / Zero-Cost Proxy](#-performance-predictor--zero-cost-proxy)
  - [④ Design Knowledge Transfer / Advisor](#-design-knowledge-transfer--advisor)
  - [⑤ End-to-End Agentic](#-end-to-end-agentic)
  - [⑥ Constraint-Aware (Hardware / Fairness)](#-constraint-aware-hardware--fairness)
  - [Cross-Domain / Modality-Specific](#cross-domain--modality-specific)
- **Part II — NAS4LLM (LLM as the search target)**
  - [Structured Pruning as Search](#structured-pruning-as-search)
  - [Width / Depth / Layer / Block & Elastic Supernet](#width--depth--layer--block--elastic-supernet)
  - [Mixture-of-Experts (MoE) Search](#mixture-of-experts-moe-search)
  - [Mixed-Precision / Quantization Search](#mixed-precision--quantization-search)
  - [Attention / Sublayer Structure Search](#attention--sublayer-structure-search)
  - [Hardware-Aware (LLM-era)](#hardware-aware-llm-era)
  - [Foundations (Pre-LLM Transformer NAS)](#foundations-pre-llm-transformer-nas)
  - [Benchmarks](#benchmarks)
- [Contributing](#contributing) · [License](#license) · [Acknowledgements](#acknowledgements)

---

## Surveys & Position Papers

| Year | Venue | Paper | Focus | Link |
|------|-------|-------|-------|------|
| 2025 | ACM | **Large Language Models for Constructing and Optimizing Neural Architectures** (Gu et al.) | The most on-point survey; taxonomy of generation-based vs. prediction-based LLM-NAS. | [arXiv](https://arxiv.org/abs/2411.10478) |
| 2026 | ACM Comp. Surveys | **A Systematic Survey on LLMs for Algorithm Design (LLM4AD)** (Liu et al.) | **Best LLM4NAS skeleton:** dedicated NAS §5.2.2 + a clean 4-role taxonomy (Optimizer / Predictor / Extractor / Designer). | [arXiv](https://arxiv.org/abs/2410.14716) |
| 2026 | Springer AI Review | **Neural Architecture Search from a Natural Language Perspective** | Connects NAS taxonomy with natural-language-driven design. | [Springer](https://link.springer.com/article/10.1007/s10462-026-11550-5) |
| 2021 | IJCAI | **Hardware-Aware NAS: Survey and Taxonomy** (Benmezoline et al.) | Canonical HW-NAS taxonomy — methodological backbone for all ⑥-class work. | [arXiv](https://arxiv.org/abs/2101.09336) |
| 2023 | — | **Neural Architecture Search: Insights from 1000 Papers** (White et al.) | Broad NAS survey used as a taxonomic anchor. | [arXiv](https://arxiv.org/abs/2301.08727) |
| 2025 | — | **Evolutionary Computation in the Era of Large Language Models** (Wu) | Survey of LLM × evolutionary optimization (covers LLMatic-style work). | [PDF](https://ira.lib.polyu.edu.hk/bitstream/10397/113668/1/Wu_Evolutionary_Computation_Era.pdf) |
| 2024 | — | **Advances in Neural Architecture Search** (Wang et al., Tsinghua) | Broad NAS advances incl. the LLM-driven strand. | [PDF](https://mn.cs.tsinghua.edu.cn/xinwang/PDF/papers/2024_Advances%20in%20Neural%20Architecture%20Search.pdf) |
| 2023 | — | **AutoML in the Age of Large Language Models: Challenges, Opportunities and Risks** (Tornede et al.) | Position paper framing NAS-for-LLM search-space & evaluation-cost challenges. | [arXiv](https://arxiv.org/abs/2306.08107) |

---

# Part I — LLM4NAS (LLM as the searcher)

## ① Direct Code Generation
*The LLM emits the architecture as code/spec; the result is then trained and scored.*

| Year | Venue | Paper | Method | What it does | HW-aware | Code |
|------|-------|-------|--------|--------------|----------|------|
| 2023 | arXiv | [Can GPT-4 Perform Neural Architecture Search? (GENIUS)](https://arxiv.org/abs/2304.10970) | ①+② | The founding probe: GPT-4 as a black-box optimizer iteratively proposing architectures over NAS-Bench search spaces; established strong zero-shot design priors. | — | [✓](https://github.com/mingkai-zheng/GENIUS) |
| 2023 | NeurIPS | [EvoPrompting: Language Models for Code-Level NAS](https://arxiv.org/abs/2302.14838) | ①+② | Frozen code LMs act as adaptive mutation/crossover operators in an evolutionary loop that generates code-level architectures; soft-prompt tuning aligns the LM. No official code release. | — | — |
| 2025 | AAAI | [AutoMMLab: Automatically Generating Deployable Models from Language Instructions](https://arxiv.org/abs/2402.15351) | ① | Translates natural-language CV task instructions into a deployable model pipeline (model selection + HPO driven by an LLM). | — | [✓](https://github.com/yang-ze-kang/AutoMMLab) |
| 2024 | GECCO | [LLMatic: NAS via LLMs and Quality Diversity](https://arxiv.org/abs/2306.01102) | ①+② | Code-generating LLM paired with Quality-Diversity search to produce diverse high-performing networks. | — | [✓](https://github.com/umair-nasir14/LLMatic) |
| 2026 | CVPR-W | [From Memorization to Creativity: LLM as a Designer of Novel Architectures](https://arxiv.org/abs/2601.02997) | ①+② | Fine-tunes a code LLM (LoRA) in a closed loop; validates generated PyTorch CNNs and feeds winners back, mining 455 novel architectures. | — | — |
| 2026 | arXiv | [LLM as a Tool, Not an Agent: Code-Mined Tree Transformations for NAS](https://arxiv.org/abs/2604.16555) | ① | Mines code-tree transformations from the LLM rather than letting it act autonomously; cheaper and more controllable. | — | — |

## ② Iterative / Evolutionary Optimization
*The LLM operates inside the search loop — proposing mutations/crossovers from performance feedback.*

| Year | Venue | Paper | Method | What it does | HW-aware | Code |
|------|-------|-------|--------|--------------|----------|------|
| 2024 | GECCO | [LLM-Guided Evolution (EoT)](https://arxiv.org/abs/2403.11446) | ② | LLM as the mutation operator inside an evolutionary search (Evolution of Trees); outperforms 13 NAS baselines. | — | [✓](https://github.com/clint-kristopher-morris/llm-guided-evolution) |
| 2025 | arXiv | [The AlphaGo Moment for Model Architecture Discovery (ASI-Arch)](https://arxiv.org/abs/2507.18074) | ② | A fully autonomous LLM hypothesizes architectural concepts, implements them as code, and trains/validates in a closed loop; 1,773 experiments → 106 SOTA linear-attention architectures. | — | [✓](https://github.com/GAIR-NLP/ASI-Arch) |
| 2025 | arXiv | [SEKI: Self-Evolution and Knowledge Inspiration NAS via LLMs](https://arxiv.org/abs/2502.20422) | ②+④ | Two-stage CoT search: self-evolve architectures from feedback, then distill common patterns into new designs; SOTA at ~0.05 GPU-days. | — | — |
| 2026 | arXiv | [Resource-Efficient Iterative LLM-Based NAS with Feedback Memory](https://arxiv.org/abs/2603.12091) | ②+④ | Dual-LLM (generator + memory) that reuses prior architecture–performance pairs in-context to refine later proposals. | — | — |
| 2026 | arXiv | [Structured Progressive Knowledge Activation for LLM-Driven Architecture Evolution](https://arxiv.org/abs/2605.04057) | ②+④ | Treats LLM-NAS as multi-round evolution over executable programs; fixes "cross-factor functional entanglement." | — | — |
| 2026 | ESWA | [DR-LLM-ENAS: Dual-Role LLMs for Evolutionary NAS](https://www.sciencedirect.com/science/article/abs/pii/S0957417426019317) | ② | One LLM generates, another evaluates, inside an evolutionary NAS loop. | — | [✓](https://github.com/baigeixiaowang/DR-LLM-NAS) |
| 2025 | NeurIPS-W | [LLM-Driven Composite NAS for Multi-Source RL State Encoding](https://arxiv.org/abs/2512.06982) | ② | LLM priors over module design guide composite-NAS for RL state encoders (mixed-autonomy traffic). | — | — |
| 2026 | Inf. Sciences | [Large Language Model Assisted Evolutionary NAS](https://www.sciencedirect.com/science/article/pii/S0020025526000411) | ②+③ | Replaces the ML surrogate in evolutionary NAS with an LLM fitness evaluator. | — | — |
| 2022 | arXiv | [Evolution through Large Models (ELM)](https://arxiv.org/abs/2206.08896) | ①+② | *Foundational:* code-trained LLMs as diff-based mutation operators in MAP-Elites (Sodarace walkers) — the precursor cited by LLMatic. | — | — |

## ③ Performance Predictor / Zero-Cost Proxy
*The LLM scores or ranks candidate architectures without training them.*

| Year | Venue | Paper | Method | What it does | HW-aware | Code |
|------|-------|-------|--------|--------------|----------|------|
| 2024 | ACL Findings | [LLM Performance Predictors Are Good Initializers for Architecture Search](https://arxiv.org/abs/2310.16712) | ③ | Foundational: repurpose an LLM as an architecture accuracy predictor (LLM-PP) to seed/warm-start NAS; ~50% fewer search hours. | ✓ Latency / GFLOPs / size predicted. | [✓](https://github.com/UBC-NLP/llmas) |
| 2025 | ICML | [RZ-NAS: Enhancing LLM-guided NAS via Reflective Zero-Cost Strategy](https://openreview.net/forum?id=9UExQpH078) | ③+④ | Zero-cost proxies score candidates; the LLM "reflects" on the proxy signal and proposes improvements. | — | [✓](https://github.com/PasaLab/RZ-NAS) |
| 2025 | NeurIPS | [Revolutionizing Training-Free NAS: Automatic Proxy Discovery via LLMs (APD)](https://openreview.net/forum?id=3naHyE5klE) | ③ | LLM (with actor-critic RL over prompts) *discovers the zero-cost proxy formula itself* — a meta-level predictor. | — | — |
| 2024 | IEEE DoCES | [LLM-Assisted Adversarial-Robustness NAS (LLMO)](https://arxiv.org/abs/2406.05433) | ③ | LLM as a fitness surrogate that scores candidate robustness without full adversarial training. | — | [✓](https://github.com/RuiZhong961230/LLMO) |
| 2026 | arXiv | [From Code to Prediction: Fine-Tuning LLMs for NN Performance Classification (NNGPT)](https://arxiv.org/abs/2605.03686) | ③ | Fine-tunes an LLM to classify which of two architecture-codes performs better; shows code encodes performance signal. | — | [✓](https://github.com/ABrain-One/nn-gpt) |
| 2025 | EMNLP | [LM-Searcher: Cross-domain NAS via Unified Numerical Encoding](https://arxiv.org/abs/2509.05657) | ③+④ | Unified numerical encoding lets the LLM ingest arch–performance history, rank/predict, and transfer across domains. | — | [✓](https://github.com/Ashone3/LM-Searcher) |

## ④ Design Knowledge Transfer / Advisor
*The LLM injects human design knowledge to build, prune, or bias the search space.*

| Year | Venue | Paper | Method | What it does | HW-aware | Code |
|------|-------|-------|--------|--------------|----------|------|
| 2025 | AAAI | [Design Principle Transfer in NAS via LLMs (LAPT)](https://arxiv.org/abs/2408.11330) | ④ | LLM extracts linguistic "design principles" from prior architectures and re-applies them to prune a new task's search space. (51+ cites) | — | [✓](https://github.com/milkmilk511/LAPT) |
| 2024 | BDMA | [GPT-NAS: Evolutionary NAS with the Generative Pre-Trained Model](https://arxiv.org/abs/2305.05351) | ④+② | Assumes GPT learned "how to build architectures"; GPT proposes plausible components as priors that shrink the evolutionary search space. | — | — |
| 2026 | CVPR-W | [CoLLM-NAS: Collaborative LLMs for Knowledge-Guided NAS](https://arxiv.org/abs/2509.26037) | ④+② | A stateful Navigator LLM (direction) + stateless Generator LLM (candidates) cooperate; SOTA on NAS-Bench-201 at 4–10× lower cost. | — | — |
| 2026 | arXiv | [Structuring Open-Ended NAS: Semi-Automated Design Knowledge Structuring (FairNAD)](https://arxiv.org/abs/2605.19247) | ④ | LLM populates a structural template by reading NAS papers, yielding a knowledge-rich search space explored with fairness-aware sampling. | — | — |

## ⑤ End-to-End Agentic
*The LLM autonomously orchestrates the whole AutoML/NAS pipeline.*

| Year | Venue | Paper | Method | What it does | HW-aware | Code |
|------|-------|-------|--------|--------------|----------|------|
| 2023 | arXiv | [AutoML-GPT: Automatic Machine Learning with GPT](https://arxiv.org/abs/2305.02499) | ⑤ | GPT routes a user's data/task description across specialized models to plan and execute the full ML pipeline, incl. model design. | — | — |
| 2026 | arXiv | [Agentic Neural Architecture Search (AgentNAS)](https://arxiv.org/abs/2607.07984) | ⑤+④ | LLM emits a seed architecture and decomposes it into named interchangeable "slots" that auto-define a task-specific search space — no manual space engineering. | — | [✓](https://github.com/alroimfebruary/AgentNAS) |

## ⑥ Constraint-Aware (Hardware / Fairness)
*LLM-driven search under explicit latency / fairness / resource budgets.*

| Year | Venue | Paper | Method | What it does | HW-aware | Code |
|------|-------|-------|--------|--------------|----------|------|
| 2025 | arXiv | [LLM-NAS: LLM-driven Hardware-Aware NAS](https://arxiv.org/abs/2510.01472) (= PEL-NAS) | ⑥+③④ | Complexity-driven space partitioning + LLM prompt/architecture co-evolution with a growing knowledge base + zero-cost predictor. | ✓ Joint accuracy/latency on HW-NAS-Bench; up to 54% lower latency, days→minutes. | — |
| 2025 | npj Digital Med. | [Pathology-NAS: LLM-guided NAS for Pathology Models](https://www.nature.com/articles/s41746-025-02042-x) | ⑥ | GPT-4 searches over a supernet for whole-slide-image models under a FLOPs budget; 99.98% acc at −45% FLOPs. *(Earlier confused with a survey — it is a method paper.)* | ✓ FLOPs budget. | [✓](https://github.com/maopopovich/Pathology-NAS) |
| 2025 | GECCO Companion | [LLM-Guided Evolution for Object Detection](https://arxiv.org/abs/2504.02280) | ⑥ | LLM-guided evolutionary search for YOLO-style detectors on KITTI; mAP 92.5→94.5%. | ✓ FPS / inference time. | [✓](https://github.com/clint-kristopher-morris/llm-guided-evolution) |
| 2025 | Sci. Reports | [LeMo-NADe (LEMONADE): Multi-Parameter Architecture Discovery with LLMs](https://www.nature.com/articles/s41598-025-97378-5) | ⑥+① | GPT-4/Gemini + an expert system discover architectures from user-defined multi-parameter constraints (FPS, CO₂, power) with no preset search space. | ✓ Power / inference speed / size / CO₂ as explicit user constraints. | — |
| 2024 | arXiv | [FL-NAS: Fairness of NAS for Resource-Constrained Devices via LLMs](https://arxiv.org/abs/2402.06696) | ⑥ | LLM-guided NAS that optimizes fairness under device resource constraints. | ✓ Resource/edge constraints. | — |
| 2026 | arXiv | [UH-NAS: LLM-Guided NAS for Robust Co-Design of Physical Neural Networks](https://arxiv.org/abs/2606.10294) | ⑥+② | LLM co-optimizes efficiency and robustness for unconventional hardware (photonic / in-memory) under non-idealities; NSGA-II loop. | ✓ Physical/unconventional hardware; energy & non-ideality aware. | — |
| 2025 | EMNLP Findings | [MONAQ: Multi-Objective NAS Querying for Time-Series on Resource-Constrained Devices](https://arxiv.org/abs/2505.10607) | ⑥ | LLM-driven multi-objective querying for efficient time-series model design on constrained devices. | ✓ Resource-constrained deployment. | — |
| 2025 | arXiv | [Controlled Generation of Image-Captioning Models Under Strict Constraints](https://arxiv.org/abs/2512.14706) | ⑥+① | Extends "LLM-as-designer" to strictly resource/parameter-constrained captioning-model generation. | ✓ Strict resource/parameter budget. | — |

## Cross-Domain / Modality-Specific
*LLM-driven NAS applied beyond standard CNN/image classification.*

| Year | Venue | Paper | Domain | What it does | HW-aware | Code |
|------|-------|-------|--------|--------------|----------|------|
| 2025 | Sci. China Inf. Sci. | [Graph Neural Architecture Search with LLMs (GNAS-LLM)](https://link.springer.com/article/10.1007/s11432-024-4539-1) | Graph (GNN) | LLMs (incl. GPT-4) as evolutionary designers generating novel, better GNN architectures beyond hand-designed spaces. | — | — |
| 2023 | arXiv | [Heterogeneous Graph NAS with GPT-4](https://arxiv.org/abs/2312.08680) | Hetero-graph | GPT-4 explores heterogeneous GNN architectures. | — | — |
| 2023 | arXiv | [Unleashing LLMs for Quantum Computing: Quantum Architecture Design](https://arxiv.org/abs/2307.08191) | Quantum | LLMs propose quantum-circuit architectures. | — | — |
| 2025 | ACM | [DyNAS-DDI: Dynamic Pairwise Architecture Search for Drug–Drug Interaction](https://dl.acm.org/doi/pdf/10.1145/3746027.3755791) | Drug discovery | LLM/NAS-style pairwise search for generalizable DDI models. | — | — |

---

# Part II — NAS4LLM (LLM as the search target)

## Structured Pruning as Search
*Pruning framed as a search over which structures (heads/neurons/layers/channels) to keep — gradient-based, importance-scored, or evolutionary.*

| Year | Venue | Paper | Algorithm & Space | What it does | HW-aware | Code |
|------|-------|-------|-------------------|--------------|----------|------|
| 2023 | NeurIPS | [LLM-Pruner: On the Structural Pruning of LLMs](https://arxiv.org/abs/2305.11627) | Gradient heuristic over DepGraph-coupled groups (heads/neurons/layers) | Task-agnostic structured pruning via dependency graph + first-order Taylor scores; LoRA recovers quality. | — | [✓](https://github.com/horseee/LLM-Pruner) |
| 2024 | ICLR | [Sheared LLaMA: Accelerating LM Pre-training via Structured Pruning](https://arxiv.org/abs/2310.06694) | Differentiable L0 masks targeting a shape (depth/width/heads) | Learns masks to reach a target architecture, with dynamic batch loading; LLaMA-2-7B → 1.3B/2.7B. | — | [✓](https://github.com/princeton-nlp/LLM-Shearing) |
| 2024 | ICLR | [SliceGPT: Compress LLMs by Deleting Rows and Columns](https://arxiv.org/abs/2401.15024) | PCA/variance selection over residual-stream width | Orthogonal projection makes row/column deletion near-lossless; ~25% compression. | ✓ Reports inference speedup & memory (llama.cpp). | [✓](https://github.com/microsoft/TransformerCompression) |
| 2024 | AAAI | [FLAP: Fluctuation-based Adaptive Structured Pruning](https://arxiv.org/abs/2312.11983) | Fluctuation-metric adaptive structure search (width) | Retraining-free structured pruning with adaptive global structure selection. | ✓ Hardware-friendly (storage/speed). | [✓](https://github.com/CASIA-IVA-Lab/FLAP) |
| 2024 | ACL Findings | [LoRAPrune: Structured Pruning Meets Low-Rank PEFT](https://arxiv.org/abs/2305.18403) | Gradient mask over channels/heads during LoRA FT | Co-designs pruning with LoRA; prunes using LoRA weights/gradients. | — | [✓](https://github.com/aim-uofa/LoRAPrune) |
| 2025 | ACL Findings | [ShortGPT: Layers in LLMs Are More Redundant Than You Expect](https://arxiv.org/abs/2403.03853) | Block-Influence ranking (depth/layer removal) | Removes lowest-influence layers; simple yet strong, orthogonal to quantization. | — | [✓](https://github.com/feifeibob/ShortGPT) |
| 2024 | arXiv | [LLM-Streamline: Streamlining Redundant Layers](https://arxiv.org/abs/2403.19135) | Cosine-similarity layer ranking + replacement (depth) | Replaces (not just drops) pruned consecutive layers with a lightweight module. | — | [✓](https://github.com/RUCKBReasoning/LLM-Streamline) |
| 2024 | EMNLP Findings | [LaCo: LLM Pruning via Layer Collapse](https://arxiv.org/abs/2402.11187) | Similarity-guided layer merging (depth) | Merges adjacent layers ("collapse") to cut depth while preserving >80% performance. | — | [✓](https://github.com/yangyifei729/LaCo) |
| 2024 | NeurIPS | [AlphaPruning: HT-SR Theory for Layer-wise Pruning](https://arxiv.org/abs/2410.10912) | Spectral (power-law) per-layer sparsity allocation | Uses Heavy-Tailed Self-Regularization shape metrics to allocate per-layer sparsity; 80% sparsity on LLaMA-7B. | — | [✓](https://github.com/haiquanlu/AlphaPruning) |
| 2024 | NeurIPS | [BESA: Blockwise Parameter-Efficient Sparsity Allocation](https://arxiv.org/abs/2402.16880) | Differentiable per-block sparsity ratios | Learns block-wise sparsity allocation by gradient descent (a differentiable NAS over ratios). | — | [✓](https://github.com/OpenGVLab/LLMPrune-BESA) |
| 2024 | NeurIPS | [DSA: Discovering Sparsity Allocation for Layer-wise Pruning](https://openreview.net/forum?id=rgtrYVC9n4) | Evolutionary search over allocation functions | Clearest NAS-style layer pruner: EA discovers the function mapping importance→per-layer sparsity. | — | [✓](https://github.com/lliai/DSA) |
| 2024 | NeurIPS | [DISP-LLM: Dimension-Independent Structural Pruning](https://arxiv.org/abs/2410.11988) | Decoupled per-layer width selection | Breaks structural coupling so layers can be pruned independently for flexible non-uniform compression. | — | [✓](https://github.com/ZhengaoLi/DISP-LLM-Dimension-Independent-Structural-Pruning) |
| 2025 | ICML | [SlimLLM: Accurate Structured Pruning for LLMs](https://arxiv.org/abs/2505.22689) | Joint channel/head/layer importance + LR recovery | Fast joint width+depth pruning with holistic scoring and linear-regression output recovery. | — | — |
| 2025 | NeurIPS | [Týr-the-Pruner: Global Sparsity Distribution Optimization](https://arxiv.org/abs/2503.09657) | Taylor-saliency search over multi-sparsity supernet | Builds a multi-sparsity supernet first, then searches the best subnetwork; ~97% performance retained. | — | [✓](https://github.com/amd-research/Tyr-the-Pruner) |
| 2024 | TMLR | [Structural Pruning of Pre-trained Language Models via NAS (Klein et al.)](https://arxiv.org/abs/2405.02267) | Multi-objective weight-sharing NAS (depth/width/heads/FFN) | Maps weight-sharing NAS onto LLM structured pruning over a fine-tuned supernet. | ✓ Multi-objective vs FLOPs/size. | [✓](https://github.com/whittle-org/plm_pruning) |
| 2023 | arXiv | [Compresso: Structured Pruning with Collaborative Prompting](https://arxiv.org/abs/2310.05015) | Differentiable L0 masks during instruction tuning | Pruning decisions learned jointly with the model; collaborative prompt has the LLM participate. | — | — |
| 2025 | arXiv | [Determining Layer-wise Sparsity via a Theoretical Perspective](https://arxiv.org/abs/2502.14770) | Arithmetic-progression 1-D allocation search | Derives a near-optimal monotonically increasing per-layer sparsity profile; 2.63×/2.23× CPU/GPU speedup. | ✓ CPU/GPU speedup reported. | — |

## Width / Depth / Layer / Block & Elastic Supernet
*Searching the macro-structure: depth, width, heads, block type — often via a once-trained supernet and a sub-network extractor.*

| Year | Venue | Paper | Algorithm & Space | What it does | HW-aware | Code |
|------|-------|-------|-------------------|--------------|----------|------|
| 2024 | ECCV-W | [LLaMA-NAS: Efficient NAS for Large Language Models](https://arxiv.org/abs/2405.18377) | One-shot NAS + evolutionary (GA) over LLaMA-2-7B subnets | Fine-tunes one supernet, then GA-finds Pareto sub-architectures (1.5× smaller, 1.3× faster). | ✓ Pareto on latency & model size. | [✓](https://github.com/Nota-NetsPresso/llama-nas) |
| 2024 | NeurIPS | [Search for Efficient Large Language Models](https://arxiv.org/abs/2409.17372) | Training-free predictor search + mask mutation | Treats a pretrained LLM as a supernet; extracts non-uniform subnets + ADMM weight reformation. | ✓ GPU memory ↓, inference ↑. | [✓](https://github.com/shawnricecake/search-llm) |
| 2024 | arXiv | [Flextron: Many-in-One Flexible LLM (NVIDIA)](https://arxiv.org/abs/2406.10260) | Sample-efficient supernet + RL router | Nested elastic sub-networks; a router picks width/depth per latency target at inference. | ✓ User-defined latency/accuracy targets. | [✓](https://github.com/NVlabs/Flextron) |
| 2025 | ICLR | [LLaMaFlex: Many-in-One LLMs via Generalized Pruning & Weight Sharing](https://openreview.net/forum?id=AyC4uxx2HW) | Zero-shot nested pruning (depth+width) | Extracts many deployable sub-models zero-shot from a pretrained base. | ✓ Memory/throughput targets. | [✓](https://github.com/NVlabs/LLaMaFlex) |
| 2024 | NeurIPS | [AmoebaLLM: Constructing Any-Shape LLMs](https://arxiv.org/abs/2411.10606) | Elastic supernet + subnetwork distillation/router | One-shot supernet training; export arbitrary-shape subnets on the accuracy–efficiency frontier. | ✓ Latency/memory front. | [✓](https://github.com/GATECH-EIC/AmoebaLLM) |
| 2024 | NeurIPS | [Compact Language Models via Pruning & Distillation (Minitron, NVIDIA)](https://arxiv.org/abs/2407.14679) | Empirical sensitivity-driven depth+width pruning + KD | Principled pruning recipe (Nemotron-4 15B → 8B/4B); +16% MMLU vs from-scratch. | — | [✓](https://github.com/NVlabs/Minitron) |
| 2024 | LREC-COLING | [LoNAS: Elastic Low-Rank Adapters for Efficient LLMs](https://aclanthology.org/2024.lrec-main.940/) | One-shot NAS over elastic LoRA-rank + subnetworks | Unifies PEFT with NAS; extract compressed sub-LLMs from a LoRA-trained supernet. | ✓ Intel HW-aware AutoML stack. | [✓](https://github.com/IntelLabs/Hardware-Aware-Automated-Machine-Learning/tree/main/LoNAS) |
| 2024 | LREC-COLING | [EFTNAS: Searching Efficient LMs in First-Order Weight-Reordered Supernets](https://aclanthology.org/2024.lrec-main.497/) | Iterative mask-threshold search over reordered supernet | First-order weight reordering improves supernet quality; mask search yields efficient subnets. | ✓ HW-aware AutoML stack. | [✓](https://github.com/IntelLabs/Hardware-Aware-Automated-Machine-Learning) |
| 2024 | NAACL Industry | [Shears: Unstructured Sparsity with Neural Low-rank Adapter Search](https://arxiv.org/abs/2404.10934) | NLS over elastic LoRA configs + sparse masks | Combines unstructured sparsity with low-rank-adapter search for PEFT-efficient sub-LLMs. | ✓ HW-aware AutoML stack. | [✓](https://github.com/IntelLabs/Hardware-Aware-Automated-Machine-Learning) |
| 2024 | NeurIPS (Spotlight) | [Stacking Your Transformers: Model Growth for Efficient Pre-Training](https://arxiv.org/abs/2405.15319) | Empirical sweep over atomic growth operators | Benchmarks depth-stack vs width-grow; a 7B grown from seed matches from-scratch at 54.6% compute. | ✓ Training-FLOPs/token savings. | [✓](https://github.com/tongxuluo/prts) |
| 2024 | ICLR-W | [Shortened LLaMA: Depth Pruning with Comparison of Retraining Methods](https://arxiv.org/abs/2402.02834) | Gradient/magnitude layer ranking (depth) | Canonical "drop layers" baseline; compares LoRA/full-FT/continued-PT recovery. | — | [✓](https://github.com/Nota-NetsPresso/shortened-llm) |
| 2025 | arXiv | [GeLaCo: An Evolutionary Approach to Layer Compression](https://arxiv.org/abs/2507.10059) | Population-based EA over layer-collapse groups | Multi-objective EA explores which adjacent layers to merge; first Pareto front for layer-merged LLMs. | — | — |
| 2024 | arXiv | [Compressing LLMs with Automated Sub-Network Search](https://arxiv.org/abs/2410.06479) | Multi-objective NAS over structural components | NAS-prunes heads/neurons/layers for Pareto-optimal sub-networks; up to 22% latency gain. | ✓ On-device latency in the objective. | — |
| 2025 | ICME | [Elastic Architecture Search for Efficient Language Models (ELM)](https://arxiv.org/abs/2510.27037) | NAS with block-aware KD loss | Elastic search space with efficient blocks for compact masked/causal LMs. | — | [✓](https://github.com/ra225/ELM) |
| 2025 | arXiv | [Nemotron Elastic: Many-in-One Reasoning LLMs (NVIDIA)](https://arxiv.org/abs/2511.16664) | Nested elastic training + input-adaptive routing | Russian-doll submodels inside a hybrid Mamba-Attention LLM for many deployment budgets. | ✓ Nested submodels per budget. | — |
| 2024 | ACL | [LLaMA Pro: Progressive LLaMA with Block Expansion](https://arxiv.org/abs/2401.02415) | *No search* — deterministic block insertion | Grows depth by adding blocks trained only on new corpus (code/math). *(Listed for context: growth, not search.)* | — | [✓](https://github.com/Alpha-VLLM/LLaMA-Pro) |
| 2024 | NAACL Industry | [SOLAR 10.7B: Depth Up-Scaling](https://arxiv.org/abs/2312.15166) | *No search* — manual layer duplication (DUS) | Duplicates a layer subset + continued PT; beats Mixtral-8x7B without MoE. *(Context only.)* | — | [model](https://huggingface.co/upstage/SOLAR-10.7B-v1.0) |
| 2025 | AAAI | [MeRino: Training-Free Math-Programming NAS for Mobile LMs](https://arxiv.org/abs/2403.07921) | Training-free entropy-driven search over mobile-gen-LM arch | On-device target (Jetson Nano); 4.9× faster, 5.5× smaller. | ✓ On-device (Jetson Nano) latency. | — |
| 2025 | arXiv | [ZeroLM: Training-Free Zero-Cost NAS for Transformers](https://arxiv.org/abs/2503.18646) | Training-free zero-cost proxy over Transformer sub-modules | Ranks Transformer sub-architectures without any training. | — | — |
| 2025 | ICLR | [W-PCA Gradient-Free Proxy for Lightweight Language Models](https://arxiv.org/abs/2504.15983) | Zero-shot predictor (params + W-PCA) over width/depth/FFN | Gradient-free zero-shot NAS beating training-based SOTA on GLUE/SQuAD for lightweight LMs. | — | — |
| 2022 | arXiv | [AutoDistill: Bayesian-Opt Multi-Objective Student Search](https://arxiv.org/abs/2201.08539) | Bayesian optimization over student-LM width+depth | Multi-objective search with TPUv4i latency in the objective. | ✓ TPUv4i latency. | — |
| 2023 | ACL Findings | [NAS for Parameter-Efficient Fine-tuning of Large Pre-trained Models](https://arxiv.org/abs/2305.16597) | Pruning-as-search over PET/adapter architecture | Searches adapter/PEFT structure rather than full backbones. | — | — |

## Mixture-of-Experts (MoE) Search
*Searching expert topology, expert count/width, routing, or expert subsetting.*

| Year | Venue | Paper | Algorithm & Space | What it does | HW-aware | Code |
|------|-------|-------|-------------------|--------------|----------|------|
| 2023 | ACL Findings | [AutoMoE: Heterogeneous MoE with Adaptive Computation](https://arxiv.org/abs/2210.07535) | Differentiable NAS (HAT-style) over per-layer expert config | NAS over heterogeneous sparse-MoE under latency/FLOPs constraints; ~4× CPU speedup. | ✓ Latency/FLOPs-constrained. | [✓](https://github.com/microsoft/AutoMoE) |
| 2024 | arXiv | [MoE-Pruner: Pruning MoE LLMs Using Router Hints](https://arxiv.org/abs/2410.12013) | Router-aware magnitude pruning (expert FFN width) | One-shot structured pruning of Mixtral using |weight|×|activation|×|router| scores. | — | — |
| 2024 | ACL | [Not All Experts Are Equal: Expert Pruning & Skipping](https://arxiv.org/abs/2402.14800) | Predictor over static prune + dynamic skip (experts) | Static expert pruning + per-token dynamic skipping for memory/throughput. | ✓ Memory & throughput. | [✓](https://github.com/lucky-lance/expert_sparsity) |
| 2025 | arXiv | [CMoE: Converting Dense LLMs to MoE](https://arxiv.org/abs/2502.04416) | Training-free FFN-neuron clustering into experts | Analyzes activation patterns to carve a dense LLM into a routed sparse MoE, reusing weights. | ✓ Inference acceleration. | — |
| 2025 | arXiv | [ToMoE: Dense → MoE via Dynamic Structural Pruning](https://arxiv.org/abs/2501.15316) | Differentiable active-param mask (MoE topology) | Trains a soft mask converting dense FFNs into top-k experts at a fixed active-param budget. | ✓ Fixed active-param budget. | — |
| 2025 | ICML | [HC-SMoE: Retraining-Free Merging of Sparse MoE](https://arxiv.org/abs/2410.08589) | Hierarchical clustering over expert outputs | Compresses MoE by clustering/merging experts by output similarity, routing-agnostic. | — | [✓](https://github.com/wazenmai/HC-SMoE) |
| 2026 | ICML | [ExpertWeaver: Unlocking Inherent MoE in Dense LLMs](https://arxiv.org/abs/2502.02737) | Training-free search over GLU activation patterns | Converts dense GLU-FFN LLMs to sparse MoE by exploiting inherent activation structure. | ✓ Active-param / inference cost. | — |
| 2025 | NeurIPS | [Shapley-MoE: Discovering Important Experts via Shapley Values](https://openreview.net/forum?id=7kQjbCQwtT) | Cooperative-game predictor over expert subsets | Shapley-value attribution to prune unimportant experts; >96% prunable in some MoEs. | — | — |
| 2025 | NeurIPS | [DiEP: Differentiable NAS for Per-Layer MoE Experts](https://arxiv.org/abs/2509.16105) | Differentiable (FBNetV2-style) over per-layer expert config | Differentiable NAS that searches where and how to place MoE experts in each layer. | — | — |

## Mixed-Precision / Quantization Search
*Searching per-layer bit-widths / precision schedules.*

| Year | Venue | Paper | Algorithm & Space | What it does | HW-aware | Code |
|------|-------|-------|-------------------|--------------|----------|------|
| 2025 | EMNLP | [AMQ: AutoML for Mixed-Precision Weight-Only Quantization of LLMs](https://arxiv.org/abs/2509.12019) | Multi-objective AutoML over per-layer bit-widths | AutoML assigns layer-wise weight bit-widths trading quality vs. memory. | ✓ Weight-memory budget. | [✓](https://github.com/dlwns147/amq) |
| 2023 | NeurIPS-W | [LLM-MQ: Mixed-Precision Quantization for Efficient LLM Deployment](https://neurips.cc/virtual/2023/81141) | Sensitivity-based heuristic over per-layer bit-widths | Allocates precision under a memory budget; ~2.8-bit avg beats uniform 2-bit. | ✓ Weight-memory + GPU-kernel-aware. | — |
| 2025 | ICLR | [Progressive Mixed-Precision Decoding for Efficient LLM Inference](https://arxiv.org/abs/2410.13461) | Phase-aware progressive precision schedule | Allocates higher precision to prefill/early-decode, lower to later tokens. | ✓ Throughput/memory efficiency. | — |

## Attention / Sublayer Structure Search
*Searching attention patterns, attention types, sublayer ordering, or activations.*

| Year | Venue | Paper | Algorithm & Space | What it does | HW-aware | Code |
|------|-------|-------|-------------------|--------------|----------|------|
| 2025 | NeurIPS | [Neural Attention Search (NAtS)](https://arxiv.org/abs/2502.13251) | Differentiable per-token attention-role search | Learns each token's attention type (Global/Local/Sliding-Window); reduces KV-cache at inference. | ✓ KV-cache / inference cost. | [✓](https://github.com/automl/NeuralAttentionSearchLinear) |
| 2022 | arXiv | [NAS on Efficient Transformers and Beyond](https://arxiv.org/abs/2207.13955) | NAS over topology + attention variant jointly | Searches both architecture and whether each layer uses softmax vs. efficient attention. | — | — |
| 2024 | arXiv | [ReLU² Wins: Efficient Activations for Sparse LLMs](https://arxiv.org/abs/2402.03804) | Search over activation-function candidates | NAS-style discovery finding ReLU² enables 90–95% neuron sparsity. | ✓ Activation sparsity → compute ↓. | — |
| 2025 | EMNLP | [Cost-Optimal GQA: Grouped-Query Attention Design](https://arxiv.org/abs/2503.09579) | Analytical recipe search over GQA grouping + model size | Derives cost-optimal grouped-query-attention configurations analytically; >50% FLOPs/memory cut. | ✓ FLOPs + memory modeled. | [✓](https://github.com/THUNLP/cost-optimal-gqa) |
| 2026 | IEEE | [HARMONY: Large-Scale Search for Efficient Hybrid Language Models](https://ieeexplore.ieee.org/document/11520487/) | Multi-objective EA over Transformer–Mamba–MoE composition | Searches the interleaving of attention / Mamba-SSM / MoE layers at scale for hybrid LLMs. | ✓ Multi-objective efficiency metrics. | — |

## Hardware-Aware (LLM-era)
*Search whose objective explicitly embeds device latency/energy/memory.*

| Year | Venue | Paper | Algorithm & Space | What it does | HW-aware | Code |
|------|-------|-------|-------------------|--------------|----------|------|
| 2025 | ICLR | [DarwinLM: Evolutionary Structured Pruning of LLMs](https://arxiv.org/abs/2502.07780) | Evolutionary (level-switch mutation) over per-module sparsity | Builds a *measured* inference-time database; speedup-preserving mutation makes target speedup a hard constraint. | ✓ Real-hardware measured latency LUT; end-to-end speedup target. | [✓](https://github.com/IST-DASLab/DarwinLM) |
| 2025 | ICML | [EvoPress: Dynamic Compression via Evolutionary Search](https://arxiv.org/abs/2410.14649) | EA with provable convergence over block/sparsity/bitwidth profile | Global dynamic compression; rejects layer-independence assumption; SOTA on Llama/Mistral/Phi. | ✓ Global compression-budget constraint. | [✓](https://github.com/IST-DASLab/EvoPress) |
| 2025 | ACM TODAES | [HAPE: Hardware-Aware LLM Pruning for On-Device Inference](https://dl.acm.org/doi/10.1145/3744244) | HW-aware post-training structured pruning (per-layer structure) | Aligns structure selection with target CPU latency/throughput; +11–21% on-device speedup, no retraining. | ✓ On-device CPU latency modeled in selection. | — |
| 2025 | ICML | [Puzzle: Distillation-Based NAS for Inference-Optimized LLMs (NVIDIA)](https://arxiv.org/abs/2411.19146) | Blockwise-local-distillation NAS + mixed-integer programming (block-wise arch) | Large-scale NAS+distillation producing Nemotron-51B/49B from Llama-70B; 2.17× H100 throughput at 98.4% accuracy. | ✓ MIP-constrained for H100 throughput/memory; end-to-end. | [✓](https://github.com/NVlabs/puzzle) |

## Foundations (Pre-LLM Transformer NAS)
*Canonical hardware-aware / Transformer-NAS templates that defined the methodology later applied to LLMs.*

| Year | Venue | Paper | Algorithm & Space | What it does | HW-aware | Code |
|------|-------|-------|-------------------|--------------|----------|------|
| 2020 | ACL | [HAT: Hardware-Aware Transformers](https://arxiv.org/abs/2005.14187) | Evolutionary search over SuperTransformer (weight sharing) | Hardware-latency-constrained (on-device LUT) Transformer NAS; the template for HW-aware LLM-NAS. | ✓ On-device latency LUT (Intel/ARM/Nvidia). | [✓](https://github.com/mit-han-lab/hardware-aware-transformers) |
| 2019 | CVPR | [HAQ: Hardware-Aware Automated Quantization](https://arxiv.org/abs/1811.08886) | RL (PPO) over per-layer bit-widths | RL agent gets latency/energy feedback from a hardware simulator; canonical HW-in-the-loop mixed-precision NAS. | ✓ HW-simulator feedback (latency/energy). | [✓](https://github.com/mit-han-lab/haq) |
| 2020 | NeurIPS | [HAWQ-V2: Hessian-Aware Trace-Weighted Quantization](https://arxiv.org/abs/1911.03852) | Hessian-trace predictor over per-layer bit-widths | Cheap second-order score replaces RL/EA for mixed-precision allocation. | ✓ (HAWQ-V3 adds latency/energy). | [✓](https://github.com/zhen-dong/hawq) |
| 2023 | NeurIPS | [ZipLM: Inference-Aware Structured Pruning of Language Models](https://arxiv.org/abs/2302.04089) | Greedy loss-runtime search over heads/neurons/layers | Given a target runtime speedup, removes worst loss-runtime-ratio components; BERT/GPT-2 era. | ✓ Matches user-specified runtime on target HW. | [✓](https://github.com/IST-DASLab/ZipLM) |
| 2022 | NeurIPS | [LiteTransformerSearch: Training-Free On-Device Search](https://arxiv.org/abs/2203.02094) | Training-free proxy (decoder params) over arch | Zero-cost proxy ranks autoregressive-LM architectures; on-device latency/memory eval. | ✓ On-device latency & peak memory. | — |
| 2021 | KDD | [NAS-BERT: Task-Agnostic Adaptive-Size BERT Compression](https://arxiv.org/abs/2105.14444) | Supernet + evolutionary over hidden/layers/heads/operators | One task-agnostic supernet queried for subnets of varying latency; foundational for LLM subnet search. | ✓ Adaptive to target latency. | — |
| 2020 | NeurIPS | [DynaBERT: Dynamic BERT with Adaptive Width and Depth](https://arxiv.org/abs/2004.04037) | Elastic width+depth sub-networks via in-place distillation | A single BERT that runs at multiple widths/depths for adaptive size/latency; foundational width/depth-search precursor. | ✓ Size/latency adaptive to deployment. | — |

## Benchmarks

| Year | Venue | Paper | What it is | Link |
|------|-------|-------|------------|------|
| 2024 | NeurIPS D&B | [HW-GPT-Bench: Hardware-Aware Architecture Benchmark for LMs](https://arxiv.org/abs/2405.10299) | Calibrated surrogate predicting latency/energy/memory across 13 devices for ~100K GPT-style architectures (NAS4LLM side). | [✓](https://github.com/automl/HW-GPT-Bench) |
| 2021 | ICML | [HW-NAS-Bench: Hardware-Aware NAS Benchmark](https://arxiv.org/abs/2109.02844) | Image-classifier HW-NAS benchmark — the evaluation target used by LLM-NAS / PEL-NAS (LLM4NAS side). | [✓](https://github.com/RICE-EIC/HW-NAS-Bench) |
| 2023 | ACL | [NAS-Bench-BERT (Training-Free NAS for RNNs & Transformers)](https://arxiv.org/abs/2306.00288) | Reproducible BERT-architecture benchmark on the FlexiBERT search space. | [✓](https://github.com/aaronserianni/training-free-nas) |

## Borderline / Adjacent (flagged, not core)

These touch LLM × architecture design but are **not** pure LLM4NAS or NAS4LLM — listed so readers can locate them and see *why* they sit at the edge.

| Year | Paper | Why borderline | Link |
|------|-------|----------------|------|
| 2024 | [ModelGPT](https://arxiv.org/abs/2402.12408) | LLM + hypernetwork generates task-specific models; closer to conditional generation than NAS. | [✓](https://github.com/IshiKura-a/ModelGPT) |
| 2024 | [GL-Agent](https://arxiv.org/abs/2309.04565) | LLM agents search the *config space* of a graph framework (PyG docs), not NN architectures. | — |
| 2024 | [DiffusionNAG](https://arxiv.org/abs/2305.16943) (ICLR'24) | Uses a *diffusion* model (not an LLM) as the architecture generator. | [✓](https://github.com/CownowAn/DiffusionNAG) |
| 2024 | [LLAMBO](https://arxiv.org/abs/2402.03921) | LLM-driven *Bayesian optimization* for HPO / black-box tuning, not architecture search. | [✓](https://github.com/tennisonliu/LLAMBO) |

---

## Contributing

Contributions are very welcome! Please see **[CONTRIBUTING.md](./CONTRIBUTING.md)** for:
- the inclusion/exclusion criteria,
- the LLM4NAS vs NAS4LLM placement rule,
- the 6-role tag definitions,
- the **hardware-awareness column convention** (the most error-prone field),
- and the per-entry markdown template.

A PR should add a row (or rows) following the existing column structure. If a paper's hardware-awareness is anything other than `—`, please state the *metric*, the *modeling approach* (lookup table / analytical / on-device measurement / end-to-end in objective), and cite where in the paper it is described.

## License

Released under the [MIT License](./LICENSE).

## Acknowledgements

This list was assembled by surveying the literature and cross-checking existing community resources, including [`zhichao-lu/Awesome-LLM-NAS`](https://github.com/zhichao-lu/Awesome-LLM-NAS), [`automl/awesome-transformer-search`](https://github.com/automl/awesome-transformer-search), and [`chenyaofo/awesome-architecture-search`](https://github.com/chenyaofo/awesome-architecture-search). Credit to the authors of all cited papers. Please open an issue or PR for corrections, broken links, or missing work.
