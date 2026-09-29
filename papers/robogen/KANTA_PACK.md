# Paper Manager pack — RoboGen (Nov 18 · Data)

**Paper:** RoboGen: Towards Unleashing Infinite Data for Automated Robot Learning via Generative Simulation  
**arXiv:** https://arxiv.org/abs/2311.01455 · [project](https://robogen-ai.github.io/) · [code](https://github.com/Genesis-Embodied-AI/RoboGen) · **ICML 2024** (PMLR 235, Vienna)  
**Authors:** Yufei Wang*, Zhou Xian*, Feng Chen*, Tsun-Hsuan Wang, Yian Wang, Katerina Fragkiadaki, Zackory Erickson, David Held, Chuang Gan (*equal) — CMU / Tsinghua IIIS / MIT CSAIL / UMass / MIT-IBM  
**PDF:** `/workspace/papers/robogen-2311.01455.pdf` (= `/workspace/robogen-quiet/paper.pdf`) · 48 pp · arXiv **2311.01455v3** (14 Jun 2024)  
**Session:** Nov 18 · due 9:30 AM · Type: **Data**  
**Role weights:** **ALL EMPTY** — **still all five produced at balanced depth** (~equal Empiricist / Reviewer / Visionary / Stakeholder / Archaeologist)  
**Kanta role:** Produce **balanced** five-role pack (no PRIMARY seat); deepen motion + synthetic-data spines + Empiricist stage-isolated smokes  
**Themes:** generative simulation · propose–generate–learn · synthetic skill demos opposite UMI/DROID/OXE real poles · BIT* MP primitives · long-horizon skill decomp  
**Sources:** Summarizer (`/workspace/papers/robogen-2311.01455-summary.md`) + Senior Research (`/workspace/robogen-quiet/DELIVERABLE.md`) + `paper_notes.md` / `env_outline/` — **Ablation Bot MISSING** (paper §4 + App B extract used)  
**Sibling links:** `/workspace/umi-quiet/` · `/workspace/droid-quiet/` · `/workspace/oxe-quiet/` · `/workspace/diffusion-policy-quiet/` · `/workspace/act-aloha-quiet/` · `/workspace/dart-quiet/` · `/workspace/pi05-quiet/` · `/workspace/openpi-comet-smoke/`

---

## 1. One-liner

**LLM proposes; SAC / BIT* / Adam learn** — a generative sim factory that mints **task–scene–reward–demo** tuples so robots practice novel skills without real-world collection cost; **complement** (don't replace) UMI/DROID/OXE real poles for BEHAVIOR.

**Contrast poles:**

| Pole | Who authors | Where | Product |
|------|-------------|-------|---------|
| **RoboGen** | **LLM/VLM** + physics sim | Generative sim (Objaverse / PartNet / soft / loco) | Endless **synthetic** skill demos + policies |
| **UMI** | **Human** + handheld gripper | Anywhere (cafe, fountain, home) | Action-rich **real** wrist demos |
| **DROID** | **Robot** teleop (Franka fleet) | 564 real scenes / 52 buildings | Robot-in-wild **real** volume |
| **OXE** | Multi-lab agglomeration | 22 robots / many labs | Cross-embodiment **real** mix |
| **Behavior-100** | Human designers | Hand-authored sim | Household ecology — RoboGen Table 1 foil |

---

## 2. Summary (exec overview + key numbers)

**RoboGen** is a **generative robotic agent** that learns diverse skills via **generative simulation**. Foundation models do **not** emit torques — they **auto-generate tasks, scenes, and training supervisions**; physics + classical/learned optimizers solve. Self-guided **propose → generate → learn** cycle (Fig. 2):

1. **Task proposal** — GPT-4 seeded by robot + PartNet-Mobility / RLBench object (or 11 example tasks for loco/soft) → name, NL, assets, joints/links.  
2. **Scene generation** — Objaverse top-\(k{=}10\) Sentence-BERT + Gemini-Pro verify; GPT-4 sizes / joint inits / spatial; soft goals Midjourney → Zero-1-to-3 → DMTet.  
3. **Training supervision** — GPT-4 decomposes; **selects** SAC RL / BIT* MP primitives / Adam traj-opt; writes rewards (EMD for soft).  
4. **Skill learning** — sequential sub-tasks; each \(N{=}8\) runs; best end-state → next init → demos.

**Why Data session:** Synthetic skill-demo factory opposite real poles — for novel articulated / soft / loco tasks where teleop is too costly. **Motion is PRIMARY:** BIT* primitives + algo-select (Fig. 5 RL-only collapse) + long-horizon decomp (avg **3.13** substeps).

| Axis | Number / claim |
|------|----------------|
| Diversity Self-BLEU (↓) | **0.284** (best vs Behavior-100 **0.299**, RLBench **0.317**, GenSim **0.378**) |
| SentBert / ViT / CLIP scene (↓) | **0.165 / 0.193 / 0.762** — wins all Table 1 columns @ **106** tasks |
| Skill SR (human video) | **0.774** on **69** tasks (manip **0.745**/50; soft ≈**0.886**/7; loco ≈**0.833**/12) |
| Validity audit | **19 / 155** fails (**13** scene + **6** reward/decomp) |
| Fig. 5 algo | **RL-only collapses** on 12 articulated; MP+RL selection critical |
| Long-horizon structure | Avg **3.13** substeps (up to **8–10**); ~**1.5** RL + ~**1.63** MP |
| Wall-clock (App.) | Plan ≤**10 min**/task; RL subgoal ~**2–3 h**; typical **4–5 h** (8×2.5 GHz CPUs) |
| SAC knobs | MLP **[256]³**, lr **3e−4**, **1M**/sub, horizon **100**/fs **2**, 6D EE |
| MP knobs | BIT* (OMPL); pre-contact **0.03 m** along normal |
| Soft / loco | Adam **300**/lr **0.05**/EMD; CEM horizon **150**/fs **4** |
| GPT temps | Proposal **0.8–1.0**; other stages **0–0.3** |

**Must-cite tattoo:** diversity **0.284/0.165/0.193/0.762** · SR **0.774** (69) · fails **19/155** · substeps **3.13** · Fig. 5 **RL-only collapse** · **LLM proposes; SAC/BIT*/Adam learn**.

**Do NOT claim:** zero human (prompts + ICL + human video judge); sim-to-real solved (§5 open); **106 = infinite** (evaluated finite snapshot; pipeline can mint more).

---

## 3. Keywords

RoboGen; generative simulation; propose-generate-learn; GPT-4; Gemini-Pro; Objaverse; PartNet-Mobility; SAC; BIT*; OMPL; motion planning primitives; Adam traj-opt; CEM; soft-body EMD; long-horizon subtasks; skill library; N=8 handoff; GenSim; Behavior-100; Genesis; infinite synthetic data; sim-to-real gap; BEHAVIOR synthetic factory; UMI/DROID/OXE complement

---

## 4. Deep dive — Motion / skill-library / planner-vs-RL (**PRIORITY**)

**Callout:** RoboGen's long-horizon competence is **decomp + choose MP or RL or traj-opt per sub-skill**, not a single end-to-end LLM policy. **BIT* motion planning is first-class**, not garnish — Fig. 5 proves it.

### Algorithm menu (§3.3–3.4, App A.3) — CLEAR

| Algo | When | Mechanism |
|------|------|-----------|
| **BIT\* MP primitives** | Approach / grasp / release; collision-free path | Sample surface → align gripper to normal → plan to pre-contact (**0.03 m**) → push to contact; OMPL BIT* |
| **SAC RL** | Contact-rich / continuous / non-pose-parameterizable (knob, loco-ish contact) | 6D EE (Δxyz + Δaxis-angle); privileged state; MLP **[256]³**; lr **3e−4**; **1M** steps/sub |
| **Adam traj-opt** | Fine soft-body shaping | **300** grad steps; lr **0.05**; EMD to target particles; horizon 150/200 |
| **CEM loco** | Legged skills (paper finds more stable than RL here) | GT dynamics; horizon **150**, fs **4**; joint-angle actions |

### Long-horizon / skill-library structure (Fig. 3, Fig. 6, App B.1)

| Knob | Paper | Empiricist use |
|------|-------|----------------|
| Decomposition | GPT-4 → shorter-horizon sub-tasks | Chain planner primitives; log stage SR |
| Retries | Each sub-task **\(N{=}8\)** runs | Best-of handoff — smoke may cut to N=2–4 |
| State glue | Highest-reward end-state → next init | Sequential skill atoms |
| Avg structure | **3.13** total; **~1.5** RL + **~1.63** MP | Hybrid **is** the product |
| Long tail | Up to **8–10** substeps | Smoke ≤3–4; defer 8–10 |

**"Skill library" status:** Paper emits **demos + skills** (inventory / demo stream) — packaged reusable API library = **TENTATIVE**. CLEAR as proto skill-library atoms with algo tags.

### Fig. 5 — CRITICAL ablation (motion FLAG)

On **12** articulated tasks: **RL-only completely fails for most tasks**; full **algorithm-selection** (MP primitives ± RL) restores success.  
**Moral:** Primitives encode grasp/approach structure that RL must otherwise discover — report action space / prim availability carefully (Reviewer confound).  
**Empiricist P0:** force RL-only vs allow MP on **one** short articulated open/close stub (not full 12-task suite).

### Motion exam bite

"RoboGen = LLM decomp + **choose BIT\* or SAC or Adam per sub-skill**; avg **3.13** substeps (~1.5 RL + 1.63 MP); \(N{=}8\) best-state handoff; **Fig. 5 RL-only collapse** — MP is PRIMARY."

---

## 5. Deep dive — Synthetic data factory (**PRIMARY DATA CONTRIBUTION**)

**FLAG:** RoboGen's scientific product is an **endless stream of skill demonstrations** (policies + trajectories) tied to generated tasks/scenes — i.e. **synthetic data + learned skills**, not a static offline dump. Session type = Data; opposite UMI/DROID/OXE **real** poles.

### Propose–generate–learn components (Fig. 2) — CLEAR

| Stage | Backend | Output |
|-------|---------|--------|
| **A. Task proposal** | GPT-4 T **0.8–1.0**; object-based (PartNet/RLBench) or example-based (11 tasks) | Name, NL, assets, joints/links |
| **B. Scene gen** | Objaverse \(k{=}10\) + Gemini-Pro caption → GPT-4 verify; LLM sizes; collision push (App A.2) | Valid sim scene + soft text→3D goals |
| **C. Supervision** | GPT-4 T **0–0.3**; 3 ICL reward examples | Sub-tasks + algo choice + reward code |
| **D. Skill learn** | SAC / BIT* / Adam / CEM on Genesis (internal) | Policies + demos; sequential handoff |

**Design doctrine (CLEAR §3):** Extract **semantics / affordances / common sense** from FMs — **not** joint torques / contact dynamics. Backend LLM/VLM modules are swappable.

### Diversity claim — Table 1 (106 RoboGen tasks)

| Metric (↓ better) | RoboGen | Behavior-100 | RLBench | MetaWorld | ManiSkill2 | GenSim |
|-------------------|---------|--------------|---------|-----------|------------|--------|
| # Tasks | **106** | 100 | 106 | 50 | 20 | 70 |
| Self-BLEU | **0.284** | 0.299 | 0.317 | 0.322 | 0.674 | 0.378 |
| SentBert emb sim | **0.165** | 0.210 | 0.200 | 0.263 | 0.194 | 0.288 |
| Scene ViT emb sim | **0.193** | 0.389 | 0.375 | 0.517 | 0.332 | 0.717 |
| Scene CLIP emb sim | **0.762** | 0.833 | 0.864 | 0.867 | 0.828 | 0.932 |

**CLEAR:** Wins all ↓ columns at matched ~100-task scale. GenSim gap = GenSim's tabletop pick-place + small Ravens pool vs RoboGen articulated/loco/soft + Objaverse.  
**Do not equate** 106 evaluated with "infinite" — pipeline can mint endlessly; evaluation is a finite snapshot.

### Validity + verification (Fig. 4, App B.3)

| Check | Result |
|-------|--------|
| Scene human fail | **13 / 155** (missing functionality; joint≠semantic open/closed; precision pairs) |
| Reward/decomp fail | **6 / 155** (undefined vars; inverted fold/unfold; continuous rhythmic motions) |
| **Total** | **19 / 155** ≈ **12%** |
| Fig. 4 size verify | **w/o size** → drastic BLIP-2 drop (dominant) |
| Fig. 4 object verify | **w/o object** → lower mean + higher variance |

### Synthetic vs real poles (session genealogy)

| Axis | RoboGen | UMI / DROID / OXE |
|------|---------|-------------------|
| Authoring cost | LLM prompts + ICL (+ audit) | Human / robot teleop logistics |
| Novelty coverage | **High** for novel artic/soft/loco once prompts exist | Bound by where humans/robots go |
| Visual/tactile realism | Sim — **gap admitted §5** | Real — gold for deploy |
| Product form | Endless demo **stream** | Finite collected corpora |
| BEHAVIOR use | Practice ground / coverage | Challenge teleop / VLA priors |

**Downstream consumers (siblings):** Diffusion Policy / ACT / π₀.₅ / OpenPI FT on exported demos; DART recovery noise optional; OXE mix discipline (`source=robogen`); UMI orthogonal at collection UX — combine only at Visionary recipe layer.

**Do not claim:** zero human involvement; sim-to-real solved; generative demos are deployment-ready without gap proxy.

---

## 6. Role-ready sections (all five — **balanced**)

### 6.1 Empiricist — BALANCED (stage-isolated generative-sim smokes)

> **Seat charge:** Course smokes that stress **proposal validity · object/size verify · planner-vs-RL · gap proxy**, ranked by GPU/RAM/TIME/API honesty. Prefer stage-isolated stubs on a **public** sim stand-in — **not** full paper-scale propose→learn / Genesis soft fleet / 1M×106.

#### Claim under measurement

Can an LLM-orchestrated propose→verify→hybrid-solve→export loop produce **directionally** valid scenes and learnable short skills on public-sim stubs, with Fig. 4/5 morals reproducible without claiming paper **0.774** / Table 1 parity?

#### Hypotheses (falsifiable — from DELIVERABLE)

1. **H-algo:** On contact-rich articulated stubs, **MP-primitive ± short RL** ≫ **RL-only** SR (Fig. 5 moral).  
2. **H-size:** Dropping **size verification** hurts asset–text alignment more than dropping object verify alone (Fig. 4).  
3. **H-object:** Object verification reduces retrieval variance / false accepts vs language-only match.  
4. **H-proposal:** Stage-A/B stub yields ≥**70%** schema-valid, collision-resolved scenes (paper scene fail ~**13/155≈8%** — smoke bar looser).  
5. **H-decomp:** LLM sub-task orderings are mostly executable with planner primitives on short chains (Fig. 3).  
6. **H-reward:** Static reward gens have non-trivial syntax/semantic fail rate; execute+repair (App B.3) lifts validity.  
7. **H-coverage:** Generative task text @ matched N has **lower** Self-BLEU / emb-sim than fixed benchmark labels (Table 1 moral at small N).  
8. **H-gap:** Policy trained on privileged/clean synth **drops** under appearance/camera DR — quantifies synthetic↔real **proxy** gap paper leaves open.  
9. **H-downstream:** Tiny RoboGen-like demos as aug to DP/ACT/π₀ help novel-task SR **or** hurt if gap large — sibling Partial.

**Falsifiers:** RL-only ≈ hybrid; size/object verify noop; proposal chaos ≫ paper fail; decomp unusable; rewards always valid; generative ≠ more diverse at small N; DR no drop; synthetic aug always helps.

#### Prioritized ablation matrix (paper extract — Ablation Bot MISSING)

| Pri | Experiment | Paper hook | Smoke | Est. GPU-h / cost |
|-----|------------|------------|-------|-------------------|
| **P0** | Planner+RL hybrid vs RL-only | Fig. 5 collapse | **Y** | 2–12 or CPU MP minutes |
| **P0** | Object verify ± size verify | Fig. 4 BLIP-2 | **Y** | 0–1 + VLM API $ |
| **P0** | Task/scene proposal stub | Fig. 2 A–B | **Y** | API $ / CPU |
| **P0** | Sim-to-real gap proxy | §5 limitation | **Y** (proxy) | 1–8 |
| **P1** | Tiny synthetic skill train | App B.2 / Tables 5–7 | **Y** | 2–8 |
| **P1** | Decomposition quality | Fig. 3 | **Partial** | API + CPU |
| **P1** | Novel-task coverage card | Table 1 | **Partial** | API + CPU |
| **P1** | Reward validity audit | App B.3 | **Y** | API + CPU |
| **P2** | Retrieval vs text-to-3D | Design §3 | **Partial/N** | High |
| **P2** | Demos → DP/ACT/π₀ FT | Downstream siblings | **Partial** | 8–24 |
| **Defer** | Full 106 propose→learn | Full system | **N** | ≫100 |
| **Defer** | Genesis soft / 12-loco CEM | Tables 6–7 | **N** | Needs Genesis/alt |

**Default course smoke order:**  
`proposal_stub` → `verify_ablation` (full / w/o object / w/o size) → `planner_vs_rl` (hybrid vs RL-lite) → `gap_proxy` → optional `skill_tiny_synth` / `diversity_card`.

#### Exact reproduce stack (public stand-in — EDIT)

```bash
# Official (verify current; soft/Genesis may be incomplete)
git clone https://github.com/Genesis-Embodied-AI/RoboGen
# Project: https://robogen-ai.github.io/
# Paper Genesis was internal at publication — course smoke uses PUBLIC stand-in

# Course scout env (quiet outlines — preferred)
export ROBOGEN_WORK_ROOT=$HOME/robogen-smoke
export OPENAI_API_KEY=...          # EDIT if live LLM stages
export GEMINI_API_KEY=...          # EDIT if verify stages
cd /workspace/robogen-quiet
bash env_outline/setup_mamba.sh
# mamba activate robogen-smoke
mkdir -p logs runs
# EDIT partition/account + sim stand-in (ManiSkill2 / Isaac / PyBullet / MuJoCo) in slurm_*.sh
# Outlines only — do NOT sbatch from quiet box
# Pin model IDs + sim name → logs/pins.txt
# NEVER imply Genesis bit-parity
```

#### Run cards (RC0–RC5)

##### RC0 — `proposal_stub` (API/CPU gate)

| Field | Spec |
|-------|------|
| Goal | Prove LLM → valid task+scene JSON |
| Input | Prompt template + robot/object seed (or mock LLM) |
| Steps | Propose N=10–20 → schema validate → optional collision push → log |
| Pass | ≥70% schema-valid + collision-resolved; wall **10–40 min** + API |
| Fail | Chaos ≫ paper 8% scene-fail moral; missing required fields |
| Script | `env_outline/slurm_proposal_stub.sh` |

##### RC1 — `verify_ablation` (Fig. 4)

| Field | Spec |
|-------|------|
| Goal | Size (± object) verify ↑ alignment |
| Setup | Freeze 5–7 retrieved meshes; toggle object VLM verify + size LLM rescale |
| Metrics | BLIP-2 / CLIP-score / human checklist |
| Expected | Size verify dominates; object verify ↓ variance |
| Budget | **0–1 GPU-h** + VLM API; **no** skill train |
| Script | `env_outline/slurm_verify_ablation.sh` |

##### RC2 — `planner_vs_rl` (Fig. 5) — MOTION P0

| Field | Spec |
|-------|------|
| Goal | Hybrid ≫ RL-only on one articulated stub |
| Setup | Open drawer / close microwave stand-in; BIT*/IK approach vs SAC-lite **≤50–100k** (cut paper 1M) |
| Metrics | SR / return; #solved substeps |
| Expected | RL-only fails contact-heavy; hybrid lifts |
| Budget | **2–12 GPU-h** or CPU MP-only minutes |
| Script | `env_outline/slurm_planner_vs_rl.sh` |

##### RC3 — `skill_tiny_synth` (P1)

| Field | Spec |
|-------|------|
| Goal | Stub rewards + SAC/BC learn **one** short skill |
| Setup | Planner demos → BC **or** SAC ≤100k; automated proxy (joint thresh / contact) |
| Pass | Reward curve ↑; proxy SR logged — **do not** claim 0.774 |
| Budget | **2–8 GPU-h** |
| Script | `env_outline/slurm_skill_tiny_synth.sh` |

##### RC4 — `gap_proxy` (P0/P1)

| Field | Spec |
|-------|------|
| Goal | Quantify synthetic↔real **proxy** gap |
| Setup | Train on privileged/synth → eval under texture/camera DR |
| Metrics | ΔSR; feature distance |
| Expected | Drop under DR — honest gap card before "infinite data" rhetoric |
| Budget | **1–8 GPU-h** |
| Script | `env_outline/slurm_gap_proxy.sh` |

##### RC5 — `diversity_card` (P1 Partial)

| Field | Spec |
|-------|------|
| Goal | Table 1 moral at small N |
| Setup | N≈20–50 generated vs same-N subsample of RLBench/MetaWorld labels |
| Metrics | Self-BLEU; SentBert/CLIP emb-sim |
| Expected | Generative lower (better) Self-BLEU / emb-sim directionally |
| Budget | API + CPU **<1 h**; **do not** claim exact Table 1 scalars |
| Script | `env_outline/slurm_diversity_card.sh` |

#### Paper hyperparams to pin (App A.3)

| Knob | Paper | Smoke cut |
|------|-------|-----------|
| SAC | lr **3e−4**, MLP **[256]³**, 1M/sub, H=100/fs=2 | ≤50–100k |
| BIT* | pre-contact **0.03 m** | keep |
| GPT T | proposal 0.8–1.0 / other 0–0.3 | pin model IDs |
| Soft Adam | 300 / lr 0.05 / EMD | **Defer** (needs Genesis/alt) |
| CEM loco | H=150 / fs=4 | **Defer** |
| Retrieval | \(k{=}10\) | freeze tiny shard |

#### Exact skill SR slices (exam copy — human video judge)

| Slice | n | Mean SR |
|-------|---|---------|
| Articulated/rigid manip (Table 5) | **50** | **0.745** |
| Soft-body (Table 6) | **7** | **≈0.886** |
| Locomotion (Table 7) | **12** | **≈0.833** |
| **All benchmarked** | **69** | **0.774** |

#### Failure modes cheat-sheet

| Symptom | Likely cause | Next action |
|---------|--------------|-------------|
| RL-only ≈ hybrid | Primatives unavailable / easy pose-only task | Switch to contact-rich artic stub |
| Size verify noop | Assets already correctly scaled | Use deliberately wrong-size baselines |
| Proposal chaos | Temp too high / bad ICL | Drop T; add schema repair loop |
| Claimed 0.774 from stub | Overclaim | Report stub proxy SR only |
| CUDA false under GPU reservation | Misconfigured env | Fail closed (run_checklist §2) |
| "Reproduced RoboGen" | Full-loop rhetoric on Partial | Document stage costs; refuse slogan |

#### What not to invent / claim

- **Do not** claim zero human (prompts + ICL + human video SR).  
- **Do not** claim sim-to-real solved (§5 open — gap proxy only).  
- **Do not** equate **106** evaluated with infinite capacity without noting snapshot.  
- **Do not** amortize paper **0.774** onto Partial stubs.  
- **Do not** imply Genesis bit-parity from public stand-in.  
- **Do not** invent Ablation-Bot authority — file **MISSING**; cite paper extract.

#### Empiricist exam bite

"RoboGen = propose→verify→hybrid-solve→export; Fig. 5 **MP critical** (RL-only collapses); Fig. 4 **size verify** dominates; SR **0.774**/69 human-judged; fails **19/155**; smoke RC0→RC2 directional — never claim zero-human / sim-to-real / 106=infinite."

---

### 6.2 Scientific Reviewer — BALANCED

**Claims under review**
1. Generative simulation can match/surpass **human-crafted** benchmark diversity.  
2. Extracting semantics (not actions) from FMs + physics learning is the right division of labor.  
3. Automatic **algo selection** (esp. MP primitives) is necessary for articulated success.  
4. Pipeline yields **valid** scenes and supervisions often enough to learn skills (~77% run success).

**Strengths**
1. **Modality discipline** — LLMs for semantics/affordances/tasks, **not** torques — first-class vs "LLM policy" papers.  
2. Table 1 diversity win at matched scale is clean and multi-metric (text + image).  
3. Fig. 5 RL-only ablation is a strong causal argument for hybrid MP+RL.  
4. Failure appendix (App B.3 / Table 8) is unusually honest — raises credibility.  
5. Scope beyond GenSim (articulated affordances, soft-body, loco) is substantive.  
6. Full-stack integration (task + scene + reward + algo select + learn) > piece-wise concurrent ideas.  
7. Design is backend-swappable in principle (GPT/Gemini/Midjourney named but not permanently wired).

**Weaknesses / threats**
1. Success rates are **human-judged videos**, not automated sensors — rater variance unreported.  
2. 106-task diversity eval vs "endless" rhetoric: evaluated finite snapshot.  
3. Backend GPT-4 / Gemini / Midjourney are **closed, moving targets** — reproducibility fragile.  
4. **No quantitative sim-to-real** transfer experiment (§5 admits gap).  
5. **Genesis opacity** — internal differentiable sim undermines soft-body reproducibility; treat as existence proof.  
6. Fig. 5 confound: hybrid wins partly because **primitives encode structure** RL must discover — report action space carefully.  
7. Diversity metrics (Self-BLEU / emb-sim) can reward verbosity/noise; pair with human "useful skill" ratings.  
8. GenSim comparison fairness — different task class / asset pool; cite as **coverage**, not pure algo superiority.  
9. Verification stack dependency (Gemini + GPT-4 + BLIP-2) — backend churn moves Fig. 4.  
10. Cost externalities: "minimal human" ≠ "minimal compute/$" (endless API + CPU).  
11. No experiment training a **generalist** (OXE/DP/VLA) on RoboGen streams — downstream assumed, not shown.  
12. Ablation Bot catalog **MISSING** this session — factorial LLM/backend/step ablations absent from paper too.

**Verdict:** Strong **systems + paradigm** paper for ICML. Core story (FM for semantics → sim for physics; hybrid learners) holds. Do **not** upgrade to "solved infinite robot data" or "BEHAVIOR data problem closed."

**Reviewer exam bite:** Demand Fig. 5 + Fig. 4 before "RoboGen just works"; cite **0.774** as human-video on 69, not automatic; punish "106 = infinite" and sim-to-real solved claims.

---

### 6.3 Visionary — BALANCED (BEHAVIOR synthetic / generative-sim recipe; **no Discord/Drive**)

**BEHAVIOR connection:**  
BEHAVIOR / Behavior-100 / Behavior-1K embody **sim-first household data hunger** — historically human-authored. RoboGen Table 1 literally ranks against **Behavior-100** and wins diversity, while citing Behavior-1K as the scalability pain generative simulation attacks. For **2026 BEHAVIOR**-class settings, RoboGen is a **design prior** for automating task+scene+demo minting — **complement** to UMI handheld / DROID robot-in-wild / OXE real mix, not a replace.

| BEHAVIOR need | RoboGen-shaped response |
|---------------|-------------------------|
| Task coverage across kitchens/rooms | Auto task proposal seeded on PartNet appliances + distractors |
| Long-horizon activities | Sub-task decomp + sequential skill chaining as **proto skill library** |
| Contact-rich articulated skills | Keep **MP primitives + RL** menu (Fig. 5 lesson) |
| Soft / deformable household | Traj-opt + EMD + text→3D goals |
| Mix with real / VLA priors | RoboGen demos as **sim column** in OXE-style soup; don't drown real wrist demos |
| Verification | Add VLM success critics before demos enter the mix (paper's own open problem) |

**CLEAR:** Design prior for BEHAVIOR-like automation.  
**TENTATIVE:** Official BEHAVIOR'26 baseline names RoboGen (organizer checklist not verified).

#### 2026 BEHAVIOR generative sim / synthetic data recipe (10 bets)

1. **Stage-isolated factory:** Ship **propose → verify → plan/RL → export demo** with pinned models — steal the **pipeline**, not Genesis lock-in.  
2. **Verification is the product:** Size + object + joint-state semantic checks (App B.3 taxonomy) before any SAC budget.  
3. **Planner-first, RL sparsely:** Match Fig. 5 — spend RL only on contact/dynamics subgoals; export demos for VLA FT.  
4. **Mix card with real:** Generative demos as labeled shard beside BEHAVIOR teleop / OXE priors — Empiricist leave-synthetic-out ablation.  
5. **Gap proxy mandatory:** Every synthetic shard publishes **DR / appearance ΔSR** (H-gap) before "infinite data" rhetoric.  
6. **Novel-task eval:** Hold out LLM-proposed tasks never seen in teleop; report coverage vs Self-BLEU vanity.  
7. **Long-horizon glue:** LLM decomp as **System-2 stages** compatible with BEHAVIOR stage votes / commit_s (siblings ACT/DP).  
8. **Do not** retrain SAC-MLP as the 2026 leaderboard policy — **export demos → π₀.₅ / N1.7**.  
9. **Sibling stack:** DROID scene-turnover + RoboGen task invent + DART recovery noise + OXE mix discipline.  
10. **One narrative line:** Hand suites taught **benchmarking**; OXE/DROID taught **real diversity**; RoboGen taught **automate the sim curriculum**; 2026 should **schedule generative practice** into the data mix with **verify + gap cards**.

#### Follow-up research
1. Closed-loop **VLM verifiers** for scene / joint-state / skill success (kill 19/155 modes).  
2. Eureka-style **reward refinement** with env feedback inside the loop.  
3. Export demos into **RLDS / Open-X** schema (`source=robogen`).  
4. **Skill library** packaging: name, language, pre/post, algo tag, demo blobs — hierarchical VLA atoms.  
5. Domain rand + rendering for **measurable** sim-to-real on held-out real appliances.  
6. Open backends + frozen versions for longitudinal reproducibility.  
7. Mobile-manip / multi-robot seeding beyond Franka + quadruped.  
8. Active querying: propose tasks that maximize coverage gaps vs BEHAVIOR/OXE mix.

#### New applications
- Rapid appliance skill farms (microwave, dishwasher, safe) before real teleop.  
- Soft-body kitchen prep curricula as stress tests for diff-sim + VLA FT.  
- Quadruped loco skill zoos as synthetic pretrain for parkour.  
- Overnight curriculum generators for courses / challenges.  
- Synthetic hard-negatives for VLA eval without safety risk.

**Visionary one-liner:** RoboGen turns BEHAVIOR's **authoring bottleneck** into a **queryable factory** — infinite sim demos where UMI/DROID cannot afford to go — then hands hierarchical VLAs a growing **skill library** if verification and schema export catch up.

**Visionary exam bite:** "For BEHAVIOR'26, steal RoboGen's **propose→verify→hybrid-solve→export**; keep MP-first (Fig. 5); publish gap proxies; race on π₀.₅/N1.7 — don't retrain SAC-MLP as the entry; **complement** UMI/DROID/OXE, don't replace."

*(No Discord / Drive checklist content.)*

---

### 6.4 Stakeholder — BALANCED (produced despite empty seat)

**Product one-liner:** An **LLM-orchestrated sim factory** that mints task–scene–reward–demo tuples so robots practice novel skills without real-world collection cost.

**Who cares**
- Labs / companies needing **many novel household skills** without teleop armies.  
- Sim-platform teams (Genesis / Isaac / MuJoCo) wanting an **auto task+scene+reward** layer.  
- BEHAVIOR-class challenge teams hungry for **coverage** before scarce gold demos.  
- **Non-consumers:** need certified **real** demos only; no sim budget; require verified sim-to-real today.

**Buy / build decision**
1. Bottleneck = **authoring tasks/rewards/scenes** in sim → adopt RoboGen-style propose–generate–learn.  
2. Bottleneck = **real visual/tactile diversity** → still fund UMI/DROID/OXE; use RoboGen as **sim prior / skill bootstrap**.  
3. Bottleneck = **tabletop pick-place only** → GenSim-class may suffice; RoboGen's value is articulated/soft/loco + algo selection.  
4. Always budget **human audit** (~12% fail on 155) + verification — do not ship unattended endless minting into training mixes.

**Risks:** GPT-4/Gemini API cost & drift; Genesis availability; silent reward inversions; sim-to-real tax; verification not automated at scale; "minimal human" masking high compute/$.

**Stakeholder exam bite:** "RoboGen buys endless **sim** skill coverage; UMI/DROID/OXE buy **real** diversity — pick the pole, then budget the verify + gap day; never ship unattended minting."

---

### 6.5 Archaeologist — BALANCED (produced despite empty seat)

**Priors RoboGen absorbs (CLEAR)**
- Sim skill learning: PyBullet / MuJoCo / SoftGym / FluidLab / DiffSkill / RoboNinja lineage.  
- Human sim benchmarks: **RLBench, Meta-World, ManiSkill2, Behavior / Behavior-1K**.  
- LLM-for-robotics: Code-as-Policies, Inner Monologue, VoxPoser, Language-to-Rewards, **Eureka**, SayCan.  
- Concurrent foils: **GenSim** (LLM writes manip code; tabletop); **Gen2Sim** (Katara et al.).  
- Assets: PartNet-Mobility / SAPIEN, Objaverse, Zero-1-to-3, Midjourney.  
- Planners/learners: SAC, BIT*/OMPL, CEM, Adam traj-opt.  
- Paradigm: **Generative Simulation** — Xian et al. 2023a (`2305.10455`).

**Genealogy (CLEAR framing)**

```
Human-authored sim benchmarks (RLBench / Behavior-100)
        + FM semantic knowledge (GPT-4 / Gemini)
        + Generative Simulation paradigm (Xian et al. 2023a)
                → RoboGen propose–generate–learn
                        → synthetic skill demos / policies
                              ∥  UMI / DROID / OXE real poles
                              ↓
                     BEHAVIOR / VLA mixes that need
                     verify + gap cards on synthetic shards
```

**Contrast poles (session lineage):**  
`OXE / DROID / UMI` = **real data** → generalist VLAs;  
`RoboGen / GenSim` = **generative sim data** → skill coverage where real collection is too costly;  
`BEHAVIOR` = **sim-first household challenge** that still needs heroic human authoring **or** RoboGen-like automation.

**Descendants / ecosystem**
- Public **RoboGen** PyBullet repo + robogen-ai.github.io — **CLEAR**.  
- **Genesis** engine (announced; release timing **TENTATIVE**).  
- Conceptual influence on later LLM-reward + generative-task systems — do not over-claim unique descent without citation checks.

**Genealogy one-liner:** Behavior-style human sim authoring ∥ GenSim code-gen → **RoboGen full-stack generative simulation** (scenes + rewards + MP/RL/traj-opt) → infinite synthetic skill stream **opposite** UMI/DROID real poles.

**Archaeologist exam bite:** "RoboGen = GPT-4 proposes tasks/scenes/rewards; physics + SAC/BIT*/Adam learn; diversity beats Behavior-100 on Table 1; ~77% skill-run success; **MP ablation matters**; complement real poles."

---

## 7. Empiricist run pack (env / scripts / GPU — from DELIVERABLE)

### Env setup

```bash
export ROBOGEN_WORK_ROOT=$HOME/robogen-smoke
export OPENAI_API_KEY=...    # EDIT
export GEMINI_API_KEY=...    # EDIT
cd /workspace/robogen-quiet
bash env_outline/setup_mamba.sh
# mamba activate robogen-smoke
mkdir -p logs runs
# EDIT: public sim stand-in + partition/account in slurm_*.sh
# Pin model IDs + sim name → logs/pins.txt
# Outlines only — do NOT sbatch from quiet box
```

| File | Role |
|------|------|
| `env_outline/conda_env.yaml` | `robogen-smoke` sketch (Torch + SB3/SAC + sentence-transformers/CLIP; **no** Genesis pin) |
| `env_outline/setup_mamba.sh` | Create/update env; EGL/OSMesa; API key placeholders |
| `env_outline/run_checklist.md` | GPU / TIME / API honesty preflight |
| `env_outline/slurm_proposal_stub.sh` | P0 task/scene proposal N=20 |
| `env_outline/slurm_verify_ablation.sh` | P0 object/size verify (Fig. 4) |
| `env_outline/slurm_planner_vs_rl.sh` | P0 hybrid vs RL-lite (Fig. 5) |
| `env_outline/slurm_skill_tiny_synth.sh` | P1 tiny synthetic skill / BC |
| `env_outline/slurm_gap_proxy.sh` | P0/P1 appearance/DR gap proxy |
| `env_outline/slurm_diversity_card.sh` | P1 Self-BLEU / emb-sim small-N |
| `env_outline/README.md` | Cluster usage notes |

### GPU / RAM / TIME budget honesty

Assumptions: Empiricist path = **stage-isolated stubs** on a **public** sim, **not** paper Genesis + 1M SAC/sub × dozens of tasks. Paper: planning ~**≤10 min** CPU; each RL subgoal ~**2–3 h** (8 threads @ 2.5 GHz); typical task **4–5 h**; 32-core ≈ **8** parallel jobs — **mostly CPU sim**. LLM/VLM stages dominate **API $ + latency**, not VRAM. SAC MLP is small GPU.

| Workload | GPU VRAM (peak) | Host RAM | Wall time (order) | Smoke |
|----------|-----------------|----------|-------------------|-------|
| Proposal stub N=20 | **0** | 8 GB | **10–40 min** + API | **Y** |
| Object/size verify @ 5–7 | **0–8 GB** or API | 16 GB | **30–90 min** | **Y** |
| MP-primitive only 1 task | **0–4 GB** | 16 GB | **minutes–1 h** | **Y** |
| SAC-lite ≤50–100k 1 sub | **4–12 GB** | 16–32 GB | **1–4 h** | **Y** |
| Paper SAC **1M**/sub × ~1.5 RL | 4–12 GB | 32 GB | **~2–3 h × 1.5**/task | **Partial** (cut) |
| Diversity Self-BLEU N=50 | 0–8 GB emb | 16 GB | **<1 h** | **Y** |
| Gap proxy (BC + DR) | **8–16 GB** | 32 GB | **2–8 h** | **Y** |
| Soft-body traj-opt suite | depends | 32 GB+ | hours×7 | **N** |
| Full 106 propose→learn | mixed | large | **days–weeks** | **N** |
| Downstream DP/ACT/π₀ FT | **12–48 GB** | 64 GB | **8–24 h** | **Partial** |

**Honesty rule:** log **(stage, model_id, T, n_tasks, env_steps, algo, verify_flags, wall_s, api_tokens/$, metric)**. If `#SBATCH --gres=gpu:N` but CUDA false → **fail**. Never amortize paper **0.774** onto a Partial stub.

**Course envelope:** target **≤ ~20–25 GPU-h + capped API $** for P0 matrix (RC0–RC2 + gap). Full paper-scale = **≫100 GPU-h + $** — do not promise.

### Success criteria vs paper claims

| Paper claim | Minimal course criterion | Not required |
|-------------|--------------------------|--------------|
| Endless diverse skill stream | Proposal stub valid varied tasks @ N=20 | 106-task Table 1 regen |
| Diversity ≥ human suites | Directional Self-BLEU / emb-sim on **small N** | Exact Table 1 scalars @ N=106 |
| Scene validity via verify | Fig. 4 moral: size (± object) ↑ alignment | Exact BLIP-2 bars |
| Training supervisions induce skills | 1 stub skill learns under gen reward **or** planner chain | Fig. 3 four LH videos |
| Algo select > RL-only | Hybrid > RL-lite on **one** articulated card | Fig. 5 12-task suite |
| ~0.774 skill SR | Automated proxy; report stub SR honestly | Human-video 69-task average |
| Minimal human supervision | Count checklist minutes on smoke | "Zero human" claim |
| Sim-to-real gap exists | Gap proxy shows ΔSR under DR | Real Franka deploy |
| Soft / loco coverage | Note-only | Tables 6–7 reproduction |

**Pass:** ≥2 P0 cards documented (**verify** and/or **algo** and/or **proposal**) with stage cost logs.  
**Fail / overclaim:** "reproduced RoboGen / matched 0.774 / beat Behavior-100 diversity" from stubs without matched protocol.

---

## 8. Slide Maker brief

| Role | Weight / timing guide | Focus |
|------|----------------------|-------|
| **Empiricist** | **balanced · ~14** | Hypotheses, RC0–RC5, reproduce cmds, GPU/API honesty, ablation matrix |
| **Reviewer** | **balanced · ~14** | Fig. 4/5 bite; human-video SR; Genesis opacity; verdict paradigm Accept |
| **Visionary** | **balanced · ~14** | BEHAVIOR propose→verify→export recipe; **no Discord/Drive** |
| **Stakeholder** | **balanced · ~14** | Buy/build vs real poles; audit budget; risks |
| **Archaeologist** | **balanced · ~14** | Genealogy Behavior∥GenSim→RoboGen∥UMI/DROID/OXE |

**Spine (paste into Slide Maker) — 5 bullets:**
1. **LLM proposes; SAC / BIT* / Adam learn** — generative sim factory mints task–scene–reward–demo tuples.  
2. **Must-cite numbers:** diversity **0.284/0.165/0.193/0.762** (Table 1 @106); SR **0.774**/69; fails **19/155**; avg **3.13** substeps.  
3. **Motion PRIMARY:** BIT* MP primitives + algo-select; Fig. 5 **RL-only collapse**; \(N{=}8\) best-state handoff; ~1.5 RL + 1.63 MP.  
4. **Synthetic data:** endless skill-demo stream opposite **UMI/DROID/OXE** real poles; Fig. 4 size/object verify critical; complement don't replace.  
5. **BEHAVIOR'26 + honesty:** propose→verify→hybrid-solve→export; gap proxies mandatory; never claim zero-human / sim-to-real solved / 106=infinite; Empiricist RC0–RC2 directional smokes.

**GitHub title sketch:** `[ECE 605] - RoboGen Making Balanced Five`

Full Slide Maker brief: `/workspace/robogen-quiet/SLIDE_BRIEF.md`

---

## Paths

| Artifact | Path |
|----------|------|
| This pack | `/workspace/robogen-quiet/KANTA_PACK.md` |
| PDF | `/workspace/robogen-quiet/KANTA_PACK.pdf` |
| Slide brief | `/workspace/robogen-quiet/SLIDE_BRIEF.md` |
| Senior Research | `/workspace/robogen-quiet/DELIVERABLE.md` |
| Summarizer | `/workspace/papers/robogen-2311.01455-summary.md` |
| Ablations | **MISSING** — use DELIVERABLE §2 / paper_notes extract |
| Paper notes | `/workspace/robogen-quiet/paper_notes.md` |
| Env outlines | `/workspace/robogen-quiet/env_outline/` |
| Paper PDF | `/workspace/papers/robogen-2311.01455.pdf` |
| Page figs | `/workspace/papers/robogen-figs/page-*.png` |

---

*Pack synthesized 2026-09-29 PT (HST) for KanBot / ECE 605 Paper Manager — Nov 18 Data · ALL seats empty · all five balanced · Ablation Bot absent · no Discord/Drive.*
