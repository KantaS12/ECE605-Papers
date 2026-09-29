# Paper Manager pack — DROID (Nov 4 · Data)

**Paper:** DROID: A Large-Scale In-The-Wild Robot Manipulation Dataset  
**arXiv:** https://arxiv.org/abs/2403.12945 · [project](https://droid-dataset.github.io) · CC-BY 4.0  
**PDF:** `/workspace/papers/droid-2403.12945.pdf` (= `/workspace/droid-quiet/paper.pdf`)  
**Session:** Nov 4 · due 9:30 AM · Type: **Data**  
**Role weights:** **Stakeholder=5 (LONGEST)** · **Scientific Reviewer=2 (substantial)** · Empiricist / Archaeologist / Visionary also filled (seats otherwise empty)  
**Kanta role:** unspecified this paper — weight Stakeholder heaviest; Reviewer substantial; produce all five  
**Nearby pressure:** **Nov 6 Midterm** — keep exam bites / exact numbers ready  
**Sources:** Summarizer + Helper ablations + Senior Research (`/workspace/droid-quiet/DELIVERABLE.md`) + `paper_notes.md` / `env_outline/`  
**Figure IDs (paper-correct):** **Fig. 8** = main co-train bars (+22% ID / +17% OOD); **Fig. 10** = matched scene-diversity ablation; **Fig. 6** = 3D interaction locations; **Fig. 9** = Cook Lentils qualitative. (Summarizer sometimes swapped Fig. 6/8 — use these.)

---

## 1. One-liner

**Same-robot many-real-scenes + protocolized diversity** beats shallower multi-embodiment pools for Franka OOD — steal DROID’s *protocol* into BEHAVIOR, don’t just drink another river.

**Contrast with OXE:** OXE = many robots / aggregated lab pools; DROID = **same Franka + Robotiq stack**, **564 authentic scenes / 52 buildings**, GUI-forced task sampling + scene churn.

---

## 2. Summary (exec overview + key numbers)

**DROID** (Distributed Robot Interaction Dataset) is a distributed **in-the-wild** Franka teleop corpus: **76k** successful trajectories (**~350 hours**) across **564 scenes**, **86 tasks (verbs)**, **52 buildings**, gathered by **50 collectors** at **13 labs** on **18** identical robot replicas over **12 months** (NA / Asia / Europe). ~**16k** fails are released but excluded from the paper’s 76k count and co-train mixes.

**Not a new foundation model.** Experiments are language-conditioned **Diffusion Policies** (Robomimic) co-trained **50/50** with small in-domain demos. Aggregate vs next-best: **+22% absolute** success **in-distribution** and **+17%** **OOD** across **6 tasks × 4 locations** (**Fig. 8**). Scene-diversity ablation at matched **7362** traj (**Fig. 10**) shows **diversity—not just hours**—drives OOD. Co-train pool in paper = first **~40k** language-ready successes (headline **76k** ≠ always train size).

| Axis | Number |
|------|--------|
| Successful traj / hours | **76k** / **350 h** |
| Scenes / buildings | **564** / **52** |
| Verbs (tasks) / collectors | **86** / **50** |
| Institutions / robots | **13** / **18** |
| Control rate | **15 Hz** Polymetis |
| Policy horizons | obs **2** / pred **16** / action **8** |
| Co-train lift (Fig. 8) | **+22% ID** / **+17% OOD** |
| Diversity ablate (Fig. 10) | 7.3k diverse **>** 7.3k top-20 scenes (OOD) |

**Motion FLAG:** chunked reactive **Diffusion Policy** (obs=2 / pred=16 / action=8 @15 Hz) — **NOT** a classical motion planner / MPC / skill graph.

---

## 3. Keywords

DROID; in-the-wild manipulation; distributed collection; Franka Panda; Robotiq; ZED stereo; Quest teleop; scene diversity; Diffusion Policy; action chunking; receding horizon; co-training; OXE contrast; BridgeData V2; camera calibration; Polymetis; CC-BY 4.0; BEHAVIOR diversity protocol; π₀ / FAST / pi05_droid lineage

---

## 4. Deep dive — Motion planning & long-horizon control (**PRIORITY**)

**Callout:** Paper policies are **chunked reactive BC**, **not** classical planners.

| Knob | Paper choice | Implication |
|------|--------------|-------------|
| Family | Diffusion Policy (DDPM/DDIM over action trajectories) | Denoising imitation, Robomimic backbone |
| Obs horizon | **2** | Short visual/proprio history |
| Prediction horizon | **16** @ 15 Hz ≈ **~1.07 s** planned | Chunk length |
| Action / exec horizon | **8** open-loop then replan | Receding-horizon / action-chunking |
| Commit window | `commit_s = 8/15 ≈ 0.53 s` | Log next to SR (π₀ / OpenPI rhyme) |
| Action space (policy) | Absolute EE + gripper | Joints logged but not main policy space |
| Cams in policy | Two exterior RGB @ 128² | Wrist collected but underused in paper recipe |
| Vision / lang | ResNet-50 + frozen DistilBERT | Pre-VLA stack — pair *data* with modern heads in 2026 |

**Long-horizon stress tests**

| Task | Class | In-domain demos | OOD | Why it matters |
|------|-------|-----------------|-----|----------------|
| Close Waffle / Place Chips | Short | 70 / 50 | Distractors / novel | Baseline reactive |
| Apple in Pot / Toasting | Medium | 60 / 150 | Distractor plate / novel toast | Multi-object |
| **Clean up Desk** | **Long** | **50** | Desk / drawer distractors | Stage completion |
| **Cook Lentils** | **Long** | **50** | Distractors + **camera shift** | 3-stage kitchen; Fig. 9 qualitative win |

Cook Lentils requires **all 3 stages** (pour → pan → stove). DROID co-train finishes stages more reliably under OOD; No Co-train / OXE struggle after 1–2 stages (**Fig. 9**). Gains attributed to **diverse co-train data + chunked BC**, not hierarchical planning.

**Syllabus placement:** When contrasting “planner vs reactive policy,” park DROID firmly in **reactive chunked imitation** (same family as Octo / π chunk executors; contrast OpenVLA single-step AR). Horizon **16/8 @ 15 Hz** is the numeric tattoo next to Octo/π₀ chunks.

**Empiricist hook:** Sweep pred ∈ {8,16,32} × exec ∈ {4,8} on a multi-stage proxy; log `commit_s`. Paper recipe is **not** the 2026 BEHAVIOR controller — steal horizons literacy, ship π₀.₅ / N1.7 heads.

---

## 5. Deep dive — Collection methodology & diversity (**PRIMARY CONTRIBUTION**)

**FLAG:** Methodology + diversity protocol is the paper’s main scientific product; policy results are the stress test.

### Hardware (identical × 18)

Franka Emika Panda 7-DoF + Robotiq 2F-85; height-adjustable wheeled desk; 2× ZED 2 exterior + ZED Mini wrist; Quest 2 → continuous 6D + gripper; Polymetis **15 Hz**; joint **and** EE logged; checkerboard extrinsics at scene start (+ App. G post-hoc: ~**36k** cam→base, all-scene cam↔cam, curated **~24k** quality set).

### Protocol knobs (steal these)

1. **Move the robot, not just the objects** — ≤~**100 traj / ~20 min** per scene → forced scene turnover.  
2. **GUI random task sampling** from collector-authored scene list → reduces easy-task bias.  
3. **Periodic scene augmentations** — nudge base, move/recalibrate cams, lighting, add/remove clutter.  
4. **Crowdsourced multi-instruction** post-label (≤**3** / episode, tasq.ai).  
5. **Success flagging** at collect time; fails retained (~16k) but excluded from paper co-train.  
6. **Shared portable stack** + public hardware guide → community multiplier.

### Diversity axes (§IV)

| Axis | DROID | Contrast |
|------|-------|----------|
| Scenes | **564** / **52** buildings | BridgeV2 **24**; OXE agg ~**311**; RT-1 **2** |
| Viewpoints | **1417** unique 3rd-person | Cams remounted / nudged |
| Interaction loci | Wider 3D first-grasp workspace (**Fig. 6**) | Tabletop-fixed priors |
| Verbs / objects | **86** long-tailed; everyday categories | Only BridgeV2 comparable verb tail |
| Embodiment | **One** public Franka stack | OXE = **22** embodiments |

### Causal-ish evidence

- **Fig. 8 (mix):** No Co-train vs OXE 50/50 vs DROID 50/50 → DROID **+22% ID / +17% OOD** vs next-best (10 rollouts/cell; 6 tasks). Co-train uses first **40k** lang-ready successes; OXE baseline ≈ Octo split minus Language Table (~**400k**).  
- **Fig. 10 (diversity @ fixed N):** **7362** from top-20 scenes vs **7362** diverse → diverse **higher OOD** (avg bars ~0.40 vs ~0.60); full DROID ≥ subsampled diverse on overlapping tasks.

**Moral:** Composition of scenes beats raw demo count; OXE breadth ≠ substitute for authentic scene entropy on the eval embodiment.

---

## 6. Role-ready sections (all five)

### 6.1 Stakeholder — WEIGHT **5** / **LONGEST**

**Product one-liner:** Open **Franka in-the-wild** flywheel — **76k / 350 h / 564 scenes** — co-train with a handful of site demos to buy **~+20%** robustness **without** buying 22 embodiments.

**Who pays / who benefits**
- **Franka labs:** clone cart (hardware guide), add **50–150** local demos, co-train 50/50 before inventing architecture.  
- **Foundation teams (OXE / OpenVLA / π / OpenPI):** DROID = open **scene-diversity** shard *and* mix-bug **canary** (OpenVLA drop).  
- **BEHAVIOR’26 organizers / entrants:** proof that **scene count × protocol** moves OOD even when traj count < OXE / RT-1.  
- **Non-consumers:** no Franka/ZED budget; need native bimanual/mobile-base data (DROID is single-arm desk/room-centric).

**Decision framing (Nov 4 → midterm Nov 6)**
1. **Have Franka + a week of teleop?** Collect 50–150 demos → paper DP recipe (or modern chunk head) **before** new architecture. Midterm answer: cite **+22/+17** and **564 scenes**.  
2. **Mixing into a VLA?** Treat DROID as a **monitored shard** — watch action loss; pre-define **kill switch** (OpenVLA loyalty lesson).  
3. **Designing BEHAVIOR data?** Copy **protocol** (random prompts, scene time-caps, view/lighting jitter, success flags) — not only vibes / another download.  
4. **Budget truth:** 13 institutions × 12 months × 50 collectors is the hidden cost; open release socializes it. One building ≠ Fig. 8.

**Moats (exact numbers)**
| Moat | Number / claim |
|------|----------------|
| Scene | **564** scenes / **52** buildings vs BridgeV2 **24** |
| Calibration | Extrinsics + App. G **36k / 24k** refined sets |
| Co-train | **+22% ID / +17% OOD** without new algorithm |
| Ecosystem | Inside OXE; π₀ open mix **9.1%** includes DROID; OpenPI **pi05_droid**; FAST ZS campus narrative |

**Risks / blockers**
- Embodiment lock-in (Franka+Robotiq+ZED).  
- Paper policy stack is **pre-VLA** — buy *data*, rent *models* (π₀.₅ / OpenVLA-FAST).  
- Mix hostility: DROID can **hurt** underfit AR tokenizers — dip ≠ “bad data.”  
- Long-horizon ceiling: Cook Lentils is a demo, not a product SLA.  
- Wrist underused in paper policies despite collection.  
- Crowd lang + GPT-4V scene tags need audit for mission-critical apps.  
- Privacy: homes/offices RGB — check redaction before redistributing subsets.

**Ops checklist (no Discord/Drive)**
- Pull CC-BY + visualizer; pin **v1.0.1**-class language coverage for OpenPI paths (~75k annotated).  
- Reproduce **one** Fig. 8 task at **10** rollouts before claiming domain benefit.  
- VLA pretrain: per-shard action metrics + drop/downweight rules.  
- BEHAVIOR: map augmentations → sim domain randomizations + real scene-rotation schedule.

**Exam bite (Nov 6):** “DROID shows **same-robot, many-real-scenes** teleop + protocolized diversity beats co-training on broader but shallower OXE pools for Franka robustness (**+22%/+17%**); later foundation models both **depend on** and sometimes **drop** DROID depending on capacity and action representation.”

---

### 6.2 Scientific Reviewer — WEIGHT **2** (substantial)

**Claim under review:** In-the-wild distributed Franka teleop with unprecedented scene diversity improves diffusion-policy success and OOD robustness vs in-domain-only and vs OXE co-training.

**Strengths**
1. Clear problem + **Table I** positioning (**564** scenes vs priors ≪100; OXE agg ~311 but many lab-like).  
2. Operationalized diversity axes (verbs, objects, scenes, viewpoints, interaction loci) — not “we collected a lot.”  
3. Protocol transparency (GUI random tasks, augs, ~20 min scene cap) → replicate *process*, not only weights.  
4. **Best causal-ish evidence = Fig. 10** (matched **7362** traj, vary scene diversity) — cite this **before** aggregate **Fig. 8** when asked what *proves* diversity.  
5. Multi-environment eval including real household kitchen.  
6. Honest non-goal: no new architecture; builds on Diffusion Policy correctly.  
7. App. G calibration upgrades with quality metrics — rare for a dataset paper.

**Weaknesses / actionable improvements**
1. **Fig. 8** averages with SE; per-task tables lighter; **10** rollouts/cell thin — demand CIs + pre-registered OOD defs.  
2. “Vs OXE” = curated Octo soup (Language Table dropped), not full 1.4M — say so.  
3. Co-train uses first **~40k** language-ready successes — **40k ≠ 76k**; freeze versioned manifests.  
4. Wrist cams under-ablated despite selling point.  
5. No zero-shot new-scene with **0** in-domain demos (authors admit open) — punish “download and it works.”  
6. Scene dedup heuristic (App. D); GPT-4V taxonomy needs human audit rates.  
7. Language IAA stats thin.  
8. Long-horizon still short-chunk replay — no hierarchical planner / options baseline.  
9. OXE confound: embodiment **match** to Franka eval may drive gains — demand Franka-only OXE slice.  
10. Fig. 10 matches N traj but top-20 scenes may differ in task/object entropy / collector skill — match verb/object histograms.  
11. Couples data claims to one DP head — cross-check Octo / VLA / π₀.  
12. Labor/safety externalities of in-the-wild collection underspecified for replication ethics.

**Verdict:** **Accept as landmark dataset paper.** Scene-diversity ablation is the scientific highlight. Overclaim risk = marketing “generalist policies” when experiments are **per-task co-trained specialists**. OpenVLA drop = **interaction effect**, not refutation.

**Reviewer exam bite:** Cite **Fig. 10** before **Fig. 8** to prove diversity; cite **40k vs 76k** for what was optimized; cite **Fig. 6** only for interaction-location coverage (not main SR).

---

### 6.3 Empiricist

**Hypotheses (from DELIVERABLE)**
1. **H-droid-lift:** 50/50 DROID-subset co-train > none on ID and especially OOD (Fig. 8 moral).  
2. **H-scene>volume:** At fixed N, high scene-count subsample > top-scenes subsample on OOD (Fig. 10).  
3. **H-vs-oxe:** On Franka-like proxies, DROID scene diversity can beat heterogeneous OXE of similar/larger N (test carefully; OXE may still win cross-embodiment).  
4. **H-chunk-horizon:** pred **16** / exec **8** @ ~15 Hz improves multi-stage completion vs single-step BC.  
5. **H-fail-exclude / H-lang-multi / H-protocol:** falsifiable variants in DELIVERABLE §3.

**Minimal matrix:** `{mix: none | droid_diverse_N | droid_top_scenes_N | oxe_subset} × {init: scratch_DP_small | octo | openvla-lora} × {FT: tiny in-domain}` + one chunk {8,16} contrast.

| Pri | Experiment | Smoke | GPU-h |
|-----|------------|-------|-------|
| P0 | Shard inventory + schema smoke | **Y** | <1 |
| P0 | Co-train mix card none / DROID / OXE | **Y** | 8–24 |
| P0 | Scene-diversity matched-N (Fig. 10) | **Y** | 10–30 |
| P1 | Horizon/chunk {8,16,32}×{4,8}; fail include; lang multi; cam-aug | **Y/Partial** | 6–24 |
| P1 | Same mix → Octo / OpenVLA-LoRA / π₀ | **Partial** | 20–60 |
| Defer / N | Full 76k DP × multi-site Franka; Cook Lentils household parity; 0-demo ZS | **N** | ≫200 |

**Success bar:** loader + ≥2 scene mix cards with **directional** SR/MSE + GPU/disk/scene-count logs — **not** Fig. 8 household parity.  
**Tattoo:** 76k / 350h / 564 / 86 / 52 / 50 / 2-16-8 / +22/+17 / 40k train / 7362 ablate.

Env outlines: `/workspace/droid-quiet/env_outline/` (see §7).

---

### 6.4 Archaeologist

```
Tool-home teleop (DobbE, Visual Imitation Made Easy)
Bridge / BridgeV2 (WidowX scene scaling)
RH20T / RoboSet / RT-1 (lab-scale teleop)
        ↓
DROID (2024): same Franka, many buildings, protocolized diversity
        ↓
OXE aggregation (DROID as a slice; ~311 scenes pooled)
        ↓
Octo (curated OXE diffusion GRP) / RT-X
        ↓
OpenVLA: DROID @10% → DROP mid-train (action-token underfit);
         Franka-DROID Wipe FT @15 Hz wins vs DP/Octo
        ↓
π₀: open 9.1% = OXE Magic Soup + Bridge + DROID
π₀-FAST: DROID ZS language generalist (campus rooms)
π₀.₅ / OpenPI: pi05_droid full FT (RLDS v1.0.1 ≈ 75k lang eps)
        ↓
BEHAVIOR’25–’26: stage-rich household; steal diversity protocol (TENTATIVE as named baseline)
```

**Intellectual move:** Flip OXE’s “many embodiments” axis to **many authentic scenes on one research-standard arm**, with **calibration + three views** baked in.

**What later papers learned**
- Positive: scene diversity = first-class generalization lever.  
- Negative: diverse ≠ easy to fit under discrete AR tokens (OpenVLA drop).  
- Systems: open Franka stack = shared eval substrate for ZS claims (FAST).

**Exam bite:** “DROID is the **in-the-wild Franka** ancestor in the OXE→VLA→π chain; remember both the **co-train gains** and the **drop/underfit** sequel.”

---

### 6.5 Visionary — BEHAVIOR + follow-ups + apps (**no Discord/Drive**)

**BEHAVIOR Challenge connection**
- BEHAVIOR needs policies that survive **new houses / layouts / long horizons**. DROID lesson: **maximize scene & viewpoint entropy under a fixed embodiment** with **protocolized anti-bias collection** — not only scale demos on easiest tasks.  
- Map GUI ideas → BEHAVIOR ops: random BDDL task sampling per scene, forced relocation across challenge scenes, distractor/lighting jitter, keep fails for recovery research.  
- Map **Fig. 10** → challenge ablations: matched-demo **many-scene vs few-scene** before claiming “needed more hours.”  
- Pair DROID-style real diversity priors with BEHAVIOR sim demos (co-train / filt / DR bridges).  
- Motion: use **16/8** as literacy baseline; ablate vs hierarchical / π₀.₅-style on long BDDL; race on **π₀.₅ / N1.7**, not 2024 ResNet DP.

**One narrative line:** OXE = **who** the robot is (embodiment soup); DROID = **where** it works (scene soup); BEHAVIOR 2026 = **what long-horizon household progress** looks like when both lessons meet stage structure.

**Follow-up research**
1. Zero-shot new scene without in-domain demos.  
2. Best fusion DROID + OXE + web/VLM (avoid naive concat).  
3. When does more scene tail hurt?  
4. Wrist-centric / 3D-calibrated policies using App. G.  
5. Action-representation × DROID fit (diffusion / FM / FAST vs naïve bins).  
6. Publish a **BEHAVIOR diversity card** (layout families, distractors, cam jitter, instance splits).

**New applications**
- Household Franka assistants: open DROID prior + hour-scale personalization.  
- Cross-campus mobile-manip shared eval rooms (FAST-style ZS).  
- 3D-aware manip / Gaussian scenes from stereo+extrinsics.  
- Data-quality tooling: calib QA, success detectors, lang relabel.  
- Education: reproducible “second robot” teaching co-training.

**Visionary exam bite:** “For BEHAVIOR, steal DROID’s **scene-churn protocol** and **diversity-ablation discipline**; steal π-series **models** — don’t ship 2024 ResNet diffusion as your 2026 entry.”

---

## 7. Empiricist run pack (from DELIVERABLE)

### Env setup
```bash
export DROID_WORK_ROOT=$HOME/src
export DROID_DATA_ROOT=$SCRATCH/droid_subsets   # EDIT: large disk — subsets only
cd /workspace/droid-quiet   # or copy env_outline to cluster
bash env_outline/setup_mamba.sh
# mamba activate droid-scout
mkdir -p logs
# EDIT partition/account in slurm_*.sh
sbatch env_outline/slurm_shard_smoke.sh
sbatch env_outline/slurm_cotrain_mix.sh
sbatch env_outline/slurm_scene_diversity.sh
sbatch env_outline/slurm_horizon_chunk.sh
```

| File | Role |
|------|------|
| `env_outline/conda_env.yaml` | mamba sketch |
| `setup_mamba.sh` | env create + subset download pointers |
| `slurm_shard_smoke.sh` | 1-shard schema inventory |
| `slurm_cotrain_mix.sh` | none / DROID / OXE mix cards |
| `slurm_scene_diversity.sh` | diverse-N vs top-scenes-N |
| `slurm_horizon_chunk.sh` | pred/exec sweep |
| `run_checklist.md` | GPU/disk honesty preflight |

**Outlines only** — not sbatch’d from quiet box. Prefer official / HF resized mirrors; full stereo RGB+depth = **TB-class** — never mirror entire corpus on a login node.

### GPU / RAM / disk honesty

| Workload | VRAM | Disk | Wall | Smoke |
|----------|------|------|------|-------|
| Shard schema smoke | 0–8 GB | 20–200 GB selective | min–hours I/O | **Y** |
| Small DP co-train scout (≤10k + tiny ID) | 12–24 GB | + ckpt | 4–12 h | **Y** |
| Scene-diversity pair | 12–24 GB | shared | 1–2 days | **Y** |
| OpenVLA-7B LoRA + mix | 24–48 GB | +15–30 GB ckpt | 8–24 h | **Partial** |
| Chunk sweep ×3 | 12–24 GB | shared | 1–2 days array | **Y** |
| Full DROID mirror / paper multi-site A/B | — | ≫1–several TB + robots | — | **N** |

**Honesty rule:** if `#SBATCH --gres=gpu:N` but no device → **fail job**; log peak VRAM + **mix card hash** + **scene cardinality** next to every SR. Never claim “full 76k” from a capped download.

### Success criteria vs paper

| Paper claim | Feasible bar | Not required |
|-------------|--------------|--------------|
| Usable in-the-wild Franka data | Loader streams multi-cam / action / lang / success | Full 76k ingest |
| Co-train ↑ robustness | DROID-subset > none on **one** OOD proxy | +22/+17 across 6 real tasks |
| Scene diversity drives OOD | diverse-N > top-scenes-N @ matched N | Exact Fig. 10 bars |
| Chunk helps long-horizon | Chunked ↑ stage completion on **one** multi-stage proxy | Household Cook Lentils robot |

**Pass:** mix cards + ≥1 directional **H-droid-lift** or **H-scene>volume** with logs.  
**Fail / overclaim:** “we reproduced Fig. 8 household results” without robots / full protocol.

---

## 8. Slide Maker brief

| Role | Weight / timing guide | Focus |
|------|----------------------|-------|
| **Stakeholder** | **5 · longest** | Product, moats, midterm decisions, OXE contrast |
| **Reviewer** | **2 · substantial** | Fig. 10 before Fig. 8; 40k vs 76k; landmark + punish |
| Empiricist | filled | Mix cards + scene-N + chunk smoke + GPU honesty |
| Archaeologist | filled | OXE→DROID→OpenVLA drop→π₀/FAST/pi05_droid→BEHAVIOR |
| Visionary | filled | Steal protocol into BEHAVIOR; no Discord/Drive |

**Spine (paste into Slide Maker):**
1. **Same Franka, many real scenes** (564 / 52 buildings) — opposite of OXE’s many-robots axis.  
2. **Protocol > vibes:** GUI random tasks, ~20 min scene churn, view/lighting augs.  
3. **Fig. 8 +22% ID / +17% OOD**; **Fig. 10** proves diversity at matched 7.3k — cite Fig. 10 before Fig. 8 for causality.  
4. **Motion:** chunked DP **obs=2 / pred=16 / action=8 @15 Hz** — reactive BC, **not** a classical planner; Cook Lentils / Clean Desk = long-horizon stress.  
5. **BEHAVIOR steal:** diversity cards + scene-churn protocol onto π₀.₅/N1.7 — don’t retrain paper DP or just drink another river; midterm Nov 6 needs these numbers.

**GitHub title sketch:** `[ECE 605] - DROID Making Stakeholder` (or Reviewer if seat assigned)

---

## Paths

| Artifact | Path |
|----------|------|
| This pack | `/workspace/droid-quiet/KANTA_PACK.md` |
| PDF | `/workspace/droid-quiet/KANTA_PACK.pdf` |
| Senior Research | `/workspace/droid-quiet/DELIVERABLE.md` |
| Summarizer | `/workspace/papers/droid-2403.12945-summary.md` |
| Ablations | `/workspace/papers/droid-2403.12945-ablations.md` |
| Env outlines | `/workspace/droid-quiet/env_outline/` |
| Paper PDF | `/workspace/papers/droid-2403.12945.pdf` |

---

*Pack synthesized 2026-09-05 PT for KanBot / ECE 605 Paper Manager — Nov 4 Data · Stakeholder=5 · Reviewer=2 · midterm Nov 6 nearby.*
