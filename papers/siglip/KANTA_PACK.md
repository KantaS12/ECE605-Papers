# Paper Manager pack — SigLIP (Sep 14)

**Paper:** Sigmoid Loss for Language Image Pre-Training (SigLIP)  
**arXiv:** https://arxiv.org/abs/2303.15343 · ICCV 2023 Oral  
**PDF:** /workspace/papers/siglip-2303.15343.pdf  
**Code:** https://github.com/google-research/big_vision  
**Session:** Sep 14 · due 9:30 AM · **You = Archaeologist (seat 1)**  
**Type:** VLA Architectures  
**Sources:** Summarizer + Senior Research (`/workspace/siglip-quiet/DELIVERABLE.md`)

---

## 1. Paper summary

SigLIP replaces CLIP’s batch-softmax InfoNCE with a **pairwise sigmoid** loss over all image–text pairs (no global normalization across the batch). A chunked/distributed implementation scales to huge batches on few chips.

- **SigLiT** (locked image): up to **84.5%** ImageNet zero-shot on 4×TPUv4 in ~2 days.
- Batch gains **saturate ~32k**; larger (e.g. 307k) can hurt.
- Public **SigLIP So400M @ 729 patches → 83.2%** ImageNet — the efficiency point that became the open VLA vision tower.
- **Why this club cares:** SigLIP-So400m → **PaliGemma** → **π₀ / π₀.₅ / OpenPI** → 2025 BEHAVIOR winners’ vision stack.

---

## 2. Keywords / model bits

| Kind | Terms |
|------|--------|
| Loss | Pairwise sigmoid, learnable temperature t + bias b (b≈−10 init), no softmax denom |
| Models | SigLiT, SigLIP, mSigLIP; ViT + text Transformer; SoViT-400M |
| Data / scale | WebLI; batch sweeps 512→1M; XM3600 multilingual |
| Systems | Chunked distributed loss; β₂=0.95; no WD on unlocked pretrained vision |
| Downstream | PaliGemma vision tower; OpenPI / π₀.₅; BEHAVIOR’25 |

**Loss (sketch):** \(L = -\frac{1}{|B|}\sum_{i,j} \log \sigma\!\big(z_{ij}(t\,x_i^\top y_j + b)\big)\) with \(z_{ij}=\pm1\) for match/non-match.

---

## 3. Role packs

### Stakeholder (18+3) — seat 3
- **Motivation:** CLIP-style pretrain is the eyes of modern VLAs; better loss/systems = better open baselines.
- **Problem:** Softmax LIP needs huge synchronized batches; costly and saturates poorly engineered.
- **Method:** Sigmoid pairwise + chunked impl; locked (SigLiT) and unlocked (SigLIP) recipes; So400M public checkpoint.
- **Findings:** Small-BS sigmoid ≫ softmax; ~32k enough; So400M hits 83.2% INet; lineage feeds PaliGemma→OpenPI→BEHAVIOR.
- **Takeaway for 2026 BEHAVIOR:** Don’t reinvent CLIP softmax — start from SigLIP/So400M (or SigLIP2) and differentiate on **action / data / System-2 / control**.

### Scientific Reviewer (8+3) — seat 5
- **Strengths:** Clean loss+systems co-design; rare mega-batch study; honest saturation; actionable ablations (bias, β₂, WD).
- **Weaknesses:** Proprietary WebLI; softmax baseline less systems-optimized; thin theory; limited dense/VLM-as-encoder eval at publication; compute-matched SOTA imperfect.
- **Actionable:** Public-data replication; report VLM/robotics transfer; publish exact So400M→PaliGemma config; isolate loss vs data when claiming “SigLIP helps VLA.”

### Empiricist (11+3) — seat 4
**H (Senior Research):** At BS≤1k, sigmoid+learned bias trains faster/stabler retrieval than matched softmax CLIP; removing bias hurts early stability. Success = qualitative Fig.2 gap, **not** paper ImageNet %.

| Priority | Experiment |
|----------|------------|
| P0 E1 | Sigmoid vs softmax on tiny paired set (BS≤1k) |
| P0 E2 | Bias b=−10 ablation |
| P0 E3 | Frozen SigLIP vs CLIP linear probe on household/robot stills |
| P1 E4 | OpenPI/π₀.₅ encoder-swap **outline only** |
| Skip | Full WebLI-scale reproduce |

**Budget:** E1/E2 ≈ 1×24–40GB overnight (6–24h); E3 cheap. Env stubs: `/workspace/siglip-quiet/env_outline/` (`open_clip_torch` / timm).

### Archaeologist (11+3) — **YOU (seat 1)** — HIGH PRIORITY

**Influential priors that shaped SigLIP**
| Prior | How it shaped this paper |
|-------|---------------------------|
| **CLIP** (Radford et al.) | Dual-encoder LIP setup SigLIP surgically replaces (softmax → sigmoid) |
| **ALIGN** | Large-scale noisy web image–text; scale narrative |
| **LiT** | Locked-image tuning path → **SigLiT** |
| **InfoNCE / SimCLR** | Contrastive softmax lineage being replaced |
| **ViT / Scaling ViTs** | Vision backbone family; SoViT shape-opt for So400M |
| **WebLI / PaLI** | Data + multilingual / VLM context |
| **Sigmoid-in-classification (e.g. ReaL)** | Motivates pairwise logistic over exclusive softmax |
| Related contemporaneous | FLIP, BASIC, CoCa, BLIP (competing LIP recipes) |

**Subsequent work that builds on SigLIP (clear line)**
| Later work | How it extends / uses SigLIP |
|------------|------------------------------|
| **PaLI-3** | Continues Google VLM stack with SigLIP-family vision |
| **PaliGemma** (arXiv 2407.07726) | **SigLIP-So400m + Gemma-2B** — the critical bridge |
| **π₀** (2410.24164) | Initializes from PaliGemma → robot VLA |
| **π₀.₅ / OpenPI** | Same vision tower lineage in open robot stacks |
| **SigLIP 2** (2502.14786) | Next-gen SigLIP (dense/multilingual improvements) |
| **BEHAVIOR’25 1st place** (2512.06951) | Diagram: SigLIP → PaliGemma → Action Expert; q≈0.26 |
| HF Transformers SigLIP APIs | Mass adoption / drop-in weights |

**One-sentence lineage for your talk:**  
*CLIP/ALIGN/LiT + WebLI → **SigLIP** → PaliGemma → π₀/OpenPI → π₀.₅ → 2025 BEHAVIOR winners (and → SigLIP 2).*

**Honest hedges:** Exact OpenPI freeze-vs-unlock of SigLIP weights in every fork is not always documented; many “uses SigLIP” papers inherit weights **without** isolating loss vs data/architecture.

### Visionary (11+3) — seat 2
- **2026 BEHAVIOR:** Treat SigLIP→PaliGemma→π₀.₅ as default open eyes; try **SigLIP 2** dense/multilingual behind the **same** action expert; measure encoder swaps in **q-score** under fixed policy compute (not COCO R@1).
- Pairwise loss invites async/hard-negatives for small robot batches; NaFlex / resolution tricks for egocentric cams.
- Skip Discord/Drive.

---

## 4. Link to long-horizon control (club thread)

SigLIP is **perception**, not a planner. BEHAVIOR failures more often sit in **chunking / stages / recovery / data** than ImageNet-0 of the vision tower — but SigLIP-So400m is still the **default open eyes** those controllers see through. Don’t claim SigLIP “solves” motion planning; claim it **standardized the backbone** the planning/control stack sits on.

---

## 5. Slide Maker brief

| Role | Timing | Focus |
|------|--------|-------|
| Stakeholder | 18+3 | Why sigmoid; So400M → PaliGemma → BEHAVIOR chain |
| Reviewer | 8+3 | Strengths + WebLI/repro asks |
| Empiricist | 11+3 | Small-BS sigmoid vs sigmoid; bias; probe |
| **Archaeologist** | **11+3** | **Prior table + subsequent timeline (HIGH)** |
| Visionary | 11+3 | 2026 encoder-swap experiments |

Image-first; notes in speaker notes; **PDF exports for Mac**. No Stakeholder redo in later roles.

---

## Paths
| Artifact | Path |
|----------|------|
| This pack | `/workspace/siglip-quiet/KANTA_PACK.md` |
| Senior Research | `/workspace/siglip-quiet/DELIVERABLE.md` |
| Summarizer dump | `/workspace/papers/siglip-2303.15343-summary.md` |
| Env outline | `/workspace/siglip-quiet/env_outline/` |
