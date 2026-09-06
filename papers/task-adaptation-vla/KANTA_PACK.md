# Paper Manager pack — 1st Place BEHAVIOR 2025 (Robot Learning Collective)

**Paper:** Task adaptation of Vision-Language-Action model: 1st Place Solution for the 2025 BEHAVIOR Challenge  
**arXiv:** https://arxiv.org/abs/2512.06951 · PDF: https://arxiv.org/pdf/2512.06951.pdf  
**Code:** https://github.com/IliaLarchenko/behavior-1k-solution  
**HF submission:** https://huggingface.co/IliaLarchenko/behavior_submission (4 ckpts)  
**Session:** Sep 9 · due 9:30 AM · **You = Empiricist (seat 1)**  
**Sources:** Paper Summarizer + Senior Research (`DELIVERABLE.md`)

---

## 1. Paper summary

Team **Robot Learning Collective** adapts **Pi0.5 / OpenPI** to the 50-task BEHAVIOR Challenge with:

- **Correlated-noise flow matching** (+ correlation-aware soft inpainting at infer)
- **Learnable mixed-layer KV attention**
- **System 2** stage tracking + voting (non-Markovian context for visual aliasing)
- **Multi-sample** FM training, **1.3×** cubic action compression
- Light **gripper / radio heuristics**
- Language dropped → **50 trainable task embeddings**; ship **4** group fine-tuned checkpoints

**Result:** 1st place — q_score **~26%** (pub **0.2605** / priv **0.2599**); binary success only **~11–12%**. Partial progress ≈ half of q-score. Authors call this a **competition report**, not a full ablation paper — opening for Empiricist.

**Train cost (paper):** 8×H200 FSDP; ~15 days multi-task + ~1 week/group FT; ~2 epochs; ~$13k. Infer: single **RTX 4090 24GB**.

---

## 2. Important model / keywords

| Kind | Terms |
|------|--------|
| Backbone | Pi0.5, OpenPI, SigLIP-So400m/14, PaliGemma, action expert ~311M |
| Training | Flow matching, correlated noise β=0.5, multi-sample N=15, FAST aux λ=0.05, FSDP |
| Control / “planning” | Action chunk H=30, exec 26, soft inpaint 4, rolling RTC-style stitch, System 2 stages, gripper recovery |
| Data | 10k demos, task embeddings (no NL), delta joints D=23, 30 Hz |
| Metrics | q_score (partial goal fraction), binary success, OmniGibson/BDDL |
| Deploy | 4 FT ckpts + task→ckpt map; websocket `serve_b1k.py` |

---

## 3. Motion planning / long-horizon control (priority lens)

**Not classical RRT/MPC.** Stack = learned action-chunk FM + rolling receding-horizon-style execution + stage hierarchy + rule recovery.

| Theme | Paper detail |
|-------|----------------|
| Horizon | Avg episode ~6.6 min; longest ~14 min |
| Chunking | Predict **H=30** → execute **26** → overlap/inpaint **4** |
| Closed vs open | Mostly open-loop *inside* chunk; replan every 26 steps; soft inpaint (t>0.3) frees early steps for reactivity; **System 2 vote + gripper rules = closed-loop overrides** |
| Compression | Cubic 26→20 @ 30 Hz (**1.3×**); base vel ×1.3; disable on gripper change |
| Hierarchy | Stages 5–15/task; vote window 3 (≥2/3 forward; rollback rules) |
| Skill chaining | Implicit via stages — **no** skill library / separate planner passes (simpler than Hi Robot CoT) |
| Absent | Classical MP, MPC, ACT temporal ensemble, skill graphs |

**Empiricist talk spine:** What fraction of “1st place VLA” is actually **outer-loop control** (gripper recovery + System 2), not train-time architecture?

---

## 4. Role-ready bullets

### Stakeholder (18+3) — seat 5
- **Motivation:** Multi-minute bimanual + mobile household chores still unsolved.
- **Problem:** 50 tasks; rank by partial-progress **q_score** on held-out instances.
- **Method:** Pi0.5 + correlated FM + System 2 + mixed-layer attn; multi-task IL; light heuristics; 4 specialized ckpts.
- **Findings:** ~26% q pub≈priv → 1st; binary ~11–12%; gripper heuristic huge on grasp subsets; multi-task → emergent recovery; undertrained (~2 epochs).

### Scientific Reviewer (8+3) — seat 4
- **Strengths:** Code+HF; coherent correlated-noise train↔infer story; honest “not rigorous science”; pub≈priv; System 2 for aliasing.
- **Weaknesses:** No factorial ablations; task-ID embs + 4 ckpts weaken open-vocab VLA claim; heuristics mixed with science; underspecified LR/seeds; gripper 2.2× on only 13×3 eps.
- **Actionable:** Ablate β, System 2, inpaint modes, 1 vs 4 ckpts; seeds + curves; separate competition heuristics from methods tables.

### Empiricist (11+3) — **YOU (seat 1)**
See §5–7 below. Headline hypothesis **H_emp** + P0 factorial E1×E2.

### Archaeologist (11+3) — seat 2
- **Priors:** BEHAVIOR-1K; π₀/π₀.₅; GR00T (last-layer XAttn foil); RT-1/2; FM / Diffusion Policy; FAST; ACT chunking; **RTC / real-time chunking**; Hi Robot (foil); SigLIP/PaliGemma.
- **Subsequent:** Too new (Dec 2025); author repo/blog/video only so far.

### Visionary (11+3) — seat 3
- **2026 BEHAVIOR:** Bar still ~26% partial avg — room for recovery data, outer-loop System 2 / VLM checklists, unify to 1 multi-task ckpt, require with/without-heuristics reporting.
- **Follow-ups:** Corrective offline RL; privileged teacher; semantic stages; dexterity residual policies; real-robot correlated FM + soft inpaint vs RTC.
- Skip Discord/Drive logistics.

---

## 5. Empiricist hypothesis (P0)

> **H_emp:** A large share of published q_score lift is **closed-loop recovery + non-Markovian stage context**, not only train-time architecture. Disabling the **gripper correction rule** drops partial success on grasp-heavy tasks (order-compatible with paper’s **~2.2×** subset claim); disabling **System 2 voting** raises order/confusion failures on visually ambiguous tasks; **both off** is worst.

**Why Sep-9 feasible:** Inference-only on HF submission ckpts — no 8×H200 month.

**Success criteria:** Directional gripper drop + System2 failure-mode shift. **Do not** gate on global q=0.26.

---

## 6. Prioritized experiments (Senior Research)

| Priority | ID | Experiment |
|----------|----|------------|
| **P0** | E1 | Gripper-rule OFF |
| **P0** | E2 | System 2 voting OFF |
| **P0** | E3 | E1×E2 factorial ← **your talk figure** |
| P1 | E4–E7 | Soft inpaint / compression / exec horizon 10–26 / 1 multi-task ckpt vs 4 |
| P2 / NOT Sep 9 | E8–E12 | Correlated noise retrain, attn freeze, multi-sample, FAST λ, full 50-task |

Suggested subset: ~8–12 tasks (grasp-heavy + radio/microwave aliasing) × 3–5 instances × 4 conditions.

---

## 7. Env setup + run scripts + GPU estimates

**Outlines on box:** `/workspace/vla-task-adapt-behavior/env_outline/`  
(`environment.yml`, `setup_mamba_slurm.sh`, `sbatch_policy_server.sh`, `sbatch_eval_array.sh`, `run_ablation_matrix.sh`)

### Setup sketch
```bash
# Clone solution + BEHAVIOR-1K; bash setup_remote.sh (upstream)
# Prefer HF submission ckpts for P0 — skip 2TB demos / 260GB RGB unless training
# mamba env for sim libs + uv for JAX serve (see env_outline/)
```

### Policy server + eval
```bash
uv run scripts/serve_b1k.py --task-checkpoint-mapping task_checkpoint_mapping.json \
  policy:checkpoint --policy.config pi_behavior_b1k_fast --policy.dir $CKPT_DIR

python BEHAVIOR-1K/omnigibson/learning/eval.py \
  log_path=$LOG_PATH policy=websocket \
  task.name=$TASK model.host=$HOST eval_instance_ids="[$IDS]"
```

Drive conditions via `run_ablation_matrix.sh` flags: gripper on/off, System2 on/off, optional compression / exec_horizon.

### Train (P2 only — not required for your talk)
```bash
uv run scripts/train.py pi_behavior_b1k_fast \
  --batch_size=2048 --num_train_steps=200000 \
  --fsdp_devices=8 --save_interval=250 --keep_period=4000 --log_interval=25
```

### GPU / RAM budget (E1–E3)
| Item | Estimate |
|------|----------|
| Smoke (1 task × 1 inst × 2 cond) | **2–6 GPU-h** |
| Full P0 matrix (~10×4×3–5 eps) | **~40–120 GPU-h** |
| Policy VRAM | **~24 GB** (4090-class) |
| Eval node sys RAM | **≥48–64 GB** (leaks; don’t pack many sims/node) |

### HF ckpt task splits (README)
- Ckpt1 (20): 2,3,5,6,10,11,13,14,15,19,23,24,25,28,29,34,42,44,47,48
- Ckpt2 (16): 0,1,7,8,9,12,16,17,18,20,21,22,26,30,43,45
- Ckpt3 (13): 4,27,31,32,33,35,36,37,38,39,41,46,49
- Ckpt4 (1): 40

---

## 8. Slide Maker brief

| Role | Timing | Spine |
|------|--------|-------|
| Stakeholder | 18+3 | Leaderboard + stack; **motion-control spine**: chunk H=30→exec26→inpaint4 + System2 + gripper |
| Reviewer | 8+3 | Strengths / missing factorials / heuristic leakage |
| **Empiricist** | **11+3** | **H_emp**, 2×2 factorial figure, env/run/GPU one-pager |
| Archaeologist | 11+3 | Lineage: ACT/RTC/π₀.₅/BEHAVIOR (no Stakeholder redo) |
| Visionary | 11+3 | 2026 bar + follow-ups (no Discord/Drive) |

**Style:** Image-first; bullets in speaker notes. Prefer **PDF** export for Mac. Center long-horizon **control** story (not classical planner).

---

## Quick paths
| Artifact | Path |
|----------|------|
| This pack | `/workspace/vla-task-adapt-behavior/KANTA_PACK.md` |
| Senior Research | `/workspace/vla-task-adapt-behavior/DELIVERABLE.md` |
| Env / sbatch | `/workspace/vla-task-adapt-behavior/env_outline/` |
| PDF paper | `/workspace/vla-task-adapt-behavior/paper.pdf` |
