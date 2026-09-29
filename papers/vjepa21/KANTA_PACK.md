# Paper Manager pack — V-JEPA 2.1 (Nov 30 · Accompanying Models)

**Paper:** V-JEPA 2.1: Unlocking Dense Features in Video Self-Supervised Learning  
**arXiv:** https://arxiv.org/abs/2603.14482 · [code](https://github.com/facebookresearch/vjepa2) · arXiv **2603.14482v3** (11–12 Jun 2026)  
**Authors:** Lorenzo Mur-Labadia\*¹˒², Matthew Muckley¹, Amir Bar¹, Mido Assran¹, Koustuv Sinha¹, Mike Rabbat¹, Yann LeCun¹, Nicolas Ballas¹†, Adrien Bardes¹† (\*work done at Meta; †joint last) — FAIR at Meta / Universidad de Zaragoza  
**PDF:** `/workspace/papers/vjepa-2.1-2603.14482.pdf` (= `/workspace/vjepa21-quiet/paper.pdf`) · **37 pp** · ~37.4 MB  
**Session:** Nov 30 · due 9:30 AM · Type: **Accompanying Models (World Models, Reasoning, Planning)**  
**Role weights:** **Archaeologist=2 (Lukas Moroz) LONGEST PRIMARY**; Stakeholder / Reviewer / Empiricist / Visionary seats empty — **still all five produced**; Archaeologist heaviest  
**Kanta role:** **NOT seated** this session — still synthesize Archaeologist-richest five-role pack for Lukas; deepen CLEAR JEPA lineage through **2.1**; world-model / dense / planning deep dive; Empiricist course-smoke run pack  
**Themes:** dense predictive loss `L_ctx` · deep multi-level self-supervision · multi-modal tokenizers · VisionMix163M · keep global motion (SSv2) · Ego4D STA / EK anticipation · Franka AC grasp · latent nav vs NWM · lineage CLEAR I-JEPA → V-JEPA → V-JEPA 2 / 2-AC → **2.1** → 2-AC on 2.1 encoder  
**Sources:** Summarizer (`/workspace/papers/vjepa-2.1-2603.14482-summary.md`) + Senior Research (`/workspace/vjepa21-quiet/DELIVERABLE.md`) + Ablation Bot (`/workspace/papers/vjepa-2.1-2603.14482-ablations.md` — **AUTHORITATIVE**) + `paper_notes.md` / `env_outline/`  
**Sibling links:** `/workspace/ijepa-quiet/` (heavy lineage) · `/workspace/siglip-quiet/` · `/workspace/pi05-quiet/` · `/workspace/openpi-comet-smoke/` · `/workspace/diffusion-policy-quiet/` · `/workspace/droid-quiet/` · `/workspace/data-scaling-laws-quiet/`

---

## 1. One-liner

**Unlock dense video features without abandoning global motion** — add distance-weighted **context loss `L_ctx`** (supervise visible tokens) + **deep multi-level self-supervision**; then scale (VisionMix163M, ViT-G, cool-down). Fixes V-JEPA 2’s weak dense maps (ADE **22.2→47.9**, NYU **0.682→0.307**) while keeping SSv2 **77.7**; transfers to Ego4D STA **7.71**, Franka AC grasp **60→70→80%** (n=10), and latent nav **~10×** faster vs NWM (**10.6 s** vs **103.2 s**).

**Contrast poles (consistency triangle):**

| Pole | What they do | Primary claim / use |
|------|--------------|---------------------|
| **DINOv2 / DINOv3 / SigLIP** | Discriminative JEA / dense image features | VLA vision towers (π₀, OpenVLA, …); dense image SOTA foil |
| **MAE / VideoMAE** | Generative MIM — reconstruct **pixels** | Strong FT; weaker frozen linear |
| **I-JEPA → V-JEPA → 2 → 2.1 → 2-AC** | Predictive **embedding** / latent world model | Semantic SSL → **video** → **dense+temporal** → **action-conditioned planning** |

**Do NOT invent / claim:** BEHAVIOR hybrid JEPA+FM as an **in-paper** result (proposal only); Franka as general manipulation SOTA (cup table-top, **n=10**/skill); ADE “beats DINOv3” (NYU does; ADE **47.9** trails DINOv3-7B **55.9**); course smoke = ViT-G / Franka 80% / NYU 0.307; π₀/OpenVLA ship V-JEPA towers.

---

## 2. Summary (exec overview + key numbers)

**Mur-Labadia et al. (arXiv 2603.14482 / Meta FAIR)** diagnose why V-JEPA 2 is strong on global motion but weak on dense spatial structure: classic `L_predict` supervises **masked tokens only**, so context tokens behave like **register / global aggregators** → noisy PCA, ADE **22.2**, NYU **0.682**. Fix = four ingredients: (i) **Dense Predictive Loss** `L_dense = L_predict + L_ctx` (distance-weighted L1 on nearby context); (ii) **Deep Self-Supervision** (multi-level predictor over 4 encoder levels); (iii) **Multi-Modal Tokenizers** (native 2D image / 3D video + modality tokens); (iv) **scale** — VisionMix163M, ViT-G 2B, high-res cool-down + distill to ViT-L/B.

**Why Accompanying Models session:** Unlike I-JEPA (image-only, zero robots), **this PDF is both** an SSL methods paper **and** a world-model robotics paper — AC Franka CEM + latent nav are **in-paper**. Continuity from Nov 25 I-JEPA: students see AMI → JEPA → video → **dense state** → planning.

| Axis | Number / claim |
|------|----------------|
| Venue / code | arXiv **2603.14482v3**; `facebookresearch/vjepa2` **CLEAR** |
| Recipe stack ADE (T1) | VJ2 **22.2** → +L_ctx **33.8** → +deep SS **38.6** → full **47.9** |
| Recipe stack NYU↓ (T1) | **0.682** → **0.474** → **0.463** → … → **0.307** |
| Recipe stack SSv2 (T1) | **72.8** → +L_ctx **62.5** (hurt) → +deep SS **72.1** → full **77.7** |
| L_ctx weighting (T2) | weighted+warmup ADE **33.8** / SSv2 **62.5** (chosen); λ_final **0.5** vid / **0.7** img |
| Ego4D STA mAP All (T4) | ViT-G **7.71** (~+35% vs STAformer **5.67**; VJ2-g **6.02**) |
| EK100 Action R@5 (T5) | ViT-G **40.8** (VJ2-g **39.7**; 2.1-g **38.4** — needs G scale) |
| Franka Grasp (T6, n=10) | VJ2 **60%** → 2.1 matched **70%** → 2.1 H=8 **80%** |
| Nav Tartan ATE / time (T7) | ViT-G **5.687** ATE; **10.6 s** vs NWM **103.2 s** (~**10×**) |
| Dense vs DINOv3 (T8) | NYU **0.307** beats DINOv3-7B **0.309**; ADE **47.9** trails **55.9** |
| Global SSv2 (T9) | ViT-G **77.7** SOTA frozen attentive; 2.1-g **76.9** slightly under VJ2-g **77.3** |
| Distill ViT-L (T11) | SSv2 **76.5** / ADE **46.7** / NYU **0.336** ≈ teacher |
| Primary train | 135k steps; 16f@256; batch 128 clips + 2304 images; EMA 0.99925 |
| Cool-down | 12k; video 64f@384; image 512; NYU 0.365→**0.307** |

**Must-cite tattoo:** NYU **0.307** · ADE **22.2→33.8→38.6→47.9** · Ego4D STA **7.71** · grasp **60 / 70 / 80** (n=10) · nav ATE **5.687** + **10.6 s vs 103.2 s** · `L_ctx` vs deep SS trade-off (SSv2 **72.8→62.5→72.1**).

**Do NOT claim:** BEHAVIOR hybrid as demonstrated; course smoke = paper absolutes; ADE beats DINOv3; +20% grasp without clarifying matched (+10 pp) vs longer-horizon (+20 pp); temporal-mask as paper ablation (fixed [1.0,1.0]).

---

## 3. Keywords

V-JEPA 2.1; dense predictive loss; context loss L_ctx; deep self-supervision; multi-modal tokenizer; VisionMix163M; video SSL; joint-embedding predictive architecture; world models; action-conditioned prediction; CEM; Franka; Ego4D STA; EPIC-KITCHENS anticipation; NYUv2 depth; ADE20K; SSv2; Navigation World Model; I-JEPA lineage; Accompanying Models; BEHAVIOR backbone ablation

---

## 4. Deep dive — World-model / dense / planning (**PRIMARY SESSION CONTRIBUTION**)

**Callout:** Scientific product of *this* PDF = **dense+temporal latent prediction** that upgrades the same AC planner / nav stack used in V-JEPA 2. Session type = Accompanying Models because agents that predict & act over continuous time need **dense state estimation**, not only global video embeddings.

### What is “world-model-ish” here — CLEAR (in-paper)

| Piece | In-paper role | World-model reading |
|-------|---------------|---------------------|
| Predictor \(P_\phi\) | 24-block ViT emb 384; context + mask tokens → multi-level emb preds | Conditional latent mask-denoiser \(P(\text{latent}_{target}\mid\text{latent}_{context},z_{\text{pos}})\) — **spatio-temporal** |
| `L_ctx` + deep SS | Supervise **all** tokens at 4 encoder levels | Dense state that preserves local geometry / depth for MPC |
| AC predictor (§3.3) | Freeze 2.1 enc; 24-layer ~300M action-cond. TF on **DROID**; CEM image-goal MPC | Same **2-AC recipe** as Assran’25 with **encoder swap** to 2.1 |
| Grasp T6 | 60%→70% matched CEM → **80%** H=8 | Dense/depth unlocks **longer rollouts** (paper: VJ2 degrades with longer horizon) |
| Nav CDiT (§3.4) | Clean-pred + DDIM on 2.1 reps; CEM N=480 | Latent nav WM; **~10×** vs SD-VAE NWM (**10.6 s** vs **103.2 s**); Tartan ATE **5.687** |
| Anticipation T4/T5 | Ego4D STA **7.71**; EK Action **40.8** | Predictive reps for embodied forecast |

### What is **not** overclaimable

- Franka = cup Reach/Grasp/Pick-&-Place; **n=10** tasks/skill avg; lab table-top — not BEHAVIOR-1K / OXE.  
- Nav = open-loop **2 s** @ 4 FPS on Tartan/Scand/Sacson — not full autonomy.  
- Nav **10×** changes representation **and** sampler (8 DDIM vs ≥128) — joint system claim.  
- Remaining Grasp/P&P failures = **gripper timing**, not spatial misunderstanding (paper attribution).  
- Hybrid JEPA imagination + FM motor for BEHAVIOR = **Visionary proposal**, **not** an in-paper result.  
- Temporal mask **not** ablated (fixed tube [1.0,1.0] = V-JEPA 2).

### CLEAR lineage path (cite these labels; do not rewrite I-JEPA pack)

| Work | What it adds | Label |
|------|--------------|-------|
| **I-JEPA** (2301.08243); `ijepa-quiet/` | Image JEPA; multi-block; emb L2 | **CLEAR** parent |
| **V-JEPA** (2404.08471) | Video feature prediction | **CLEAR** |
| **V-JEPA 2** (2506.09985) | Internet-scale + first **2-AC** Franka CEM (≤62 h DROID) | **CLEAR** |
| **V-JEPA 2.1** (**this**, 2603.14482) | Dense unlock + better AC/nav with 2.1 encoder | **CLEAR** (was TENTATIVE in I-JEPA pack) |
| **V-JEPA 2-AC on 2.1** | §3.3 reuses Assran’25 AC codebase; encoder swap | **CLEAR** extension |
| `facebookresearch/vjepa2` | ViT-g/G + distilled B/L | **CLEAR** |

### Design rhyme vs VLA towers (**TENTATIVE** — do not overclaim)

π₀ / π₀.₅ / OpenVLA / GR00T packs **do not cite** V-JEPA 2.1. Their towers = SigLIP/DINOv2; “predictive” = FM/diffusion action experts. Architecture **rhyme** only. BEHAVIOR’25 winners ride **π₀.₅ + FM + SigLIP** — **parallel** world-interface bet, not JEPA child.

**World-model exam bite:** "V-JEPA 2.1 = dense-state upgrade to video JEPA: `L_ctx`+deep SS unlock NYU **0.307** / ADE **47.9** while SSv2 stays **77.7**; same AC CEM planner → grasp **60→70→80** (n=10); nav **10.6 s vs 103.2 s**. Robotics inventum = **state estimation**, not a new planner — cite T6/T7 carefully; do **not** invent BEHAVIOR hybrid as in-paper."

---

## 5. Deep dive — Dense / SSL recipe (`L_ctx` + deep SS)

**Callout:** Fig. 3 + Tables 1–2 are the session’s methods spine. Mask-only JEPA → context-as-register → weak dense; supervise visible tokens (weighted) + hierarchical loss restores Pareto.

### Architecture checklist (Empiricist pin)

| Piece | Spec (paper default) |
|-------|----------------------|
| Image tokenizer | 2D conv **16×16** |
| Video tokenizer | 3D conv **16×16×2** (tubelet=2) + modality emb + **3D RoPE** |
| x-encoder \(E_\theta\) | ViT-B/L/g/G; multi-level outs → MLP fuse |
| y-encoder \(E_{\bar\theta}\) | EMA mom **0.99925**; full unmasked; multi-level targets |
| Predictor \(P_\phi\) | **24** blocks, emb **384**; multi-level preds |
| Losses | L1; `L_dense = L_predict + L_ctx`; \(\lambda_i = \lambda / \sqrt{d_{\min}(i,M)}\) |
| Deep SS levels | 4 (ViT-G indices **[12,24,36,48]**) |
| Mask | Spatial **[0.15,0.7]**; aspect **[0.75,1.5]**; temporal **[1.0,1.0]** (= VJ2; **not ablated**) |
| Primary / cool-down | 135k @ 16f/256; +12k @ 64f/384 + img 512 |
| Final λ | **0.5** video / **0.7** image (T12) |

### Ablation Bot AUTHORITATIVE — must-hit deltas

| Ablation | Paper locus | Headline numbers |
|----------|-------------|------------------|
| + Context Loss | T1 · Fig 3 | ADE **22.2→33.8**; NYU **0.682→0.474**; SSv2 **72.8→62.5**; IN **82.2→72.6** |
| λ weighting | T2 | weighted+warmup **33.8 / 62.5** best; fixed λ=0.5 → ADE 27.5 / SSv2 **53.8** |
| + Deep SS | T1 · T13 | SSv2 **62.5→72.1**; ADE **33.8→38.6**; NYU **0.463**; Last vs 4-layer gap shrinks |
| + VisionMix | T1 · T3 | ADE **38.6→40.8**; NYU **0.418** |
| + Multi-modal Tok | T1 | ADE **40.8→41.4**; recognition flat |
| + Scale + cool-down | T1 · T14 | ADE **47.9**; NYU **0.307**; SSv2 **77.7** |
| Robot grasp | T6 | **60 / 70 / 80** (n=10); H=8 helps 2.1 |
| Nav vs NWM | T7 | ATE **5.687**; **10.6 s vs 103.2 s** |
| Temporal mask | — | **No paper ablation** — extension only |

**Taxonomy / recipe exam bite:** "`L_ctx` unlocks dense (ADE **22.2→33.8**, NYU **0.682→0.474**) but nukes globals (SSv2 **−10**); deep SS restores SSv2 **72.1** and lifts ADE **38.6**; full recipe NYU **0.307** / SSv2 **77.7** / ADE **47.9**. Temporal mask fixed — do not claim as ablation."

---

## 6. Role-ready sections (all five — Archaeologist RICHEST)

### 6.1 Archaeologist — WEIGHT 2 LONGEST PRIMARY (Lukas Moroz)

> **Seat charge:** Deepen **CLEAR / TENTATIVE** lineage I-JEPA → V-JEPA → V-JEPA 2 / 2-AC → **V-JEPA 2.1** → 2-AC on 2.1 encoder. Map consistency triangle vs DINOv3/SigLIP and MAE/VideoMAE. Fence robot claims to **Tables 6–7** (n=10; system confounds). Longest talk block (**~16** slides). **Kanta not seated** — Lukas carries lineage.

#### Priors — CLEAR (cited / direct parents)

| Prior | Role relative to V-JEPA 2.1 | Label |
|-------|----------------------------|-------|
| **LeCun 2022 AMI** | Names JEPA; predictive embeddings | **CLEAR** |
| **I-JEPA** Assran’23 (2301.08243); `ijepa-quiet/` | Image JEPA; multi-block; emb loss; EMA | **CLEAR** |
| **V-JEPA** Bardes’24 (2404.08471) | Video feature-prediction JEPA | **CLEAR** |
| **V-JEPA 2** Assran’25 (2506.09985) | Scale + **2-AC** planning recipe; immediate parent | **CLEAR** |
| **BYOL / stop-grad + EMA** | Collapse avoidance reused | **CLEAR** |
| **DINO / DINOv2 / DINOv3** | Dense-feature foil & table baselines | **CLEAR** (cited) |
| **Registers** Darcet’23 | Explains context-as-global-aggregator hypothesis | **CLEAR** |
| **LVD-142M / DINOv2 curation** | Image half of VisionMix163M | **CLEAR** |
| **NWM** Bar’25 | Nav baseline (SD-VAE); this paper swaps latent | **CLEAR** |
| **DROID** Khazatsky’24 | AC post-train robot data | **CLEAR** |
| **data2vec / MAE / VideoMAE** | Mask-predict / generative foils | **CLEAR** |

#### This paper — CLEAR inventum

1. **Diagnosis:** mask-only JEPA → weak dense maps (ADE **22.2** / NYU **0.682**).  
2. **Dense Predictive Loss:** supervise **all** tokens; distance-weighted `L_ctx` near masks.  
3. **Deep Self-Supervision:** multi-level predictor restores globals while lifting dense.  
4. **Native multi-modal tokenizers** (fix image-as-static-video hack).  
5. **VisionMix163M** rebalance (LVD-142M in; ImageNet out; YT/SSv2 up).  
6. **Empirical package:** STA **7.71**, EK **40.8**, NYU **0.307** (beats DINOv3-7B), SSv2 **77.7**; grasp **60→80**; nav **~10×**.  
7. Distilled ViT-L/B family for deployability.

#### Subsequent — CLEAR

| Work | What it adds | Label |
|------|--------------|-------|
| `facebookresearch/vjepa2` 2.1 weights | Reproducible encoders + distillates | **CLEAR** |
| V-JEPA 2-AC recipe on 2.1 encoder | This paper §3.3 | **CLEAR** extension |

#### Subsequent / parallel — TENTATIVE (do **not** overclaim)

| Work | Relation | Label |
|------|----------|-------|
| π₀ / π₀.₅ / OpenVLA / GR00T | No V-JEPA cites; SigLIP/DINO towers | **Architecture rhyme** only |
| BEHAVIOR winners (π₀.₅ stacks) | FM + SigLIP — **parallel** WM bet | **TENTATIVE** as “same agenda” |
| Hybrid JEPA-rollout + FM expert | Research proposal for BEHAVIOR | **TENTATIVE** — **not in-paper** |
| DINO-Foresight / dense→WM adjacent | Cited adjacent research | **TENTATIVE** sibling agenda |
| Point-cloud / other-modality JEPA | Pending local PDFs | **TENTATIVE** |

#### Genealogy diagram (CLEAR framing)

```
Predictive coding / LeCun AMI / JEPA abstract
    → I-JEPA (Assran 2023) — image multi-block latent prediction
         [pack: /workspace/ijepa-quiet/]
        → V-JEPA (Bardes 2024) — video mask-denoising JEPA
            → V-JEPA 2 (Assran 2025) — scale + AC world-model / robot CEM
                → **V-JEPA 2.1 (this paper, 2026)** — dense loss + deep SS + multimodal tok
                    → frozen backbone for anticipation / depth / seg / VOS
                    → AC predictor (DROID) + CEM manipulation; CDiT nav WM
                        → optional 2026 BEHAVIOR video / WM backbone *candidate*
                          (Empiricist probes; not default ship)
```

#### Consistency triangle (session tattoo)

```
DINOv2/v3 / SigLIP  —— discriminative dense (image-first) —— VLA towers
MAE / VideoMAE      —— generative pixels                    —— FT-heavy
I-JEPA→V-JEPA→2→2.1→2-AC —— predictive latents + now dense —— planning WM
```

#### Deepen-toward-robotics note (Lukas / Archaeologist)

V-JEPA 2 proved **action-conditioned latent planning** can zero-shot Franka with image goals. V-JEPA 2.1’s inventum for robotics is **not a new planner**, but **state estimation quality**: depth/local structure that makes CEM over longer horizons useful (**80%** grasp) and latent nav **~10×** cheaper than VAE latents. For BEHAVIOR / VLA groups: ask whether swapping SigLIP for frozen V-JEPA 2.1 **hurts language grounding** but **helps contact geometry / depth / tracking** — open, measurable. **Do not invent** robot claims beyond Tables **6–7**; **do not** present JEPA+FM hybrid as demonstrated.

#### What to cite V-JEPA 2.1 for (2026 talks)

- Dense + temporal SSL in one JEPA recipe.  
- Context-loss insight (supervise visible tokens).  
- Deep hierarchical JEPA supervision.  
- Frozen-backbone anticipation + depth SOTA-ish numbers (NYU **0.307**).  
- World-model planning hooks (grasp / nav) **with** honest N and system confounds.  
- **Not** for: drop-in OpenPI SigLIP replacement without probe evidence; full BEHAVIOR win claims; “temporal mask ablation.”

**Archaeologist exam bite:** "V-JEPA 2.1 (Mur-Labadia’26) = dense unlock for video JEPA: `L_ctx` ADE **22.2→33.8** / NYU **0.682→0.474** but SSv2 **−10**; deep SS restores **72.1** + ADE **38.6**; full NYU **0.307** / STA **7.71** / grasp **60/70/80** (n=10) / nav **10.6 vs 103.2 s**. CLEAR line I-JEPA→V-JEPA→2→**2.1**→2-AC-on-2.1; triangle = DINOv3/SigLIP | MAE | JEPA→2.1→2-AC. Do **not** invent BEHAVIOR hybrid as in-paper."

---

### 6.2 Stakeholder — produced despite empty seat

**Buy / why schedule this for “Accompanying Models”:**
- Direct continuation of Nov 25 I-JEPA into **temporal + dense + planning** — full AMI→JEPA→robot path in one syllabus arc.  
- Rare dual paper: SSL methods **and** world-model robotics (Franka AC + nav) in the **same PDF**.  
- Dense unlock answers the V-JEPA 2 objection (“great SSv2, useless depth/seg”) — NYU **0.307** competes with DINOv3-7B.  
- Open code (`vjepa2`) + distilled B/L → adoptable.  
- Fits theme: agents over continuous time need **dense** state, not only global video embeddings.

**Risks / costs / limits:**
- Pretrain = Meta-scale (ViT-G 2B, VisionMix163M, 135k+12k, batch 2304 images) — use released weights; not weekend repro.  
- Franka: **n=10**/skill, single cup table-top — promising but narrow; don’t oversell as general manip SOTA.  
- ADE/Cityscapes still trail DINOv3 — cluttered-scene gap admitted.  
- At matched 1B, 2.1-g slightly under VJ2 on SSv2 (**76.9 vs 77.3**) and EK Action (**38.4 vs 39.7**) — gains need ViT-G.  
- VidQA mixed; TemporalBench/TOMATO regressions on same data.  
- VLA market still on SigLIP/DINO — JEPA wins **WM/planning** track, not “drop-in VLM tower.”  
- **Kanta not seated** — Archaeologist (Lukas) carries lineage; syllabus owner still decides adoption.

**Decision ask:** **Required** for world-model / planning track after I-JEPA + V-JEPA 2; optional skim for pure VLA-backbone bake-offs unless evaluating depth/anticipation probes. Pair: V-JEPA 2 (2506.09985) § on 2-AC + this paper for dense unlock.

**Stakeholder exam bite:** "Buy V-JEPA 2.1 as the **dense-state upgrade** + in-paper AC/nav demos; buy released ViT-L distill for deploy — don’t buy it as a silent SigLIP replacement or as BEHAVIOR SOTA from citing 0.307 alone."

---

### 6.3 Scientific Reviewer — produced despite empty seat

**Claims under review**
1. Supervising **context tokens** (`L_ctx`) unlocks dense spatial structure in video JEPA.  
2. **Deep SS** recovers global motion understanding after the context-loss hit.  
3. Dense 2.1 features improve **zero-shot AC planning** (grasp) and enable **faster latent nav**.  
4. Recipe scales to ViT-G + cool-down with competitive dense+global Pareto.

**Strengths**
1. Clean causal story: context-as-register hypothesis → `L_ctx` ablation → PCA + linear probes → recipe stack (Fig. 5 / T1).  
2. Reports **both** dense and global; admits trade-offs when adding `L_ctx` alone.  
3. Robotics reuses public AC codebase + same task configs as V-JEPA 2 — fairer encoder swap.  
4. Dense eval protocol matched to DINOv3 — credible NYU claim.  
5. Distillation shows 2B is not vanity scale (ViT-L ≈ teacher).  
6. Ablation Bot corpus rich (T1/T2/T13 + robot/nav).

**Weaknesses / ask-for-revision style**
1. Robot **n=10**/skill; no CIs / multi-object / multi-env.  
2. Abstract “+20% grasp” mixes matched-horizon (**+10 pp**) and longer-horizon (**+20 pp**) — clarify.  
3. ViT-g slightly under VJ2 on some globals — say so loudly that ViT-G is required.  
4. ADE far from DINOv3-7B (**47.9 vs 55.9**); “SOTA dense” overstated outside depth.  
5. Cool-down confounds resolution with LR decay.  
6. Nav 10× changes latent **and** DDIM steps — system claim.  
7. T1 cumulative, not full factorial — Empiricist must isolate on fixed tiny data.  
8. Distillation drops deep SS — dense quality of B/L may lag teacher more than global tables show.

**Verdict:** Strong methods + transfer paper that closes the dense gap in the JEPA line and re-runs planning with better state. Accept for Accompanying Models as **required** after I-JEPA/V-JEPA 2; when citing robots, demand **T6 matched vs longer-horizon** and **T7 system caveat**.

**Reviewer exam bite:** Demand **T1+T2+Fig3** before slogan; cite grasp as **60/70/80 (n=10)** not bare “+20%”; punish BEHAVIOR-hybrid-as-result and temporal-mask-as-paper-ablation.

---

### 6.4 Empiricist — produced despite empty seat (course smoke; all seats consume)

> **Seat charge:** Course smokes that stress **`L_ctx` · λ weighting · deep SS · frozen probes**, ranked by GPU honesty. Prefer tiny short clips + ViT-Tiny/S — **not** VisionMix / ViT-G / Franka CEM.

#### Claim under measurement

Matched short pretrain: weighted **`L_ctx`** improves a dense proxy vs masked-only; without deep SS, global clip accuracy **drops**; multi-level SS **recovers** globals while keeping most dense gains (directional T1/T2) — without claiming paper absolute %.

#### Hypotheses (falsifiable — from DELIVERABLE)

1. **H-dense:** Weighted `L_ctx` improves dense proxy (toy depth / patch-neighbor / mini-seg) vs masked-only.  
2. **H-tradeoff:** Without deep SS, `L_ctx` **hurts** global clip classification (SSv2-lite / Kinetics-tiny).  
3. **H-deep:** Multi-level supervision recovers global accuracy while keeping most dense gains.  
4. **H-temporal (extension only):** Stronger temporal tube masking *may* matter for motion probes — **not a paper ablation**.  
5. **H-prior:** Frozen 2.1 features beat V-JEPA 2 on dense probes when public ckpts exist (T8 sign).  
6. **H-transfer (E8):** Better dense features → lower latent goal-distance / higher proxy grasp on **sim** Push-T / LIBERO-lite / DROID stills — **not** paper Franka 80%.

**Falsifiers:** `L_ctx` ≯ masked-only on dense; no global drop without deep SS; deep SS fails to recover; public 2.1 ≯ 2 on dense with no caveat; smoke claimed as Franka 80%.

#### Prioritized ablation matrix (Ablation Bot + DELIVERABLE)

| # | Experiment | Paper hook | Smoke | Pri |
|---|------------|------------|-------|-----|
| **E1** | `L_ctx` on vs masked-only | T1, Fig 3 | **Y — PRIMARY** | **P0** |
| **E2** | λ fixed / warmup / weighted lite | T2 | **Partial** | **P0** |
| **E3** | Deep SS levels {1,4} | T1, T13 | **Y — PRIMARY** | **P0** |
| **E4** | Multi-modal tokenizer | T1 | Partial | P1 |
| **E5** | Temporal mask (extension) | — | Partial | P2 |
| **E6/E7** | Frozen 2 vs 2.1 probes | T4/T5/T8/T9 | **Y / Partial** | **P0** |
| **E8** | Tiny AC / embedding nav proxy | T6/T7 | Partial | P1 |
| — | VisionMix / cool-down / ViT-G / Franka / NWM | T1/T6/T7/T14 | **N** | Skip |

**Min package:** **E1 + E3 + E7**, then **E2** if GPU free; **E8** as joint measurement with public weights. Label any E5 **extension**.

#### Exact reproduce stack (EDIT)

```bash
# Official
git clone https://github.com/facebookresearch/vjepa2
# Pin commit → logs/pins.txt

# Course scout env (quiet outlines — preferred)
export VJEPA21_WORK_ROOT=$SCRATCH/vjepa21-smoke
cd /workspace/vjepa21-quiet
bash env_outline/setup_mamba.sh
# mamba activate vjepa21-smoke
mkdir -p logs runs checkpoints configs
# EDIT partition/account in slurm_*.sh
# Outlines only — do NOT sbatch from quiet box
# NEVER claim paper NYU 0.307 / STA 7.71 / Franka 80% / 10× nav from smoke
```

#### Run cards (from DELIVERABLE)

##### E1 — `lctx_on_off` (**PRIMARY**)

| Field | Spec |
|-------|------|
| Paper | ADE **22.2→33.8**; NYU **0.682→0.474**; SSv2 **72.8→62.5** |
| Goal | Dense proxy ↑; global may ↓ without deep SS |
| Train | ViT-S/Tiny; 4–8f@128–224; BS 8–16; ~5k–30k steps |
| Pass | Directional T1; **not** absolute ADE/NYU |
| Budget | **~12–36 GPU-h** · VRAM **12–22 GB** |
| Script | `env_outline/slurm_e1_lctx.sh` |

##### E3 — `deepss_levels` (**PRIMARY**)

| Field | Spec |
|-------|------|
| Paper | SSv2 **62.5→72.1**; ADE **33.8→38.6** |
| Goal | Global recovers when levels↑; dense mostly kept |
| Pass | Levels {1,4} logged; directional T1/T13 |
| Budget | **+0.5–1× E1** |
| Script | `env_outline/slurm_e3_deepss.sh` |

##### E7 — `frozen_probe` (**PRIMARY readout**)

| Field | Spec |
|-------|------|
| Paper | T4/T5/T8/T9 protocol |
| Goal | Public 2.1 vs 2 same-sign dense gap on mini probes |
| Pass | Probe curve logged; disclaimer vs paper ViT-G |
| Budget | **1–6 GPU-h** (frozen) |
| Script | `env_outline/slurm_e7_frozen_probe.sh` |

##### E2 / E5 / E8 — follow-ons

E2 λ mini-grid (Partial); E5 temporal-mask **extension only**; E8 sim AC / embedding-distance proxy — ties Accompanying Models → robotics **without** Franka queue.

#### Numbers to memorize (exam)

| Fact | Value |
|------|-------|
| ADE recipe T1 | **22.2 / 33.8 / 38.6 / 47.9** |
| NYU recipe T1 | **0.682 / 0.474 / 0.463 / 0.307** |
| SSv2 recipe T1 | **72.8 / 62.5 / 72.1 / 77.7** |
| T2 weighted+warmup | ADE **33.8** / SSv2 **62.5** |
| Ego4D STA ViT-G | **7.71** |
| EK Action ViT-G | **40.8** |
| Grasp T6 | **60 / 70 / 80** (n=10) |
| Nav T7 | ATE **5.687**; **10.6 s vs 103.2 s** |
| NYU vs DINOv3-7B | **0.307 vs 0.309** |
| Distill ViT-L SSv2 / ADE | **76.5 / 46.7** |

#### Failure modes cheat-sheet

| Symptom | Likely cause | Next action |
|---------|--------------|-------------|
| No dense lift from `L_ctx` | Steps too few / clips too tiny | Extend steps; try denser stills mix |
| Global doesn’t drop | λ too small / data too easy | Raise λ; check mask coverage |
| Deep SS no recovery | Levels miswired / fused wrong | Unit-test multi-level shapes |
| OOM | Paper BS / long clips | Cut frames+res first; BS 8 |
| Claimed 0.307 / 80% from smoke | Overclaim | Report directional only |
| Temporal-mask logged as paper abl. | Mislabel | Relabel **extension** |

#### What not to invent / claim

- **Do not** claim course smoke = NYU **0.307** / STA **7.71** / Franka **80%** / **10×** nav.  
- **Do not** claim BEHAVIOR q-score ↑ from citing dense numbers.  
- **Do not** launch VisionMix / ViT-G / Franka CEM from course queue.  
- **Do not** claim temporal-mask as paper ablation.  
- **Do not** claim π₀/OpenVLA use V-JEPA towers.

**Empiricist exam bite:** "Mur-Labadia’26: `L_ctx` ADE **22.2→33.8** / SSv2 **−10**; deep SS restores **72.1**; Empiricist E1+E3+E7 on tiny clips for **direction** — never claim ViT-G / Franka / 10× from smoke."

---

### 6.5 Visionary — produced despite empty seat (BEHAVIOR recipe; **no Discord/Drive**)

**BEHAVIOR connection:**  
2025 BEHAVIOR winners ride **π₀.₅ + flow-matching action experts + SigLIP/PaliGemma** — generative short-horizon motor interface. V-JEPA 2 / 2.1 / 2-AC ≈ **latent predictive imagination** + CEM / diffusion planning with image goals and almost no task demos. **Dense unlock matters:** household tasks need contact geometry, object boundaries, depth for grasp affordances — exactly what V-JEPA 2 lacked and 2.1 adds (NYU **0.307**, VOC **85.0**, tracking ~70 J&F).

**Proposed hybrid (do NOT invent as existing / in-paper result):** V-JEPA 2.1 latent rollout for long-horizon feasibility / goal imagination + FM action expert for motor execution. Research agenda only.

| BEHAVIOR need | V-JEPA 2.1-shaped response |
|---------------|----------------------------|
| Visual trunk choice | Default stays **SigLIP-class**; 2.1 = **video/dense/WM ablation** |
| Depth / contact geometry | Frozen 2.1 features where image SSL lacks temporal consistency |
| Long-horizon imagination | AC / CDiT latent planners on 2.1 reps (cite T6/T7 carefully) |
| Goal-image planning | CEM on action-cond. latents (2-AC recipe + denser encoder) |
| Language instructions | Keep SigLIP parallel / projector — JEPA unlabeled |
| Eval honesty | Encoder+planner ablations on challenge data — citing 0.307 ≠ q-score |

**CLEAR:** Dense+temporal JEPA is a viable SSL path that upgrades Meta’s planning demos.  
**TENTATIVE:** Official BEHAVIOR’26 baseline ships JEPA trunk; hybrid JEPA-rollout + FM wins organizer board.

#### 2026 BEHAVIOR video backbone / world-model recipe (7 bets)

1. **Default language-aligned trunk stays SigLIP-class** (`pi05-quiet`); V-JEPA 2.1 is the **video/dense/WM ablation**, not the silent default.  
2. **When to try 2.1:** depth-sensitive manip, contact-rich stages, short-horizon visual-goal MPC, multi-frame tracking — validate with Empiricist frozen probes first.  
3. **Prefer frozen ViT-L/B distill → LoRA** over course-scale video pretrain; VisionMix is politics.  
4. **Pair modalities:** JEPA lacks native language — add projector / keep SigLIP parallel.  
5. **World-model path is this paper’s strength:** AC + latent nav are “stage imagination” candidates; still need stage structure / RLC-style wrappers from elsewhere.  
6. **Honesty:** citing Ego4D **7.71** or Franka +20 pp does **not** move BEHAVIOR q-score.  
7. **Smoke ladder:** I-JEPA mask lessons (`ijepa-quiet`) → 2.1 `L_ctx`+deepSS tiny clips → frozen household probes → tiny AC head — never jump to ViT-G.

#### Follow-up research
1. Action-conditioned JEPA on OXE / DROID / BEHAVIOR-1K with **2.1** encoder — sample efficiency vs π₀ BC.  
2. Goal-image CEM inside BEHAVIOR scenes vs language-goal VLAs.  
3. Dense aux heads (depth/seg/track) for WM planning.  
4. Cross-embodied latents (camera + proprio).  
5. Uncertainty-aware predictors for contact-rich failure anticipation.  
6. Scale / denser WM heads (paper §5 future work).

#### New applications (**beyond Discord/Drive**)
- Zero-shot rearrange with **image goals** on new kitchens (2-AC + denser encoder).  
- Fast latent navigation for indoor robots (**~10×** vs VAE latents).  
- Video anticipation / safety monitors (predictor residual as surprise; STA-style).  
- Simulation-light world models for household robotics pretrain.  
- Educational: “representations should be predictive **and** locally grounded.”

**Visionary one-liner:** V-JEPA 2.1 is the **dense-state upgrade** to Meta’s video world-model line — the piece that makes latent planning spatially trustworthy for grasping and fast navigation; for BEHAVIOR’26 treat it as imagination / dense backbone **ablation** against SigLIP/DINOv3/V-JEPA 2 while policy glue stays π₀.₅ / N1.7 — **hybrid is proposal, not result**.

**Visionary exam bite:** "For BEHAVIOR, default **SigLIP**; ablate **V-JEPA 2.1** dense/WM features via Empiricist probes; cite T6/T7 carefully (n=10; system 10×); **do not** invent JEPA+FM hybrid as in-paper — **no Discord/Drive**."

*(No Discord / Drive checklist content.)*

---

## 7. Empiricist run pack (env / scripts / GPU — from DELIVERABLE)

### Env setup

```bash
export VJEPA21_WORK_ROOT=$SCRATCH/vjepa21-smoke
cd /workspace/vjepa21-quiet
bash env_outline/setup_mamba.sh
# mamba activate vjepa21-smoke
mkdir -p logs runs checkpoints configs
# EDIT: partition/account in slurm_*.sh
# Pin facebookresearch/vjepa2 commit → logs/pins.txt
# Outlines only — do NOT sbatch from quiet box
```

| File | Role |
|------|------|
| `env_outline/conda_env.yaml` | `vjepa21-smoke` sketch |
| `env_outline/conda_env.md` | Human-readable mamba steps |
| `env_outline/setup_mamba.sh` | Create env + clone pointer |
| `env_outline/run_checklist.md` | GPU honesty preflight |
| `env_outline/slurm_e1_lctx.sh` | Context-loss on/off — **E1 PRIMARY** |
| `env_outline/slurm_e3_deepss.sh` | Deep SS levels — **E3 PRIMARY** |
| `env_outline/slurm_e5_temporal_mask.sh` | Temporal mask **extension** |
| `env_outline/slurm_e7_frozen_probe.sh` | Frozen probes — **E7 PRIMARY** |
| `env_outline/README.md` | Index + honesty rules; siblings |

### GPU / RAM / TIME budget honesty

Assumptions: ViT-Tiny/S, 4–8f@128–224, BS **8–16** — **not** ViT-G / VisionMix / 64f cool-down / Franka.

| Workload | GPU VRAM (peak) | Host RAM | Wall time (order) | Smoke |
|----------|-----------------|----------|-------------------|-------|
| E1 `L_ctx` on/off | **12–22 GB** | 64 GB | **12–36 h** | **Y** |
| E3 deep-SS ×{1,4} | same–+20% | 64 GB | +0.5–1× E1 | **Y** |
| E2 λ mini-grid | same | 64 GB | overnight | Partial |
| E5 temporal mask ×3 (extension) | same | 64 GB | +few h each | Partial |
| E7 frozen probe (public ckpt) | **6–16 GB** | 32–64 GB | **1–6 h** | **Y** |
| E4 / E8 tokenizer / AC proxy | **8–24 GB** | 64 GB | overnight–day | Partial |
| Paper ViT-L 135k VisionMix | multi-GPU | node | **days** | **N** |
| Paper ViT-G + cool-down | multi-node | — | **week-class** | **N** |
| Franka CEM eval | lab + 1×A100 | — | lab schedule | **N** |

**Honesty rule:** log `(arch,lctx,lambda,deep_levels,frames,res,steps,BS,probe,wall_s,mem)`. Cut **frames and resolution** before lying about budgets — video ≫ I-JEPA image smoke. Never amortize paper **0.307 / 7.71 / 80% / 10×** onto a Partial stub.

**Course envelope:** target **≤ ~60–90 GPU-h** for E1+E3+E7 (+E2); add E8 if public weights cached. Paper ViT-G / Franka story = **cite only**.

### Success criteria vs paper claims

| Paper claim | Minimal course criterion | Not required |
|-------------|--------------------------|--------------|
| `L_ctx` unlocks dense (T1/Fig3) | E1: dense proxy ↑ vs masked-only | Matching ADE 33.8 / NYU 0.474 |
| Dense hurts global until deep SS | E1→E3: global recovers | Matching 72.8 / 77.7 SSv2 |
| Weighted `L_ctx` best trade-off (T2) | E2 direction: weighted ≥ fixed mid | Full λ schedule fidelity |
| Beats V-JEPA 2 on dense (T8) | E7: same-sign gap on mini probes | 0.307 RMSE / 47.9 mIoU |
| +20 pp grasp / better planning (T6) | E8: proxy ↑ **or** document blocker | Franka 80% / CEM wall-clock |
| 10× faster nav (T7) | Cite; optional embedding CEM toy | 5.687 ATE / 10.6 s |
| SOTA Ego4D / EK | Cite frozen-ckpt if available | Full STA/EK training |
| Scales to ViT-G + cool-down | **Cite only** | Re-running it |

**Pass:** Empiricist executes **E1 + E3 + E7** with paper-aligned **directional** rankings + GPU honesty logged.  
**Fail / overclaim:** "reproduced Mur-Labadia’26 / matched 0.307 / verified Franka 80% / 10× nav / BEHAVIOR win" from smoke alone.

---

## 8. Slide Maker brief

| Role | Weight / timing guide | Focus |
|------|----------------------|-------|
| **Archaeologist** | **2 · LONGEST · ~16** | CLEAR/TENTATIVE lineage; genealogy; consistency triangle; inventum; T6/T7 fence; exam bite |
| **Stakeholder** | empty seat · ~12–14 | Buy dense+WM track; risks (scale, n=10, ADE gap); decision ask |
| **Reviewer** | empty seat · ~12–14 | T1+T2+Fig3; grasp 60/70/80 clarity; verdict Accept |
| **Empiricist** | empty seat · ~12–14 | H-dense/H-tradeoff/H-deep; E1+E3+E7; GPU honesty; exam numbers |
| **Visionary** | empty seat · ~12–14 | BEHAVIOR SigLIP default + 2.1 ablation; hybrid = proposal; **no Discord/Drive** |

**Spine (paste into Slide Maker) — 5 bullets:**
1. **Unlock dense video features via `L_ctx` + deep SS** — supervise visible tokens (weighted) + multi-level prediction; keep global motion (SSv2 **77.7**).  
2. **Must-cite:** NYU **0.307**; ADE **22.2→33.8→38.6→47.9**; Ego4D STA **7.71**; grasp **60 / 70 / 80** (n=10); nav ATE **5.687** + **10.6 s vs 103.2 s**; `L_ctx` vs deep SS trade-off (SSv2 **72.8→62.5→72.1**).  
3. **SSL / WM triangle:** DINOv3/SigLIP (JEA towers) \| MAE/VideoMAE (pixel MIM) \| **I-JEPA→V-JEPA→2→2.1→2-AC** (predictive emb / dense WM).  
4. **Lineage (Archaeologist=2 Lukas LONGEST):** CLEAR I-JEPA → V-JEPA → V-JEPA 2 / 2-AC → **2.1** → 2-AC on 2.1 encoder — robotics inventum = **state estimation**, not new planner; cite T6/T7 carefully.  
5. **BEHAVIOR’26 + honesty:** default SigLIP; 2.1 = dense/WM ablation; Empiricist E1+E3+E7 directional only; **do not invent** JEPA+FM hybrid as in-paper — **no Discord/Drive**.

**GitHub title sketch:** `[ECE 605] - V-JEPA 2.1 Archaeologist Making Two`

Full Slide Maker brief: `/workspace/vjepa21-quiet/SLIDE_BRIEF.md`

---

## Paths

| Artifact | Path |
|----------|------|
| This pack | `/workspace/vjepa21-quiet/KANTA_PACK.md` |
| PDF | `/workspace/vjepa21-quiet/KANTA_PACK.pdf` |
| Slide brief | `/workspace/vjepa21-quiet/SLIDE_BRIEF.md` |
| Senior Research | `/workspace/vjepa21-quiet/DELIVERABLE.md` |
| Summarizer | `/workspace/papers/vjepa-2.1-2603.14482-summary.md` |
| Ablations | `/workspace/papers/vjepa-2.1-2603.14482-ablations.md` (**AUTHORITATIVE**) |
| Paper notes | `/workspace/vjepa21-quiet/paper_notes.md` |
| Env outlines | `/workspace/vjepa21-quiet/env_outline/` |
| Paper PDF | `/workspace/papers/vjepa-2.1-2603.14482.pdf` |
| Page figs | `/workspace/papers/vjepa21-figs/page-*.png` |
| Lineage sibling | `/workspace/ijepa-quiet/` |

---

*Pack synthesized 2026-09-29 PT (HST) for KanBot / ECE 605 — Nov 30 Accompanying Models · Archaeologist=2 (Lukas) LONGEST · Kanta NOT seated · all five produced · Ablation Bot AUTHORITATIVE · Tables 6–7 n=10 careful · no Discord/Drive · no invented BEHAVIOR hybrid as in-paper.*
