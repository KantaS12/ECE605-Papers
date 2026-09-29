# Paper Manager pack — UMI (Nov 16 · Data)

**Paper:** Universal Manipulation Interface: In-The-Wild Robot Teaching Without In-The-Wild Robots  
**arXiv:** https://arxiv.org/abs/2402.10329 · [project](https://umi-gripper.github.io) · [code (MIT)](https://github.com/real-stanford/universal_manipulation_interface) · **RSS 2024** (Best Systems Paper Award Finalist — project page)  
**Authors:** Cheng Chi*, Zhenjia Xu*, Chuer Pan, Eric Cousineau, Benjamin Burchfiel, Siyuan Feng, Russ Tedrake, Shuran Song (*equal) — Stanford / Columbia / TRI  
**PDF:** `/workspace/papers/umi-2402.10329.pdf` (= `/workspace/umi-quiet/paper.pdf`) · 18 pp · arXiv **2402.10329v3** (6 Mar 2024)  
**Session:** Nov 16 · due 9:30 AM · Type: **Data**  
**Role weights:** **Empiricist=1 (Kanta — LONGEST / PRIMARY)** · **Scientific Reviewer=4 (substantial)** · **Visionary=2 (substantial)** · Stakeholder / Archaeologist seats empty — **still all five produced**  
**Kanta role:** **Empiricist** — deepen env/run/GPU/scripts + exact reproduce commands; produce all five  
**Themes:** handheld gripper · in-the-wild teaching **without** robots on-site · relative SE(3) · latency matching · wrist fisheye · Diffusion Policy chunks  
**Sources:** Summarizer + Ablation Bot + Senior Research (`/workspace/umi-quiet/DELIVERABLE.md`) + `paper_notes.md` / `env_outline/`  
**Sibling links:** `/workspace/diffusion-policy-quiet/` · `/workspace/act-aloha-quiet/` · `/workspace/droid-quiet/` · `/workspace/oxe-quiet/` · `/workspace/dart-quiet/` · `/workspace/pi05-quiet/` · `/workspace/openpi-comet-smoke/` · `/workspace/openvla-quiet/`

---

## 1. One-liner

**In-the-wild teaching without in-the-wild robots** — relative EE + latency honesty turns **human logistics** into robot-ready demos; complementary to **DROID** (robot-in-wild) and **ALOHA** (lab teleop).

**Contrast poles:**

| Pole | Who collects | Where | Action interface |
|------|--------------|-------|------------------|
| **UMI** | **Human** + handheld gripper | Anywhere (cafe, fountain, home) | Relative SE(3) EE + wrist fisheye + latency match |
| **DROID** | **Robot** teleop (Franka fleet) | 564 real scenes / 52 buildings | Absolute EE @ 15 Hz (paper DP) |
| **ALOHA/ACT** | Lab leader–follower | Lab tables | Joint-space, embodiment-specific |

---

## 2. Summary (exec overview + key numbers)

**UMI** is a **data collection + policy learning** framework: collect demos anywhere with a portable **~$371** GoPro + 3D-printed parallel-jaw gripper (no robot during collection), recover metric 6DoF EE via **ORB-SLAM3 + GoPro IMU**, train **Diffusion Policy** on **relative SE(3) action chunks** (**Ta=6** @ **10 Hz**, or 20 Hz tossing), deploy zero-shot on **UR5 / Franka FR2** with **inference-time latency matching**.

**Why Data session:** Interface design (FoV, IMU-SLAM, latency, relative actions, multimodal DP) — not just “more scenes” — unlocks **action diversity** (toss, fold, wash) from handheld demos that prior handheld systems limited to grasp/pick-place.

| Axis | Number / claim |
|------|----------------|
| Cup lab (UR5) | **20/20 = 100%** |
| Cup → Franka (same ckpt) | **18/20 = 90%** (2 joint-limit) |
| Absolute / Delta / Rel action | **25% / 80% / 100%** |
| No fisheye (69° crop) | **55%** |
| Toss w/ latency / w/o | **105/120 = 87.5%** / **69/120 = 57.5%** |
| Cloth fold / no inter-grip | **70%** / **30%** |
| Dish wash ViT / ResNet | **70%** / **0%** |
| Wild cup OOD | **1400** demos / **30** loc / **12** person-h → **43/60 = 71.7%**; narrow-only **0%** |
| SLAM ATE / RPE | **6.1 mm / 3.5°** · inter-grip **10.1 mm / 0.8°** |
| Throughput vs teleop | **>3×** spacemouse (cup); toss demos teleop **0** in 15 min |
| BoM | Gripper **$73** + GoPro **$298**; mass **780 g** |
| DP chunks | **Ta=6** @ **10 Hz** (toss **20 Hz**); DDIM **50/16** |
| Example exec latency | Arm **~100 ms** / gripper **~120 ms** (Fig. 5 — measure per install) |
| GPUs (Tab. A1) | Lab **4×A10g** (dish **8×**); wild **8×A100** — **wall-clock hours NOT in paper** |

**Must-cite tattoo:** cup **100%/90%** · toss **87.5%** (no lat **57.5%**) · wild **1400/30 → 71.7%** vs narrow **0%** · rel **100%** vs abs **25%** · fisheye **100%** vs 55° **55%** · **π₀.₅ / GR00T CLEAR cite** UMI.

---

## 3. Keywords

UMI; handheld gripper; in-the-wild teaching; GoPro; fisheye wrist camera; side mirrors; ORB-SLAM3; visual-inertial SLAM; relative trajectory action; latency matching; Diffusion Policy; action chunking; DDIM; Ta=6; bimanual inter-gripper proprioception; continuous gripper width; soft TPU fingers; zero-shot transfer; RSS 2024; DROID contrast; ALOHA contrast; π₀.₅; GR00T-N1; BEHAVIOR handheld recipe

---

## 4. Deep dive — Motion / latency / relative EE (**PRIORITY**)

**Callout:** UMI is where **DP’s chunk control loop** meets a **deployable handheld data interface**. Relative EE + latency matching are the motion-control upgrades that make wild human demos robot-ready.

### Chunking / horizons (Tab. A1 — CLEAR)

| Knob | Paper choice | Implication |
|------|--------------|-------------|
| Family | **Diffusion Policy** (DDIM) | Multimodal human modes (cup CW/CCW) |
| Action horizon **Ta** | **6** all tasks | Chunk length |
| Control freq | **10 Hz** (cup/fold/wash/wild); **20 Hz** (toss) | Commit ≈ **0.6 s** @10 Hz; **0.3 s** @20 Hz |
| Obs | I-To ∈ {1,2}; P-To = **2** | Short visual + relative proprio history ≈ velocity |
| DDIM | **50** train / **16** infer | Leave room inside control period after matching |
| Speed scale | **0.5×** quasi-static; **1.0×** toss | Smooth tracking vs release velocity |
| Action alphabet | **Relative SE(3) traj** w.r.t. EE at chunk start \(t_0\) + continuous gripper width | Not world-abs; not delta-to-previous |

**Relative vs Absolute vs Delta (Fig. 6 / PD2.1):**  
For a chunk starting at \(t_0\), each action is the desired pose at \(t\) **relative to EE pose at \(t_0\)**. Absolute needs SLAM↔base calib (brittle → **25%**). Delta accumulates step error → **80%**. Relative flat under calib noise → **100%**. Also enables **move robot base mid-episode** (Fig. 10).

### Latency matching (PD1) — CRITICAL for dynamic

Fig. 5 illustrative deploy latencies: **Arm exec ≈ 100 ms**, **Gripper exec ≈ 120 ms** (examples — measure per install; App. A).

1. Measure camera / proprio / gripper / arm latencies (QR rolling clock for cam; ≈½ RTT if no HW stamp; cross-corr for exec).  
2. Align streams to **highest-latency** (usually camera); downsample RGB to **10–20 Hz**; interpolate proprio/gripper to \(t_{obs}\).  
3. Predict action chunk from last obs; **discard outdated** actions covering obs+infer+exec delay; send remaining **ahead** by measured exec latency.

**Toss moral:** with matching **105/120 = 87.5%**; disable (set all latencies → 0) → **69/120 = 57.5%** (−30 pp). Jitter barely hurts grasp; hurts toss velocity + gripper–arm release timing.

### Wrist cams / FoV / mirrors

- **Sole** observation modality (no external cam in policy).  
- **155° fisheye** raw (no undistort) — crop→69° → **55%**.  
- Side mirrors = implicit stereo; must **digitally reflect + swap L/R** (raw mirrors **hurt**: 85% < no-mirror 90%).

### Long-horizon / bimanual / mobile hooks

| Hook | Number | Control use |
|------|--------|-------------|
| Dish wash 7-step | **70%** ViT; ResNet **0%** | Stage-wise SR; recovery demos (ketchup re-dirty) |
| Cloth fold | **70%**; no inter-grip **30%** | Dual-arm sync via relative inter-gripper proprio |
| Base shift mid-rollout | Fig. 10 qualitative | Relative EE ⇒ mobile-manip ready if objects in reach |
| Soft TPU fingers | continuous width | Contact-rich / faucet / toss release |

**Syllabus takeaway for Kanta:** Treat **latency calib as a controller parameter**; ablate with horizon (H-commit). Link DP Push-T morals (~0.5–1 s commit) — UMI toss wants shorter or better-matched.

---

## 5. Deep dive — Collection methodology (handheld scaling) (**PRIMARY DATA CONTRIBUTION**)

**FLAG:** UMI’s scientific product is the **portable teaching interface** (hardware HD1–HD6 + policy PD1–PD2), not a new generative head. Collection **without robots on-site** is the Data-session spine.

### Hardware BoM (CLEAR)

| Spec | Value |
|------|-------|
| Mass / envelope | **780 g** · L310×W175×H210 mm |
| Finger stroke | **80 mm**; soft **95A TPU** ribs (series-elastic via width) |
| Cost | Printed gripper **$73** + GoPro+acc **$298** |
| Sole sensor | Wrist **GoPro Hero 9** + **155°** fisheye + built-in IMU (GPMF) |
| Mirrors | Physical side mirrors in peripheral FoV |
| Gripper width | ArUco/fiducial **continuous** (not binary) |
| Robot twin | Same finger/cam geometry; **Schunk WSG-50**; Media Mod → **Elgato HD60X** UVC |
| Arms validated | **UR5** + **Franka Emika FR2** (custom 90° WSG mount) |

### Pipeline (App. B)

1. Optional bimanual QR clock sync (±1/60 s) + gripper width cal.  
2. **Map-then-localize:** ~1 min scene mapping video → demos relocalized into shared map (enables inter-gripper pose). Optional fiducials **only during mapping**.  
3. Repeated demos (one MP4 / episode) → upload → **single script** → SLAM + fiducials → Diffusion Policy zarr.  
4. **Kinematic filter:** drop demos infeasible for target robot base/DoF before BC.

### Throughput (§VII / Fig. 11) — same operator, **15 min**

| Task | Bare hand | UMI | Spacemouse teleop |
|------|-----------|-----|-------------------|
| Cup arrange | fastest | **~48%** of hand; **>3×** teleop (111 vs 35 CPH) | slowest |
| Dynamic toss | fastest | **~64%** of hand (149 CPH) | **0** successful demos |

### In-the-wild scaling (Fig. 9)

- **1400** demos / **30** locations / **3** demonstrators / **12 person-hours** / **15** train cup styles.  
- Unseen cafe + fountain: **43/60 = 71.7%** (train cups 28/40=70%; unseen cups 15/20=75%).  
- Narrow-domain-only + same CLIP ViT family: **0%** — “doesn’t even move toward the cup.”  
- **Moral:** scene diversity in **hand-held** demos is the generalization knob (sibling to DROID scene cards; opposite *who* goes into the wild).

### Open data / code

- Example session + `cup_in_the_wild.zarr.zip` + pretrained `cup_wild_vit_l` on real.stanford.edu/umi/  
- SLAM fork: `cheng-chi/ORB_SLAM3` + Docker `chicheng/orb_slam3`  
- Deploy forks: `umi-on-legs`, `umi-arx` (ARX X5)

**Do not equate** UMI collection scale with DROID **76k** — different axis (action-rich human-handheld vs robot-in-wild volume).

---

## 6. Role-ready sections (all five)

### 6.1 Empiricist — WEIGHT **1** / **LONGEST** (Kanta PRIMARY)

> **Seat charge:** Course smoke ablations that stress **relative action · latency · embodiment transfer · policy-from-UMI-demos**, ranked by GPU/RAM/TIME honesty. Prefer offline/proxy metrics before real-robot eval. This section + `env_outline/` is the pack’s richest deliverable.

#### Claim under measurement

Can a GoPro+printed gripper + IMU-SLAM + latency-matched relative-trajectory Diffusion Policy transfer dynamic/bimanual/long-horizon human demos to UR5/Franka with reproducible directional numbers on proxies?

#### Hypotheses (falsifiable — from DELIVERABLE)

1. **H-rel:** On identical demos, **relative traj** → lower held-out SE(3) err **and** higher proxy success than **absolute** (calib-sensitive) and ≥ **delta** (accumulates). Paper cup: **100 / 25 / 80%**.  
2. **H-lat-dyn:** Disabling latency matching drops **sharply on dynamic** toss-proxies (paper **−30 pp**) and mildly on quasi-static grasp.  
3. **H-lat-curve:** U-shape / cliff — under- and over-compensation (wrong signed latency) both hurt velocity tracking more than position reaching.  
4. **H-emb:** EE-relative + wrist-cam policy transfers across kinematic models **iff** ingest applied kin-filter and wrist FoV matched; joint-limit fails dominate residual (paper Franka 2/20).  
5. **H-dp-umi:** Short DP smoke on relative chunks fits multimodal human modes better than MSE-MLP on same shard (link `/workspace/diffusion-policy-quiet/`).  
6. **H-bimanual:** Without **inter-gripper relative proprio**, dual-arm stage sync fails even if per-arm BC loss looks fine (paper **70→30%**).  
7. **H-wild:** Holding N fixed, **multi-scene hand-held** mix beats single-scene on scene-OOD cup proxy (Fig. 9; sibling DROID).  
8. **H-commit:** Effective commit seconds \(T_a \Delta t\) should land near DP Push-T morals (~0.5–1 s) for quasi-static and **shorter or better latency-matched** for toss.

**Falsifiers:** abs ≥ rel under perfect synthetic calib; latency matching noop on toss-proxy; transfer works without kin-filter; ResNet matches ViT on dish-proxy with matched steps.

#### Prioritized ablation matrix (Ablation Bot + DELIVERABLE)

| Pri | Experiment | Paper hook | Smoke | Est. GPU-h |
|-----|------------|------------|-------|------------|
| **P0** | Relative vs Absolute vs Delta | Cup: Rel **100%**, Abs **25%**, Delta **80%** | **Y** | 2–8 |
| **P0** | Latency matching on/off (+ sweep) | Toss: **87.5%** vs **57.5%** | **Y** | 1–4 (eval) |
| **P0** | Demo ingest (SLAM→rel chunks + kin filter) | ATE **6.1 mm / 3.5°** | **Y** | **CPU** <1 |
| **P1** | Policy train smoke (DP on tiny UMI-like shard) | Fig. 5 interface | **Y** | 4–12 |
| **P1** | Embodiment transfer stub (IK/feasibility) | Franka **90%** | **Partial** | 1–3 |
| **P1** | Inter-gripper relative proprio on/off | Cloth **70%** vs **30%** | **Partial** | 4–10 |
| **P2** | Fisheye / FoV crop | No fisheye **55%** | **Partial** | 4–10 |
| **P2** | Side-mirror digital reflect | raw 85% → reflect **100%** | **N** | — |
| **P2** | ViT-CLIP FT vs ResNet scratch | Dish **70%** vs **0%** | **Partial** | 8–20 |
| **P3** | Wild mix vs narrow-only OOD | Fig. 9: **71.7%** vs **0%** | **Partial** | 8–24 |
| **Defer** | Full real 4-task 20-trial protocol | §V | **N** | 50–200+ |

**Default course smoke order:**  
`demo_ingest_smoke` → `relative_action_ablation` × {rel, abs, delta} → `latency_sweep` × {0, match, 2×match} → `policy_train_smoke` (rel only) → `embodiment_transfer_stub`.

#### Exact reproduce stack (CLEAR from GitHub README / summary)

```bash
# Env (official UMI)
# Ubuntu 22.04; system: libosmesa6-dev libgl1-mesa-glx libglfw3 patchelf
git clone https://github.com/real-stanford/universal_manipulation_interface
cd universal_manipulation_interface
mamba env create -f conda_environment.yaml
conda activate umi

# SLAM (Docker fork)
# cheng-chi/ORB_SLAM3 · image chicheng/orb_slam3
python run_slam_pipeline.py <session_dir>

# Dataset build
python scripts_slam_pipeline/07_generate_replay_buffer.py \
  -o …/dataset.zarr.zip <session>

# Train — 1×GPU (RTX 3090 24GB tested)
python train.py \
  --config-name=train_diffusion_unet_timm_umi_workspace \
  task.dataset_path=…

# Train — multi-GPU
accelerate --num_processes <n> train.py --config-name=…

# Wild cup data + ckpt (public)
wget …/cup_in_the_wild.zarr.zip
wget …/cup_wild_vit_l_1img.ckpt

# Real-robot eval (needs hardware)
python eval_real.py \
  --robot_config=example/eval_robots_config.yaml \
  -i <ckpt> -o <outdir>
# SpaceMouse; C=start / S=stop
```

**Course scout env (quiet outlines — preferred for class smokes):**

```bash
export UMI_WORK_ROOT=$HOME/src
export UMI_DATA_ROOT=$HOME/data/umi_subsets   # EDIT
cd /workspace/umi-quiet
bash env_outline/setup_mamba.sh
# mamba activate umi-scout
mkdir -p logs
# EDIT partition/account in slurm_*.sh — outlines only; do NOT sbatch from quiet box
# Pin official UMI + DP commits → logs/pins.txt
```

#### Run cards (RC0–RC4)

##### RC0 — `demo_ingest_smoke` (CPU gate)

| Field | Spec |
|-------|------|
| Goal | Prove demo → obs/action tensors |
| Input | Public UMI sample **or** synthetic SE(3)+gripper+RGB stubs (10–50 eps) |
| Steps | decode → SLAM/stub poses → **relative** chunks → kin-filter UR5-like & Franka-like → zarr/hdf5 + manifest |
| Pass | Schema OK; 0 NaNs; relative chunk starts at Identity; filter report; wall **<30 min CPU** |
| Fail | Dropped frames unaligned; abs poses leaked into “rel”; filter removes >50% without reason |
| Script | `env_outline/slurm_demo_ingest_smoke.sh` |

##### RC1 — `relative_action_ablation`

| Field | Spec |
|-------|------|
| Goal | Rel vs Abs vs Delta on **same** shards |
| Train | DP or light temporal CNN; **1 seed**; 2k–10k steps |
| Axes | `action_repr ∈ {relative_traj, absolute, delta}`; fix obs/batch/lr/horizon |
| ID | Held-out action MSE / rotation geodesic |
| OOD | SE(3) origin noise / base-frame jitter at eval only (stress abs) |
| Expected | Abs explodes under jitter; Delta drifts; Rel flat — ranking matches cup 100/80/25 |
| Budget | **1 GPU**, ~**2–8 h** for 3 runs |
| Script | `env_outline/slurm_relative_action_ablation.sh` (array 0–2) |

##### RC2 — `latency_sweep`

| Field | Spec |
|-------|------|
| Goal | Reproduce toss moral without full robot if needed |
| Setup | Fixed ckpt from RC1-rel **or** teacher traj; **eval-only** |
| Grid | `lat_mode ∈ {zero, measured_match, 0.5×, 2×, sign_flip}`; optional obs delay \(d\in\{0,1,2,4\}\) |
| Metrics | Velocity-profile MSE on toss phase; release timing; jerk; proxy bin success if env exists |
| Expected | `measured_match` best; `zero` ≈ −30 pp moral; `sign_flip` worst |
| Budget | **≤1–4 GPU-h** (mostly infer) |
| Script | `env_outline/slurm_latency_sweep.sh` |

##### RC3 — `policy_train_smoke`

| Field | Spec |
|-------|------|
| Goal | End-to-end short DP on UMI-relative data |
| Config sketch | `To=2`, `Tp≈16`, `Ta≈6–8` (EDIT pin from UMI repo); DDIM 10–16; batch 32–64; img 224 or 128; relative EE 6D/9D + gripper |
| Steps | Scout **5k–20k** (not paper full); early-stop on val action err |
| Pass | Finite grads; VRAM logged; val err < train×3; ≥1 qualitative rollout gif |
| Fail | OOM → drop batch/res; NaN quat → use 6D rot |
| Budget | **4–12 GPU-h** |
| Script | `env_outline/slurm_policy_train_smoke.sh` |

##### RC4 — `embodiment_transfer_stub`

| Field | Spec |
|-------|------|
| Goal | Test transfer **without** claiming Franka 90% |
| Method | RC3 chunks → FK/IK for robot A vs B; count joint-limit / singularity / collide |
| Pass | Feasibility A≈B within ~10 pp when kin-filter used; gap widens when filter off |
| Budget | CPU / **<1–3 GPU-h** if replaying vision |
| Script | `env_outline/slurm_embodiment_transfer_stub.sh` |

#### Exact demo counts & success (exam copy)

| Task | Demos | Eval | UMI success | Key ablation |
|------|-------|------|-------------|--------------|
| Cup lab | 305 | 20 | **100%** UR5 / **90%** FR2 | Abs **25%**; no fisheye **55%**; delta **80%** |
| Toss | 280 | 120 obj | **87.5%** | No latency **57.5%** |
| Cloth fold | 250 | 20 | **70%** | No inter-grip **30%** |
| Dish wash | 258 | 20 | **70%** | ResNet **0%** |
| Cup wild | 1400 / 30 loc | 60 | **71.7%** | Narrow-only **0%** |
| SLAM MoCap | 14 tasks | — | ATE **6.1mm/3.5°** | RPE **10.1mm/0.8°** |

#### Tab. A1 train budget (cite epochs/GPUs only — **no invented wall-clock hours**)

| Task | Ta | Freq | Speed | Vision | Epochs | Batch | GPUs |
|------|-----|------|-------|--------|--------|-------|------|
| Cup lab | 6 | 10 | 0.5× | ViT-B/16 CLIP | 250 | 512 | **4×A10g** |
| Toss | 6 | 20 | 1.0× | ResNet-34 | 350 | 1024 | **4×A10g** |
| Cloth | 6 | 10 | 0.5× | ResNet-34 | 100 | 1024 | **4×A10g** |
| Dish | 6 | 10 | 0.5× | ViT-B/16 CLIP | 90 | 224 | **8×A10g** |
| Cup wild | 6 | 10 | 0.5× | **ViT-L/14** CLIP | 50 | 512 | **8×A100** |

All: DDIM **50/16**; D-Lr **3e-4**; V-Lr **3e-5** (CLIP) or **3e-4** (ResNet).

#### Latency calibration recipe (App. A)

1. Camera: film rolling QR of system clock → \(l_{cam}=t_{recv}-t_{display}-l_{display}\).  
2. Proprio: HW stamp delta or ≈½ RTT.  
3. Gripper/arm exec: cross-correlate commanded vs measured → \(l_{action}=l_{e2e}-l_{obs}\).  
4. At run: sync to cam; drop stale; command ahead by \(l_{action}\).

#### Expected ID / OOD proxies (course pass)

| Proxy | ID | OOD / stress | Pass bar |
|-------|----|--------------|----------|
| Action repr | Clean held-out | Origin / base-frame jitter | Rel best under jitter; Abs collapses |
| Latency | Matched delay=calib | Delay≠calib, dynamic subset | Match > zero on velocity MSE |
| Policy smoke | Train scene val | Held-out scene folder | Loss↓; no NaN; gif OK |
| Embodiment | Robot A IK OK | Robot B same chunks | Kin-filter on ⇒ transfer gap small |
| Wild mix (stretch) | Multi-scene train | Unseen scene | Multi-scene > single-scene @ matched N |

**Not required for pass:** paper 100%/87.5%/70% real-robot rates, 3 seeds, full 305+280+250+258 retrain, physical tossing bins.

#### Failure modes cheat-sheet

| Symptom | Likely cause | Next action |
|---------|--------------|-------------|
| Abs ≈ Rel on clean data | Missing origin jitter OOD | Add eval noise (H-rel stress) |
| Rel worse than Delta | Relative transform bug | Unit-test Identity@t0; compare Fig. 6 |
| Latency noop | Sweeping only position tasks | Restrict to high-speed segments |
| OOM mid-train | ViT+bs64+224 | ResNet18 / 128² / grad accum |
| Transfer “fails” | Comparing joint-space policies | Ensure EE-relative + wrist obs |
| Ingest drops all frames | Kin-filter wrong URDF | Loosen limits; log reject reason |
| Wild ckpt fails outdoor | Direct sunlight (README known) | Document domain; don’t overclaim |

#### What not to invent / claim

- **Do not invent GPU wall-clock hours** (paper omits them — cite epochs × GPU counts only).  
- Do not claim external third-person cams in UMI policy (wrist-only).  
- Do not equate UMI collection with DROID robot-in-wild teleop.  
- Do not claim “70% OOD” without the **43/60** breakdown.  
- Do not claim ACT drop-in is empirically equal (stated, **not** ablated).

#### Empiricist exam bite

“UMI = handheld wild teaching **without** robots on-site; relative EE **100%** vs abs **25%**; latency matching turns toss **87.5%←57.5%**; fisheye **100%←55%**; wild **1400/30 → 71.7%** vs narrow **0%**; DP **Ta=6** @10 Hz; smoke RC0→RC2 for directional proofs — never invent GPU hours.”

---

### 6.2 Scientific Reviewer — WEIGHT **4** (substantial)

**Claim under review:** Careful handheld interface + latency-matched relative-trajectory Diffusion Policy enables zero-shot transferable dynamic/bimanual/long-horizon skills from in-the-wild human demos.

**Strengths**
1. **Systems completeness:** names four prior failure modes (FoV, SfM precision, latency, multimodal capacity) and closes each with concrete design (HD*/PD*).  
2. **Ablations that bite:** fisheye, mirrors+reflect, relative vs abs/delta, latency, inter-gripper, ViT vs ResNet — large effect sizes, matched initials (Ablation Bot ranks #1–#8).  
3. **Cross-embodiment:** same cup ckpt UR5→Franka **90%** with honest joint-limit fails.  
4. **OOD data lesson:** ViT-L + narrow data = **0%**; diversity required — antidote to “just finetune CLIP.”  
5. Open hardware+software sufficient for third-party reproduce (SLAM difficulty disclosed).  
6. Correctly builds on **Diffusion Policy** rather than inventing a new generative head.  
7. Fair matched initial-state protocol across methods.

**Weaknesses / threats**
1. **Small eval n** (often 20 episodes); operator-judged success — videos help, stats thin; no multi-seed train variance.  
2. **SLAM texture dependence** admitted; wild success conditioned on mappable scenes.  
3. **Kinematic filtering** pushes embodiment limits to post-hoc data drop, not embodiment-aware learning.  
4. Still slower than bare hand; bulk/weight (**780 g**) limits marathon collection.  
5. Parallel-jaw only — no dexterous-hand transfer story.  
6. Latency numbers are **per-install**; Fig. 5’s 100/120 ms are examples, not universals.  
7. “~70% OOD” blends train/test cups and two scenes — report **43/60** precisely.  
8. Absolute-action baseline may be a strawman under oracle calib — paper’s point is calib-free deploy, so report clean **and** noisy-origin conditions.  
9. ACT-as-drop-in claimed but **not empirically compared** — TENTATIVE if asserted equal.  
10. ViT vs ResNet on dish: architecture × pretrain × task difficulty entangled.  
11. Object-level 105/120 tossing vs episode-level elsewhere — keep metric defs straight.  
12. Confounded “system” win: hard to isolate hardware vs action repr vs latency vs DP capacity — Empiricist RC1/RC2 correctly hold data fixed.

**Verdict:** Strong **systems + data-interface** paper; empirics are real-robot and ablation-rich. Main risk is overgeneralizing from cup-centric OOD and SLAM-friendly scenes. Accept-level RSS systems contribution.

**Reviewer exam bite:** Cite **relative 100/80/25** and **latency −30 pp** before claiming “UMI just works”; demand **43/60** not “~70%”; punish “ACT equals DP” without an ablate.

---

### 6.3 Visionary — WEIGHT **2** (substantial; BEHAVIOR + follow-ups; **no Discord/Drive**)

**BEHAVIOR connection:**  
UMI is a **data-engine prior** for household long-horizon stacks that BEHAVIOR-class benchmarks stress (wash, fold, multi-step kitchen). Relative wrist-centric EE + DP/FM chunks align with **π₀ / π₀.₅ / OpenPI** control habits; **π₀.₅** (2504.16054) and **GR00T-N1** (2503.14734) cite UMI (**CLEAR** from local fulltexts). For 2026 BEHAVIOR, UMI-style **handheld scaling** is the complementary pole to **DROID robot-in-wild** and **ALOHA lab teleop** — useful when organizers need **cheap scene diversity** without shipping Frankas.

**2026 BEHAVIOR handheld / in-the-wild teaching recipe (steal the recipe, not necessarily the GoPro BOM):**

1. **Teach anywhere:** prioritize wrist-centric portable demonstrators over lab-only teleop for scene diversity.  
2. **Standardize the interface, not the arm:** relative EE (+ continuous gripper) + wrist FoV + **latency sheet** per deployment — then swap UR/Franka/mobile bases.  
3. **Latency card mandatory:** every BEHAVIOR deploy publishes measured obs/infer/exec latencies and whether matching is on — especially for dynamic tasks.  
4. **Diversity cards:** log scene / object / demonstrator ids (Fig. 9); run Empiricist matched-N wild vs narrow ablations before claiming generalization.  
5. **Compose with fleet data:** mix UMI-style hand-held shards with DROID/OXE robot shards under schema tags (`source=handheld|robot`, `action=rel_ee`, `latency_matched=bool`).  
6. **Race on modern policies:** keep UMI interface; swap DP→**π₀.₅ / OpenPI / N1-class** FM heads with Empiricist horizon×latency grid.  
7. **Long-horizon + recovery:** dish-style stage SR and mid-episode perturbation recovery = first-class BEHAVIOR metrics.  
8. **Honest non-goals:** do not require reproducing OptiTrack SLAM ATE or 4 real tasks in course smoke; require RC0–RC2 directional proofs + written latency/action card.

**Follow-up research**
1. Embodiment-aware policies that consume kinematically-invalid demos (paper limitation #1).  
2. Textureless-room action recovery (third-person + gripper fiducials).  
3. Lighter / higher-DoF handheld (dex hand) while keeping observation alignment.  
4. Swap DP→**flow matching** (π₀-style) under same UMI interface; keep latency matching.  
5. RTC / soft-inpaint async chunks on top of UMI’s latency model for high-Hz arms.  
6. Mobile manipulator + UMI relative EE (paper already moves base mid-rollout).  
7. Internet-scale distributed MP4 collection (§IX vision) with SLAM QA filters.

**New applications**
- Rapid skill cloning in homes/restaurants without robot install during teach.  
- Cross-robot skill libraries (one demo set → UR/Franka/ARX).  
- Dynamic sports-like tossing/sorting beyond quasi-static BC.  
- Bimanual deformable + articulated appliance tasks as stress tests for VLA finetunes.

**Visionary one-liner:** UMI turns “in-the-wild” from **robot logistics** into **human logistics** — the missing portable **action-rich** demo pipe that later VLAs still cite when they need wrist-aligned, latency-honest manipulation data.

**Visionary exam bite:** “For BEHAVIOR’26, steal UMI’s **relative EE + latency card + handheld scene diversity**; race on π₀.₅/N1 — don’t retrain 2024 ResNet DP as the entry; **π₀.₅/GR00T CLEAR cite** UMI.”

*(No Discord / Drive checklist content.)*

---

### 6.4 Stakeholder (produced despite empty seat)

**Product one-liner:** Open **GoPro gripper → Diffusion Policy** pipe that buys **action-diverse in-the-wild demos** without wheeling a robot into the cafe.

**Who cares**
- Labs wanting **wild visual diversity** without Franka fleets (vs DROID cost).  
- Companies shipping policies across **multiple arms** (relative EE + twin wrist cam).  
- Course / BEHAVIOR teams needing a **reproducible handheld BC** recipe.  
- **Non-consumers:** no SLAM expertise; need dex hands; pure language-VLA without wrist cams.

**Buy / build decision**
1. Goal = **dynamic + bimanual BC from humans** → build UMI (~$400 + printer) before teleop carts.  
2. Goal = **76k Franka trajectories in real buildings** → DROID-style robot-in-wild still wins scale.  
3. Goal = **fine bimanual lab skills** → ALOHA/ACT joint teleop still strong.  
4. Always budget a **latency calibration day** on first deploy — not optional for toss-class tasks.

**Risks:** SLAM support burden; sunlight/domain shift; parallel-jaw ceiling; operator-eval success inflation; 780 g marathon fatigue.

**Stakeholder exam bite:** “UMI buys action-rich wild demos at ~$400 without robot logistics; DROID buys scene-volume with robots; ALOHA buys lab bimanual precision — pick the pole, then budget the latency day.”

---

### 6.5 Archaeologist (produced despite empty seat)

**Priors UMI absorbs (CLEAR)**  
- **Diffusion Policy** (Chi et al., 2303.04137) — generative action chunks + receding control [paper [9]].  
- Handheld quasi-static grippers: Grasping-in-the-Wild [41], Visual Imitation Made Easy [50], Dobb-E [36] — UMI’s foil for **action poverty**.  
- **ORB-SLAM3** + Urban GoPro/IMU calibration stack.  
- Soft/series-elastic fingers (SEED / Alspach TRI).  
- ACT [53] named as potential drop-in (**not** ablated).

**Contemporaneous contrast**  
- **ALOHA/ACT** lab puppet teleop (joint-space, embodiment-specific).  
- **DROID** (Mar 2024, close arXiv neighbor) robot-in-wild Franka teleop — opposite “who goes into the wild.”  
- Mobile ALOHA / GELLO — still robot-present collection.

**Descendants / citations (CLEAR from local packs/fulltexts)**  
- **π₀.₅** (2504.16054) bibliography cites UMI.  
- **GR00T-N1** (2503.14734) cites UMI.  
- Downstream deploy: umi-on-legs, umi-arx; community handheld forks.  
- Conceptual line into wrist-cam + relative-EE + chunk policy stacks (OpenPI recipes, BEHAVIOR finetunes) — **TENTATIVE** as named default baseline.

**Genealogy**

```
Lab teleop (ALOHA/ACT)  ∥  Diffusion Policy chunks
              ↓
     UMI handheld wild teaching (human-in-wild, relative EE + latency)
              ∥  DROID robot-in-wild (Franka fleet)
              ↓
     VLA / FM chunk policies (π₀ / π₀.₅ / GR00T) that still need
     UMI-class wrist-aligned, latency-honest demos
```

**Genealogy one-liner:** Lab teleop ∥ DP chunks → **UMI handheld wild teaching** ∥ DROID robot-in-wild → VLA/FM policies that still cite UMI for wrist-aligned demos.

**Archaeologist exam bite:** “UMI = in-the-wild **without** in-the-wild robots; relative EE + latency matching + fisheye wrist; DP Ta=6; OOD cup **~72%** after 1400/30; **π₀.₅/GR00T CLEAR cite**.”

---

## 7. Empiricist run pack (env / scripts / GPU — from DELIVERABLE)

### Env setup

```bash
export UMI_WORK_ROOT=$HOME/src
export UMI_DATA_ROOT=$HOME/data/umi_subsets   # EDIT — subsets / synthetic OK
cd /workspace/umi-quiet
bash env_outline/setup_mamba.sh
# mamba activate umi-scout
mkdir -p logs
# EDIT: clone official UMI + Diffusion Policy; pin commits → logs/pins.txt
# EDIT partition/account in slurm_*.sh
# Outlines only — do NOT sbatch from quiet box
```

| File | Role |
|------|------|
| `env_outline/conda_env.yaml` | `umi-scout` sketch (torch + DP/UMI deps placeholders) |
| `env_outline/setup_mamba.sh` | Create/update env; data/log dirs |
| `env_outline/run_checklist.md` | Ordered RC0→RC4 checklist |
| `env_outline/slurm_demo_ingest_smoke.sh` | CPU/light ingest gate |
| `env_outline/slurm_relative_action_ablation.sh` | Array over `{rel,abs,delta}` |
| `env_outline/slurm_latency_sweep.sh` | Eval-only latency grid |
| `env_outline/slurm_policy_train_smoke.sh` | 1-GPU short DP train |
| `env_outline/slurm_embodiment_transfer_stub.sh` | IK/feasibility cross-robot |

### GPU / RAM / TIME budget honesty

Assumptions: scout DP (CNN or small ViT), image ≤224, batch 32–64, **not** full paper real-robot protocol. **Wall-clock hours for paper full trains are NOT reported — do not invent them.**

| Setup | GPU VRAM (peak) | Host RAM | Time / run (order) |
|-------|-----------------|----------|--------------------|
| RC0 ingest (50 eps) | — | 8–16 GB | **<0.5 h CPU** |
| RC1 action-repr ×3 short | **10–18 GB** | 32 GB | **2–8 h** total A100 |
| RC2 latency sweep (infer) | same as ckpt | 16–32 GB | **<1–4 h** |
| RC3 policy smoke 5k–20k | **12–24 GB** (ViT higher) | 32–64 GB | **4–12 h** A100; ~1.5–2× on 4090 |
| RC4 embodiment stub | ≤RC3 or CPU | 16 GB | **<1–3 h** |
| Dish-like ViT FT stretch | **20–40 GB** | 64 GB | **8–20 h** — defer if tight |
| Paper full 4-task real eval | robot time + multi-day train | — | **Out of course smoke** |

**Course envelope:** target **≤ ~25 GPU-h** for P0–P1 matrix (RC0–RC4). Full HD/PD × multi-seed × real robots = **hundreds of GPU-h + robot hours** — do not promise.

**Deploy latency budget:** paper-scale arm/gripper exec ~**100–120 ms** examples; DP infer (DDIM×16) must leave room inside control period after matching — link `/workspace/diffusion-policy-quiet/`.

### Success criteria vs paper claims

| Claim (paper) | Minimal course criterion | Stretch |
|---------------|--------------------------|---------|
| Relative ≫ Absolute (100% vs 25%) | RC1: Rel beats Abs on origin-jitter OOD | Match clean-ID ranking vs Delta |
| Relative ≥ Delta (100% vs 80%) | Rel ≥ Delta on ID MSE / proxy | Full cup-proxy success |
| Latency matching critical (−30 pp) | RC2: match > zero on **dynamic** velocity/timing | Quantify −pp on real toss |
| Cross-embodiment 90% | RC4: feasibility transfer with kin-filter | Real Franka 18/20 |
| Inter-gripper (70% vs 30%) | Toggle improves sync on dual-arm shard | Real cloth 14/20 |
| In-the-wild ~70% OOD | Multi-scene > single-scene @ matched N | Cafe/fountain protocol |
| DP fits multimodal | RC3 samples both rotate modes if present | Cup 20/20 |
| SLAM ATE ~6.1 mm / 3.5° | Ingest reports pose quality proxy | Reproduce §VII on hardware |
| 3× teleop throughput | **Out of scope** (hardware study) | — |

**Pack pass bar:** Empiricist can execute RC0–RC2 from outlines and obtain **paper-aligned directional** results on proxies; RC3–RC4 attempted or blocked only by missing public shards (document blocker).

---

## 8. Slide Maker brief

| Role | Weight / timing guide | Focus |
|------|----------------------|-------|
| **Empiricist** | **1 · LONGEST** | Hypotheses, RC0–RC4, exact reproduce cmds, GPU honesty, ablation matrix — **~16–18 slides** |
| **Reviewer** | **4 · substantial** | Ablations that bite; 43/60 not “~70%”; verdict Accept systems |
| **Visionary** | **2 · substantial** | BEHAVIOR handheld recipe; π₀.₅/GR00T cite; **no Discord/Drive** |
| Stakeholder | filled (empty seat) | Buy/build poles vs DROID/ALOHA; latency day |
| Archaeologist | filled (empty seat) | Genealogy ALOHA∥DP→UMI∥DROID→π₀.₅/GR00T |

**Spine (paste into Slide Maker) — 5 bullets:**
1. **In-the-wild teaching without in-the-wild robots** — human logistics → robot-ready demos via relative EE + latency honesty.  
2. **Must-cite numbers:** cup **100%/90%**; toss **87.5%** (no lat **57.5%**); wild **1400/30 → 71.7%** vs narrow **0%**; rel **100%** vs abs **25%**; fisheye **100%** vs **55%**.  
3. **Motion:** DP **Ta=6** @10 Hz (toss 20 Hz); relative SE(3) w.r.t. chunk \(t_0\); latency match (discard stale + send-ahead; ~100/120 ms examples).  
4. **Collection:** ~$371 GoPro gripper; map-then-localize SLAM; >3× teleop throughput; complementary to **DROID** (robot-in-wild) and **ALOHA** (lab teleop).  
5. **BEHAVIOR’26 + lineage:** steal relative EE + latency card + handheld diversity; race on π₀.₅/N1; **π₀.₅/GR00T CLEAR cite** UMI; Empiricist RC0–RC2 directional smokes — **never invent GPU hours**.

**GitHub title sketch:** `[ECE 605] - UMI Making Empiricist`

Full Slide Maker brief: `/workspace/umi-quiet/SLIDE_BRIEF.md`

---

## Paths

| Artifact | Path |
|----------|------|
| This pack | `/workspace/umi-quiet/KANTA_PACK.md` |
| PDF | `/workspace/umi-quiet/KANTA_PACK.pdf` |
| Slide brief | `/workspace/umi-quiet/SLIDE_BRIEF.md` |
| Senior Research | `/workspace/umi-quiet/DELIVERABLE.md` |
| Summarizer | `/workspace/papers/umi-2402.10329-summary.md` |
| Ablations | `/workspace/papers/umi-2402.10329-ablations.md` |
| Paper notes | `/workspace/umi-quiet/paper_notes.md` |
| Env outlines | `/workspace/umi-quiet/env_outline/` |
| Paper PDF | `/workspace/papers/umi-2402.10329.pdf` |
| Page figs | `/workspace/papers/umi-figs/page-*.png` |

---

*Pack synthesized 2026-09-29 PT (HST) for KanBot / ECE 605 Paper Manager — Nov 16 Data · Empiricist=1 (Kanta PRIMARY) · Reviewer=4 · Visionary=2 · all five seats produced.*
