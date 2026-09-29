# Paper Manager pack — LeWorldModel / LeWM (Dec 2 · Accompanying Models)

**Paper:** LeWorldModel: Stable End-to-End Joint-Embedding Predictive Architecture from Pixels  
**arXiv:** https://arxiv.org/abs/2603.19312 · [code](https://github.com/lucas-maes/le-wm) · [site](https://le-wm.github.io) · arXiv **2603.19312v3** (3 Jun 2026)  
**Authors:** Lucas Maes\*¹, Quentin Le Lidec\*², Damien Scieur¹˒³, Yann LeCun², Randall Balestriero⁴ (\*equal) — Mila & UdeM / NYU / Samsung SAIL / Brown  
**PDF:** `/workspace/papers/leworldmodel-2603.19312.pdf` (= `/workspace/leworldmodel-quiet/paper.pdf`) · **28 pp** · ~5.4 MB  
**Session:** Dec 2 · due 9:30 AM · Type: **Accompanying Models (World Models, Reasoning, Planning)**  
**Role weights:** **Empiricist=3 (Nykolas Rekasius) LONGEST PRIMARY**; Archaeologist / Stakeholder / Reviewer / Visionary seats empty — **still all five produced**; Empiricist heaviest  
**Kanta role:** **NOT seated** this session — still synthesize Empiricist-richest five-role pack for Nykolas; deepen reproducible claims, planning speed/stability, training recipe without fragile heuristics; WM/planning + taxonomy deep dive; course-smoke run pack  
**Themes:** compact **~15M** E2E JEPA WM from pixels · **SIGReg** anti-collapse · drops EMA / stop-grad / frozen encoder / multi-term VICReg · **~48×** faster planning than DINO-WM · **SIM-ONLY** (Push-T / OGBench-Cube / Two-Room / Reacher) — **not** a Meta V-JEPA descendant · sibling recipe **PLDM→LeWM** vs Meta **I-JEPA→…→2-AC**  
**Sources:** Summarizer (`/workspace/papers/leworldmodel-2603.19312-summary.md`) + Senior Research (`/workspace/leworldmodel-quiet/DELIVERABLE.md`) + Ablation Bot (`/workspace/papers/leworldmodel-2603.19312-ablations.md` — **AUTHORITATIVE**) + `paper_notes.md` / `env_outline/`  
**Sibling links:** `/workspace/ijepa-quiet/` (heavy JEPA lineage) · `/workspace/vjepa21-quiet/` (Meta dense/AC foil) · `/workspace/siglip-quiet/` · `/workspace/pi05-quiet/` · `/workspace/openpi-comet-smoke/` · `/workspace/diffusion-policy-quiet/`

---

## 1. One-liner

**Stable end-to-end JEPA world model from raw pixels** — next-embedding **MSE + SIGReg** only (~15M params, 1 GPU / few hours); drops EMA / stop-grad / frozen DINOv2 / PLDM’s 7-term VICReg. Compact [CLS] latent → CEM planning **0.98 s vs DINO-WM 47 s** (~**48×**); Push-T **96.0±2.83** vs PLDM **78.0±5.0** / DINO **92.0±1.63**; fixed-FLOP Push-T **90 vs 13**. **Sim-only** — not Meta V-JEPA 2-AC.

**Contrast poles (consistency triangle + control-WM branch):**

| Pole | What they do | Primary claim / use |
|------|--------------|---------------------|
| **DINOv2 / SigLIP** | Discriminative JEA / dense image features | VLA towers; **DINO-WM** freeze |
| **MAE / Dreamer / IRIS** | Generative pixels / reward-tied WMs | FT or RL imagination |
| **I-JEPA → V-JEPA → 2 → 2.1 → 2-AC** | Meta predictive SSL → AC planning | Real-robot CEM (Franka) — **parallel** line |
| **PLDM → LeWM** | Compact **E2E** JEPA-WM from pixels | Fast sim latent MPC; **this paper** |

**Do NOT invent / claim:** Franka / BEHAVIOR / DROID numbers (none in paper); LeWM as Meta V-JEPA descendant; BEHAVIOR hybrid JEPA+FM as in-paper result; course smoke = Push-T **96%** / **48×** exact; “zero heuristics” while ignoring predictor dropout **0.1** + AdaLN zero-init + BN projector (allowed stabilizers).

---

## 2. Summary (exec overview + key numbers)

**Maes & Le Lidec et al. (arXiv 2603.19312)** attack fragile JEPA training for **action-conditioned latent world models**: EMA target encoders, stop-grad, frozen foundation towers, or PLDM’s six+ VICReg-style loss weights. Fix = **two-term** objective \(L = L_{\mathrm{pred}} + \lambda\,\mathrm{SIGReg}(Z)\) (MSE next-embedding + Sketched-Isotropic-Gaussian Regularizer from **LeJEPA**). Architecture: ViT-Tiny encoder (~5M) + AdaLN transformer predictor (~10M) ≈ **15M**; joint E2E from RGB; image-goal **CEM+MPC** in compact latent.

**Why Accompanying Models session:** Completes the JEPA lecture arc after I-JEPA (idea) and V-JEPA 2.1 / 2-AC (scale + real robot): **how to train a tiny AC WM without brittle heuristics**, with a spectacular systems hook (**0.98 s vs 47 s**). Placement = **sibling / alternative training recipe** in the control-domain E2E JEPA-WM niche — **not** a descendant of V-JEPA 2-AC.

| Axis | Number / claim |
|------|----------------|
| Venue / code | arXiv **2603.19312v3**; `lucas-maes/le-wm` + le-wm.github.io **CLEAR** |
| Params / train | **~15M**; single GPU, few hours; **10** epochs typical; bs **128**; 224²; frame-skip **5** |
| Loss | \(L_{\mathrm{pred}}\) MSE + λ SIGReg; default **λ=0.1**, **M=1024**; tunable HPs **6→1** vs PLDM |
| Push-T SR (Tab. 5, 3 seeds) | LeWM **96.0±2.83**; DINO-WM **92.0±1.63**; PLDM **78.0±5.0** (~**+18 pp** vs PLDM) |
| Planning latency (Fig. 3) | LeWM **0.98 s** vs DINO-WM **47 s** (~**48×**); ~**200×** fewer tokens |
| Fixed-FLOP SR (Fig. 3) | Push-T **90 vs 13**; OGBench-Cube **74 vs 48** (LeWM vs DINO-WM) |
| Recon ablation (Tab. 7) | w/o **96.0±2.83** → with **86.0±7.54** (recon **hurts**) |
| λ band (Fig. 16) | SR **>80%** for λ∈**[0.01, 0.2]**; peak ~**0.09**; **sharp drop at λ=0.5** |
| Dropout (Tab. 9) | 0.0→**78**; **0.1→96.0**; 0.2→85.33; 0.5→66.67 |
| Predictor size (Tab. 6) | tiny **80.67**; **small 96.0**; base **86.7** |
| Encoder (Tab. 8) | ViT **96.0** ≈ ResNet-18 **94.0** |
| Solver (Tab. 10) | CEM **96**; Adam **84**; RMSProp **67**; SGD **26** (LeWM Push-T) |
| Two-Room caveat | PLDM/DINO **>** LeWM (~87 vs ~97–100) — SIGReg vs low-intrinsic-dim |
| Scope | **Sim-only** Push-T / Cube / TwoRoom / Reacher — **no Franka numbers** |

**Must-cite tattoo:** Push-T **96.0±2.83** · latency **0.98 vs 47** (~**48×**) · fixed-FLOP **90 vs 13** · recon **96→86** · λ∈**[0.01, 0.2]** / dies at **0.5** · **TwoRoom caveat** · **no Franka**.

**Do NOT claim:** real-robot transfer; course smoke = paper absolutes; LeWM replaces V-JEPA 2-AC; BEHAVIOR hybrid as demonstrated; “first E2E JEPA” without fencing vs PLDM definitional boundary.

---

## 3. Keywords

LeWorldModel; LeWM; JEPA; SIGReg; Sketched-Isotropic-Gaussian Regularizer; end-to-end world model; latent planning; CEM; MPC; representation collapse; PLDM; DINO-WM; Push-T; OGBench-Cube; Two-Room; Reacher; reward-free offline; physical probing; violation-of-expectation; LeJEPA; Accompanying Models; BEHAVIOR hybrid (TENTATIVE)

---

## 4. Deep dive — World-model / planning + taxonomy (**PRIMARY SESSION CONTRIBUTION**)

**Callout:** Scientific product of *this* PDF = a **stable, compact, action-conditioned JEPA world model from pixels** that makes latent CEM **sub-second**. Session type = Accompanying Models because students need the **training-stability + planning-latency** half of the JEPA→control story after Meta’s scale/Franka half.

### Taxonomy (Fig. 2 moral — three-way map)

| Family | Collapse / objective trick | Latent | Planning | Foil role |
|--------|---------------------------|--------|----------|-----------|
| **Foundation-freeze WM** (DINO-WM) | Freeze DINOv2 | Dense patch tokens (~**200×**) | CEM; **47 s** | Speed foil |
| **E2E multi-term JEPA-WM** (PLDM) | 7-term VICReg-style | Compact | CEM; speed ≈ LeWM | Fragility foil |
| **E2E two-term JEPA-WM** (**LeWM**) | **SIGReg** only | Compact [CLS] ~192-d | CEM; **0.98 s** | **This inventum** |
| **Generative / reward WMs** (Dreamer, TD-MPC, IRIS) | Pixel recon / reward | Pixels or states | RL imagination | Task-tied foil |
| **Meta SSL→AC** (V-JEPA 2 / 2.1 / 2-AC) | EMA+SG in SSL; AC post-train | Video / AC tokens | Franka CEM (**CLEAR** prior pack) | **Parallel** line — different domain |

### What is “world-model-ish” here — CLEAR (in-paper)

| Piece | In-paper role | World-model reading |
|-------|---------------|---------------------|
| Encoder \(z_t=\mathrm{enc}_\theta(o_t)\) | ViT-Tiny → [CLS] → MLP+BN | Pixels → compact state |
| Predictor \(\hat z_{t+1}=\mathrm{pred}_\phi(z_t,a_t)\) | 6L AdaLN TF; dropout 0.1 | Action-conditioned dynamics \(P(z'|z,a)\) |
| Loss \(L_{\mathrm{pred}}+\lambda\mathrm{SIGReg}\) | MSE next-emb + isotropic Gaussian | Dynamics + anti-collapse **without** EMA/SG |
| CEM+MPC (§3.2) | H=5; pop=300; cost \(\|\hat z_H-z_g\|_2^2\) | Image-goal latent control |
| Speed Fig. 3 | **0.98 s** vs **47 s**; fixed-FLOP **90 vs 13** | Deploy hook = **token compression** |
| Probes + VoE | Tab. 1 / Fig. 8 | Latent encodes physics; teleport surprise \(p<0.01\) |

### What is **not** overclaimable

- **Sim-only** continuous control — Push-T / OGBench-Cube / Two-Room / Reacher. **No Franka / BEHAVIOR / DROID / Bridge numbers.**  
- Two-Room: LeWM **underperforms** PLDM/DINO — SIGReg Gaussian prior vs low-complexity data (admitted).  
- Cube: DINO-WM slightly ahead (~86 vs ~74).  
- Speedup is largely **cheaper dynamics steps** (compact \(z\)), not fewer CEM iters — report solver-matched.  
- Dropout **0.1** is an allowed regularizer (78→96), not “zero heuristics.”  
- Hybrid LeWM imagination + π₀.₅/FM for BEHAVIOR = **Visionary TENTATIVE**, **not** in-paper.  
- LeWM is **not** a Meta V-JEPA descendant — sibling **control E2E** recipe (**PLDM→LeWM**).

### SIBLING recipe (session tattoo — do not collapse lines)

```
Meta SSL JEPA:     I-JEPA → V-JEPA → V-JEPA 2 → 2.1 → 2-AC (Franka CEM)
                   [packs: ijepa-quiet / vjepa21-quiet]

Control E2E JEPA-WM:  PLDM (fragile multi-term VICReg) → **LeWM (MSE+SIGReg)**  ← THIS PAPER

Foundation freeze:    DINO-WM (DINOv2 frozen + predictor; slow tokens)
```

**World-model exam bite:** "LeWM = compact ~15M E2E JEPA WM from pixels: MSE+SIGReg, no EMA/SG/freeze/multi-VICReg; Push-T **96.0±2.83**; plan **0.98 vs 47 s** (~48×); fixed-FLOP **90 vs 13**; recon hurts **96→86**; λ∈[0.01,0.2] / dies at 0.5; **TwoRoom caveat**; **sim-only — no Franka**. Sibling **PLDM→LeWM**, not Meta 2-AC child."

---

## 5. Deep dive — SIGReg / heuristics-off recipe (Empiricist spine methods)

**Callout:** App. G + Figs. 15–16 / Tabs. 5–10 are the session’s methods spine. Fragile multi-term / EMA JEPA → two-term SIGReg → stable curves + one HP + fast latent CEM.

### Architecture checklist (Empiricist pin)

| Piece | Spec (paper default) |
|-------|----------------------|
| Encoder | ViT-Tiny /14, 12L, 3H, dim **192** (~5M); alt ResNet-18 |
| Projector (enc) | 1-layer MLP + **BatchNorm** (final LN blocks SIGReg) |
| Predictor | Transformer ~ViT-S, **6L**, **16H**, dropout **0.1** (~10M); AdaLN actions (zero-init) |
| Projector (pred) | Twin of enc projector |
| History | N=**3** (PushT/Cube); N=**1** (TwoRoom); causal AR |
| Losses | \(L_{\mathrm{pred}}=\|\hat z_{t+1}-z_{t+1}\|_2^2\); SIGReg = M random projections + Epps–Pulley |
| Defaults | **M=1024**, **λ=0.1**; batch **128**; sub-traj **4**; frame-skip **5**; 224² |
| Planner | CEM 300 samples; **30** iters PushT / **10** else; elite 30; H=**5** (=25 env steps) |
| Total | **~15M**; 10 epochs; reward-free offline \(o,a\) only |
| **Forbidden in recipe** | EMA target, stop-grad, frozen pretrained enc, multi-term VICReg, pixel recon (unless ablating) |

### Ablation Bot AUTHORITATIVE — must-hit deltas

| Ablation | Paper locus | Headline numbers |
|----------|-------------|------------------|
| Push-T multi-seed | Tab. 5 | LeWM **96.0±2.83**; PLDM **78.0±5.0**; DINO **92.0±1.63** |
| Planning speed / FLOPs | Fig. 3 | **0.98 s vs 47 s** (~**48×**); fixed-FLOP Push-T **90 vs 13**; Cube **74 vs 48** |
| λ SIGReg | Fig. 16 | **>80%** for λ∈**[0.01, 0.2]**; peak ~**0.09**; dies at **0.5** |
| M / knots | Fig. 15 | Largely **insensitive** — λ is the real HP |
| Emb dim | Fig. 15 | Drop below ~**184**; saturate above |
| Decoder / recon | Tab. 7 | w/o **96.0** vs with **86.0±7.54** |
| Predictor dropout | Tab. 9 | 0→**78**; **0.1→96**; 0.2→85; 0.5→67 |
| Predictor size | Tab. 6 | tiny **80.67**; **small 96.0**; base **86.7** |
| Encoder arch | Tab. 8 | ViT **96.0** ≈ RN18 **94.0** |
| Solver | Tab. 10 | CEM **96** / Adam **84** / RMSProp **67** / SGD **26** |
| Two-Room / Cube | Fig. 6 | TwoRoom LeWM behind; Cube DINO slightly ahead |
| **Not factorial** | §3.1 | EMA/SG/freeze claimed removed — **no** within-LeWM add-back table |

**Taxonomy / recipe exam bite:** "LeWM drops EMA/SG/freeze/multi-VICReg for MSE+SIGReg (λ-only); Push-T **96±2.83**; recon hurts **96→86**; λ band [0.01,0.2] / cliff at 0.5; dropout 0.1 allowed; CEM ≫ SGD; **sim-only**."

---

## 6. Role-ready sections (all five — Empiricist RICHEST)

### 6.1 Empiricist — WEIGHT 3 LONGEST PRIMARY (Nykolas Rekasius)

> **Seat charge:** Deepen **reproducible claims**, **planning speed/stability**, and the **heuristics-off training recipe**. Longest talk block (**~16–18** slides). Course smokes that stress **SIGReg E2E · λ cliff · planning wall-time · lite baselines**, ranked by GPU honesty. Prefer PushT-lite / synthetic + ViT-Tiny — **not** full 4-env 10-epoch / foundation DINO bake-off. **Kanta not seated** — Nykolas carries Empiricist.

#### Claim under measurement

Matched short pretrain: **SIGReg-only E2E** trains without collapse while **no-reg** collapses; mid-λ planning ≫ λ∈{0, 0.5}; compact latent CEM ≪ high-token DINO stub wall-time; lewm-smoke ≥ pldm_lite on PushT-proxy — **directional**, not paper **96% / 48×**.

#### Hypotheses (falsifiable — from DELIVERABLE)

1. **H-stable (#1):** SIGReg-E2E (no SG/EMA/freeze) trains without collapse; no-reg collapses (latent std/rank).  
2. **H-fragile (#1):** EMA+SG unnecessary under SIGReg; VICReg-lite shows noisier multi-loss / higher variance (Tab. 5 moral).  
3. **H-lambda (#2):** Planning proxy high for λ∈{0.01, 0.1}; crashes/dynamics-hurt at 0.5; λ=0 fails (Fig. 16).  
4. **H-speed (#3):** Compact CLS latent plans ≫faster wall-clock than high-token DINO stub at matched CEM; at fixed wall-time, compact ≥ stub proxy SR.  
5. **H-rank (#4):** lewm-smoke ≥ pldm_lite; competitive with dino_wm_lite — **not** require 96/78/92.  
6. **H-decode (#5):** Pixel recon aux hurts or flats planning proxy (Tab. 7 96→86).  
7. **H-drop (#6):** Dropout p=0.1 > 0 and ≫ 0.5 (Tab. 9).  
8. **H-pred-size (#7):** ViT-S ≥ ViT-T; ViT-B not required (Tab. 6).  
9. **H-phys (#12):** Probes recover pose; VoE teleport surprise > color (Fig. 8).  
10. **H-lineage:** Logs must mark LeWM = SIGReg E2E (no EMA target) vs I-JEPA/V-JEPA EMA lineage + AC MPC.

**Falsifiers:** no-reg does not collapse; λ=0.5 ≥ λ=0.1; DINO-stub faster at matched tokens; recon raises SR; dropout 0 ≥ 0.1; smoke claimed as Push-T 96% / 48× / Franka.

#### Prioritized ablation matrix (Ablation Bot AUTHORITATIVE + DELIVERABLE)

| # | Experiment | Paper hook | Smoke | Pri |
|---|------------|------------|-------|-----|
| **RC1** | Heuristics off: SIGReg E2E vs noreg / ema_sg / vicreg_lite | §3; Fig. 18–19; Tab. 5 | **Y — PRIMARY** | **P0** |
| **RC2** | λ sweep `{0, 0.01, 0.1, 0.5}` (+ optional M) | Fig. 16 / 15 | **Y — PRIMARY** | **P0** |
| **RC3** | Planning speed / fixed wall-time vs DINO stub | Fig. 3 | **Y — PRIMARY** | **P0** |
| **RC4** | vs PLDM-lite / DINO-WM-lite on PushT-lite | Fig. 6 / Tab. 5 | **Y / Partial** | **P0** |
| **RC5** | Decoder / recon on vs off | Tab. 7 | **Y** | **P1** |
| **RC6a** | Predictor dropout `{0, 0.1, 0.5}` | Tab. 9 | **Y** | **P1** |
| **RC6b** | Predictor tiny vs small | Tab. 6 | Partial | P2 |
| **RC2b** | SIGReg M `{64, 1024}` | Fig. 15 | **Y** cheap | P1 |
| **RC3b** | Solver CEM vs Adam | Tab. 10 | Partial | P1 |
| **RC7** | Physical probe + VoE stub | Tab. 1 / Fig. 8 | Partial | P2 |
| — | Full 4-env 10-ep; foundation DINO; exact Tab. 5 3-seed; TwoRoom paper protocol | — | **N** | Skip |

**Min package:** **RC1 + RC2 + RC3 + RC4** → RC5 → RC6a → optional RC2b/RC3b/RC7.

#### Exact reproduce stack (EDIT)

```bash
# Official
git clone https://github.com/lucas-maes/le-wm
# Pin commit → logs/pins.txt ; site https://le-wm.github.io

# Course scout env (quiet outlines — preferred)
export LEWM_WORK_ROOT=$SCRATCH/lewm-smoke
cd /workspace/leworldmodel-quiet
bash env_outline/setup_mamba.sh
# mamba activate lewm-scout
mkdir -p logs runs checkpoints configs
# EDIT partition/account when fleshing slurm_*.sh from DELIVERABLE §3 cards
# Outlines only — do NOT sbatch from quiet box
# NEVER claim paper Push-T 96% / 0.98s / 48× / fixed-FLOP 90 from smoke
# NEVER invent Franka / BEHAVIOR numbers
```

#### Run cards (from DELIVERABLE — Empiricist PRIMARY)

##### RC1 — `heuristics_off_smoke` (**#1 — PRIMARY**)

| Field | Spec |
|-------|------|
| Paper | No SG/EMA/pretrained; \(L=L_{\mathrm{pred}}+\lambda\mathrm{SIGReg}\); vs PLDM 7-term / JEPA EMA+SG / DINO freeze |
| Goal | SIGReg-E2E trains + plans without collapse; noreg collapses |
| Train | ViT-Tiny enc + ViT-S pred (or smaller); 1 seed; 2k–15k steps on tiny shard; λ=0.1 |
| Axes | `recipe ∈ {sigreg_e2e, noreg, ema_sg_jepa, vicreg_lite}` |
| Metrics | \(L_{\mathrm{pred}}\), SIGReg; latent std / PCA rank; open-loop MSE; optional CEM proxy SR |
| Pass | ≥2 recipes finish; sigreg non-collapse; noreg collapse or worse proxy — **not** paper 96% |
| Budget | **~4–12 GPU-h** · VRAM **10–20 GB** |
| Script | `env_outline/slurm_heuristics_off_smoke.sh` (named in DELIVERABLE; flesh from RC1 card) |

##### RC2 — `lambda_sweep` (**#2 — PRIMARY**)

| Field | Spec |
|-------|------|
| Paper | Fig. 16; λ∈[0.01,0.2] SR>80%; peak ~0.09; λ=0.5 hurts |
| Grid | `λ ∈ {0.0, 0.01, 0.1, 0.5}`; optional `M ∈ {64, 1024}` at λ=0.1 |
| Pass | ≥3 λ points; mid > extremes on planning proxy or latent health+rollout MSE |
| Budget | **~3–10 GPU-h** |
| Script | `env_outline/slurm_lambda_sweep.sh` |

##### RC3 — `planning_speed_bench` (**#3 — PRIMARY**)

| Field | Spec |
|-------|------|
| Paper | Fig. 3: **47 s → 0.98 s**; ~48×; fixed-FLOP **90 vs 13** |
| Goal | encode+CEM wall-time + SR vs high-token DINO stub under same CEM `(pop=300, iters≤10–30, H=5)` |
| Pass | Timing table; speedup ratio ≫1 vs DINO-stub; deterministic seed |
| Budget | **≤1–4 GPU-h** (infer) |
| Script | `env_outline/slurm_planning_speed_bench.sh` |

##### RC4 — `vs_jepa_wm_baselines` (**#4 — PRIMARY**)

| Field | Spec |
|-------|------|
| Paper | Tab. 5 / Fig. 6: **96 > 92 > 78**; +18 pp vs PLDM; TwoRoom caveat |
| Train | Short matched `{lewm_smoke, pldm_lite, dino_wm_lite}`; pixels-only |
| Pass | lewm ≥ pldm_lite on proxy; competitive with dino_lite; 1-seed variance note |
| Budget | **~6–16 GPU-h** |
| Script | `env_outline/slurm_vs_jepa_wm_baselines.sh` |

##### RC5 / RC6 / RC7 — follow-ons

RC5 decoder on/off (Tab. 7 moral); RC6a dropout `{0,0.1,0.5}`; RC6b tiny vs small; RC7 VoE/probe stub — after P0 green.

#### Numbers to memorize (exam)

| Fact | Value |
|------|-------|
| Push-T Tab. 5 | LeWM **96.0±2.83** · DINO **92.0±1.63** · PLDM **78.0±5.0** |
| Latency Fig. 3 | **0.98 s vs 47 s** (~**48×**) |
| Fixed-FLOP Push-T / Cube | **90 vs 13** · **74 vs 48** |
| Recon Tab. 7 | **96.0 → 86.0** |
| λ Fig. 16 | **[0.01, 0.2]** >80%; peak ~**0.09**; cliff at **0.5** |
| Dropout Tab. 9 | **0→78 / 0.1→96 / 0.5→67** |
| Pred size Tab. 6 | tiny **80.67** / small **96.0** / base **86.7** |
| Encoder Tab. 8 | ViT **96** / RN18 **94** |
| Solver Tab. 10 | CEM **96** / Adam **84** / SGD **26** |
| Params / HP | **~15M** · HPs **6→1** · O(log n) vs O(n⁶) |
| Scope | **Sim-only** · **no Franka** · TwoRoom caveat |

#### Failure modes cheat-sheet

| Symptom | Likely cause | Next action |
|---------|--------------|-------------|
| All recipes collapse | λ too high / projector BN missing | λ=0.1; add BN after LN |
| noreg does not collapse | Accidental SIGReg left on | Audit loss; log recipe flag |
| λ sweep flat | Steps too short / weak eval | Longer train; use CEM proxy not only \(L_{\mathrm{pred}}\) |
| No speedup vs DINO | Token counts matched accidentally | Log `#tokens`; patch tokens for DINO stub |
| PLDM-lite ≈ LeWM | VICReg-lite not multi-term | Port ≥3–7 terms or label “not PLDM” |
| CEM SR noise | Goals unreachable / H too short | Match H=5 / skip=5; filter reachable |
| OOM at 224² bs128 | Smoke too close to paper | 128², bs32–64, ViT-T |
| “Heuristics off” but EMA in code | Copied I-JEPA trainer | Delete target EMA; single encoder |
| Claimed 96% / 48× / Franka from smoke | Overclaim | Report directional only |

#### What not to invent / claim

- **Do not** claim course smoke = Push-T **96%** / latency **0.98 s** / **48×** / fixed-FLOP **90**.  
- **Do not** invent **Franka / BEHAVIOR / DROID** numbers.  
- **Do not** launch full 4-env 10-ep / foundation DINO bake-off from course queue.  
- **Do not** call dropout “zero heuristics.”  
- **Do not** present LeWM as Meta V-JEPA 2-AC replacement.

**Empiricist exam bite:** "Maes’26 LeWM: MSE+SIGReg E2E (~15M); Push-T **96.0±2.83**; **0.98 vs 47 s**; fixed-FLOP **90 vs 13**; recon **96→86**; λ∈[0.01,0.2]/cliff 0.5; Empiricist RC1+RC2+RC3+RC4 on PushT-lite for **direction** — never claim 96%/48×/Franka from smoke."

---

### 6.2 Archaeologist — produced despite empty seat

> **Seat charge:** Place LeWM on the JEPA map without rewriting I-JEPA / V-JEPA 2.1 CLEAR labels. Emphasize **SIBLING** control recipe (**PLDM→LeWM**) vs Meta (**I-JEPA→…→2-AC**). Fence **sim-only**.

#### Priors — CLEAR (cited / direct)

| Prior | Role relative to LeWM | Label |
|-------|----------------------|-------|
| **LeCun 2022 AMI** | Names JEPA | **CLEAR** |
| **I-JEPA** Assran’23 | Emb prediction; EMA/SG | **CLEAR** (SSL parent idea) |
| **V-JEPA / V-JEPA 2** | Video JEPA; scale; planning mention | **CLEAR** (cited; different goal) |
| **V-JEPA 2.1 / 2-AC** | Dense unlock; Franka CEM | **CLEAR** prior packs; **not cited** here; **parallel** |
| **PLDM** Sobal’25 | E2E JEPA-WM multi-term VICReg | **CLEAR** closest foil |
| **DINO-WM** Zhou’25 | Frozen DINOv2 WM + CEM | **CLEAR** foundation foil |
| **LeJEPA / SIGReg** (2511.08544) | Provable Gaussian anti-collapse | **CLEAR** inventum substrate |
| **VICReg / Dreamer / TD-MPC / IRIS** | Reg / generative foils | **CLEAR** |
| **CEM / MPC / Push-T / OGBench** | Planner + envs | **CLEAR** |

#### This paper — CLEAR inventum

1. Claimed **stable E2E JEPA from raw pixels** with **two-term** objective (MSE + SIGReg).  
2. Removes EMA, stop-grad, frozen encoder, PLDM’s six+ weights → **λ-only** tuning.  
3. Compact **15M** AC WM; **~48×** planning vs DINO-WM; Push-T **96.0±2.83**.  
4. Physics suite: probing + VoE + emergent temporal straightening.  
5. Open code `lucas-maes/le-wm`.

#### Subsequent — CLEAR / TENTATIVE

| Work | Relation | Label |
|------|----------|-------|
| `lucas-maes/le-wm` + le-wm.github.io | Repro train/plan | **CLEAR** |
| Meta I-JEPA→…→2-AC chain | Remains CLEAR robotics path (**do not demote**) | **CLEAR** parallel |
| BEHAVIOR / real Franka with LeWM | **Not shown** | **TENTATIVE** |
| SIGReg E2E + V-JEPA 2.1 dense features | Open merge | **TENTATIVE** |
| π₀ / OpenVLA / GR00T | No LeWM cites; SigLIP/DINO towers | **Architecture rhyme** only |
| Hybrid LeWM+FM | Research proposal | **TENTATIVE** — **not in-paper** |

#### Genealogy (CLEAR framing)

```
Predictive coding / LeCun AMI / JEPA abstract
    → I-JEPA (Assran 2023) — image multi-block latent prediction
         [pack: /workspace/ijepa-quiet/]
        → V-JEPA → V-JEPA 2 → 2.1 → 2-AC (Franka CEM)
             [pack: /workspace/vjepa21-quiet/]   ← PARALLEL Meta line

Control-domain E2E JEPA-WM:
    PLDM (Sobal 2025) — multi-term VICReg E2E from pixels
        → **LeWM (this, 2026)** — MSE + SIGReg; compact latent CEM
            → optional 2026 BEHAVIOR local WM brick (Visionary TENTATIVE)
```

**Archaeologist exam bite:** "LeWM (Maes’26) = sibling control E2E JEPA-WM (**PLDM→LeWM**), **not** Meta V-JEPA child; inventum = SIGReg two-term stability + **0.98 vs 47 s** planning; Push-T **96±2.83**; **sim-only / no Franka**; keep Meta 2-AC CLEAR for real robot."

---

### 6.3 Stakeholder — produced despite empty seat

**Buy / why schedule this for “Accompanying Models”:**
- Completes JEPA arc: I-JEPA (idea) → V-JEPA 2.1/2-AC (scale + real robot) → **LeWM (tiny AC WM without brittle heuristics)**.  
- Answers student pain: “JEPA collapses / needs EMA / six loss knobs.”  
- Systems hook: **0.98 s vs 47 s**; **15M** on one GPU — syllabus-friendly vs Meta-scale.  
- Clean three-way map (Fig. 2): E2E vs foundation-freeze vs reward/recon WMs.  
- Open code + teachable Alg. 3.

**Risks / costs / limits:**
- **Sim-only** — no Franka/BEHAVIOR; do **not** replace V-JEPA 2-AC robotics lecture.  
- Loses on Two-Room; slightly trails DINO on Cube — honesty required.  
- Short horizon (H=5 / 25 steps); hierarchical WM admitted as future.  
- Needs action-labeled offline coverage; low diversity hurts SIGReg.  
- Empiricist seat weighted — expect deep number grilling; **Kanta not seated**.

**Decision ask:** **Required companion** after I-JEPA + V-JEPA 2-AC for “training stability & planning latency”; pair with PLDM/DINO-WM as foils. Optional if track only covers Meta SSL→Franka.

**Stakeholder exam bite:** "Buy LeWM as the **teachable stable small-WM recipe** + sub-second CEM story; don’t buy it as a Franka/BEHAVIOR result or as a silent V-JEPA 2-AC replacement."

---

### 6.4 Scientific Reviewer — produced despite empty seat

**Claims under review**
1. Two-term MSE+SIGReg yields **stable E2E JEPA from pixels** without EMA/SG/freeze/multi-term VICReg.  
2. Tunable loss HPs collapse to **λ**; robust band enables bisection.  
3. Compact latent enables **~48×** faster planning than foundation WMs at competitive / better control.  
4. Latents encode physical structure (probes + VoE).

**Strengths**
1. Clean inventum: collapse problem → two-term objective → measurable stability + speed.  
2. Fair foils: E2E (PLDM), foundation (DINO-WM), GC-RL/BC.  
3. Ablations isolate thesis (recon hurts; λ band; M inert; dropout 0.1).  
4. Physics suite beyond reward hacks; candid limitations (TwoRoom, short H, action labels).  
5. Tab. 5 3-seed variance; Ablation Bot corpus rich.

**Weaknesses / ask-for-revision**
1. “First stable E2E JEPA” depends on definitional boundaries vs PLDM/LeJEPA.  
2. Fig. 6 other-env numbers chart-heavy — want full numeric table.  
3. CEM 300×30 dominates wall-clock; matched-iteration budgets beyond Fig. 3 needed.  
4. Two-Room failure needs a fix (adaptive λ/dim), not only diagnosis.  
5. **No real-robot** transfer — reviewers will ask for Franka/Bridge smoke.  
6. Dropout is still a heuristic; BN projector / AdaLN zero-init unablated.  
7. EMA/SG removal claimed but **not** factorial within LeWM.

**Verdict:** Strong methods + systems paper for the **small E2E control-WM** niche. Accept for Accompanying Models as **required companion** after I-JEPA/V-JEPA 2-AC; when citing robots, demand **V-JEPA 2-AC**, not LeWM; when citing stable E2E recipe, cite **LeWM / SIGReg**.

**Reviewer exam bite:** Demand Tab. 5 + Fig. 3 + Fig. 16 + Tab. 7 before slogan; cite Push-T **96.0±2.83** not bare “+18%”; punish Franka-as-result and BEHAVIOR-hybrid-as-result; fence TwoRoom caveat.

---

### 6.5 Visionary — produced despite empty seat (BEHAVIOR recipe; **no Discord/Drive**)

**BEHAVIOR connection:**  
2025/’26 BEHAVIOR winners ride **π₀.₅ + flow-matching + SigLIP/PaliGemma** — generative motor policies. LeWM shares the **planning interface** V-JEPA 2-AC popularized (optimize actions so imagined latents match a **goal image**) and adds **training stability + real-time latent MPC** on one GPU — attractive as a **local world-model module** beside a VLA, **not** the whole stack. **Do not invent** LeWM robot/BEHAVIOR scores — paper is Push-T / Cube / TwoRoom / Reacher only.

**Proposed hybrid (TENTATIVE — do NOT invent as existing / in-paper result):** π₀.₅/FM executes short-horizon motor chunks; **LeWM-style latent CEM** scores long-horizon feasibility / image-goal subgoals / VoE surprise as anomaly. Alt: V-JEPA 2.1 dense state → **LeWM-sized** SIGReg predictor for fast MPC.

| BEHAVIOR need | LeWM-shaped response |
|---------------|----------------------|
| Visual trunk | Default stays **SigLIP-class**; LeWM = **local WM / planner** ablation |
| Real-time MPC Hz | Compact [CLS] CEM (cite **0.98 vs 47 s** carefully — sim) |
| Training stability | E2E MSE+SIGReg; refuse EMA/SG stacks unless ablated necessary |
| Goal-image planning | Same CEM interface as 2-AC / DINO-WM |
| Language instructions | Keep SigLIP parallel — LeWM unlabeled |
| Low-diversity subtasks | Watch TwoRoom moral — adapt λ / prior |
| Eval honesty | Citing Push-T **96%** ≠ BEHAVIOR q-score |

**CLEAR:** Stable 2-term E2E JEPA + compact latent CEM is a viable small-WM path.  
**TENTATIVE:** Official BEHAVIOR’26 baseline ships LeWM brick; hybrid LeWM-rollout + FM wins board.

#### 2026 BEHAVIOR world-model / planning recipe (8 bets)

1. **Default WM recipe:** E2E JEPA + **single anti-collapse** (SIGReg or successor) — refuse EMA/SG unless RC1 says necessary.  
2. **λ card:** publish λ (or bisection log) like latency cards — one-number honesty.  
3. **Plan in compact latent:** prefer CLS/pooled for MPC frequency; keep dense features (V-JEPA 2.1) for perception probes.  
4. **Solver card:** CEM vs first-order (Tab. 10) must be reported.  
5. **Env complexity gate:** SIGReg Gaussian priors can hurt low-diversity household sub-tasks (TwoRoom).  
6. **Physics unit tests:** VoE teleport/color + pose probes as CI before rollouts.  
7. **Compose stacks:** LeWM imagination × π₀.₅/OpenPI/DP executors; tag `wm=lewm_sigreg`, `planner=cem_h5`, `pixels_only=true`.  
8. **Honest non-goals:** course smoke ≠ 4-env paper SR; BEHAVIOR needs scene diversity beyond PushT — this pack = **WM stability + plan speed**, not demos.

#### Follow-up research
1. SIGReg-E2E on BEHAVIOR-1K / OXE play — planning Hz vs DINO-WM.  
2. Image-goal CEM in BEHAVIOR scenes vs language-goal VLA.  
3. VoE surprise as **safety monitor** for contact-rich skills.  
4. Hierarchical LeWM for multi-room tasks (paper limitation).  
5. Distill V-JEPA 2.1 → 15M LeWM encoder for edge robots.  
6. Adaptive SIGReg for low-intrinsic-dim regimes (TwoRoom fix).

#### New applications (**beyond Discord/Drive**)
- Real-time latent MPC for tabletop rearrange with image goals.  
- Offline “what-if” policy evaluation inside compact JEPA imagination.  
- Physics-violation alarms for teleop assist.  
- Teaching artifact: smallest complete JEPA-WM you can train overnight.

**Visionary one-liner:** LeWM is the **fast, stable, teachable JEPA world-model brick** — pair with V-JEPA 2-AC (real robot) and π₀.₅ (motor skill); **hybrid is proposal, not result**.

**Visionary exam bite:** "For BEHAVIOR, default **SigLIP + FM**; ablate **LeWM-style** local SIGReg WM via Empiricist RC1–RC3; cite Push-T / **0.98 vs 47** as sim morals only; **do not invent** LeWM+FM hybrid as in-paper — **no Discord/Drive**."

*(No Discord / Drive checklist content.)*

---

## 7. Empiricist run pack (env / scripts / GPU — from DELIVERABLE)

### Env setup

```bash
export LEWM_WORK_ROOT=$SCRATCH/lewm-smoke
cd /workspace/leworldmodel-quiet
bash env_outline/setup_mamba.sh
# mamba activate lewm-scout
mkdir -p logs runs checkpoints configs
# EDIT: partition/account when fleshing slurm_*.sh
# Pin lucas-maes/le-wm commit → logs/pins.txt
# Outlines only — do NOT sbatch from quiet box
```

| File | Role |
|------|------|
| `env_outline/conda_env.yaml` | `lewm-scout` sketch |
| `env_outline/setup_mamba.sh` | Create env + dirs |
| `env_outline/run_checklist.md` | Ordered RC0→RC7 checklist |
| `env_outline/README.md` | Index + honesty; siblings |
| `env_outline/slurm_*.sh` | Named in DELIVERABLE §3–4 / README (RC1–RC7 cards) — flesh from run cards before cluster |

### GPU / RAM / TIME budget honesty

Assumptions: ViT-Tiny/S smoke, image ≤224 (prefer 128), batch 32–128, PushT-lite / synthetic — **not** full 4-env 10-epoch paper protocol.

| Workload | GPU VRAM (peak) | Host RAM | Wall time (order) | Smoke |
|----------|-----------------|----------|-------------------|-------|
| RC0 ingest | — | 8–16 GB | **<0.5 h CPU** | **Y** |
| RC1 heuristics ×2–4 | **10–20 GB** | 32–64 GB | **4–12 h** | **Y** |
| RC2 λ ×4 | **10–20 GB** | 32 GB | **3–10 h** | **Y** |
| RC3 planning speed (infer) | ckpt | 16–32 GB | **≤1–4 h** | **Y** |
| RC4 baselines ×3 | **12–24 GB** | 64 GB | **6–16 h** | **Y / Partial** |
| RC5 decoder ×2 | **10–20 GB** | 32 GB | **2–6 h** | **Y** |
| RC6 dropout/size | **10–20 GB** | 32 GB | **2–12 h** | Partial |
| RC7 VoE/probe | **4–12 GB** | 16–32 GB | **1–4 h** | Partial |
| Paper ~15M, 1 GPU, few hours, 10 ep ×4 envs | ~12–24 GB | — | Paper claim — **still larger than course matrix** | **N** (cite) |
| Full DINO-WM foundation + Tab. 5 3-seed | multi-day | — | **Out of course** | **N** |

**Honesty rule:** log `(arch,recipe,lambda,M,dropout,frames,res,steps,BS,#tokens,ms_encode,ms_cem,proxy_sr,wall_s,mem)`. Cut **resolution and batch** before lying about budgets. Never amortize paper **96% / 0.98 s / 48× / 90** onto a Partial stub. **No Franka**.

**Course envelope:** target **≤ ~40–60 GPU-h** for P0 matrix (RC0–RC4 + RC5). Full App. G × 3 seeds × 4 envs = **well above** course.

### Success criteria vs paper claims

| Paper claim | Minimal course criterion | Not required |
|-------------|--------------------------|--------------|
| Stable E2E JEPA; no SG/EMA/pretrained | RC1: sigreg trains; noreg collapses | Fig. 18 exact curves |
| Tunable HPs 6→1 (λ) | RC2: mid-λ best | Exact bisection demo |
| Plans ~48× faster than foundation WM | RC3: compact ≪ DINO-stub wall-time | Exact **0.98 / 48×** |
| Push-T +18% vs PLDM; competitive vs DINO | RC4: lewm ≥ pldm_lite; ~dino | **96 / 78 / 92** absolutes |
| No decoder better (Tab. 7) | RC5 directional | Exact 96 vs 86 |
| Dropout 0.1 sweet spot | RC6a | Full `{0,0.1,0.2,0.5}` |
| Physical probes + VoE | RC7 stubs | Tab. 1 / Fig. 8 significance |
| TwoRoom / Cube / Reacher full | Cite + optional 1 lite env | Full Fig. 6 4-env |
| Franka / BEHAVIOR | **Cite absence** | Inventing numbers |

**Pass:** Empiricist executes **RC1 + RC2 + RC3** (+ **RC4** if GPU) with paper-aligned **directional** rankings + GPU honesty logged.  
**Fail / overclaim:** "reproduced Maes’26 / matched 96% / verified 48× / Franka / BEHAVIOR win" from smoke alone.

---

## 8. Slide Maker brief

| Role | Weight / timing guide | Focus |
|------|----------------------|-------|
| **Empiricist** | **3 · LONGEST · ~16–18** | H-stable/λ/speed/rank; RC1–RC4; GPU honesty; must-cite tattoo; exam bite |
| Archaeologist | empty seat · ~12–14 | SIBLING PLDM→LeWM vs Meta I-JEPA→2-AC; sim-only fence |
| Stakeholder | empty seat · ~12–14 | Buy stability+latency module; risks (sim-only, TwoRoom); decision ask |
| Reviewer | empty seat · ~12–14 | Tab.5+Fig.3+Fig.16+Tab.7; verdict Accept companion; no Franka-as-LeWM |
| Visionary | empty seat · ~12–14 | BEHAVIOR SigLIP+FM default + LeWM local WM ablation; hybrid=TENTATIVE; **no Discord/Drive** |

**Spine (paste into Slide Maker) — 5 bullets:**
1. **Stable ~15M E2E JEPA WM from pixels** — MSE + **SIGReg** only; drops EMA / stop-grad / frozen / multi-VICReg.  
2. **Must-cite:** Push-T **96.0±2.83**; latency **0.98 vs 47** (~**48×**); fixed-FLOP **90 vs 13**; recon **96→86**; λ∈**[0.01, 0.2]** / cliff **0.5**; **TwoRoom caveat**; **no Franka**.  
3. **Taxonomy:** DINO-WM (freeze) \| Dreamer/IRIS (recon/reward) \| Meta **I-JEPA→…→2-AC** (parallel) \| **PLDM→LeWM** (this).  
4. **Lineage (Empiricist=3 Nykolas LONGEST):** sibling control recipe **PLDM→LeWM**, **not** Meta V-JEPA descendant; inventum = heuristics-off stability + compact latent CEM speed.  
5. **BEHAVIOR’26 + honesty:** default SigLIP+FM; LeWM = local WM ablation; Empiricist RC1–RC4 directional only; **do not invent** LeWM+FM hybrid as in-paper — **no Discord/Drive**.

**GitHub title sketch:** `[ECE 605] - LeWorldModel Empiricist Making Three`

Full Slide Maker brief: `/workspace/leworldmodel-quiet/SLIDE_BRIEF.md`

---

## Paths

| Artifact | Path |
|----------|------|
| This pack | `/workspace/leworldmodel-quiet/KANTA_PACK.md` |
| PDF | `/workspace/leworldmodel-quiet/KANTA_PACK.pdf` |
| Slide brief | `/workspace/leworldmodel-quiet/SLIDE_BRIEF.md` |
| Senior Research | `/workspace/leworldmodel-quiet/DELIVERABLE.md` |
| Summarizer | `/workspace/papers/leworldmodel-2603.19312-summary.md` |
| Ablations | `/workspace/papers/leworldmodel-2603.19312-ablations.md` (**AUTHORITATIVE**) |
| Paper notes | `/workspace/leworldmodel-quiet/paper_notes.md` |
| Env outlines | `/workspace/leworldmodel-quiet/env_outline/` |
| Paper PDF | `/workspace/papers/leworldmodel-2603.19312.pdf` |
| Page figs | `/workspace/papers/leworldmodel-figs/page-*.png` |
| Lineage siblings | `/workspace/ijepa-quiet/` · `/workspace/vjepa21-quiet/` |
| Code / site | https://github.com/lucas-maes/le-wm · https://le-wm.github.io |

---

*Pack synthesized 2026-09-29 PT (HST) for KanBot / ECE 605 — Dec 2 Accompanying Models · Empiricist=3 (Nykolas) LONGEST · Kanta NOT seated · all five produced · Ablation Bot AUTHORITATIVE · sim-only / no Franka · SIBLING PLDM→LeWM ≠ Meta 2-AC · no Discord/Drive · no invented BEHAVIOR hybrid as in-paper.*
