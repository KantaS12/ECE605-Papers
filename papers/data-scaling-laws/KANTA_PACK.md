# Paper Manager pack — Data Scaling Laws (Nov 23 · Data)

**Paper:** Data Scaling Laws in Imitation Learning for Robotic Manipulation  
**arXiv:** https://arxiv.org/abs/2410.18647 · [project](https://data-scaling-laws.github.io/) · [code](https://github.com/Fanqi-Lin/Data-Scaling-Laws) · [HF data](https://huggingface.co/datasets/Fanqi-Lin/Processed-Task-Dataset) · **ICLR 2025 Oral**  
**Authors:** Fanqi Lin\*, Yingdong Hu\*, Pingyue Sheng, Chuan Wen, Jiacheng You, Yang Gao (\*equal) — Tsinghua / Shanghai Qi Zhi Institute / Shanghai AI Lab  
**PDF:** `/workspace/papers/data-scaling-laws-2410.18647.pdf` (= `/workspace/data-scaling-laws-quiet/paper.pdf`) · 34 pp · arXiv **2410.18647v4**  
**Session:** Nov 23 · due 9:30 AM · Type: **Data**  
**Role weights:** **Empiricist=3 (Nykolas Rekasius) LONGEST PRIMARY**; other seats empty — **still all five produced**; Empiricist heaviest  
**Kanta role:** Produce **Empiricist-richest** five-role pack; deepen diversity-scaling spine + motion/policy notes + mini-N reproduce checklist (RC0–RC5)  
**Themes:** data scaling laws · env/object/pair diversity ≫ demo depth · power-law OOD · UMI→Franka · Diffusion Policy + ACT temporal ensemble · DINOv2 ViT capacity · K≈50 plateau · 32×50 recipe  
**Sources:** Summarizer (`/workspace/papers/data-scaling-laws-2410.18647-summary.md`) + Senior Research (`/workspace/data-scaling-laws-quiet/DELIVERABLE.md`) + Ablation Bot (`/workspace/papers/data-scaling-laws-2410.18647-ablations.md` — **AUTHORITATIVE**) + `paper_notes.md` / `env_outline/`  
**Sibling links:** `/workspace/umi-quiet/` · `/workspace/diffusion-policy-quiet/` · `/workspace/droid-quiet/` · `/workspace/oxe-quiet/` · `/workspace/act-aloha-quiet/` · `/workspace/openvla-quiet/` · `/workspace/pi05-quiet/` · `/workspace/openpi-comet-smoke/` · `/workspace/dart-quiet/` · `/workspace/robogen-quiet/`

---

## 1. One-liner

**Diversity (envs / objects / pairs) ≫ demo depth** — for single-task IL, OOD gap follows **power laws** in \(M,N,\text{pairs}\); demos/pair **plateau ~50**; recipe **\(M{=}N{=}32\) pairs × \(K{\approx}50\)** → ~**90%** zero-shot on novel env+object (DP + DINOv2 FT + ACT TE).

**Contrast poles:**

| Pole | What they scale | Primary claim |
|------|-----------------|---------------|
| **This paper (Lin'25)** | Envs × objects × demos (**single-task** DP) | Power-law OOD env/object gen; diversity > demo depth |
| **OXE** | Embodiments × labs × skills | Positive transfer across robots; foundation mix |
| **DROID** | Scenes (same Franka) | Scene diversity ablation (+OOD); co-train recipe |
| **UMI** | Handheld interface + in-wild locations | Collection *mechanism* this paper uses |
| **Octo / OpenVLA / π₀** | Multi-task generalist + model/compute | Task-level + language; this paper **excludes** task scaling |

**Do NOT invent:** universal multi-task / multi-embodiment Chinchilla laws — paper fits **data axes** \(M,N,K\) only inside **single-task IL + DP**.

---

## 2. Summary (exec overview + key numbers)

**Lin et al. (ICLR 2025 Oral)** ask whether **data scaling alone** can yield **single-task** imitation policies that deploy **zero-shot** to any same-category object in any environment. They collect **>40 000** UMI handheld demos, train **Diffusion Policy** + **DINOv2 ViT-L/14** (full FT) + **ACT temporal ensemble**, and run **>15 000** real Franka rollouts under a **stage-scored** protocol.

**Headline recipe (CLEAR §4.3 / §5):** collect in **as many environments as possible**, **one unique object per env**, **\(K \approx 50\)** demos/pair; at **32 pairs** (~**1600** demos/task) expect ~**90%** zero-shot on unseen env+object — validated on Pour Water, Mouse Arrangement, Fold Towels, Unplug Charger (Fold+Unplug: **one afternoon / four collectors**).

**Why Data session:** Supplies the data cluster's clearest **quantitative** answer to "how much data?" — **scenes/objects, not more demos of the same table.**

| Axis | Number / claim |
|------|----------------|
| Study scale | **>40k** demos · **>15k** real rollouts · **4** tasks · **~90%** demos valid post-SLAM |
| Power-law form | \(Y=1-S=\beta X^{\alpha}\); \(X\in\{N,M,\text{pairs}\}\); **no** \(Y_\infty\) (only 6 pts) |
| PW exponents (obj / env / pairs) | **−0.703 / −0.844 / −0.579** (\|r\| 0.960–0.987) |
| MA exponents (obj / env / pairs) | **−0.697 / −0.466 / −0.683** (\|r\| 0.942–0.991) |
| MA → score 0.99 | **1,191** pairs (extrapolation; **unverified**) |
| Demo vs score \(r\) | PW **−0.62**; MA **−0.79** — **no clear demo power-law** |
| Plateaus | ~**800** total @ M=16,N=64; **400/800/1600** for 8/16/32 pairs → **K=50** |
| Object gen @ 8 / 32 objs | score **>0.8** / **>0.9** |
| Env gen moral | **Harder** than object (shallower early slope) |
| Multi-obj @ M=16 | **Negligible** vs 1 obj/env (Fig. 6) |
| Table 1 SR (32×50) | Pour **85%** · Mouse **92.5%** · Fold **87.5%** · Unplug **90%** |
| Table 1 scores | 0.922 / 0.933 / 0.95 / 0.887 (±CIs) |
| ViT-S / B / L | **0.66 / 0.81 / 0.90** |
| U-Net S / B / L | **0.88 / 0.90 / 0.83** (scale **hurts**) |
| FT / LoRA / LfS / frozen | **0.90 / 0.72 / 0.03 / 0.00** |
| Params / max train | **>396M**; **5e5** steps; **~75 h on 8×A800** |
| Policy stack | DP CNN U-Net + DDIM-16; Ah=16 @ 5 Hz; TE=8 @ −0.01; BF16 |

**Must-cite tattoo:** Fig.5 exponents · **K=50** plateau · Table1 **85–92.5%** · ViT **0.66/0.81/0.90** vs U-Net **0.88/0.90/0.83** · frozen/LfS/LoRA **0.00/0.03/0.72** · MA→0.99 needs **1191** pairs · diversity ≫ depth · **32×50**.

**Do NOT claim:** multi-task/language laws; multi-embodiment Chinchilla; course smoke = Tab.1 ~90%; 1191-pair verified; MSE replaces human scores.

---

## 3. Keywords

data scaling laws; imitation learning; robotic manipulation; power law; optimality gap; environment generalization; object generalization; env–object pairs; demonstration diversity; UMI; Diffusion Policy; DINOv2; ACT temporal ensemble; Franka; zero-shot deployment; Kaplan analogy; ICLR 2025 Oral; K=50; 32×50 recipe; BEHAVIOR data budget; ViT vs U-Net; LoRA vs FT

---

## 4. Deep dive — Diversity scaling (**PRIMARY DATA CONTRIBUTION**)

**Callout:** Scientific product = **empirical power laws on diversity axes** for single-task IL OOD — not a foundation model, not a planner, not a VLA. Session type = Data; quantitative backbone for "spend on scenes/objects, not depth."

### Formal axes (§3) — CLEAR

| Symbol | Meaning |
|--------|---------|
| \(M\) | # training environments |
| \(N\) | # training manipulation objects (same category) |
| \(K\) | # demos per env–object pair |
| \(S\) | Normalized test score on **unseen** envs and/or objects |
| \(Y\) | Optimality gap \(Y=1-S\) (fit target) |

**Power-law form (CLEAR §4.2):** \(Y=\beta\cdot X^{\alpha}\) with \(X\in\{N,M,\text{pairs}\}\). Log-linear OLS. Footnote: 3-param \(Y=\beta X^{\alpha}+Y_\infty\) **not** fit — only **6** points.

### Fig. 5 power-law fits (must-cite) — VERIFIED

| Task | Axis \(X\) | Fit | Pearson \(r\) (log–log) |
|------|------------|-----|-------------------------|
| Pour Water | # objects | \(Y=0.825\,X^{-0.703}\) | −0.987 (\|r\|=0.987) |
| Pour Water | # envs | \(Y=1.180\,X^{-0.844}\) | −0.960 |
| Pour Water | # pairs | \(Y=1.068\,X^{-0.579}\) | −0.980 |
| Mouse Arrangement | # objects | \(Y=0.826\,X^{-0.697}\) | −0.991 |
| Mouse Arrangement | # envs | \(Y=0.827\,X^{-0.466}\) | −0.966 |
| Mouse Arrangement | # pairs | \(Y=1.263\,X^{-0.683}\) | −0.942 |

**pdftotext trap:** Fig.5 exponents are **negative** (gap↓); Ablation Bot corrected. Mouse pairs → \(Y=0.01\) (score 0.99) ⇒ \(X\approx\mathbf{1191}\) (**CLEAR** claim; **unverified** experimentally).

### Diversity sweeps (100% demo norms) — Ablation Bot AUTHORITATIVE

| Sweep | Pour Water (M/N/pairs 1→32) | Mouse Arrangement |
|-------|----------------------------|-------------------|
| Objects Fig.2 | **0.133 → 0.939** | **0.217 → 0.929** |
| Envs Fig.3 | **0.144 → 0.956** | **0.217 → 0.867** |
| Pairs Fig.4 | **0.050 → 0.875** | **0.125 → 0.921** |

**Morals:** (1) Env gen **harder** than object. (2) With more diversity, fewer demos/unit needed. (3) Joint diversity saturates demo-fraction curves fastest. (4) App.G.2: diversity still wins at **matched total demos**.

### Demo saturation (Fig. 7) — NEGATIVE power-law

| Setting | Plateau (~) |
|---------|-------------|
| Max pool \(M=16,N=64\) | **~800** total demos |
| 8 / 16 / 32 pairs | **~400 / ~800 / ~1600** |
| Recommended \(K\) | **50 demos / pair** (similar difficulty) |

**Demo–performance \(r\):** PW −0.62, MA −0.79 — **weak**; more hours on same (env,obj) ≠ more OOD.

### Collection recipe (§4.3 → Table 1)

- Maximize **#envs**; **1 unique object per env**.  
- **\(M=N=32\)** pairs × **\(K\approx50\)** → ~**1600** demos/task.  
- Multi-obj/env helps only when \(M\) **small**; at \(M=16\) gap ≈0 (Fig. 6).

| Task | Norm. score | Success rate |
|------|-------------|--------------|
| Pour Water | \(0.922\pm0.075\) | **85.0±19.4%** |
| Mouse Arrangement | \(0.933\pm0.088\) | **92.5±9.7%** |
| Fold Towels | \(0.95\pm0.062\) | **87.5±17.1%** |
| Unplug Charger | \(0.887\pm0.14\) | **90.0±14.1%** |

**Variance warning:** Table 12 — e.g. Pour Water env#2 only **40%** SR while others 80–100%.

### Capacity (Table 2) — Pour Water, 32 pairs, 50% demos

| Ablation | Scores |
|----------|--------|
| Train strategy | FT **0.90** · LoRA **0.72** · LfS **0.03** · frozen **0.00** |
| Visual encoder | ViT-S **0.66** · ViT-B **0.81** · ViT-L **0.90** |
| Action U-Net | small **0.88** · base **0.90** · large **0.83** |

**Takeaway:** vision pretrain + **full FT** + encoder size matter; action U-Net scale **does not** (may hurt). **LoRA MSE trap:** LoRA can win MSE yet lose stage score — primary = human/stage proxy.

**Diversity exam bite:** "OOD gap \(Y=1-S\approx\beta X^{\alpha}\) with \(\alpha\sim-0.5\) to \(-0.8\) on #envs/#objects/#pairs; demos plateau (~50/pair); recipe **32×50** → ~90% ZS; diversity ≫ depth; **not** a Chinchilla multi-task law."

---

## 5. Deep dive — Motion / policy / control notes

**Callout:** Motion is **supporting**, not classical planner / not VLA primary. Stack = **Diffusion Policy** (chunked visuomotor diffusion) + **ACT temporal ensemble** for smooth chunk stitching. Embodiment = **single** Franka deploy; collection = **human+UMI**.

### Policy / control stack (§3, App. C/F) — CLEAR

| Knob | Paper default |
|------|---------------|
| Policy family | **Diffusion Policy** CNN 1D U-Net noise predictor |
| Sampler | **DDIM**, 16 inference steps |
| Vision | **DINOv2 ViT-L/14**, **fully fine-tuned** |
| Action smoothing | **ACT temporal ensemble**: ens steps=**8**, adaptation **−0.01** |
| Obs resolution | 224×224 wrist RGB (GoPro Hero 10 fisheye) |
| Action horizon | **16** @ env freq **5 Hz** → ~3.2 s chunk |
| Obs horizon | **2** default; **3** for Pour (0.25 s) / Unplug (0.5 s) distant history |
| Optim | AdamW; lr action **3e−4**, encoder **3e−5**; batch **256**; BF16 |
| Deploy | Franka Panda + Weiss WSG-50; soft 95A TPU fingers; NVIDIA **4090** |

### Motion / LH hooks (Empiricist / BEHAVIOR)

| Hook | Where | Use |
|------|-------|-----|
| Handheld UMI teaching | Sec.3 | Diversity collect without robot fleet — `/workspace/umi-quiet/` |
| Relative EE + DP chunks | Policy | Ah=16 @ 5 Hz; TE=8 jerky-switch fix |
| Distant obs history | App.C | LH / stage-ambiguity ≠ more demos |
| Multi-stage scoring | App.D | Stage-wise SR for LH credit (0–3 pts/step) |
| Dynamic unplug | Unplug Charger | Speed / yank; UMI latency sibling |
| Non-prehensile push | Mouse | Push-before-grasp stage |
| Precision pour | Pour Water | Fine motion + history |
| Fold deformation | Fold Towels | Soft-object LH |
| Sibling stacks | DP / ACT / π₀.₅ / OpenVLA | Keep **diversity mix cards**; swap head |

**Implication:** Env/object **pair-count**, **demos/pair**, and **vision capacity** are **separate knobs** (P1/P2/P5/P8) — do **not** collapse into "more demos."

**Motion exam bite:** "Lin'25 = **DP + DINOv2 FT + ACT TE** on UMI→Franka; not classical planner, not VLA primary; Ah=16@5Hz, TE=8; distant history for Pour/Unplug; diversity axes are the science."

---

## 6. Role-ready sections (all five — Empiricist RICHEST)

### 6.1 Empiricist — WEIGHT 3 LONGEST PRIMARY (Nykolas Rekasius)

> **Seat charge:** Course smokes that stress **#envs · #objects · demo K · power-law fit · capacity**, ranked by GPU honesty. Prefer **ruthlessly mini-N** `M,N∈{1,2,4}`, `K∈{10,25,50}` + toy power-law + tiny ViT-S vs B — **not** paper M=32 / 8×A800 / 15k rollouts / Tab.1 ~90%.

#### Claim under measurement

Single-task Diffusion Policy OOD success on new envs/objects is a **power-law function of diversity counts**, not of raw demonstration count past a modest \(K\).

#### Hypotheses (falsifiable — from DELIVERABLE)

1. **H-env (P1):** Held-out env stage-proxy **rises monotonically** with `M∈{1,2,4}` at fixed object + nested demos (Pour 0.144→…→0.956 moral; env harder than object).  
2. **H-obj (P2):** Held-out object proxy rises with `N∈{1,2,4}` at fixed env (Pour 0.133→…→0.939).  
3. **H-demo-sat (P5):** At fixed small `#pairs`, `K∈{10,25,50}` shows **diminishing returns**; no strong power-law vs K alone.  
4. **H-power (P4):** Gap \(Y=1-\mathrm{score}\) vs \(X\in\{M,N,\mathrm{pairs}\}\) approx log-linear with **negative** \(\alpha\); demo-total alone is weak.  
5. **H-vit (P8):** Under matched mini-N + steps, **ViT-B > ViT-S** on OOD proxy (0.81 vs 0.66 moral).  
6. **H-unet-flat (P10):** U-Net base ≯ small on OOD; large may hurt (defer large).  
7. **H-ft (P9):** Full FT ≫ LoRA on **stage proxy** even if MSE ranks LoRA better.  
8. **H-div>vol (P7):** At matched total demos, higher M or N beats low-diversity high-K.  
9. **H-recipe (P11):** Mini analogue of high-pair / K≈50 beats same total demos on ≤2 pairs for joint OOD — **without** claiming Tab.1 90%.

**Falsifiers:** M=4 ≯ M=1 on env-OOD; N flat; K linear power-law with high r; ViT-S≈ViT-B; U-Net large ≫ base; LoRA ≥ FT on stage proxy.

#### Prioritized ablation matrix (Ablation Bot AUTHORITATIVE P1–P12)

| Pri | Experiment | Paper hook | Smoke | Est. GPU-h |
|-----|------------|------------|-------|------------|
| **P1** | #envs `M∈{1..32}` | Fig.3 / Tab.5,9 | **Y — RC1 PRIMARY** | 4–14 |
| **P2** | #objects `N∈{1..32}` | Fig.2 / Tab.4,8 | **Y — RC2 PRIMARY** | 4–14 |
| **P3** | #env–object pairs | Fig.4 / Tab.6,10 | Partial after P1+P2 | 4–12 |
| **P4** | Power-law fit gap vs M/N/pairs | Fig.5 | **Y — RC4** | <1–2 CPU |
| **P5** | Demo quantity K | Fig.7 **NEGATIVE** | **Y — RC3 PRIMARY** | 4–12 |
| **P6** | Objects-per-env heatmap | Fig.6 | Partial | 4–10 |
| **P7** | Constant-total-demo replot | App.G.2 | Partial | 2–8 |
| **P8** | Encoder ViT-S/B/L | Tab.2b | **Y — RC5** (S vs B) | 6–16 |
| **P9** | Encoder strategy FT/LoRA/LfS/frozen | Tab.2a | Fold into RC5 | — |
| **P10** | U-Net size | Tab.2c | **Y — RC5** | (shared) |
| **P11** | Cross-task recipe 32×50 | Tab.1 | Partial — RC6 | 8–20 |
| **P12** | MSE power-law | App.G.1 | N / light | <1 |
| **Defer** | Full M=32 / 1191-pair / 8×A800 / 15k | — | **N** | ≫200 |

**Min package (Ablation Bot):** P1 + P2 + P5 → P4 → P8/P10 → P11.  
**Default course smoke order:**  
`shard_ingest` → **`env_diversity_sweep` (P1)** → **`object_diversity_sweep` (P2)** → **`ndemo_sweep` (P5)** → **`powerlaw_fit` (P4)** → **`capacity_x_data` (P8/P9/P10)** → optional P3/P7/P11.

#### Exact reproduce stack (EDIT)

```bash
# Official
git clone https://github.com/Fanqi-Lin/Data-Scaling-Laws
# Project: https://data-scaling-laws.github.io/
# HF: https://huggingface.co/datasets/Fanqi-Lin/Processed-Task-Dataset
# Pin commits → logs/pins.txt

# Course scout env (quiet outlines — preferred)
export DSLAW_WORK_ROOT=$HOME/dslaw-smoke
cd /workspace/data-scaling-laws-quiet
bash env_outline/setup_mamba.sh
# mamba activate dslaw-scout
mkdir -p logs runs mix_cards configs
# EDIT partition/account in slurm_*.sh
# Outlines only — do NOT sbatch from quiet box
# NEVER claim Tab.1 ~90% / 1191-pair / paper-scale from mini-N
```

#### Run cards (RC0–RC6) — Empiricist LONGEST

##### RC0 — `shard_ingest_smoke` (CPU-first gate)

| Field | Spec |
|-------|------|
| Goal | Demo → tensors + **diversity card schema** (`env_id`,`obj_id`,`pair_id`) |
| Input | Public release shard **or** synthetic: ≥4 env × ≥4 obj × ≤50 demos; wrist RGB stub; relative EE+grip; stage labels |
| Pass | Schema OK; 0 NaNs; mix card lists M,N,K; wall **<30 min CPU** |
| Fail | Missing env/obj tags; abs labeled as rel; silent pair drops |
| Script | `env_outline/slurm_shard_ingest_smoke.sh` |

##### RC1 — `env_diversity_sweep` (**P1 PRIMARY**)

| Field | Spec |
|-------|------|
| Paper | Pour norm ≈**0.144→0.956**; Mouse ≈**0.217→0.867**; env harder |
| Goal | Monotonic ↑ env-OOD with mini `M∈{1,2,4}` (directional) |
| Train | Small DP (ResNet18 or ViT-S); **1 seed**; nested envs (App. C) |
| OOD | Hold out ≥2 envs never in train; same object category |
| Pass | 3 M runs; M=4≥M=2≥M=1 on env-OOD; plot — **not** 0.956 |
| Budget | **~4–14 GPU-h**; VRAM **10–20 GB** |
| Script | `env_outline/slurm_env_diversity_sweep.sh` (`#SBATCH -a 0-2`) |
| Cross-link | `/workspace/droid-quiet/` scene-diversity; `/workspace/umi-quiet/` wild vs narrow |

##### RC2 — `object_diversity_sweep` (**P2 PRIMARY**)

| Field | Spec |
|-------|------|
| Paper | Pour ≈**0.133→0.939**; Mouse ≈**0.217→0.929**; 8 objs >0.8, 32 >0.9 |
| Goal | Monotonic ↑ object-OOD with mini `N∈{1,2,4}` |
| Pass | N=4≥N=2≥N=1 on object-OOD; steeper early than RC1 |
| Budget | **~4–14 GPU-h** |
| Script | `env_outline/slurm_object_diversity_sweep.sh` |

##### RC3 — `ndemo_sweep` (**P5 PRIMARY**)

| Field | Spec |
|-------|------|
| Paper | **NEGATIVE** power-law vs demos; plateau ~800; recommend **K=50** |
| Goal | Diminishing returns on `K∈{10,25,50}` at fixed mini pairs (e.g. 4) |
| Pass | ≥3 K points; clear diminishing ΔOOD; **weak** log-log r vs K |
| Fail | Non-nested K; steps∝̸K → small-K undertrained |
| Budget | **~4–12 GPU-h** |
| Script | `env_outline/slurm_ndemo_sweep.sh` |

##### RC4 — `powerlaw_fit_smoke` (**P4**)

| Field | Spec |
|-------|------|
| Paper | Mouse pairs `y=1.263·x^{−0.683}` (r=0.942); 0.99 ⇒ **1,191** pairs |
| Goal | Fit \(Y=\beta X^{\alpha}\) on RC1/RC2 smoke; emit prediction JSON |
| Pass | `logs/powerlaw_fit.json` with α<0, β, r; predict X for target 0.95; note 1191 unverified |
| Budget | **CPU <1 h** |
| Script | `env_outline/slurm_powerlaw_fit_smoke.sh` (outline in DELIVERABLE; stub if missing) |

##### RC5 — `capacity_x_data` (**P8/P9/P10**)

| Field | Spec |
|-------|------|
| Paper | ViT 0.66/0.81/0.90; FT 0.90 / LoRA 0.72 / LfS 0.03 / frozen 0.00; U-Net 0.88/0.90/0.83 |
| Goal | Course: **ViT-S vs B**, **U-Net small vs base**, **FT vs LoRA** on one mini-N card |
| Pass | B>S on OOD; U-Net base ≯≫ small; FT>LoRA on **proxy** (log MSE trap) |
| Defer | ViT-L, U-Net large, LfS/frozen full cells |
| Budget | **~6–16 GPU-h**; VRAM **12–24 GB** |
| Script | `env_outline/slurm_capacity_x_data.sh` (outline in DELIVERABLE) |

##### RC6 — (stretch) `recipe_train_smoke` (**P11 partial**)

Mini analogue of high-pair × K≈50 vs concentrated demos; **not** real 32×50 / Tab.1 90%. Optional P3 pairs `{1,2,4}` / P7 matched-T if GPU remains. Script outline: `env_outline/slurm_recipe_train_smoke.sh`.

#### Paper hyperparams to pin (App. C) + smoke cuts

| Knob | Paper | Smoke cut |
|------|-------|-----------|
| Batch | **256** | **32–64** |
| Encoder | DINOv2 ViT-L full FT | ViT-S / ResNet18; FT vs LoRA cell |
| U-Net | base | small vs base; defer large |
| Steps | up to **5e5** / 75 h 8×A800 | short steps scaled to data |
| Nested subsets | always | **keep** (fairness) |
| Checkpoint | final | keep |
| Eval | 8 unseen × 5 trials; human stage scores | locked OOD shard + stage-proxy / offline MSE |
| TE / Ah | TE=8; Ah=16 @ 5 Hz | keep if DP stack |

#### Numbers to memorize (exam — Nykolas)

| Fact | Value |
|------|-------|
| Total demos / rollouts | >40k / >15k |
| Power-law \(Y\) | \(1-\text{score}\) |
| PW obj/env/pair α | −0.703 / −0.844 / −0.579 |
| MA obj/env/pair α | −0.697 / −0.466 / −0.683 |
| \|r\| range (score laws) | 0.942–0.991 |
| MA → score 0.99 | **1191** pairs |
| Demo–perf \(r\) (weak) | −0.62 (PW), −0.79 (MA) |
| Plateau @ 32 pairs | ~**1600** → \(K=50\) |
| Multi-obj @ \(M=16\) | Negligible vs 1 obj/env |
| Verification SR (4 tasks) | ~**85–92.5%** |
| ViT-S/B/L | 0.66 / 0.81 / 0.90 |
| U-Net S/B/L | 0.88 / 0.90 / 0.83 |
| Frozen / LfS / LoRA / FT | 0.00 / 0.03 / 0.72 / **0.90** |
| Params / max train | >396M; 5e5 steps; 75 h / 8 A800 |

#### Failure modes cheat-sheet

| Symptom | Likely cause | Next action |
|---------|--------------|-------------|
| Flat M curve | Object-OOD used / non-nested | Eval env-OOD only; nest envs |
| Flat N curve | Same | Fix object holdout |
| No K plateau | Steps∝̸K | Scale steps; nest K |
| α>0 in fit | Used score instead of gap | Fit \(Y=1-\mathrm{score}\) |
| ViT-S≈B | Undertrain B | Match steps/aug |
| LoRA "wins" | Looking at MSE | Stage-proxy primary |
| Cross-run incomparable | Different eval batches | Lock one eval shard (App.E.2) |
| OOM | Paper bs256 / ViT-L | bs32–64; defer L |
| Claimed Tab.1 90% from stub | Overclaim | Report mini-N directional only |
| Positive α from pdftotext | Superscript minus dropped | Use Ablation Bot negative exponents |

#### What not to invent / claim

- **Do not** invent universal multi-task / multi-emb Chinchilla laws.  
- **Do not** amortize Tab.1 ~90% / Fig.5 r>0.94 onto Partial stubs.  
- **Do not** trust MSE alone (LoRA trap).  
- **Do not** claim 1191-pair verified.  
- **Do not** claim course smoke = paper 8×A800 / 15k rollouts.  
- **Do not** confuse ACT temporal ensemble with ACT-as-primary-policy.  
- **Do not** claim OXE-style embodiment scaling was studied.

#### Empiricist exam bite (Nykolas)

"Lin et al. (ICLR'25 Oral): OOD gap \(Y=1-S\approx\beta X^{\alpha}\) with \(\alpha\sim-0.5\) to \(-0.8\) on #envs/#objects/#pairs (\(r\sim-0.94\) to \(-0.99\)); demos plateau (~50/pair); recipe **32×50** → ~90% zero-shot; diversity ≫ depth; encoder scale helps, U-Net scale doesn't; Empiricist RC1–RC3 mini-N directional — never claim Chinchilla / Tab.1 parity from stubs."

---

### 6.2 Scientific Reviewer — produced despite empty seat

**Claims under review**
1. Single-task IL OOD generalization follows approximate **power laws** in #envs / #objects / #pairs.  
2. **Diversity ≫ demo depth** once \(K\) hits a modest plateau (~50).  
3. Efficient recipe **32 pairs × 50 demos** transfers across tasks of similar difficulty (~90% SR).  
4. Visual encoder capacity + FT matter; action U-Net width does not.

**Strengths**
1. Rare **real-robot** scaling study with >15k rollouts and **blind** multi-policy eval.  
2. Clean operationalization of OOD as **env × object** (not synthetic single-factor).  
3. Actionable recipe validated on **two held-out tasks** with afternoon-scale collection.  
4. Honest metric analysis (MSE failures; LoRA trap) and nested-subset design.  
5. Fig. 5 fits with high \|r\| + explicit 1191-pair prediction (even if unverified).  
6. Capacity ablations (Table 2) cleanly separate vision vs action scaling.  
7. Ablation Bot corpus rich despite no section titled "Ablation" (Sec.4–6+App.G).

**Weaknesses / threats**
1. Only **4** tasks; similar tabletop difficulty — dexterous/LH exponents unknown.  
2. **6-point** fits; no \(Y_\infty\); extrapolation to 1191 pairs **unverified**.  
3. Single algorithm (**DP**) + UMI noise floor confound "data law" vs "interface law."  
4. Human scoring subjectivity mitigated by blind batches but not eliminated.  
5. No task/language axis — cannot adjudicate generalist VLA scaling.  
6. Selection bias: UMI-friendly tasks only (texture-poor / large occluders avoided).  
7. Per-env variance (Table 12: Pour env2 **40%**) under-discussed in headline ~90%.  
8. Cross-batch incomparability (App.E.2) — replication must freeze eval shard.  
9. Compute bar (8×A800 × 75 h) limits community full-grid replication.  
10. **pdftotext exponent-sign trap** — cite Ablation Bot / re-fit, not raw OCR.  
11. No multi-embodiment / RL / multi-task exponent transfer shown.  
12. Concurrent RUMs / generalist papers — scope carefully vs "solved robot scaling."

**Verdict:** Strong **empirical systems** paper with genuine scaling-law substance for the **single-task IL** regime. Do **not** over-claim as universal robot Chinchilla law. **CLEAR** for data-cluster citations on diversity budgets; **TENTATIVE** for multi-task / RL / multi-embodiment transfer of exponents.

**Reviewer exam bite:** Cite **Fig. 5 + §4.3 recipe + Table 1**; caveat **§7** (single-task, IL-only, 4 tasks, DP-only); punish "universal Chinchilla" and "course smoke = 90%" claims.

---

### 6.3 Visionary — produced despite empty seat (BEHAVIOR recipe; **no Discord/Drive**)

**BEHAVIOR connection:**  
BEHAVIOR'26 stresses **many household tasks × house-scale scenes × object variation**. This paper gives the **per-primitive diversity floor**: before buying more demos of the same (scene, object), hit **~32 pairs × ~50**. Complements **DROID scene-churn** and **UMI handheld** as the **measurement** of how much churn is enough. Aligns OXE visionary pack's ~**20k** challenge-demo framing (**TENTATIVE** global): if ~100 tasks, naïve equal split ≈200 demos/task — **below** this paper's 1600 if each task is its own policy → either (a) **share** visual/action priors across tasks (π₀.₅ / OpenPI / N1) so per-task demos can be fewer, or (b) **concentrate** 32×50 budgets on hardest contact-rich tasks and rely on transfer for easier ones.

| BEHAVIOR need | Lin'25-shaped response |
|---------------|------------------------|
| Scene coverage | Maximize #envs; mix cards `n_scenes / n_objects / n_pairs / K / total_N` |
| Object variation | Same-category OOD; 1 unique obj/env at high M |
| Demo budget allocation | Default ~**50**/pair; after plateau spend on **new** pairs |
| Long-horizon chores | Distant-history + TE + stage-wise metrics (not more demos) |
| Policy race | Keep diversity cards when swapping DP → π₀.₅ / OpenPI / N1 |
| Eval honesty | Blind multi-checkpoint scoring; freeze eval shard |

**CLEAR:** Design prior for BEHAVIOR-like per-skill diversity budgeting.  
**TENTATIVE:** Official BEHAVIOR'26 baseline names this recipe; exact 20k→unit mapping.

#### 2026 BEHAVIOR data-scaling / mix recipe (9 bets)

1. Publish BEHAVIOR mix cards: `n_scenes / n_objects / n_pairs / demos_per_pair / total_N`.  
2. Default ~**50 demos/pair**; spend budget on **new scenes/objects** after RC3-style plateau.  
3. Gate "more data" claims with **P1/P2/P7 matched-N** Empiricist cards.  
4. Scale **vision FT** (or VLM towers), not fat action heads, when data is diverse.  
5. Tag `source=umi_handheld|droid_robot|oxe|behavior`; mix under shared schemas.  
6. Use P4-style fits to **budget** new scenes for a target stage-SR — extrapolations = hypotheses.  
7. Keep distant-history + TE + stage-wise metrics for LH chores.  
8. Re-estimate laws under **π₀.₅ / OpenPI / N1** — does α change?  
9. Honest non-goals: course smoke ≠ 40k demos / 15k rollouts / Tab.1 90% / 1191 pairs.

#### Follow-up research
1. Repeat Fig. 5 with **ACT / π₀-FM / VLA** heads — do exponents move?  
2. Fit laws on **task count** + language (OpenVLA/Octo axis).  
3. RL fine-tune scaling on top of IL diversity laws (§7 ask).  
4. Multi-embodiment: does \(M_{\text{robot}}\) enter the same power family as \(M_{\text{env}}\)?  
5. Verify **1191-pair** Mouse extrapolation (or measure \(Y_\infty\)).  
6. Dexterous / deformable / long-horizon \(K\) thresholds.  
7. Closed-loop collector GUI: force new (env,object) before \(K>50\).

#### New applications
- **Collector GUI quota:** force new (env, object) before allowing \(K>50\).  
- **Data marketplace SKU:** sell "32-pair skill packs" not "10k demo dumps."  
- **Eval harness:** blind multi-checkpoint scoring as standard for scaling claims.  
- **Sim BEHAVIOR:** privileged state expands pair diversity, then real \(K=50\) polish.

**Visionary one-liner:** For BEHAVIOR'26, treat Lin'25 **32×50** as the **single-skill diversity unit**; spend the ~20k pool on covering units + shared generalist prior — don't deepen easy pairs.

**Visionary exam bite:** "For BEHAVIOR, treat Lin'25 **32×50** as the **single-skill diversity unit**; publish mix cards; scale vision FT not U-Net width; re-fit α under π₀.₅/OpenPI — **no Discord/Drive**."

*(No Discord / Drive checklist content.)*

---

### 6.4 Stakeholder — produced despite empty seat

**Product one-liner:** For a **single household skill**, you do **not** need 10k demos of one kitchen — you need **~32 varied scenes/objects × ~50 demos** (~**1.6k**/skill) with a modern chunked visuomotor policy to reach ~**90%** zero-shot on new kitchens/objects.

**Who cares**
- **Data ops / robot fleet leads:** reallocates collector time from depth → scene churn (DROID-aligned).  
- **BEHAVIOR / challenge teams:** per-skill floor under a ~20k total demo budget (**TENTATIVE** global from OXE pack).  
- **Foundation-model labs:** explains why OXE width helps but **per-skill coverage** still needed for zero-shot without FT.  
- **Hardware startups:** UMI afternoon collection → deployable Franka skill is a sales demo.  
- **Non-consumers:** need certified multi-task/language gen today; no real-eval budget; require multi-emb laws this paper doesn't fit.

**Buy / build decision**
1. Bottleneck = **same-scene demo depth** → stop; reallocate to new (env,obj) pairs at K≈50.  
2. Bottleneck = **real visual diversity / robot-in-wild** → still fund DROID/OXE; use this paper as **per-skill floor**.  
3. Bottleneck = **multi-task language gen** → this paper does **not** solve it; buy OpenVLA/π₀-class + keep diversity cards.  
4. Always budget **real stage-scored eval** (MSE lies) + env-stratified QA (Table 12 variance).

**Risks:** Real eval expensive (>15k rollouts); UMI SLAM / texture limits skill class; ~90% average hides **40%** environments; recipe may under-serve high-dexterity / multi-minute tasks; compute bar for full grids; overclaiming Chinchilla universality.

**Stakeholder exam bite:** "Budget **diversity pairs**, not demo depth; **32×50** is the published efficient frontier for single-task ~90% OOD — env-stratify QA; don't buy 10k same-kitchen dumps."

---

### 6.5 Archaeologist — produced despite empty seat

**Priors this paper absorbs (CLEAR)**
- Kaplan et al. 2020 / Henighan et al. 2020 — LLM/CV **scaling-law method** (power law on scale axis).  
- **UMI** (Chi'24, 2402.10329) — collection + deploy interface.  
- **Diffusion Policy** (Chi'23, 2303.04137) — policy family.  
- **ACT** (Zhao'23, 2304.13705) — **temporal ensemble only** (not primary policy).  
- **OXE** (2310.08864) — related data scaling; different goal (X-emb FT vs zero-shot single-task).  
- **DROID** (2403.12945) — scene diversity prior; complementary.  
- DINOv2 — visual backbone.

**Genealogy (CLEAR framing)**

```
Kaplan/Henighan LLM–CV scaling-law *method*
        + UMI handheld in-wild collection
        + Diffusion Policy + ACT temporal ensemble
                → Lin et al. IL data-axis laws (env × object × demos)
                        → 32×50 single-skill diversity unit
                              ∥  OXE multi-emb foundation / DROID scene co-train
                              ↓
                     BEHAVIOR / VLA mixes that need
                     per-skill diversity cards + vision FT
```

| Ancestor | Relation | CLEAR/TENTATIVE |
|----------|----------|-----------------|
| Kaplan 2020 | Method analogy (power law) | **CLEAR** cite; exponents **not** transferable |
| Hoffmann/Chinchilla | Compute–data optimality | **Not fitted here** — **TENTATIVE** analogy only |
| UMI 2402.10329 | Collection + deploy | **CLEAR** |
| Diffusion Policy 2303.04137 | Policy family | **CLEAR** |
| ACT 2304.13705 | Temporal ensemble only | **CLEAR** |
| OXE / DROID | Related data scaling; different goals | **CLEAR** contrast |
| Octo / OpenVLA / π₀ | Generalist scale | Cited related; paper **stops before** task axis |

**Contrast poles (session lineage):**  
`OXE / DROID / UMI` = breadth of **collection mechanisms**;  
`Lin'25` = **measurement** of how much env×object diversity is enough for single-skill ZS;  
`Octo/OpenVLA/π₀` = **task/language** scale this paper explicitly excludes.

**Descendants / ecosystem**
- Public code + HF processed dataset + project site — **CLEAR**.  
- Conceptual influence on BEHAVIOR mix design / collector quotas — **TENTATIVE** without organizer checklist.  
- Do not over-claim unique descent into VLAs without citation checks.

**Genealogy one-liner:** Kaplan *idea* → robot IL *env/object* laws (Lin'25 Oral); collection via UMI; DP+ACT-TE stack; **not** an OXE/Chinchilla replacement.

**Archaeologist exam bite:** "Lin'25 = Kaplan-style power laws on **#envs/#objects/#pairs** for single-task DP+UMI; K=50 plateau; 32×50 → ~90% ZS; complement OXE/DROID — do not transfer exponents to multi-task/multi-emb."

---

## 7. Empiricist run pack (env / scripts / GPU — from DELIVERABLE)

### Env setup

```bash
export DSLAW_WORK_ROOT=$HOME/dslaw-smoke
cd /workspace/data-scaling-laws-quiet
bash env_outline/setup_mamba.sh
# mamba activate dslaw-scout
mkdir -p logs runs mix_cards configs
# EDIT: partition/account in slurm_*.sh
# Pin project + UMI + DP commits → logs/pins.txt
# Outlines only — do NOT sbatch from quiet box
```

| File | Role |
|------|------|
| `env_outline/conda_env.yaml` | `dslaw-scout` sketch |
| `env_outline/setup_mamba.sh` | Create/update env; data/logs/mix_cards |
| `env_outline/run_checklist.md` | Ordered RC0→RC5 (+stretch) |
| `env_outline/slurm_shard_ingest_smoke.sh` | CPU ingest + mix-card gate |
| `env_outline/slurm_env_diversity_sweep.sh` | Array M∈{1,2,4} — **P1 PRIMARY** |
| `env_outline/slurm_object_diversity_sweep.sh` | Array N∈{1,2,4} — **P2 PRIMARY** |
| `env_outline/slurm_ndemo_sweep.sh` | Array K∈{10,25,50} — **P5 PRIMARY** |
| `env_outline/slurm_powerlaw_fit_smoke.sh` | P4 fit + predict (DELIVERABLE outline) |
| `env_outline/slurm_capacity_x_data.sh` | P8/P9/P10 ViT-S/B + U-Net + FT/LoRA (outline) |
| `env_outline/slurm_obs_horizon_lh.sh` | Distant history / TE stub (outline) |
| `env_outline/slurm_recipe_train_smoke.sh` | P11 mini recipe analogue (outline) |
| `env_outline/README.md` | Cluster usage; Ablation Bot pointer; siblings |

**Note:** Primary P1/P2/P5 scripts are present under `env_outline/`; P4/P8–P11 scripts are specified in DELIVERABLE — flesh before sbatch.

### GPU / RAM / TIME budget honesty

Assumptions: scout DP (ResNet18 / ViT-S/B), image ≤224, batch **32–64** (paper **256**), **mini-N** `M,N∈{1,2,4}`, `K∈{10,25,50}` — **not** M=32 / 8×A800 / 5e5-step.

| Workload | GPU VRAM (peak) | Host RAM | Wall time (order) | Smoke |
|----------|-----------------|----------|-------------------|-------|
| RC0 ingest | — | 8–16 GB | **<0.5 h CPU** | **Y** |
| RC1 env M×3 | **10–20 GB** | 32–64 GB | **4–14 h** total | **Y** |
| RC2 obj N×3 | same | 32–64 GB | **4–14 h** total | **Y** |
| RC3 K×3 | same | 32–64 GB | **4–12 h** total | **Y** |
| RC4 power-law fit | — | 8 GB | **<1 h CPU** | **Y** |
| RC5 capacity ×4 | **12–24 GB** | 64 GB | **6–16 h** total | **Y** |
| RC6/RC7 stretch | as RC1 | 32–64 GB | **4–20 h** | Partial |
| Paper full ViT-L bs256 @ 32×50 | **8×A800** | large | **~75 h** largest | **N** |
| Paper 15k rollouts / 1191-pair | robot + mega data | — | **Out of course** | **N** |

**Honesty rule:** log `(M,N,K,encoder,ft_mode,steps,eval_shard,wall_s,metric)`. If `#SBATCH --gres=gpu:N` but CUDA false → **fail**. Never amortize Tab.1 ~90% onto a Partial stub. Prefer **3-point mini grids** over paper `{1..32}`.

**Course envelope:** target **≤ ~40–55 GPU-h** for P1+P2+P5+P4+P8/P10. Full paper grids × 4 tasks × real eval = **hundreds–thousands GPU-h + robot hours** — do not promise.

### Success criteria vs paper claims

| Paper claim | Minimal course criterion | Not required |
|-------------|--------------------------|--------------|
| Env scale M↑ → score↑ (0.144→0.956 Pour) | RC1: M=4≥M=2≥M=1 env-OOD | Full M grid / paper norms |
| Object scale N↑ (0.133→0.939 Pour) | RC2: N=4≥N=2≥N=1 object-OOD | Full N grid |
| Demo K saturates; no clear demo power-law | RC3: diminishing returns K=10→50 | Fig.7 plateau locations |
| Power-law gap vs M/N/pairs (neg. α) | RC4: α<0 fit; prediction JSON | Match Fig.5 r; 1191-pair |
| ViT-S/B/L 0.66/0.81/0.90 | RC5: B>S on proxy | ViT-L |
| U-Net 0.88/0.90/0.83 null/neg | RC5: base ≯≫ small | Include large |
| FT 0.90 ≫ LoRA 0.72 | RC5: FT>LoRA on **stage proxy** | Frozen/LfS cells |
| Diversity wins @ constant total demos (P7) | Matched-T card or replot | App.G.2 full |
| Recipe 32×50 → ~90% SR (P11) | Mini recipe > concentrated | Real Tab.1 |
| MSE power-law weaker (P12) | Optional; do not gate on MSE | App.G.1 |

**Pass:** Empiricist executes RC0 + **P1 RC1** + **P2 RC2** + **P5 RC3** (+ P4 fit) with **paper-aligned directional** rankings.  
**Fail / overclaim:** "reproduced Lin'25 / matched Tab.1 90% / verified 1191 pairs / Chinchilla for robots" from mini-N stubs.

---

## 8. Slide Maker brief

| Role | Weight / timing guide | Focus |
|------|----------------------|-------|
| **Empiricist** | **3 · LONGEST · ~16–18** | Hypotheses, RC0–RC5, reproduce cmds, GPU honesty, ablation matrix P1–P12, exam numbers |
| **Reviewer** | empty seat · ~12–14 | Fig.5 + Tab.1 + §7 caveats; verdict single-task Accept |
| **Visionary** | empty seat · ~12–14 | BEHAVIOR 32×50 unit + mix cards; **no Discord/Drive** |
| **Stakeholder** | empty seat · ~12–14 | Buy diversity pairs not depth; env-stratify QA |
| **Archaeologist** | empty seat · ~12–14 | Kaplan→Lin'25∥OXE/DROID; UMI+DP+ACT-TE genealogy |

**Spine (paste into Slide Maker) — 5 bullets:**
1. **Diversity (envs/objects/pairs) ≫ demo depth** — OOD gap power-laws in \(M,N,\text{pairs}\); demos plateau.  
2. **Must-cite:** Fig.5 exponents (PW −0.703/−0.844/−0.579; MA −0.697/−0.466/−0.683); **K=50** plateau; Table1 **85–92.5%**; ViT **0.66/0.81/0.90** vs U-Net **0.88/0.90/0.83**; frozen/LfS/LoRA **0.00/0.03/0.72**; MA→0.99 needs **1191** pairs.  
3. **Recipe:** \(M{=}N{=}32\) pairs × \(K{\approx}50\) (~1600 demos) → ~**90%** ZS; 1 unique obj/env; env gen harder than object.  
4. **Motion/policy:** **DP + DINOv2 FT + ACT TE** (not classical planner / not VLA primary); Ah=16@5Hz; TE=8; vision FT ≫ U-Net width.  
5. **BEHAVIOR'26 + honesty:** treat **32×50** as single-skill diversity unit; Empiricist RC1–RC3 mini-N; **never** invent multi-task/multi-emb Chinchilla or claim Tab.1 parity from stubs.

**GitHub title sketch:** `[ECE 605] - Data Scaling Laws Empiricist Making Three`

Full Slide Maker brief: `/workspace/data-scaling-laws-quiet/SLIDE_BRIEF.md`

---

## Paths

| Artifact | Path |
|----------|------|
| This pack | `/workspace/data-scaling-laws-quiet/KANTA_PACK.md` |
| PDF | `/workspace/data-scaling-laws-quiet/KANTA_PACK.pdf` |
| Slide brief | `/workspace/data-scaling-laws-quiet/SLIDE_BRIEF.md` |
| Senior Research | `/workspace/data-scaling-laws-quiet/DELIVERABLE.md` |
| Summarizer | `/workspace/papers/data-scaling-laws-2410.18647-summary.md` |
| Ablations | `/workspace/papers/data-scaling-laws-2410.18647-ablations.md` (**AUTHORITATIVE**) |
| Paper notes | `/workspace/data-scaling-laws-quiet/paper_notes.md` |
| Env outlines | `/workspace/data-scaling-laws-quiet/env_outline/` |
| Paper PDF | `/workspace/papers/data-scaling-laws-2410.18647.pdf` |
| Page figs | `/workspace/papers/data-scaling-laws-figs/page-*.png` |

---

*Pack synthesized 2026-09-29 PT (HST) for KanBot / ECE 605 Paper Manager — Nov 23 Data · Empiricist=3 (Nykolas) LONGEST · other seats empty · all five produced · Ablation Bot AUTHORITATIVE · no Discord/Drive · no invented multi-task/multi-emb Chinchilla laws.*
