# OpenPI Comet — Paper Manager pack (smoke run)

**Paper:** Openpi Comet: Competition Solution For 2025 BEHAVIOR Challenge  
**arXiv:** https://arxiv.org/abs/2512.10071 · PDF: https://arxiv.org/pdf/2512.10071.pdf  
**Code:** https://github.com/mli0603/openpi-comet · HF: sunshk/openpi_comet  
**Schedule (Sep 2 smoke):** Stakeholder=1 (Kanta), Reviewer=2, Empiricist=4, Archaeologist=2, Visionary=3  
**Sources:** Paper Summarizer + Senior Research (Ablation Experimenter) + Ablation inventory

---

## 1. Paper summary

Team Comet adapts public **π₀.₅ (OpenPI)** to **50** long-horizon BEHAVIOR-1K household tasks with a data mix (teleop + planner + offline RL), multi-task pre-train coverage (pt1→pt50), **Rejection Sampling Fine-Tuning (RFT)**, and **receding-horizon** inference — **no** modular skill chaining.

- **Contest:** 2nd place — test Q **0.2514**, **22/50** tasks (winner 0.2599).
- **Post-challenge** (refined balancing): val Q **0.3453**, SR **0.1500** (above winner’s val Q 0.2605).
- **Story:** Public VLA + data flywheel is competitive; hard half of the suite and a **0.345 → 0.611** oracle gap remain.

**Method in one line:** visual (head+wrist RGB) + language + proprio → transformer action expert → continuous control; pre-train ~1.5k hours; RFT N=3 (~8500 rollouts/round → **1469** kept); serve with receding horizon, action horizon **32**, absolute joints **30 Hz**, state on.

---

## 2. Important model / keywords

| Kind | Terms |
|------|--------|
| Backbone | π₀.₅, OpenPI, VLA, flow/diffusion action expert |
| Benchmark | BEHAVIOR-1K, BDDL, Q-score, NeurIPS 2025 BEHAVIOR Challenge |
| Training | SFT, multi-task pre-train (pt1/7/10/50), RFT, rejection sampling |
| Inference | Receding horizon, temporal ensemble, action horizon H=32 |
| Data / control | Absolute vs delta joints, 30 Hz vs 15 Hz, proprio state, RGB vs depth vs point cloud |
| Metrics | Success rate (SR), Q-score, theoretical-best oracle (0.611) |
| Stack | JAX, OmniGibson, H200 batch 64/device |

**Default ablation baseline (Tables 3–4):** Receding Horizon, H=32, RGB 224×224, absolute joints, 30 Hz, state on — task `turning_on_radio`.

---

## 3. Role-ready bullets (for slides / talk)

### Stakeholder (18+3) — Kanta on this schedule
- **Motivation:** Minute-scale household skill chaining fails; BEHAVIOR is the public scoreboard.
- **Problem:** 50 full BDDL activities; rank by test Q; bet one end-to-end public VLA can compete without a skill graph.
- **Method:** π₀.₅ + **motion-planner/offline-RL data** + coverage ladder + 3-round RFT + **receding-horizon** control (H=32), abs/30Hz/state/RGB — no modular skill planner at inference.
- **Findings:** Contest 2nd (0.2514); post-challenge val Q 0.3453; wrong controller/Hz/frame → ~0%; high-res doubles probe SR; oracle 0.611 = headroom.
- **Takeaway:** Public VLA + flywheel works; gap is post-train efficiency + hard tasks.

### Scientific Reviewer (8+3)
- **Strengths:** Reusable recipe; coverage ablation; specified RFT; inference/data ablations; honest headroom; code out.
- **Weaknesses:** Q-score undefined; ablations mostly on easy `turning_on_radio`; post-challenge missing test Q; oracle easy to misread; no error bars; no modular baseline.
- **Actionable fixes:** Define Q; hard-task ablation panel; post-challenge test Q; mean±std; publish balance weights; hierarchical baseline; held-out RFT; turn oracle into a router.

### Empiricist (11+3)
- **H1:** Under RH + abs joints + proprio + RGB, **H=32 ≫ H=8** on `turning_on_radio` (paper 0.30 vs 0.00).
- **Implement:** clone openpi-comet + BEHAVIOR-1K; warm-start HF `pi05-b1kpt12-cs32`; two short SFT configs (~2k steps); `serve_b1k` receding_horizon; eval 5–10 instances.
- **Success:** ordinal gap H32−H8 ≥ +0.15 (or H32>0, H8≈0). Absolute 0.30 / full Q **out of scope** for smoke.
- **Lighter alt:** fix H32 ckpt; eval-only control-mode (RH vs temporal ensemble).

### Archaeologist (11+3)
- **Priors that shaped it:** π₀ (Black et al.); RT-1/RT-2; Octo/OpenVLA; BEHAVIOR-1K (Li et al. 2024); skill-chaining lit (rejected); LLM-style RFT transplanted; PiRL/RLinf (why online RL is hard here).
- **Lineage:** Adaptation study, not a new VLA architecture.
- **Subsequent:** Peer-reviewed follow-ups unclear; team released repo, post-challenge ckpts, HF RFT set (`comet-1.5k`).

### Visionary (11+3) — content only (no Discord/Drive)
- **→ 2026 BEHAVIOR:** Edge = sample-efficient post-train (DAgger/experts/rewards), skill curriculum, eval-time perception fidelity — not opaque longer pretrain alone.
- **Follow-ups:** Privileged DAgger; checkpoint router; hard-task curricula; resolution/compute Pareto; sim-to-real of the *recipe*.
- **Don’t over-claim:** Not a new foundation model; not real-robot deployment; household agent not solved.

---

## 4. Empiricist run pack (if you are Empiricist)

### Env setup (outline)
Full files: `/workspace/openpi-comet-smoke/env_outline/`  
(`environment.yml`, `setup_mamba.sh`, sbatch scripts)

```bash
# Login node sketch (upstream prefers uv; hybrid mamba OK on Slurm)
mamba env create -f env_outline/environment.yml
mamba activate openpi-comet

git clone https://github.com/mli0603/openpi-comet.git
git clone https://github.com/StanfordVL/BEHAVIOR-1K.git
# Then: uv sync / install per Comet README; separate behavior env for OmniGibson[eval]
# Warm-start: HF pi05-b1kpt12-cs32 (or pi05_base)
```

### Train (smoke E01 — H=32 vs H=8)
```bash
export XLA_PYTHON_CLIENT_MEM_FRACTION=0.9
# Configs: pi05_b1k-radio_h32 / pi05_b1k-radio_h8 — action_horizon 32|8,
# tasks=["turning_on_radio"], num_train_steps≈2000 (paper 15–20k)
uv run scripts/train.py pi05_b1k-radio_h32 --exp_name="smoke_h32_${SLURM_JOB_ID}"
uv run scripts/train.py pi05_b1k-radio_h8  --exp_name="smoke_h8_${SLURM_JOB_ID}"
```
sbatch: `sbatch_smoke_train_h32.sbatch`, `sbatch_smoke_train_h8.sbatch`

### Eval
```bash
# Job A — policy server
uv run scripts/serve_b1k.py \
  --task_name=turning_on_radio \
  --control_mode=receeding_horizon \
  --max_len=32 \
  policy:checkpoint \
  --policy.config=pi05_b1k-base \
  --policy.dir=$PATH_TO_CKPT

# Job B — BEHAVIOR eval (behavior conda env)
python OmniGibson/omnigibson/learning/eval.py \
  policy=websocket \
  task.name=turning_on_radio \
  log_path=$LOG_PATH
```
sbatch: `sbatch_smoke_eval.sbatch`

### Estimated GPU time (E01 smoke)
| Stage | Budget |
|-------|--------|
| SFT H=32, ~2k steps | ~2–6 GPU-h |
| SFT H=8, same | ~2–6 GPU-h |
| Eval 5–10 instances ×2 ckpts | ~10–40 GPU-h wall (sim-bound) |
| **Total** | **~15–50 GPU-h** |
| RAM | LoRA **>22.5 GB** or full FT **>70 GB** — prefer **1×A100-80 / H100 / H200** |

### Prioritized smokes (from Senior Research)
| Priority | ID | Experiment |
|----------|----|------------|
| P0 | E01 | Horizon H=8 vs 32 on `turning_on_radio` |
| P0 | E02 | Control mode RH vs temporal ensemble (eval-only) |
| P0 | E03 | Resolution 224 → 720/480 |
| P0 | E08 | Single-task SFT from released HF ckpt (prerequisite) |
| Not smoke | E11–E14 | Full pt50, full RFT, 50-task Q |

Full table: `/workspace/openpi-comet-smoke/DELIVERABLE.md`

---

## 5. Slide Maker brief (timing + visual style)

| Role | Talk | Q&A | Visual bias |
|------|------|-----|-------------|
| Stakeholder | 18 min | 3 | System diagram (Fig1), leaderboard, coverage ladder |
| Reviewer | 8 min | 3 | Strength/weakness cards; one ablation critique slide |
| Empiricist | 11 min | 3 | H1 diagram; horizon table; env/run one-pager |
| Archaeologist | 11 min | 3 | Citation timeline / lineage map |
| Visionary | 11 min | 3 | BEHAVIOR 2026 bridge; follow-up map |

**Style:** Prefer **images as the slide** (model figure, tables as figures); put bullet talking points in **speaker notes**. Avoid repeating Stakeholder background in later roles.

---



---

## 6. Deep dive: Motion planning & long-horizon control (priority topic)

This is the paper’s real engineering story: **not** a classical online motion planner at test time, but (1) **motion-planner / offline-RL data** in pre-training, plus (2) **receding-horizon action-chunk control** at inference, with carefully chosen horizon / action representation / rate / state.

### 6.1 What “planning” means in OpenPI Comet

| Layer | What they do | What they deliberately *don’t* do |
|-------|----------------|-------------------------------------|
| **Data** | Mix human teleop with **~3.6K motion-planner demos + offline RL rollouts** (~0.4K hours) | Rely only on noisy human demos |
| **Architecture** | Single end-to-end **π₀.₅** VLA (vision + language + proprio → continuous actions) | Modular skill graph / separate local policies + explicit skill chaining at deploy |
| **Inference** | **Receding Horizon**: predict an action segment of length **H**, execute it, then **re-plan** from new observations | Temporal Ensemble / Receding Temporal (open-loop smoothing → ~0% SR) |
| **Training** | Coverage ladder pt1→pt50 + RFT flywheel | Online RL in OmniGibson (too slow; split GPU needs) |

**Paper quote (paraphrased core claim):** Temporal Ensemble / Receding Temporal fail closed-loop stability (~0 SR). Receding Horizon “executes all the predicted action segment and performs re-planning after finishing manipulation,” and is necessary for long-horizon manipulation because smoothing open-loop predictions accumulates error.

### 6.2 Motion-planner data (why it matters)

Beyond the official **10k** teleop demos (>1,200 h):

- Extra **~3.6K** trajectories = **motion-planner demonstrations + offline RL rollouts**.
- **Planner demos:** precise, **low-noise** manipulation sequences (cleaner state–action pairs for contact-rich moves).
- **Offline RL rollouts:** broader behavioral variability (coverage the teleop set lacks).
- Together they “substantially enrich state–action coverage beyond human demonstrations.”

**Stakeholder angle:** Comet’s “planning” advantage starts in the **dataset**, not in a runtime planner module. The VLA *imitates* high-quality planned / optimized trajectories, then executes with closed-loop chunking.

**Reviewer angle:** Planner + RL stacks are **underspecified** (which planner? cost function? what offline RL algorithm?). Hard to reproduce the data mix exactly from the paper alone — code/HF help, but isolation of planner-data ablation vs teleop-only is not in Tables 3–4.

### 6.3 Why they reject classical skill-chaining planners

Intro argument:

- Long-horizon household tasks need **orchestrated interdependent behaviors**; compounding error + shifting state distributions kill open-loop / short-horizon VLAs.
- Common fix: decompose into subtasks + local policies — but that **doesn’t solve skill chaining** (reliable transitions between skills).
- Many chaining methods need online adaptation or modular stacks **incompatible** with large-scale offline end-to-end VLA training.
- Comet’s bet: push one public VLA with **data + training + inference design**, no skill graph at inference.

**Archaeologist hooks:** Konidaris skill chaining; SCAR; DiffSkill / hierarchical methods — cited as the rejected alternative lineage.

### 6.4 Inference-time control = the “online planner”

Default recipe that works on `turning_on_radio` ablations:

1. **Control mode = Receding Horizon** (SR **0.25** vs Temporal Ensemble **0.00** / Receding Temporal **0.00**)
2. **Action horizon H = 32** (non-monotonic: 8→0.00, 16→0.10, 50→0.25, **32→0.30**)
3. **Absolute joint** actions (delta → 0.00)
4. **30 Hz** sampling (15 Hz → 0.00)
5. **Proprioceptive state on** (off → 0.00)
6. RGB OK; high-res Head720/Wrist480 doubles SR (0.30→0.60); depth hurts; point cloud ≈ RGB but costly

**Intuition to say out loud:**

- **Too short H:** myopic; can’t anticipate multi-stage behaviors.
- **Too long H:** conflicting temporal dependencies; short-term control becomes unreliable.
- **H=32** ≈ sweet spot between “look ahead” and “stay corrigible via replan.”
- **Absolute joints + proprio + 30 Hz** = observability + temporal precision for closed-loop optimization.
- **Manip:Nav = 2:1 reweight does nothing** (0.30=0.30): navigation dominates frames (`move to` ~33%), but **resampling ≠ fixing** heterogeneous long-horizon dynamics.

### 6.5 Navigation vs manipulation (mobile manip “planning”)

- Tasks need **coordinated navigation + precise manipulation** (multi-room, containers, multi-object).
- Skill imbalance: `move to` 33.3%, `pick up from` 24.4%, `place in` 8.8%, long tail 11.5%.
- Hard tasks (e.g. 48/49): long trajectories, 5–12+ skills, frequent **nav↔manip** switches = temporal credit assignment stress test.
- Easy bring-up tasks: avg length **<250 frames** — used to validate control stack before scaling.

**Empiricist implication:** Smoke on `turning_on_radio` tests the **control/horizon stack**, not full mobile multi-room planning. Say that explicitly in talk.

### 6.6 RFT as iterative “plan repair” offline

Algorithm 1 sketch:

1. Start from human demos + pretrained π.
2. Perturb initial pose → roll out → keep **BDDL successes only** → merge → retrain.
3. N=3 rounds; ~8500 traj/round → **1469** kept after dedup + task balancing.
4. Reject online RL: sim wall-clock 1h–1d/task; RT-core vs Tensor-core GPU split.

This is an **offline data flywheel** that improves robustness under pose disturbance — complementary to motion-planner demos (precision) and RH inference (closed-loop).

### 6.7 Talking points by role (motion-planning lens)

**Stakeholder:** “We didn’t ship a skill planner. We taught π₀.₅ on planner-quality trajectories, then ran it with receding-horizon re-planning. Wrong controller or wrong H → zero success.”

**Reviewer:** “Strong inference ablations, but planner/RL data contribution never isolated; almost all control ablations on one easy task; ‘re-planning after finishing manipulation’ wording needs precise chunk-execute semantics vs classic MPC.”

**Empiricist:** Primary smoke = **H=32 vs H=8 under RH** (and optional control-mode eval-only). That *is* the motion-control claim you can falsify in a week of GPU.

**Archaeologist:** Place Comet between (a) classical / learned skill-chaining planners and (b) short-horizon VLAs — adaptation study that imports LLM-style RFT and MPC-like chunking into OpenPI.

**Visionary → 2026 BEHAVIOR:** Next wins likely from **structured long-horizon reasoning** (authors’ own conclusion), privileged DAgger / denser rewards, curricula over nav–manip imbalance, and maybe a **router over checkpoints** (oracle union Q 0.611) — not just longer open-loop chunks.

### 6.8 One diagram to put on a slide

```
[Human teleop] + [Motion planner demos] + [Offline RL]
                    │
                    ▼
              π₀.₅ pretrain (pt coverage)
                    │
                    ▼
              RFT loop (pose perturb → success filter)
                    │
                    ▼
     Serve: Receding Horizon, H=32, abs joints, 30Hz, state
                    │
                    ▼
         execute chunk → observe → replan → …
```


## Quick paths on box
| Artifact | Path |
|----------|------|
| This pack | `/workspace/openpi-comet-smoke/KANTA_PACK.md` |
| Senior Research deliverable | `/workspace/openpi-comet-smoke/DELIVERABLE.md` |
| Ablation inventory | `/workspace/papers/2512.10071-ablations.md` |
| PDF | `/workspace/openpi-comet-smoke/paper.pdf` or `/workspace/papers/2512.10071.pdf` |
| Env / sbatch | `/workspace/openpi-comet-smoke/env_outline/` |
