# Paper Manager pack — I-JEPA (Nov 25 · Accompanying Models)

**Paper:** Self-Supervised Learning from Images with a Joint-Embedding Predictive Architecture (I-JEPA)  
**arXiv:** https://arxiv.org/abs/2301.08243 · [code](https://github.com/facebookresearch/ijepa) · **CVPR 2023** (arXiv v3, 13 Apr 2023)  
**Authors:** Mahmoud Assran\*, Quentin Duval, Ishan Misra, Piotr Bojanowski, Pascal Vincent, Michael Rabbat, Yann LeCun, Nicolas Ballas (\*massran@meta.com) — Meta FAIR / McGill / Mila / NYU  
**PDF:** `/workspace/papers/ijepa-2301.08243.pdf` (= `/workspace/ijepa-quiet/paper.pdf`) · **17 pp** · arXiv **2301.08243v3**  
**Session:** Nov 25 · due 9:30 AM · Type: **Accompanying Models (World Models, Reasoning, Planning)**  
**Role weights:** **Archaeologist=1 (Kanta Saito) LONGEST PRIMARY**; **Stakeholder=2 (Lukas Moroz)**; Reviewer / Empiricist / Visionary seats empty — **still all five produced**; Archaeologist heaviest  
**Kanta role:** Produce **Archaeologist-richest** five-role pack; deepen CLEAR/TENTATIVE JEPA lineage → V-JEPA → V-JEPA 2-AC; world-model / planning deep dive; SSL taxonomy; Empiricist course-smoke run pack  
**Themes:** predict in embedding space (not pixels) · multi-block masking · EMA target encoder · no hand-crafted view augs · JEPA vs MAE vs JEA · efficiency vs MAE · Clevr Dist · lineage CLEAR to V-JEPA → V-JEPA 2-AC robotics (do **NOT** claim robot results from this paper)  
**Sources:** Summarizer (`/workspace/papers/ijepa-2301.08243-summary.md`) + Senior Research (`/workspace/ijepa-quiet/DELIVERABLE.md`) + Ablation Bot (`/workspace/papers/ijepa-2301.08243-ablations.md` — **AUTHORITATIVE**) + `paper_notes.md` / `env_outline/`  
**Sibling links:** `/workspace/siglip-quiet/` · `/workspace/paligemma-quiet/` · `/workspace/openvla-quiet/` · `/workspace/pi05-quiet/` · `/workspace/openpi-comet-smoke/` · `/workspace/diffusion-policy-quiet/` · `/workspace/droid-quiet/` · `/workspace/data-scaling-laws-quiet/` · `/workspace/oxe-quiet/`

---

## 1. One-liner

**Predict missing parts in embedding space, not pixels** — from one **context** block, predict **EMA target-encoder** embeddings of several **multi-block** targets (no SimCLR-style view augs); multi-block is the critical inductive bias (**54.2** vs ~**15–20** on IN-1% lin); emb targets ≫ pixels (**66.9** vs **40.7**); image birth certificate of Meta’s JEPA line → **V-JEPA → V-JEPA 2-AC** (robot planning is **CLEAR subsequent**, not this PDF).

**Contrast poles (consistency triangle):**

| Pole | What they do | Primary claim / use |
|------|--------------|---------------------|
| **DINOv2 / SigLIP** | Discriminative JEA / contrastive–sigmoid towers | Dense / language-aligned features → **VLA vision towers** (π₀, OpenVLA, …) |
| **MAE** | Generative MIM — reconstruct **pixels** | Strong FT; weaker frozen linear; texture-rich |
| **I-JEPA → V-JEPA → V-JEPA 2-AC** | Predictive **embedding** / latent world model | Semantic SSL without view augs → **video** → **action-conditioned planning** |

**Do NOT invent / claim:** robot SR, BEHAVIOR q-score, Franka pick-place, or V-JEPA 2-AC planning numbers **from this 2023 image paper**. Cite **V-JEPA 2-AC** for robotics. Do not claim π₀ / OpenVLA / GR00T cite or use I-JEPA (they don’t — SigLIP/DINOv2 won the tower market).

---

## 2. Summary (exec overview + key numbers)

**Assran et al. (CVPR 2023 / Meta FAIR)** instantiate LeCun’s **JEPA** on images: learn highly **semantic** representations **without hand-crafted multi-view augmentations** by predicting **target-block embeddings** from a single **context** block. Architecture = **context ViT** (sparse, MAE-style visible patches) + **EMA target ViT** (full image; mask applied to **outputs**) + **narrow predictor ViT** (emb **384**; positional mask tokens). Loss = patch-wise **L2 in embedding space**.

**Why Accompanying Models session:** Supplies the **foundational** non-generative SSL brick whose **CLEAR** Meta follow-ons are **V-JEPA** (video feature prediction) and **V-JEPA 2 / 2-AC** (action-conditioned world model + zero-shot Franka planning). This paper itself has **zero** robotics / planning / action experiments — value is **idea + lineage + mask/target recipe**.

| Axis | Number / claim |
|------|----------------|
| Venue / code | **CVPR 2023**; `facebookresearch/ijepa` **CLEAR** |
| IN-1K linear (T1) | ViT-H/14 **79.3%** (300 ep); ViT-H/16@448 **81.1%** (300 ep) |
| IN-1% (T2) | H/14 **73.3%**; H/16@448 **77.3%** |
| Transfer H/14 (T3) | CIFAR100 **87.5** · Places205 **58.4** · iNat18 **47.6** |
| Local H/14 (T4) | Clevr/Count **86.7** · Clevr/Dist **72.4** (≪ DINO Dist **53.4**) |
| Multi-block vs alts (T6) | **54.2** vs raster **15.5** / block **20.2** / random **17.6** |
| Emb vs pixels (T7) | **66.9** vs **40.7** (ViT-L/16; 500 vs 800 ep) |
| Output vs input mask (T11) | **67.3** vs **56.1** |
| Predictor depth 12 vs 6 (T12) | **66.9** vs **64.0** |
| Full FT (T15) | I-JEPA H/16@448 **87.1%** (300 ep) vs MAE H/14@448 **87.8%** (1600 ep) |
| Compute | ViT-H/14 **&lt;1200 GPU-h**; **16×A100 &lt;72 h**; **~10×** cheaper than MAE H/14; **~2.5×** faster than iBOT S/16 |
| Per-iter vs MAE | ~**7%** slower/iter; ~**5×** fewer epochs → net win |
| Default mask | **M=4** targets scale **(0.15, 0.2)**; context **(0.85, 1.0)**; avg context ratio ~**0.25** |
| Collapse stop | EMA mom **0.996 → 1.0**; asymmetric predictor; **no** InfoNCE |

**Must-cite tattoo:** multi-block **54.2** vs ~**15–20** · emb vs pixels **66.9** vs **40.7** · IN1K H/14 **79.3** / H/16@448 **81.1** · 1% **73.3 / 77.3** · Clevr Dist **72.4** · compute **&lt;1200 GPU-h / 16×A100 &lt;72 h / ~10× vs MAE** · **V-JEPA 2-AC** as CLEAR subsequent for robots.

**Do NOT claim:** robot results from this PDF; course smoke = 79% H/14; π₀/OpenVLA use I-JEPA; “no augs” means no crop-like block sampling (mask sampling is the inductive bias).

---

## 3. Keywords

I-JEPA; Joint-Embedding Predictive Architecture; self-supervised learning; Vision Transformer; multi-block masking; representation-space prediction; EMA target encoder; masked image modeling; non-generative SSL; world models; predictive embeddings; energy-based models; LeCun AMI; V-JEPA; V-JEPA 2-AC; MAE foil; DINOv2/SigLIP foil; Clevr Dist; CVPR 2023; BEHAVIOR backbone ablation; Accompanying Models

---

## 4. Deep dive — World-model / planning / predictive embeddings (**PRIMARY SESSION CONTRIBUTION**)

**Callout:** Scientific product of *this* PDF = **static within-image latent prediction** (parts from context). Session type = Accompanying Models because the **JEPA program** is Meta’s explicit path to **latent world models** and later **action-conditioned planning**. Treat robotics claims as **lineage**, not in-paper results.

### What is “world-model-ish” here — CLEAR

| Piece | In-paper role | World-model reading |
|-------|---------------|---------------------|
| Predictor \(g_\phi\) | Narrow ViT; context tokens + **positional mask tokens** → predicted target embeddings | Conditional latent predictor \(P(\text{latent}_{target}\mid\text{latent}_{context},z_{\text{pos}})\) — **spatial** “future/parts,” not temporal |
| Loss in emb space | L2 on EMA target patches | Discard unpredictable pixel noise; keep **predictable structure** (pose / parts — Fig. 6 RCDM) |
| Multi-block targets | Several large-ish semantic regions | Forces prediction of **object-level** content, not texture fill |
| No view-aug JEA | Single view | Prior = **mask geometry**, not invariance to color/crop stacks |
| LeCun AMI [48] | Cited parent | JEPA = predictive embeddings + EBM agenda toward AMI |

### What is **not** in this paper

- No temporal dynamics, video, proprioception, or actions.  
- No CEM / MPC / planning loop.  
- No robot, BEHAVIOR, DROID, or VLA experiments.  
- Predictor visualizations (Sec 8 / Fig. 6) are **qualitative** RCDM decodes — suggestive, not quantitative planning proof.

### CLEAR subsequent robotics path (cite these, not Assran’23 numbers)

| Work | What it adds | Label |
|------|--------------|-------|
| **V-JEPA** (Bardes et al. 2024, arXiv **2404.08471**); `facebookresearch/jepa` | Same JEPA recipe on **video**; feature prediction only; frozen video+image probes (e.g. ViT-H/16: K400 **81.9**, SSv2 **72.2**, IN1K **77.9**) | **CLEAR** |
| **V-JEPA 2** (Assran et al. 2025, arXiv **2506.09985**); `facebookresearch/vjepa2` | Internet-scale video JEPA (~**1M+ h**) + post-train | **CLEAR** |
| **V-JEPA 2-AC** | Action-conditioned world model on **&lt;62 h** DROID; **zero-shot** Franka pick-place via **CEM** + image goals (no env-specific task reward) | **CLEAR** robotics transfer |
| V-JEPA 2.1 (dense predictive loss / multi-modal tokenizers) | Meta repo docs | **TENTATIVE**-as-of-pack |

### Design rhyme vs VLA towers (**TENTATIVE** — do not overclaim)

π₀ / π₀.₅ / OpenVLA / GR00T packs under `/workspace/` **do not cite** I-JEPA/V-JEPA. Their “predictive” pieces are **action experts** (flow matching / diffusion) on **SigLIP / DINOv2** tokens — architecture **rhyme** with “predict latent futures,” **not** JEPA lineage. BEHAVIOR’25 winners ride **π₀.₅ + FM + SigLIP** — a **parallel** world-interface bet, not V-JEPA 2-AC.

**World-model exam bite:** "I-JEPA = static **within-image** latent multi-block prediction (emb, not pixels); robotics payoff is **CLEAR** only at **V-JEPA 2-AC** (action-cond. + CEM); this PDF has **zero** robot numbers — cite lineage, not Assran’23 SR."

---

## 5. Deep dive — SSL taxonomy (JEPA vs MAE vs JEA)

**Callout:** Fig. 2 EBM framing is the session’s conceptual spine. Three families:

| Family | Predict / match | Space | Hand-crafted views? | Off-the-shelf semantic? |
|--------|-----------------|-------|---------------------|-------------------------|
| **Joint-Embedding (JEA)** — SimCLR, BYOL, DINO, VICReg, Barlow, iBOT | Similar embeddings for augmented views | Embedding | **Yes** | High; aug bias; multi-view cost |
| **Generative / MIM** — MAE, BEiT, SimMIM | Reconstruct missing patches | **Pixels / tokens** | No (masking) | Lower linear; needs FT |
| **JEPA (this paper)** | Predict **target embeddings** from context | **Embedding** | **No** (mask prior) | High linear + good local; single view |

### Architecture checklist (Empiricist pin)

| Piece | Spec (paper default) |
|-------|----------------------|
| Context encoder \(f_\theta\) | ViT; **only visible** context patches; **no [CLS]**; eval = **avg-pool** target-encoder |
| Target encoder \(f_{\bar\theta}\) | Same ViT; **full** image; EMA **0.996 → 1.0** |
| Predictor \(g_\phi\) | Narrow ViT emb **384**; depth **6** (B) / **12** (L/H) / **16** (G) |
| Targets | Mask **output** of target-encoder; **M=4** blocks scale **(0.15, 0.2)**, aspect **(0.75, 1.5)** |
| Context | 1 block scale **(0.85, 1.0)**; remove target overlap → sparse informative (~**0.25** patches) |
| Loss | \(\frac{1}{M}\sum_i\sum_{j\in B_i}\|\hat s_{y_j}-s_{y_j}\|_2^2\) |
| Opt | AdamW; BS **2048**; LR 1e-4→1e-3 (15-ep warmup)→1e-6 cosine; WD **0.04→0.4** |

### Ablation Bot AUTHORITATIVE — must-hit deltas

| Ablation | Paper locus | Headline numbers |
|----------|-------------|------------------|
| Mask strategy | T6 · ViT-B/16 300ep · IN-1% lin | multi-block **54.2** · raster **15.5** · block **20.2** · random **17.6** |
| Target space | T7 · ViT-L/16 | emb **66.9** (500ep) · pixels **40.7** (800ep) |
| Target scale | T8 | sweet spot **(0.15,0.2)→54.2**; too small **19.2**; too large **33.6** |
| Context scale | T9 | (0.40) **31.2** → (0.85) **54.2** |
| # targets | T10 | 1→4: **9.0 → 54.2** |
| Mask where | T11 · ViT-H/16 | output **67.3** · input **56.1** |
| Predictor depth | T12 · ViT-L/16 500ep | 6→**64.0**; 12→**66.9** |
| Predictor width | T14 · 1% FT | 384→**70.7**; 1024→**68.4** (bottleneck helps) |
| Data/model scale | T5 | IN22k lifts semantic transfer; G/16 patch tradeoff on Clevr |

**Taxonomy exam bite:** "Three SSL poles: **JEA** (view-invariance) · **MAE** (pixel MIM) · **I-JEPA** (emb multi-block prediction); multi-block **54.2≫15–20**; emb≪pixels **66.9 vs 40.7**; Clevr Dist **72.4** keeps local structure JEA discards."

---

## 6. Role-ready sections (all five — Archaeologist RICHEST)

### 6.1 Archaeologist — WEIGHT 1 LONGEST PRIMARY (Kanta Saito)

> **Seat charge:** Deepen **CLEAR / TENTATIVE** lineage from predictive coding → LeCun AMI / JEPA → **I-JEPA** → **V-JEPA → V-JEPA 2 / 2-AC**. Map consistency triangle vs DINOv2/SigLIP and MAE. Explicitly fence robot claims to **CLEAR subsequent** papers. Longest talk block (~16–18 slides).

#### Priors — CLEAR (cited / direct parents)

| Prior | Role relative to I-JEPA | Label |
|-------|-------------------------|-------|
| **LeCun 2022** AMI v0.9 [48] | Names **JEPA**; predictive embeddings + EBM; AMI agenda | **CLEAR** (cited) |
| **LeCun et al. EBM tutorial** [49] | Energy assign compatible/incompatible inputs — Fig. 2 framing | **CLEAR** |
| **Rao & Ballard / Friston** predictive coding [59, 31] | Cognitive motivation: predict missing / latent causes | **CLEAR** (cited) |
| **Siamese SSL:** SimCLR, MoCo, **BYOL**, SimSiam, **DINO**, SwAV, **MSN**, **iBOT** | View-invariance JEA **foil**; EMA / stop-grad collapse tricks **reused** | **CLEAR** |
| **VICReg**, **Barlow Twins** | Non-contrastive redundancy-reduction JEAs | **CLEAR** |
| **MAE**, BEiT, SimMIM, Context Encoder, DAE | Generative / MIM **foil**; I-JEPA reuses sparse-encoder trick | **CLEAR** |
| **data2vec**, Context Autoencoders | Closest: predict **representations** of masked patches; I-JEPA differs by **multi-block** + efficiency + image-only scope here | **CLEAR** |
| **CPC** / CPC images | Local predictive coding with **contrastive** patch discrimination — paper distinguishes: I-JEPA is **non-contrastive** block prediction | **CLEAR** (related-work contrast) |
| **ViT** / DeiT | Backbone | **CLEAR** |

#### This paper — CLEAR inventum

1. First **image** instantiation of JEPA with **multi-block** masking + EMA target + narrow **positional** predictor.  
2. Demonstrates: **no view augs** + **embedding-space loss** → semantic linear probes **and** competitive local tasks + **major compute win** vs MAE.  
3. Predictor RCDM visualizations as evidence the predictor captures **pose / part structure** while discarding unpredictable detail — philosophical JEPA claim made visual.  
4. Mask design **replaces** view-aug invariance as the semantic prior.

#### Subsequent — CLEAR

| Work | What it adds | Label |
|------|--------------|-------|
| **V-JEPA** (2404.08471); `facebookresearch/jepa` | Video JEPA; feature prediction; strong frozen probes | **CLEAR** (official follow-on; cites I-JEPA) |
| **V-JEPA 2** (2506.09985); `facebookresearch/vjepa2` | Internet-scale video + planning path | **CLEAR** |
| **V-JEPA 2-AC** | &lt;62 h DROID → zero-shot Franka CEM + image goals | **CLEAR** robotics |
| Official `facebookresearch/ijepa` | Reproducible IN configs / checkpoints | **CLEAR** |

#### Subsequent — TENTATIVE / design rhyme (do **not** overclaim)

| Work | Relation | Label |
|------|----------|-------|
| **DINOv2** | Strongest modern **discriminative** dense features; complementary foil, often paired with SigLIP in VLAs | **Design foil**, not child |
| **π₀ / π₀.₅, OpenVLA, GR00T-N1** | Local packs: **no** I-JEPA/V-JEPA cites; action experts on SigLIP/DINOv2 | **Architecture rhyme** only |
| Point-cloud / other-modality JEPA (Meta blog) | Pending pack-local PDFs | **TENTATIVE** |
| BEHAVIOR winners (π₀.₅ stacks) | FM + SigLIP — **parallel** world-model bet, not V-JEPA 2-AC | **TENTATIVE** as “same agenda” |

#### Genealogy diagram (CLEAR framing)

```
Predictive coding (Rao & Ballard ’99; Friston)
    → Energy-Based Models / LeCun tutorial [49]
        → LeCun AMI v0.9 (2022) — JEPA abstract [48]
            → data2vec / CAE / MAE  (repr vs pixel / token targets; MIM foil)
            → BYOL / DINO / iBOT   (EMA / Siamese collapse tricks; JEA foil)
                → **I-JEPA (this paper, CVPR 2023)** — image JEPA + multi-block mask
                    → **V-JEPA** (2024) — video feature prediction
                        → **V-JEPA 2 / 2-AC** (2025) — action-cond. world model + robot CEM
                            → optional visual-backbone *candidate* vs SigLIP/DINOv2 in VLA stacks
                              (Empiricist E8; not default ship)
```

#### Consistency triangle (session tattoo)

```
DINOv2 / SigLIP  —— discriminative JEA / contrastive-sigmoid —— VLA vision towers (π₀…)
MAE              —— generative MIM (pixels)                  —— strong FT, weaker linear
I-JEPA → V-JEPA → V-JEPA 2-AC —— predictive embedding / world model —— planning robotics
```

#### Deepen-toward-robotics note (Kanta)

I-JEPA’s durable robotics-facing idea is **not** “better ImageNet backbone for BC,” but **predictor-as-world-model**: learn \(P(\text{latent}_{target}\mid\text{latent}_{context},z)\) without wasting capacity on pixel noise. **V-JEPA 2-AC** is the first **CLEAR** Meta demonstration that this scales to **action-conditioned** latent dynamics + zero-shot manip planning. For BEHAVIOR / VLA groups: ask whether an **action-conditioned JEPA head** on frozen video features beats / complements FM action experts — **open research**, not established in this 2023 paper.

#### What to cite I-JEPA for (2026 talks)

- Semantic SSL **without** SimCLR-style augs.  
- **Latent** prediction vs MAE pixels.  
- Multi-block masking as the critical inductive bias.  
- Efficient scaling narrative (cite Fig 1/5; don’t re-run).  
- Birth certificate of the JEPA → V-JEPA 2-AC line.  
- **Not** for: robot SR, BEHAVIOR q-score, VLA action heads, “π₀ uses JEPA.”

**Archaeologist exam bite:** "I-JEPA (Assran’23 CVPR) = image JEPA: emb multi-block prediction + EMA + no view augs; multi-block **54.2≫15–20**; emb **66.9≫** pixels **40.7**; CLEAR line → **V-JEPA → 2-AC** for robots — **do not** claim robot numbers from this PDF; triangle = DINOv2/SigLIP | MAE | I-JEPA→V-JEPA→2-AC."

---

### 6.2 Stakeholder — WEIGHT 2 (Lukas Moroz)

**Buy / why schedule this paper in “Accompanying Models”:**
- Foundational **non-generative** SSL brick that Meta later scaled into **video world models + robot planning** (V-JEPA 2-AC).  
- Practical win even for static vision: **ViT-H/14 &lt;72 h on 16 A100**, linear **79.3%**, 1% **73.3%**, without view-aug engineering.  
- Dual competence: **semantic** (closes gap to DINO/iBOT) **and** **local** (Clevr depth **72.4** ≫ DINO **53.4**) — less “invariance over-bias.”  
- Open code (`facebookresearch/ijepa`); clear recipe to reimplement / ablate.  
- Conceptual clarity: three-way taxonomy **contrastive / generative / predictive** — reduces confused “SSL = contrastive” talk.

**Risks / costs / limits:**
- Paper itself is **image-only**; no planning / action / robot numbers — value is **lineage + idea**, not a drop-in world model.  
- Linear-probe SOTA for view-aug methods still contested by iBOT/DINO at smaller compute narratives; need scale (H/16@448) to match.  
- Full FT still **0.7 pp** behind MAE H/14@448 (87.1 vs 87.8) despite **5.3×** fewer epochs — if KPI is end-to-end FT only, MAE remains fine.  
- Predictor + EMA target ≈ **7%** slower/iter than MAE; savings come from **epoch count**.  
- Multi-block hyperparams are **sensitive** (T6–T10): wrong mask → collapse to mid-teens 1% accuracy.  
- Downstream VLA stacks (π₀, OpenVLA, GR00T) **did not adopt** I-JEPA encoders — SigLIP/DINO won the “vision tower” market. JEPA’s robotics payoff today is **world-model / planning** (V-JEPA 2-AC), not replacing SigLIP in PaliGemma.

**Decision ask:** Treat I-JEPA as **required reading for world-model track**; optional for VLA backbone bake-offs unless evaluating latent predictors.

**Stakeholder exam bite:** "Buy I-JEPA as the **JEPA birth certificate** + efficient semantic SSL recipe; buy V-JEPA 2-AC for robot planning demos — don’t buy I-JEPA as a drop-in π₀ tower replacement."

---

### 6.3 Scientific Reviewer — produced despite empty seat

**Claims under review**
1. Predicting in **representation space** with multi-block masking yields semantic SSL **without** hand-crafted view augs.  
2. Multi-block masking is **critical** (≫ random / single-block / raster).  
3. I-JEPA is **more compute-efficient** than MAE at matched semantic quality (GPU-hour plots).  
4. Representations remain strong on **local** tasks (Clevr) vs view-invariance methods.

**Strengths**
1. Clean taxonomy (Fig. 2) and crisp instantiation (Fig. 3).  
2. Ablations that isolate the thesis: emb ≫ pixels (**66.9 vs 40.7**); multi-block ≫ alts (**54.2 vs ≤20.2**); output-masking ≫ input-masking.  
3. Reports both **semantic** and **local** probes — rare honesty vs invariance-biased SSL.  
4. Efficiency claims backed by GPU-hour plots (Figs. 1, 5), not only epochs.  
5. Ablation Bot corpus rich (T6–T14 + App C).

**Weaknesses / ask-for-revision style**
1. “No hand-crafted augs” still uses **block sampling** as inductive bias — soft-aug debate.  
2. Table 2 mixes FT vs linear for 1% (“whichever works best”) — hurts strict ranking.  
3. Predictor viz via RCDM is qualitative only.  
4. Limited failure-mode analysis (texture bias, long-tail, multi-object beyond Clevr).  
5. Closest concurrent (data2vec) comparison could be tighter on matched compute.  
6. IN-centric ablations — “semantic” ≈ ImageNet class features; robot OOD untested here.  
7. Compute claims mix architectures (H/14 vs S/16 iBOT) — directionally persuasive, not iso-param.  
8. Collapse theory thin: EMA “essential” empirically; limited analysis without it.

**Verdict:** Strong **methods** paper that cleanly carves the third SSL pole and seeds the JEPA program. Accept for Accompanying Models as **foundational reading**; when citing for robotics, demand **V-JEPA 2-AC**, not this paper’s experiments.

**Reviewer exam bite:** Demand **T6 + T7 + Figs 1/5** before slogan; caveat image-only scope; punish “I-JEPA did robot planning” and “course smoke = 79% H/14.”

---

### 6.4 Empiricist — produced despite empty seat (course smoke; all seats consume)

> **Seat charge:** Course smokes that stress **mask strategy · emb vs pixels · linear probe**, ranked by GPU honesty. Prefer CIFAR-100 / IN-100 / tiny IN subset + ViT-Tiny/S — **not** paper ViT-H/14 IN-1K 300ep @ BS2048.

#### Claim under measurement

Matched short pretrain: **multi-block** masking and **EMA embedding** targets beat random/block masks and pixel reconstruction on semantic linear / k-NN probes (directional T6/T7), without claiming paper absolute %.

#### Hypotheses (falsifiable — from DELIVERABLE)

1. **H-mask:** On tiny data, multi-block still beats random / single-block on linear/k-NN after matched short pretrain (qualitative T6).  
2. **H-space:** Matched arch+steps: predicting **EMA target embeddings** beats **pixel reconstruction** on semantic probes; pixel path may win a low-level toy — falsifiable split (T7 vs T4 tension).  
3. **H-predictor:** Too-shallow predictors underfit positional conditioning → worse probe; depth helps until compute wall (T12 direction).  
4. **H-transfer:** After smoke pretrain, linear probe on CIFAR100 / IN-100 rises above train-from-scratch supervised tiny baseline at same steps; absolute % **will not** match paper H/14.  
5. **H-ckpt (E8):** Public I-JEPA ViT features remain competitive with MAE on household robot stills for linear/retrieval probes — optional bridge to BEHAVIOR perception.

**Falsifiers:** multi-block ≯ random; emb ≯ pixels on semantic probe; depth flat/negative; probe ≤ scratch; public JEPA ≪ MAE on robot stills with no caveat.

#### Prioritized ablation matrix (Ablation Bot + DELIVERABLE E1–E11)

| # | Experiment | Paper hook | Smoke | Pri |
|---|------------|------------|-------|-----|
| **E1** | Mask: multi vs random vs block (vs raster) | T6 | **Y — PRIMARY** | **P0** |
| **E2** | JEPA emb+EMA vs pixel decoder | T7 | **Y — PRIMARY** | **P0** |
| **E3** | Target / context / #targets mini-grid | T8–T10 | **Y** | **P0** |
| **E4** | Predictor depth {2,4,6} | T12 | **Y** | **P1** |
| **E5** | Mask output vs input | T11 | Partial | **P1** |
| **E6** | Frozen linear / k-NN first | T1–T3 lite | **Y — PRIMARY** | **P0** |
| **E7** | Clevr or toy count/depth | T4 | Partial | **P2** |
| **E8** | Frozen public ckpt on robot/household stills | Transfer hook | **Y** | **P1** |
| **E9** | EMA off / stop-grad | §3 | **Y** | **P2** |
| **E10** | Full ViT-H/14 IN-1K 300ep BS2048 | Fig1/5 | **N** | **Skip** |
| **E11** | IN-22K / ViT-G | T5 | **N** | **Skip** |

**Min package:** **E1 + E2 + E6**, then **E3** if GPU free; **E8** as Archaeologist/Empiricist joint measurement with public weights.

#### Exact reproduce stack (EDIT)

```bash
# Official
git clone https://github.com/facebookresearch/ijepa
# Pin commit → logs/pins.txt

# Course scout env (quiet outlines — preferred)
export IJEPA_WORK_ROOT=$SCRATCH/ijepa-smoke
cd /workspace/ijepa-quiet
bash env_outline/setup_mamba.sh
# mamba activate ijepa-smoke
mkdir -p logs runs checkpoints configs
# EDIT partition/account in slurm_*.sh
# Outlines only — do NOT sbatch from quiet box
# NEVER claim paper 79.3% H/14 / <1200 GPU-h / 10× MAE from smoke
```

#### Run cards (from DELIVERABLE)

##### E1 — `mask_strategy` (**PRIMARY**)

| Field | Spec |
|-------|------|
| Paper | multi-block **54.2** vs raster **15.5** / block **20.2** / random **17.6** |
| Goal | multi-block **>** random & block on tiny linear/k-NN by clear margin |
| Train | ViT-S/16 or Tiny; CIFAR100@224 or IN-100; BS 256–512; ~20k–80k steps |
| Pass | 3 mask runs logged; directional T6; **not** 54.2 absolute |
| Budget | **~6–18 GPU-h** · VRAM **8–16 GB** |
| Script | `env_outline/slurm_e1_mask.sh` |

##### E2 — `jepa_vs_mae_proxy` (**PRIMARY**)

| Field | Spec |
|-------|------|
| Paper | emb **66.9** vs pixels **40.7** |
| Goal | emb+EMA **>** pixel decoder on semantic probe |
| Pass | Matched mask geometry + steps; probe delta in JEPA direction |
| Budget | **~6–18 GPU-h** |
| Script | `env_outline/slurm_e2_jepa_vs_mae.sh` |

##### E6 — `linear_probe` (**PRIMARY**)

| Field | Spec |
|-------|------|
| Paper | T1–T3 protocol |
| Goal | Frozen probe / k-NN every N steps; beats scratch tiny baseline |
| Pass | Probe curve logged; disclaimer vs paper H/14 |
| Budget | **0.5–3 GPU-h** (frozen) |
| Script | `env_outline/slurm_e6_linear_probe.sh` |

##### E3 / E4 / E8 — follow-ons

E3 mini-grid (4–6 short runs) after E1 scaffold; E4 predictor depth {2,4,6}; E8 frozen public I-JEPA vs MAE/DINO/SigLIP on BEHAVIOR/DROID stills (linear/retrieval) — ties Accompanying Models → VLA stacks **without** pretrain.

#### Numbers to memorize (exam)

| Fact | Value |
|------|-------|
| Multi-block / raster / block / random | **54.2 / 15.5 / 20.2 / 17.6** |
| Emb / pixels | **66.9 / 40.7** |
| IN linear H/14 / H/16@448 | **79.3 / 81.1** |
| IN-1% H/14 / H/16@448 | **73.3 / 77.3** |
| CIFAR100 / Places / iNat (H/14) | **87.5 / 58.4 / 47.6** |
| Clevr Count / Dist | **86.7 / 72.4** |
| Output / input mask | **67.3 / 56.1** |
| Pred depth 12 / 6 | **66.9 / 64.0** |
| FT I-JEPA / MAE | **87.1 / 87.8** (300 vs 1600 ep) |
| Compute | **&lt;1200 GPU-h**; 16×A100 **&lt;72 h**; ~**10×** vs MAE |

#### Failure modes cheat-sheet

| Symptom | Likely cause | Next action |
|---------|--------------|-------------|
| All masks ~same | Steps too few / data too tiny | Extend steps; try IN-100 |
| Emb ≯ pixels | Pixel path undertrained or wrong eval | Match steps; semantic probe only |
| Collapse (probe ~chance) | EMA off / LR too high | Restore EMA; check mom schedule |
| OOM | BS 2048 paper | BS 256–512; ViT-S/Tiny |
| Claimed 79% from smoke | Overclaim | Report directional only |
| E8 JEPA ≪ SigLIP on language retrieval | Expected | JEPA unlabeled; pair projector or keep SigLIP default |

#### What not to invent / claim

- **Do not** claim course smoke = paper H/14 **79.3%** / **&lt;1200 GPU-h** / **10× MAE**.  
- **Do not** claim robot SR / BEHAVIOR wins from this paper.  
- **Do not** launch E10/E11 from course queue.  
- **Do not** compare smoke CIFAR % to paper IN linear without disclaimer.  
- **Do not** claim π₀/OpenVLA use I-JEPA towers.

**Empiricist exam bite:** "Assran’23: multi-block **54.2≫15–20**; emb **66.9≫** pixels **40.7**; Empiricist E1+E2+E6 on tiny data for **direction** — never claim H/14 IN parity or robot SR from smoke / this PDF."

---

### 6.5 Visionary — produced despite empty seat (BEHAVIOR recipe; **no Discord/Drive**)

**BEHAVIOR connection:**  
2025 BEHAVIOR winners ride **π₀.₅ + flow-matching action experts + SigLIP/PaliGemma** — a **generative action** world-interface, not a JEPA latent dynamics model. I-JEPA → V-JEPA 2-AC is the **parallel bet**: plan by optimizing actions so predicted **latents** match a **goal image embedding**, with almost no task demos. Opportunity: **hybrid** — V-JEPA-style latent rollout for long-horizon feasibility / imagination; FM expert for short-horizon motor execution.

| BEHAVIOR need | I-JEPA-shaped response |
|---------------|------------------------|
| Visual trunk choice | Default stays **SigLIP-class**; I-JEPA = **ablation candidate** (Empiricist E8) |
| Semantic ID w/o language | Try frozen JEPA features where view-aug SSL biases hurt |
| Long-horizon imagination | Invest in **video JEPA / V-JEPA 2-AC**, not static I-JEPA alone |
| Goal-image planning | CEM on action-cond. latents (cite **2-AC**, not this PDF) |
| Eval honesty | Encoder bake-offs on challenge frames — citing 79% IN does **not** move q-score |

**CLEAR:** Design prior that latent multi-block prediction is a viable SSL path and seeds world-model research.  
**TENTATIVE:** Official BEHAVIOR’26 baseline ships JEPA trunk; hybrid JEPA-rollout + FM wins organizer board.

#### 2026 BEHAVIOR visual-backbone / world-model recipe (6 bets)

1. **Default challenge trunk stays SigLIP-class** (OpenPI) or organizer N1.7 vision — I-JEPA is an **ablation candidate**, not the default ship.  
2. **When to try JEPA features:** tasks heavy on **semantic object identity without language**, or domains where view-aug SSL biases hurt (unusual wrist fisheye / sim materials) — validate with Empiricist **E8** before any FT.  
3. **Prefer frozen-probe → LoRA encoder FT** over from-scratch I-JEPA pretrain on BEHAVIOR RGB (E10-scale compute is politics, not student path).  
4. **Do not replace language alignment blindly:** BEHAVIOR instructions / stage labels still favor **SigLIP / VLM** towers; JEPA is unlabeled-image SSL — pair with a language projector if used.  
5. **World-model path ≠ this PDF:** for stage-aware long horizon, invest in **video JEPA / latent dynamics** siblings + RLC-style stage structure — not static I-JEPA alone.  
6. **Honesty:** citing Assran et al. 79% IN linear does **not** move BEHAVIOR q-score; only encoder ablations on challenge data do.

#### Follow-up research
1. Action-conditioned I/V-JEPA on OXE / DROID / BEHAVIOR-1K video — sample efficiency vs π₀-style BC.  
2. Goal-image CEM planning (V-JEPA 2-AC recipe) inside BEHAVIOR scenes vs language-goal VLAs.  
3. Uncertainty-aware predictors for contact-rich failure anticipation.  
4. Cross-embodied latent predictor (camera + proprio tokens).  
5. Contrastive-vs-JEPA bake-off for **affordance** probes.

#### New applications (**beyond Discord/Drive**)
- Zero-shot rearrange with **image goals** on new kitchens (V-JEPA 2-AC recipe).  
- Video anticipation / anomaly for safety monitors (predictor residual as surprise).  
- Self-supervised pretrain for **simulation-free** world models in household robotics.  
- Educational: teach “representations should be predictive, not reconstructive” as the AMI slogan made concrete.

**Visionary one-liner:** I-JEPA is the **static-image birth certificate** of Meta’s world-model line; the actionable robotics paper is **V-JEPA 2 / 2-AC** — but you cannot understand that line without this one. For BEHAVIOR’26, treat I-JEPA as a **visual-backbone ablation** against SigLIP/DINO/MAE while policy glue stays chunked FM / π₀.₅ / N1.7.

**Visionary exam bite:** "For BEHAVIOR, default **SigLIP**; ablate **I-JEPA** features via E8; world-model ambition rides **V-JEPA 2-AC**, not this still-image PDF — **no Discord/Drive**."

*(No Discord / Drive checklist content.)*

---

## 7. Empiricist run pack (env / scripts / GPU — from DELIVERABLE)

### Env setup

```bash
export IJEPA_WORK_ROOT=$SCRATCH/ijepa-smoke
cd /workspace/ijepa-quiet
bash env_outline/setup_mamba.sh
# mamba activate ijepa-smoke
mkdir -p logs runs checkpoints configs
# EDIT: partition/account in slurm_*.sh
# Pin facebookresearch/ijepa commit → logs/pins.txt
# Outlines only — do NOT sbatch from quiet box
```

| File | Role |
|------|------|
| `env_outline/conda_env.yaml` | `ijepa-smoke` sketch |
| `env_outline/conda_env.md` | Human-readable mamba steps |
| `env_outline/setup_mamba.sh` | Create env + clone pointer |
| `env_outline/run_checklist.md` | GPU honesty preflight |
| `env_outline/slurm_e1_mask.sh` | Mask-strategy array — **E1 PRIMARY** |
| `env_outline/slurm_e2_jepa_vs_mae.sh` | Emb vs pixel — **E2 PRIMARY** |
| `env_outline/slurm_e6_linear_probe.sh` | Frozen probe — **E6 PRIMARY** |
| `env_outline/README.md` | Index + honesty rules; siblings |

### GPU / RAM / TIME budget honesty

Assumptions: ViT-Tiny/S/16, CIFAR100 or IN-100, BS **256–512** (paper **2048**), short steps — **not** ViT-H/14 IN-1K 300ep / 16×A100.

| Workload | GPU VRAM (peak) | Host RAM | Wall time (order) | Smoke |
|----------|-----------------|----------|-------------------|-------|
| E1/E2 ViT-S, CIFAR100, ~50k steps | **8–16 GB** | 32–64 GB | **6–18 h** | **Y** |
| E3 mini-grid (4–6 short) | same | same | +0.5–1× E1 | **Y** |
| E4 predictor depth ×3 | same–slightly less | same | +few h | **Y** |
| E5 output vs input mask | **12–24 GB** (40GB pref.) | 64 GB | overnight | Partial |
| E6 linear probe only | **4–10 GB** | 32 GB | **0.5–3 h** | **Y** |
| E8 frozen public ckpt on robot stills | **6–12 GB** | 32 GB | **1–4 h** | **Y** |
| E7 Clevr | **8–16 GB** | 64 GB | data-bound | Partial |
| Paper ViT-H/14 IN-1K 300ep BS2048 | **16×A100** | node-class | **&lt;72 h** wall / **&lt;1200 GPU-h** | **N** |
| IN-22K / ViT-G | multi-node | — | days+ | **N** |

**Honesty rule:** log `(arch,mask,target_space,pred_depth,steps,BS,probe,wall_s,mem)`. If `#SBATCH --gres=gpu:N` but CUDA false → **fail**. Never amortize paper **79.3%** / **10× MAE** onto a Partial stub.

**Course envelope:** target **≤ ~40–60 GPU-h** for E1–E4+E6; reserve E8 if public weights cached. Paper H/14 story = **cite only**.

### Success criteria vs paper claims

| Paper claim | Minimal course criterion | Not required |
|-------------|--------------------------|--------------|
| Multi-block critical (T6) | E1: multi > random & block on tiny probe | Matching **54.2%** |
| Emb ≫ pixels (T7) | E2: emb+EMA > pixel on semantic probe | Matching **66.9 vs 40.7** |
| Context/targets scales (T8–10) | E3: default-ish beats extremes | Full App C grid |
| Deeper predictor helps (T12) | E4: depth↑ improves or plateaus | Need depth-12 on ViT-L |
| Strong linear w/o view augs (T1) | Smoke probe beats scratch; E8 vs DINO frozen | **79.3%** H/14 |
| Faster than MAE / iBOT (§7) | Optional steps-to-threshold | **10×** GPU-h claim |
| Local tasks competitive (T4) | E7 optional toy | Full Clevr parity |
| Scales to H/14 in &lt;72 h on 16×A100 | **Cite only** | Re-running it |

**Pass:** Empiricist executes **E1 + E2 + E6** with paper-aligned **directional** rankings + GPU honesty logged.  
**Fail / overclaim:** "reproduced Assran’23 / matched 79.3% / verified 10× MAE / robot planning from I-JEPA" from smoke or this PDF alone.

---

## 8. Slide Maker brief

| Role | Weight / timing guide | Focus |
|------|----------------------|-------|
| **Archaeologist** | **1 · LONGEST · ~16–18** | CLEAR/TENTATIVE lineage; genealogy; consistency triangle; V-JEPA→2-AC fence; inventum; exam bite |
| **Stakeholder** | **2 · ~12–14** | Buy world-model track; risks (image-only; VLAs didn’t adopt); decision ask |
| **Reviewer** | empty seat · ~12–14 | T6+T7+Figs1/5; verdict foundational Accept; robotics cite 2-AC |
| **Empiricist** | empty seat · ~12–14 | H-mask/H-space; E1–E6 run cards; GPU honesty; exam numbers |
| **Visionary** | empty seat · ~12–14 | BEHAVIOR SigLIP default + JEPA ablation; V-JEPA 2-AC hybrid; **no Discord/Drive** |

**Spine (paste into Slide Maker) — 5 bullets:**
1. **Predict in embedding space, not pixels** — multi-block context→target JEPA; no SimCLR view augs; EMA + asymmetric predictor.  
2. **Must-cite:** multi-block **54.2** vs ~**15–20**; emb vs pixels **66.9** vs **40.7**; IN1K H/14 **79.3** / H/16@448 **81.1**; 1% **73.3/77.3**; Clevr Dist **72.4**; compute **&lt;1200 GPU-h / 16×A100 &lt;72 h / ~10× vs MAE**.  
3. **SSL triangle:** DINOv2/SigLIP (JEA towers) \| MAE (pixel MIM) \| **I-JEPA→V-JEPA→2-AC** (predictive emb / world model).  
4. **Lineage (Archaeologist):** LeCun AMI → I-JEPA inventum → **CLEAR** V-JEPA → **V-JEPA 2-AC** robotics — **do NOT** claim robot results from this PDF.  
5. **BEHAVIOR’26 + honesty:** default SigLIP; I-JEPA = backbone ablation (E8); Empiricist E1+E2+E6 directional only; world-model ambition = video JEPA / 2-AC — **no Discord/Drive**.

**GitHub title sketch:** `[ECE 605] - I-JEPA Archaeologist Making One`

Full Slide Maker brief: `/workspace/ijepa-quiet/SLIDE_BRIEF.md`

---

## Paths

| Artifact | Path |
|----------|------|
| This pack | `/workspace/ijepa-quiet/KANTA_PACK.md` |
| PDF | `/workspace/ijepa-quiet/KANTA_PACK.pdf` |
| Slide brief | `/workspace/ijepa-quiet/SLIDE_BRIEF.md` |
| Senior Research | `/workspace/ijepa-quiet/DELIVERABLE.md` |
| Summarizer | `/workspace/papers/ijepa-2301.08243-summary.md` |
| Ablations | `/workspace/papers/ijepa-2301.08243-ablations.md` (**AUTHORITATIVE**) |
| Paper notes | `/workspace/ijepa-quiet/paper_notes.md` |
| Env outlines | `/workspace/ijepa-quiet/env_outline/` |
| Paper PDF | `/workspace/papers/ijepa-2301.08243.pdf` |
| Page figs | `/workspace/papers/ijepa-figs/page-*.png` |

---

*Pack synthesized 2026-09-29 PT (HST) for KanBot / ECE 605 — Nov 25 Accompanying Models · Archaeologist=1 (Kanta) LONGEST · Stakeholder=2 · all five produced · Ablation Bot AUTHORITATIVE · no Discord/Drive.*
