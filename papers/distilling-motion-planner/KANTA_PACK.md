# Paper Manager pack — Distilling Motion Planner / MoPA-PD (Dec 9 · Alternative approaches)

**Paper:** Distilling Motion Planner Augmented Policies into Visual Control Policies for Robot Manipulation  
**arXiv:** https://arxiv.org/abs/2111.06383 · [project/code](https://clvrai.com/mopa-pd) · arXiv **2111.06383v1** (11 Nov 2021) · **CoRL 2021**  
**Authors:** I-Chun Arthur Liu²\*, Shagun Uppal¹\* (equal), Gaurav S. Sukhatme²†, Joseph J. Lim¹‡, Peter Englert², Youngwoon Lee¹§ — CLVR + RESL, **USC**  
**PDF:** `/workspace/papers/distilling-motion-planner-2111.06383.pdf` (= `/workspace/distill-mopa-quiet/paper.pdf`) · **16 pp** · ~10.5 MB  
**Session:** Dec 9 · due 9:30 AM · Type: **Alternative approaches**  
**Role weights:** **ALL EMPTY** — **still all five produced at balanced depth** (~equal Archaeologist / Stakeholder / Scientific Reviewer / Empiricist / Visionary); **Club Pack** (no LONGEST seat; no weight-3 over-index)  
**Kanta role:** Produce **balanced** five-role Club Pack; deepen privileged-MP→vision-student spine + Empiricist Ablation Bot smokes; Visionary → **BEHAVIOR obstructed household**; **skip Discord/Drive**  
**Themes:** privileged classical **RRT-Connect** at train · **MoPA-RL** teacher · visual **BC smoothing** · asymmetric SAC · **planner-free** pixel policy · shorter paths than expert · **DR zero-shot** distractors (**sim-to-sim**) · **CLEAR parent** Yamada MoPA-RL 2020 · **NOT** ancestor of ACT/DP/VLA/JEPA  
**Sources:** Summarizer (`/workspace/papers/distilling-motion-planner-2111.06383-summary.md`) + Senior Research (`/workspace/distill-mopa-quiet/DELIVERABLE.md`) + Ablation Bot (`/workspace/papers/distilling-motion-planner-2111.06383-ablations.md` — **AUTHORITATIVE**) + `paper_notes.md` / `env_outline/`  
**Sibling links:** `/workspace/dart-quiet/` · `/workspace/diffusion-policy-quiet/` · `/workspace/act-aloha-quiet/` · `/workspace/robogen-quiet/` · `/workspace/leworldmodel-quiet/` · `/workspace/behavior-1st-place-summary.md`

---

## 1. One-liner

**Privileged classical MP (RRT-Connect) + state RL at train → distill via visual BC smoothing + BC-guided asymmetric vision SAC into a planner-free pixel policy** that beats the privileged teacher on path length, hits ~100% ASR on three obstructed Sawyer tasks, and **DR zero-shots** distractors (**sim-to-sim** only).

**Contrast poles (consistency triangle — CLEAR labels; do not invent ancestry):**

| Pole | What they do | Relation to MoPA-PD |
|------|--------------|---------------------|
| **Classical MP (RRT-Connect / OMPL)** | Collision-free joint paths | **CLEAR:** train-time **inside** MoPA-RL expert; **removed** at MoPA-PD deploy |
| **MoPA-RL (Yamada et al. CoRL 2020)** | State SAC + MP-augmented action space | **CLEAR parent** being distilled |
| **Visual BC / CoL LfD** | Imitate demos / IL+RL mix | Stage-1 baseline / foil that fails or stays long-AEL |
| **ACT / Diffusion Policy** | Chunked / diffusion visuomotor from **teleop** demos | **CLEAR contrast** — different data source & architecture; **not** MoPA-PD children |
| **VLA / FM (π₀, OpenVLA, OXE)** | Large multi-task VLA from heterogeneous demos | **CLEAR contrast** — **NOT** ancestor; no cited chain |
| **JEPA / LeWM / Cosmos** | Latent / generative world models | **CLEAR contrast poles only** — do **not** place MoPA-PD in JEPA/Cosmos genealogy |
| **DART / DAgger** | Covariate-shift recovery (noise / query) | Shared vocabulary (Ross et al.); MoPA-PD fix = BC→online RL + expert buffer |
| **RoboGen / RelMoGen** | MP primitives **online** in generative sim | **Opposite deploy story** — they keep MP; MoPA-PD **amortizes** it away |

**Do NOT invent / claim:** MoPA-PD as ancestor of ACT/DP/π₀/OpenVLA/JEPA/Cosmos; sim-to-real success (paper = **sim-to-sim** DR); course smoke = Tab.1 5-seed Assembly **100%**; “vision-only learning” erasing privileged **asymmetric critic** at train; Discord/Drive checklist.

---

## 2. Summary (exec overview + key numbers)

**Liu\* & Uppal\* et al. (CoRL 2021 / arXiv 2111.06383)** attack obstructed-scene visuomotor learning: sparse exploration around obstacles + high-dim vision. Prior **MoPA-RL** (Yamada et al. CoRL 2020) solves exploration with a **privileged geometric-state** planner (RRT-Connect for large ∆q; direct SAC for contact-rich small moves), but that state and planner compute are unavailable at real deploy. **MoPA-PD** throws the planner and full geometric state away at inference via two stages:

1. **Visual BC** on MoPA-RL low-level (direct-A) trajectories — removes MP dependency **and** smooths jittery RRT paths.  
2. **Vision RL** (asymmetric actor-critic SAC) guided by **smoothed BC** trajectories in expert buffer Re; actor←BC, critic←MoPA-RL Q; **1:3** Re:Rπ mix; **small α**.

**Why Alternative approaches / Dec 9:** Teaches a durable **hybrid classical↔learned** pattern — use MP when geometry is known in sim; ship a visual policy that no longer needs it. Antecedent / alternative to “VLA-from-demo only,” **not** a child of foundation-model stacks.

| Axis | Number / claim |
|------|----------------|
| Venue / code | CoRL **2021**; arXiv **2111.06383v1**; https://clvrai.com/mopa-pd **CLEAR** |
| Expert parent | **MoPA-RL** Yamada et al. CoRL **2020** (**CLEAR**) |
| Planner | **RRT-Connect** (Kuffner & LaValle) — train-only inside MoPA-RL |
| Obs / robot | MuJoCo Sawyer **7-DoF**; RGB **32×32**; horizon **250**; γ **0.99** |
| Budget | MoPA-RL **1M** + visual **2M** = **3M** env steps; **5** seeds × **100** eps |
| **Ours Push** (Tab.1 ASR/AEL/ADR) | **100.0 / 32.0 / 110.8** |
| **Ours Lift** | **99.0 / 42.0 / 101.7** |
| **Ours Assembly** | **100.0 / 61.7 / 84.5** |
| Expert MoPA-RL Push AEL | **111.0** → Ours **32.0** (~**3.5×** shorter) |
| BC-Visual Lift ASR | **62.0** (shows Stage-2 necessity) |
| w/o BC smoothing Assembly | **ASR 0.0** (with: **100.0**) |
| MoPA-Asym. SAC / Asym. SAC | **0.0** ASR all (or near-zero) |
| DR zero-shot (Tab.2) | ASR **>96%** all; worst Lift Scen.1 **96.7** |
| Sub-opt teacher (Tab.4) | Lift @0.5M MoPA **41.4** → Ours **99.0**; Push @0.2M **69.2→100** |
| BC wall-clock (Tab.8) | Push **30** / Lift **39** / Asm **120** min |
| Scope | **Sim-only**; real-robot / vision-critic FT = future work |

**Must-cite tattoo:** Push **100 / 32.0 / 110.8** · Lift **99 / 42 / 101.7** · Assembly **100 / 61.7 / 84.5** · expert Push AEL **111→32** · Assembly ASR **0** w/o BC smooth · DR ≥**96.7** · **refuse sim-to-real laundering**.

**Do NOT claim:** course smoke = paper Tab.1 5-seed Assembly 100%; DR distractors = sim-to-real; MoPA-PD ancestry of modern VLA/JEPA; “vision-only” without noting privileged critic; full 3M×5×3 from Push-lite Partial.

---

## 3. Keywords

MoPA-PD; Motion Planner Augmented Policy Distillation; MoPA-RL; RRT-Connect; visual behavioral cloning; trajectory smoothing; asymmetric actor-critic; SAC; expert replay buffer; entropy α; domain randomization; obstructed manipulation; Sawyer Push / Lift / Assembly; privileged train-time planning; planner-free deploy; Alternative approaches; BEHAVIOR clutter transit (TENTATIVE recipe)

---

## 4. Deep dive — Privileged MP → planner-free vision (**PRIMARY SESSION CONTRIBUTION**)

**Callout:** Scientific product of *this* PDF = a **distillation recipe** that amortizes classical motion planning into a reactive visual controller. Session type = **Alternative approaches** because Dec 9 must teach **why classical robotics remains a training superpower** even when the shipped policy must be vision-only — **without** claiming MoPA-PD founded ACT/DP/VLA.

### Pipeline (Fig. 1 moral)

```
Classical RRT-Connect + state SAC (MoPA-RL, Yamada 2020)
        │  rollouts in direct-A space → D_mp
        ▼
Visual BC (MSE on 32×32 + proprio)  ──► smooths MP jitter; removes MP from action path
        │  BC trajs → expert buffer Re (NOT raw D_mp)
        ▼
Asymmetric SAC (actor: image+joints; critic: full state)
        │  init actor←BC, critic←MoPA-RL Q; 1:3 Re:Rπ; small α
        ▼
Planner-free pixel policy (+ optional DR) ──► deploy: no MP, no object state
```

### Components privilege table

| Component | Role | Privilege / deploy |
|-----------|------|--------------------|
| RRT-Connect | Collision-free large ∆q inside MoPA-RL | **Train-only** (via expert) |
| MoPA-RL SAC (state) | Expert exploring obstructed scenes | Train-only; D_mp + critic weights |
| Visual BC | Distill + **smooth**; actor init | Train; produces Re |
| Expert buffer Re | Guided exploration / anti-forgetting | Train; **BC traj**, not raw MP |
| Agent buffer R_π | SAC experience | Train |
| Asymmetric critic Q_ψ(s,a) | Value under full state | **Train-only** privilege |
| Visual actor π_θ(o, joints) | Deployed control | **Deploy** (no MP, no object state) |
| Domain randomization | Appearance invariance | Train; enables zero-shot distractors |
| Entropy α (small) | Exploit privileged prior | Train hyperparam |

### What is “alternative” here — CLEAR

| Claim | Status |
|-------|--------|
| MoPA-PD distills **MoPA-RL** (Yamada CoRL 2020) | **CLEAR** |
| Train-time **RRT-Connect**; removed at deploy | **CLEAR** |
| BC smoothing necessary (Assembly ablation ASR **0→100**) | **CLEAR** |
| Asymmetric actor-critic (Pinto et al.) used | **CLEAR** |
| Beats teacher on AEL (Push **111→32**) | **CLEAR** |
| DR zero-shot distractors >96% ASR (**sim-to-sim**) | **CLEAR** |
| Contrast to ACT / DP / VLAs as **different poles** | **CLEAR** (session genealogy) |
| MoPA-PD is **ancestor of** π₀ / OpenVLA / JEPA / Cosmos | **FALSE — do not claim** |
| Real-robot MoPA-PD success | **Not in paper** (future work) |
| Official BEHAVIOR’26 baseline | **TENTATIVE** / unverified |

### Architecture checklist (Empiricist pin — App. B)

| Piece | Spec (paper) |
|-------|----------------|
| MoPA-RL nets | 3× FC **256**, ReLU |
| Visual actor / BC | **3-layer CNN** → 3× FC **256** LeakyReLU → Gaussian head |
| SAC | Adam lr **1e-5**, γ **0.99**, buffer **10^6**, batch **256**, image **32×32** |
| BC | Adam lr **5e-4**, ~**1M** pairs, 9:1 split, batch **512**; pick earliest epoch with **100% val success** |
| Sample mix | Re : R_π = **1:3** |
| Reward scales | Push/Lift/Asm **0.8 / 0.5 / 1.0**; success thresh **0.05**; bonus **+150** |
| Action dims | Push **7** / Lift **8** / Asm **7** |

**Exam bite:** "MoPA-PD = MoPA-RL expert (RRT-Connect+SAC) → visual BC smoothes jitter → asym. SAC with BC buffer + weight init + small α → ~100% Sawyer clutter, shorter than expert (Push AEL **111→32**), DR zero-shot distractors (**sim-to-sim**); MP gone at deploy; **CLEAR parent** Yamada 2020; **NOT** ACT/DP/VLA/JEPA ancestor."

---

## 5. Deep dive — Ablations (Ablation Bot AUTHORITATIVE)

**Callout:** Ablation Bot extract **AUTHORITATIVE** at `/workspace/papers/distilling-motion-planner-2111.06383-ablations.md`. Focus ranks: (1) planner-on vs distill, (2) BC vs MoPA-PD, (3) obstructed SR, (4) domain randomization. Course Empiricist: **Y #1/#2/#4**; **Partial #3/#5**; cite **#6/#7**; **N** full 3-task×5-seed×3M.

| Rank | Factor | Paper numbers | Course smoke |
|------|--------|---------------|--------------|
| **1** | Planner-on vs distill (Tab.1 / Fig.3) | Ours ASR **100/99/100**; MoPA Asym **0/0/0**; MoPA-RL **98.2/95.0/99.8**; Push AEL **32 vs 111** | **Y — RC1 PRIMARY** |
| **2** | BC-Visual vs Ours | BC ASR **99.4/62.0/97.0** vs **100/99/100**; BC AEL ~**108–118** vs **32/42/61.7** | **Y — RC2 PRIMARY** (1 task) |
| **3** | Domain-rand zero-shot (Tab.2) | ASR **>96%**; Orig **99.7/99.3/100**; S1 **100/96.7/100**; S2 **99.3/97.3/100** | **Partial — RC4** |
| **4** | BC trajectory smoothing on/off | w/o Asm **0.0**; with **100.0** | **Y — RC3 PRIMARY** |
| **5** | Actor–critic weight init | w/o: ~0 SR till **1.2M**; with: ≈1.0 before **1.0M** | **Partial — RC5** |
| **6** | Entropy α | smaller log(α) faster/stabler; teacher log(α) Push **−0.63** / Lift **−2.7** / Asm **−0.29** | cite / optional |
| **7** | Sub-optimal MoPA expert (Tab.4) | Lift **41.4→99.0**; Push **69.2→100** | Partial / cite |
| **Defer** | Full 3M×5×3 + long MoPA-RL retrain; real-robot | — | **N** |

**Min Empiricist package:** RC0 → **RC1 (#1) + RC2 (#2) + RC3 (#4)** → Partial RC4/RC5 → cite #6/#7. Log **ASR, AEL, ADR, EE jerk, MP_calls_at_infer=0, Re source, init, log(α), peak VRAM**.

**Recipe exam bite:** "Smoothing is load-bearing (Asm **0** without it); MoPA-Asym SAC = **0** proves ‘just put MP in the visual loop’ fails; distillation collapses MP into a reactive policy — that *is* the point."

---

## 6. Role-ready sections (all five — **BALANCED Club Pack**)

> **Balanced pack.** Equal depth across five roles. No seat weight-3. Ablation Bot focus areas (planner-on vs distill, BC vs MoPA-PD, obstructed SR, domain-rand) shared across Empiricist RCs + Reviewer/Archaeologist honesty.

### 6.1 Archaeologist — balanced (~12–14)

> **Seat charge:** Place MoPA-PD carefully in classical-MP ↔ learned-control history. **CLEAR parent = MoPA-RL Yamada 2020**. Fence false FM/JEPA ancestry. Equal depth with other four.

#### Priors — CLEAR

| Prior | Role | Label |
|-------|------|-------|
| **MoPA-RL** Yamada et al. CoRL 2020 | Direct parent expert | **CLEAR** |
| **RRT-Connect** Kuffner & LaValle 2000 | Collision-free large moves | **CLEAR** |
| **Asymmetric actor-critic** Pinto et al. RSS 2018 | Vision actor / state critic | **CLEAR** |
| **SAC** Haarnoja et al. ICML 2018 | RL backbone | **CLEAR** |
| **BC / ALVINN** Pomerleau 1989; **covariate shift** Ross et al. 2011 | Stage-1 + diagnosis | **CLEAR** |
| RelMoGen / PBCS; neural MP (Qureshi, Strudel, Jurgenson & Tamar) | Related MP+RL / MP distill | **CLEAR** related |
| CoL (Goecks et al. 2020); DQfD; DAPG | LfD baselines that fail here | **CLEAR** |
| IKEA furniture env Lee et al.; Tobin DR | Task / transfer priors | **CLEAR** |

#### Genealogy (CLEAR framing — no false FM lineage)

```
Classical MP (RRT-Connect)
   + state SAC in clutter (MoPA-RL, Yamada 2020)
        → privileged expert demos + critic
             → visual BC (smooth + remove MP)
                  → BC-guided asymmetric vision SAC (MoPA-PD)
                       → planner-free pixel policy (+ DR zero-shot sim-to-sim)
```

#### Subsequent / contrast — label carefully

| Item | Note | Label |
|------|------|-------|
| ACT / Diffusion Policy | Teleop-demo visuomotor IL; different pole | **CLEAR contrast** |
| π₀ / OpenVLA / OXE | Large VLA; **no cited ancestry from MoPA-PD** | **FALSE ancestry — do not claim** |
| LeWM / JEPA / Cosmos | Latent / generative WM poles | **CLEAR contrast only** |
| RoboGen MP menu | Keeps MP online — opposite deploy | **CLEAR parallel / contrast** |
| Soft influence on “privileged sim → vision deploy” recipes | Pattern family; verify per paper | **TENTATIVE** |

**Archaeologist one-liner:** *Classical MP is the privileged teacher, not the product — MoPA-PD is the CoRL’21 bridge from Yamada MoPA-RL to planner-free vision, and a contrast ancestor/alternative to teleop-demo VLAs, not a JEPA/Cosmos child.*

**Archaeologist exam bite:** "MoPA-PD = RRT-Connect MoPA-RL → BC smooth → asym SAC → planner-free vision; **CLEAR parent** Yamada 2020; Push AEL **111→32**; Asm **0** w/o smooth; **NOT** ACT/DP/VLA/JEPA ancestor."

---

### 6.2 Stakeholder — balanced (~12–14)

> **Seat charge:** Why schedule in Alternative approaches; buy the **pattern**, not the 2021 Sawyer checkpoint.

**Buy / why schedule Dec 9:**
- Locks **privileged-MP → vision student** before pure VLA weeks.  
- Clean answer to “why not only VLA-from-demo?”: in **heavily obstructed sim**, privileged MP exploration is what makes demos exist; distillation is the productization step.  
- HIGH motion-planning / obstruction hook without a full TAMP stack.  
- Hard numbers: Push **100/32/110.8**, Asm **0→100** with smoothing, DR ≥**96.7**.

**Buy / build vs modern stacks**

| Situation | Recommendation |
|-----------|----------------|
| Sim has accurate geometry; need vision deploy; clutter is the hard part | **Buy the MoPA-PD pattern**: MP/TAMP or MoPA-style teacher → BC smooth → vision RL/IL |
| Abundant real teleop / OXE-scale demos; open-world language | Prefer **ACT / DP / VLA**; MoPA-PD complementary sim recipe |
| No reliable collision geometry even in sim | Stage-0 dies — don’t force it |
| Need foundation-model generalization across embodiments | MoPA-PD is **narrow Sawyer MuJoCo** — use as **idea**, not checkpoint |

**Risks / costs:**
- Sim-only; authors flag real fine-tune / vision-critic as future work — **sim-to-real tax unpaid**.  
- 32×32 images — not modern wrist/cam stacks.  
- Depends on working MoPA-RL (or other MP-RL) teacher; Stage-0 engineering cost.  
- Asymmetric critic assumes sim state — real online RL needs a different critic story.  
- Don’t pitch as competing with π₀-scale generalists; pitch as **clutter exploration prior**.

**Decision ask:** **Required Alternative-approaches classic** for privileged-teacher distillation; pair with DART (recovery demos) and ACT/DP (visuomotor IL) as **contrast**, not ancestors. Buy the **pattern**.

**Stakeholder exam bite:** "Schedule MoPA-PD to lock **privileged-MP → vision student** before VLA weeks; buy the pattern, not the Sawyer checkpoint; police sim-to-real laundering of Tab.2."

---

### 6.3 Scientific Reviewer — balanced (~12–14)

**Claims under review**
1. Distilling MP-augmented state RL into vision via BC+guided RL removes MP/state at deploy.  
2. BC smoothing is necessary (esp. Assembly).  
3. Weight init + expert buffer + small α yield high sample efficiency vs Asym. SAC / CoL / MoPA-Asym. SAC.  
4. DR enables zero-shot distractor / appearance transfer at >96% ASR (**sim-to-sim**).  
5. Method can exceed expert path quality (AEL/ADR).

**What holds**
- Table 1 is decisive: vision-from-scratch and “MP inside visual MoPA” both **zero out**; MoPA-PD hits ~100% with **better AEL than the state expert**.  
- BC-smoothing ablation is a clean causal knife (Assembly **0 vs 100**).  
- Init ablation (Fig. 7) strongly supports transferring MoPA-RL critic + BC actor.  
- Sub-optimal expert Table 4 shows distillation is not mere cloning.  
- Related-work honesty: distinguishes neural MP papers that stay state-based / non-manip.

**What is soft / threatened**
- **No real-robot results** — transfer claims are **sim-to-sim** DR, not sim-to-real.  
- Single embodiment (Sawyer), three tasks, **32×32** RGB — external validity to household cams unknown.  
- CoL / Asym. SAC hyperparameters may be disadvantaged in clutter; still, zeros across seeds are stark.  
- Asymmetric critic = lingering privilege at train; paper’s own future work admits need for vision critic.  
- “Outperforms SOTA” scoped to 2021 obstructed Sawyer suite — not modern visuomotor IL.

**Verdict:** Strong **CoRL systems/methods** paper for the privileged-teacher distillation claim in obstructed sim. Accept the mechanism + ablations. **Do not** upgrade to “solved real clutter manipulation” or “ancestor of modern VLAs.”

**Reviewer exam bite:** Mechanism and ablations are crisp; sim-to-sim DR is real; real-world and modern-cam generalization remain open — grade as **Alternative approaches classic**, not as foundation-model prehistory. Demand **MP@infer=0** evidence, not only ASR.

---

### 6.4 Empiricist — balanced (~12–14; Ablation Bot AUTHORITATIVE)

> **Seat empty this session** but produce full course smoke cards for Paper Manager continuity. **Balanced** — equal depth with other roles; richest logging on RC1/RC2/RC3. Ablation Bot ranks **1–7** govern order.

#### Claim under measurement

MoPA-PD (planner-off) beats Asym SAC / matches or beats MoPA-RL on Push ASR @ matched visual budget; AEL directionally ≪ teacher/BC; **MP_calls@infer = 0**; BC-smoothed Re ≥ raw; DR Partial holds OOD within tolerance — **directional**, **not** Tab.1 5-seed Assembly 100%.

#### Hypotheses (falsifiable; Ablation Bot ranks)

1. **H-planner (#1):** MoPA-PD ≫ MoPA Asym / Asym SAC on Push ASR; AEL(MoPA-PD) ≪ AEL(MoPA-RL demos); **MP_calls@infer = 0**.  
2. **H-bc (#2):** On 1 task, MoPA-PD > BC-Visual on Lift-style hardness **or** on AEL↓/ADR↑ even when BC ASR is already high.  
3. **H-smooth (#4):** Re = BC-smoothed > Re = raw MoPA-RL on ASR and/or convergence; Assembly moral: raw Re → **ASR 0**.  
4. **H-dr (#3 Partial):** DR-trained policy retains high ASR on original + distractor stub (Tab.2 >96% moral — directional).  
5. **H-init (#5 Partial):** BC+MoPA-critic init reaches nonzero ASR far earlier than random (Fig.7).  
6. **H-alpha (#6):** Smaller log(α) → faster/stabler visual SAC when teacher prior exists.  
7. **H-teacher (#7):** Suboptimal MoPA teacher still yields high ASR after distill (Tab.4 Lift 41.4→99).

**Falsifiers:** MoPA-Asym ≥ Ours; BC AEL ≤ Ours when both succeed; raw Re ≥ smooth; DR collapses on Scen.1; random init ≈ BC init; large α best; distill needs perfect teacher; claiming Tab.1 Assembly 100% from Push-lite.

#### Prioritized matrix

| # | Experiment | Ablation Bot | Smoke | Pri |
|---|------------|--------------|-------|-----|
| **RC1** | `planner_removal_bakeoff` | **#1** | **Y — PRIMARY** | **P0** |
| **RC2** | `bc_vs_mopapd` (1 task) | **#2** | **Y — PRIMARY** | **P0** |
| **RC3** | `bc_smoothing_ablation` | **#4** | **Y — PRIMARY** | **P0** |
| **RC4** | `domain_rand_transfer` | **#3** | **Partial** | P1 |
| **RC5** | `weight_init_ablation` | **#5** | **Partial** | P1 |
| **RC6** | `entropy_alpha_sweep` | **#6** | cite / optional | P2 |
| **RC7** | `suboptimal_teacher` | **#7** | Partial / cite | P2 |
| — | Full 3M×5×3; real-robot; fresh long MoPA-RL | — | **N** | Skip |

**Min package:** RC0 → **RC1 + RC2 + RC3** → Partial RC4/RC5 → cite #6/#7.

#### Exact reproduce stack (EDIT)

```bash
# Official
# clone / pin https://clvrai.com/mopa-pd (+ Yamada MoPA-RL deps); prefer frozen demos
export MOPA_PD_WORK_ROOT=$SCRATCH/mopa-pd-smoke
cd /workspace/distill-mopa-quiet
bash env_outline/setup_mamba.sh
# mamba activate mopa-pd-scout
mkdir -p logs runs checkpoints demos
# Outlines only — do NOT sbatch from quiet box
# NEVER claim Tab.1 Assembly 100% / DR≥96.7 absolute / sim-to-real from Push-lite smoke
# ALWAYS log MP_calls_at_infer for MoPA-PD (=0)
```

#### Run cards (from DELIVERABLE)

##### RC0 — `demo_ingest_smoke`

| Field | Spec |
|-------|------|
| Goal | Load/collect MoPA-RL (or stand-in) demos → `(s,o,a,r,s')` + 32×32 RGB; direct-A labels |
| Pass | Schema OK; 0 NaNs; wall **<1 h** |
| Script | `env_outline/slurm_demo_ingest_smoke.sh` |

##### RC1 — `planner_removal_bakeoff` (**#1 — PRIMARY**)

| Field | Spec |
|-------|------|
| Paper | Tab.1 — Ours **100/99/100**; MoPA Asym **0/0/0**; Push AEL **32 vs 111** |
| Goal | Matched-budget Push: MoPA-PD vs Asym SAC (+ opt MoPA-Asym); prove planner-off distill wins |
| Hard req | Log **MP_calls_at_infer=0** for MoPA-PD |
| Pass | MoPA-PD ASR ≫ Asym SAC; AEL directionally ≪ teacher/BC |
| Script | `env_outline/slurm_baseline_planner_removal.sh` |

##### RC2 — `bc_vs_mopapd` (**#2 — PRIMARY**)

| Field | Spec |
|-------|------|
| Paper | BC ASR **99.4/62.0/97.0** vs Ours **100/99/100**; BC AEL ~**108–118** vs **32/42/61.7** |
| Goal | **1 task** head-to-head; prioritize **AEL/ADR** (and Lift if budget) |
| Pass | MoPA-PD AEL < BC AEL when both succeed **or** MoPA-PD ASR ≫ BC on Lift-lite |
| Script | `env_outline/slurm_bc_vs_mopapd.sh` |

##### RC3 — `bc_smoothing_ablation` (**#4 — PRIMARY**)

| Field | Spec |
|-------|------|
| Paper | w/o smoothing Assembly **0**; with **100**; Fig.8 jitter |
| Goal | Re=BC vs Re=raw; Push primary; Assembly cite/stub |
| Pass | Smooth ≥ raw on ASR or speed; jerk ↓ |
| Script | `env_outline/slurm_bc_smoothing_ablation.sh` |

##### RC4 / RC5 — Partial DR / init

| Card | Spec |
|------|------|
| RC4 DR | Short DR Push; eval original + distractor stub; OOD within ~10–15 pp of ID (directional) — `slurm_domain_rand_transfer.sh` |
| RC5 init | `{init: on\|off}` @ matched Push; init dominates early ASR — `slurm_weight_init_ablation.sh` |

#### Numbers to memorize (exam)

| Fact | Value |
|------|-------|
| Ours Push / Lift / Asm | **100/32.0/110.8** · **99/42/101.7** · **100/61.7/84.5** |
| Expert Push AEL | **111 → 32** |
| w/o BC smooth Asm | **ASR 0.0** |
| MoPA-Asym / Asym SAC | **0** ASR |
| BC Lift ASR | **62.0** |
| DR worst cell | Lift Scen.1 **96.7** |
| Sub-opt Lift | MoPA **41.4 → Ours 99.0** |
| Scope | Course = Push lite; **sim-to-sim** DR; **no** sim-to-real laundering |

#### GPU / RAM / TIME honesty (course)

| Workload | VRAM | Wall (order) | Smoke |
|----------|------|--------------|-------|
| Demo ingest + BC | 4–12 GB | 0.5–2 h | **Y** |
| RC3 smoothing Push | 8–16 GB | 4–12 h | **Y** |
| RC1 planner-removal | 8–16 GB | 8–20 h | **Y** |
| RC2 BC vs MoPA-PD | 8–16 GB | 4–16 h | **Y** |
| RC4/RC5 Partial | 8–16 GB | 4–16 h | Partial |
| Full 3M×5×3 + Assembly | 16–24 GB+ | ≫2–5 days | **N** |
| Fresh MoPA-RL + RRT from scratch | 8–16 GB + CPU MP | +1–3 days | **N** if demos exist |
| Real-robot | — | — | **N** |

**Empiricist exam bite:** "Quote **100/32/110.8**, **Assembly 0 without BC smooth**, DR ≥**96.7**; RC1+RC2+RC3 = planner-off + BC bake-off + smoothing with **MP@infer=0**; refuse to launder sim-to-sim distractors into sim-to-real; Ablation Bot AUTHORITATIVE ranks 1–7."

---

### 6.5 Visionary — balanced (~12–14) — BEHAVIOR obstructed household

> **Seat charge:** Decision-oriented **BEHAVIOR obstructed household** hooks. Privileged-info-at-train → reactive vision deploy. Pair with DART recovery. **No Discord/Drive checklist.**

#### BEHAVIOR connection (honest)

BEHAVIOR-class household activities are dense with **obstruction, clutter, and multi-stage contact**. MoPA-PD’s durable idea is not the Sawyer checkpoint — it is:

> **When sim geometry is known, use classical MP (or TAMP) as a privileged explorer/teacher; distill to a vision policy that runs without that privilege.**

| BEHAVIOR need | MoPA-PD-shaped response |
|---------------|-------------------------|
| Collision-heavy rooms | Train-time RRT/BIT\*/TAMP teachers for reach/place primitives |
| Vision-only deploy | Asymmetric train / vision actor deploy; then real fine-tune |
| Long / multi-stage activities | Stage-wise teachers + **BC smooth** before vision RL/VLA distill |
| Distractors / appearance shift | Keep **DR** (or modern DA) — but Tab.2 ≠ layout/clutter OOD |
| Hybrid with modern policies | MP-teacher demos as **one column** beside teleop/VLA demos — don’t replace π₀.₅ with 32×32 SAC |

#### Visionary bets (balanced; not monopoly)

1. **Sim-privileged planner teacher → deploy-reactive VLA:** In OmniGibson / BEHAVIOR-sim, run privileged MP/MoPA-like teacher for hard clutter transit; distill into π₀.₅ / N1.7 / DP with **BC then light RL/FT**.  
2. **Mandatory smoothing card:** Never dump raw RRT waypoints into Re; BC or trajectory-optimize first; log jerk.  
3. **Privileged critic / asymmetric train only:** Keep state critic in sim FT; **forbid** state at BEHAVIOR eval — reviewer’s MP@infer=0 analog.  
4. **Pair with DART recovery:** Planner-distill covers **transit**; DART-style recovery teleop covers **slip/miss**.  
5. **Do not ship online RRT on the robot** for the leaderboard unless latency allows; MoPA-PD’s point is **amortize**.  
6. **DR is not enough:** Appearance DR ≠ layout/clutter OOD — add instance / furniture variation.  
7. **Metric honesty:** Report collision rate, path length, stage success — not only final ASR.  
8. **Substrate:** Race on π₀.₅ / GR00T N1.7; treat MoPA-PD as **distill recipe**, not a 32×32 reimplementation.  
9. **Empiricist honesty:** Push-lite validates smoothing/init/planner-removal; transfer claims need BEHAVIOR-sim Partial — never Tab.1 Assembly 100% as proxy.  
10. **One narrative line:** MoPA taught **plan when the action is big**; MoPA-PD taught **practice the plan until the reactive visual policy no longer needs the planner**; 2026 should **distill privileged clutter transit into VLA deploy policies**, then **rehearse recoveries** (DART) where distill still fails.

#### Follow-up research

1. Replace 32×32 CNN with modern encoders; keep privileged MP teacher.  
2. Vision critic / self-supervised critic for real asymmetric→symmetric transition.  
3. MP-teacher demos into RLDS/OXE-schema shards labeled `teacher=mopa|rrt`.  
4. Combine BC-smooth with action chunking (ACT/DP) on planner-taught traj.  
5. Measure whether privileged-MP demos help BEHAVIOR obstruction categories more than equal hours of teleop.  
6. Planner jitter metrics as first-class data-quality filters before IL.

#### New apps (**beyond Discord/Drive**)

- Furniture / cabinet insert curricula in household sim.  
- Cluttered fridge / shelf reach-behind with MP teachers.  
- Synthetic hard-negative obstacle layouts for VLA eval.  
- Latency-critical deploy: bake away RRT online cost into a reactive visual policy.

*(No Discord / Drive checklist content.)*

**Visionary one-liner:** *For BEHAVIOR obstructed households, keep classical MP as a **sim teacher**, distill (BC-smooth + modern IL/RL/VLA heads) into vision students — MoPA-PD is the 2021 proof that the teacher can disappear at deploy without disappearing from the curriculum.*

**Visionary exam bite:** "MoPA-PD = **planner-distill recipe** for clutter transit beside **recovery-demo** (DART) and modern VLA substrates; cite Push **100/32/110.8**, Asm **0** w/o smooth, DR ≥**96.7** as morals; Empiricist smokes directional only; **no Discord/Drive**."

---

## 7. Empiricist run pack (env / scripts / GPU — from DELIVERABLE)

### Env setup

```bash
export MOPA_PD_WORK_ROOT=$SCRATCH/mopa-pd-smoke
cd /workspace/distill-mopa-quiet
bash env_outline/setup_mamba.sh
# mamba activate mopa-pd-scout
mkdir -p logs runs checkpoints demos
# EDIT: pin clvrai.com/mopa-pd + MoPA-RL; prefer frozen demos
# Outlines only — do NOT sbatch from quiet box
```

| File | Role |
|------|------|
| `env_outline/conda_env.yaml` | mamba sketch (Torch + MuJoCo + optional MoPA/RRT) |
| `env_outline/setup_mamba.sh` | env create; MuJoCo EGL/OSMesa notes |
| `env_outline/run_checklist.md` | GPU / TIME honesty preflight |
| `env_outline/README.md` | cluster usage |
| `env_outline/slurm_demo_ingest_smoke.sh` | RC0 |
| `env_outline/slurm_baseline_planner_removal.sh` | **RC1 / #1 PRIMARY** |
| `env_outline/slurm_bc_vs_mopapd.sh` | **RC2 / #2 PRIMARY** |
| `env_outline/slurm_bc_smoothing_ablation.sh` | **RC3 / #4 PRIMARY** |
| `env_outline/slurm_domain_rand_transfer.sh` | RC4 / #3 Partial |
| `env_outline/slurm_weight_init_ablation.sh` | RC5 / #5 Partial |
| `env_outline/slurm_entropy_alpha_sweep.sh` | RC6 / #6 optional |
| `env_outline/slurm_suboptimal_teacher.sh` | RC7 / #7 Partial |

### Success criteria vs paper claims

| Paper claim | Minimal course criterion | Not required |
|-------------|--------------------------|--------------|
| Distill removes MP + state at infer | MoPA-PD eval with **MP_calls=0** and image+joint obs only | Bit-identical RRT teacher |
| BC smoothing fixes jitter / hard tasks | RC3: smooth Re ≥ raw on Push ASR/speed; jerk ↓ | Assembly 100% from smoke |
| Weight init for sample-efficiency | RC5: init ≫ random early ASR | Exact Fig.7 1.2M cutoff |
| Low α helps with teacher prior | Cite / optional RC6 | Full Fig.5 grid |
| Beats pure visual RL / visual+MP / naive LfD | RC1: MoPA-PD ≫ Asym SAC; AEL < BC | CoL full reimpl; MoPA-Asym if MP broken |
| Beats privileged MoPA on AEL/ADR | Directional AEL ↓ vs BC and teacher demos | Tab.1 absolute ADR 110.8 |
| Suboptimal teacher distillable | RC7 Partial directional | Tab.4 full |
| DR zero-shot ≥96% ASR | RC4 Partial: OOD holds within tolerance | Absolute ≥96% / Scen.2 furniture |
| Full paper protocol | **Document N** | Never course |

**Pass:** documented smoke with Ablation Bot **#1 + #2 + #4** confirmed directionally + **MP@infer=0** + logged knobs.  
**Fail / overclaim:** “reproduced CoRL Tab.1 5-seed Assembly 100%” / “sim-to-real” from Push lite Partial.

---

## 8. Slide Maker brief

| Role | Weight / timing guide | Focus |
|------|----------------------|-------|
| Archaeologist | **balanced · ~12–14** | CLEAR/TENTATIVE; MoPA-RL parent; false FM ancestry fence; genealogy; exam bite |
| Stakeholder | **balanced · ~12–14** | Buy pattern not checkpoint; risks (sim-only, 32×32); decision ask |
| Scientific Reviewer | **balanced · ~12–14** | Tab.1+smoothing+init; verdict Accept Alternative classic; fence sim-to-real / VLA ancestry |
| Empiricist | **balanced · ~12–14** | H-planner/bc/smooth; RC1+RC2+RC3; GPU honesty; must-cite tattoo; exam bite |
| Visionary | **balanced · ~12–14** | BEHAVIOR obstructed household; planner-distill + DART; **no Discord/Drive** |

**Spine (paste into Slide Maker) — 5 bullets:**
1. **Privileged classical MP (RRT-Connect) at train → visual BC smooth → asym SAC → planner-free pixel policy**; shorter paths than expert; DR zero-shot distractors (**sim-to-sim**).  
2. **Must-cite:** Push **100/32.0/110.8**; Lift **99/42/101.7**; Assembly **100/61.7/84.5**; expert Push AEL **111→32**; Assembly ASR **0** w/o BC smooth; DR ≥**96.7**; **refuse sim-to-real laundering**.  
3. **Taxonomy:** classical MP \| **MoPA-RL parent (Yamada 2020)** \| MoPA-PD distill \| ACT/DP/VLA (**CLEAR contrast, not children**) \| JEPA/LeWM/Cosmos (**contrast poles only**).  
4. **Lineage (Club Pack — all seats empty, balanced):** CLEAR parent MoPA-RL; inventum = BC-smooth + asym SAC distill that **removes** MP at deploy and **beats** teacher AEL; **NOT** ancestor of ACT/DP/VLA/JEPA.  
5. **BEHAVIOR’26 + honesty:** MoPA-PD = **planner-distill recipe** for clutter transit beside DART recovery + modern VLA substrates; Empiricist RC1–RC3 directional; **do not invent** sim-to-real / Assembly 100% from Push smoke — **no Discord/Drive**.

**GitHub title sketch:** `[ECE 605] - Distilling Motion Planner / MoPA-PD Making Club Pack`

Full Slide Maker brief: `/workspace/distill-mopa-quiet/SLIDE_BRIEF.md`

---

## Paths

| Artifact | Path |
|----------|------|
| This pack | `/workspace/distill-mopa-quiet/KANTA_PACK.md` |
| PDF | `/workspace/distill-mopa-quiet/KANTA_PACK.pdf` |
| Slide brief | `/workspace/distill-mopa-quiet/SLIDE_BRIEF.md` |
| Senior Research | `/workspace/distill-mopa-quiet/DELIVERABLE.md` |
| Summarizer | `/workspace/papers/distilling-motion-planner-2111.06383-summary.md` |
| Ablations | **AUTHORITATIVE** — `/workspace/papers/distilling-motion-planner-2111.06383-ablations.md` |
| Paper notes | `/workspace/distill-mopa-quiet/paper_notes.md` |
| Env outlines | `/workspace/distill-mopa-quiet/env_outline/` |
| Paper PDF | `/workspace/papers/distilling-motion-planner-2111.06383.pdf` |
| Text extract | `/workspace/papers/distilling-motion-planner-2111.06383.txt` · `paper_clean.txt` |
| Page figs | `/workspace/papers/mopa-pd-figs/page-01.png` … `page-16.png` |
| Sibling links | `/workspace/dart-quiet/` · `/workspace/diffusion-policy-quiet/` · `/workspace/act-aloha-quiet/` · `/workspace/robogen-quiet/` · `/workspace/leworldmodel-quiet/` |
| Code | https://clvrai.com/mopa-pd |

---

*Pack synthesized 2026-09-29 PT (HST) for KanBot / ECE 605 — Dec 9 Alternative approaches · ALL seats EMPTY · balanced five-role Club Pack · Ablation Bot AUTHORITATIVE ranks 1–7 · CLEAR parent MoPA-RL Yamada 2020 · NOT ACT/DP/VLA/JEPA ancestor · must-cite Push 100/32.0/110.8 · Lift 99/42/101.7 · Assembly 100/61.7/84.5 · expert Push AEL 111→32 · Assembly ASR 0 w/o BC smooth · DR ≥96.7 · refuse sim-to-real laundering · Visionary BEHAVIOR obstructed household · no Discord/Drive · outlines only — no cluster execution.*
