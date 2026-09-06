# Paper Manager pack — ACT / ALOHA (Oct 12)

**Paper:** Learning Fine-Grained Bimanual Manipulation with Low-Cost Hardware  
**arXiv:** https://arxiv.org/abs/2304.13705 · RSS 2023  
**Site:** https://tonyzhaozh.github.io/aloha/  
**PDF:** /workspace/papers/act-aloha-2304.13705.pdf  
**Session:** Oct 12 · due 9:30 AM · Empiricist=3, Archaeologist=5; others empty — full five packs  
**Sources:** Summarizer + Senior Research (`/workspace/act-aloha-quiet/DELIVERABLE.md`)

---

## 1. Paper summary

**ALOHA** = <$20k leader–follower bimanual teleop (WidowX→ViperX, 4 RGB, 50 Hz). **ACT** = ~80M CVAE+Transformer predicting **action chunks** (k×14 absolute joints) + **temporal ensembling**. Fine contact-rich skills from ~10–20 min demos (e.g. Open Cup 84%, Slot Battery 96%, Ziploc 88%, Shoe 92%).

**Club line:** Parallel 2023 chunk ancestor with Diffusion Policy → π₀/π₀.₅ FM chunks → RTC → BEHAVIOR.

---

## 2. Keywords

ALOHA, ACT, action chunking, temporal ensembling, CVAE, bimanual teleop, absolute joints @ 50 Hz, Diffusion Policy lineage, π₀ chunks, RTC, BEHAVIOR

**Default:** k=100 (~2s @ 50 Hz); L1+βKL (β=10); test z=0; TE every-step query + exp-weighted overlap.

---

## 3. Motion / control spine (IMPORTANT — do not conflate)

| Scheme | Mechanism |
|--------|-----------|
| **ACT TE** | Every-step query; exp-avg overlapping preds for same t |
| **DP receding** | Predict T_p, exec T_a<T_p, replan |
| **π₀ open-loop** | H=50; exec prefix; **no TE** (hurt early for them) |
| **RTC soft-inpaint** | Async; freeze prefix; soft-mask remainder |
| **BEHAVIOR’25** | Predict 30 / exec 26 / keep 4 + correlated FM |

**CLEAR:** chunking is shared; TE ≠ receding ≠ open-loop ≠ soft-inpaint.

Map: `k/50` ↔ DP `Ta·Δt` ↔ π₀/Comet `H/Hz`.

---

## 4. Role packs

### Stakeholder (18+3)
Cheap bimanual data engine + chunking control prior. Not a foundation model (per-task ~80M). Follow-ons: Mobile ALOHA, ALOHA 2. 2026 BEHAVIOR stacks inherit chunk horizons/overlap from this fork.

### Scientific Reviewer (8+3)
**Strengths:** hardware+algo synergy; hard tasks with subtask tables; ablations for k/TE/CVAE; honest failures.  
**Critiques:** real 1 seed×25 thin; BeT/RT-1 without chunks in main tables; z=0 discards multimodality; no language; 30 fps cams vs 50 Hz control underspecified.  
**Verdict:** Right cite for action chunking + low-cost bimanual teleop.

### Empiricist (11+3) — weight 3
**H:** Mid chunk (~2s) beats k=1 and episode-long; TE reduces jerk (+~3pp paper); CVAE critical for human demos, ≈noop for scripted.

| Priority | Experiment |
|----------|------------|
| P0 Y | k∈{1,10,50,100,200,400} (expect peak ~100); TE on/off |
| P0 Partial | CVAE on/off human vs scripted |
| P1 | Chunking on BC-MLP; target vs delta; L1 vs L2 |
| Defer | Full real ALOHA 6-task / paper seed protocol |

Budget: ~8–12 GB; quiet P0–P1 ≲40–60 GPU-h; sim Transfer Cube first. Env: `/workspace/act-aloha-quiet/env_outline/`.  
Tattoo: k=1→100 success **1%→44%**; CVAE on human **35.3%→2%** without.

### Archaeologist (11+3) — HIGH weight 5
**Priors:** BC/ALVINN; DAgger compounding; Lai chunking psych; Transformers/BERT; CVAE/β-VAE; BeT/RT-1/VINN; joint-space teleop; ResNet.  
**Pivot:** popularized action chunking + TE for high-Hz visuomotor BC on open low-cost bimanual HW.  
**Subsequent:** DP ∥ chunks+receding; Mobile ALOHA; ALOHA 2; π₀ cites ACT (FM chunks, no TE); π₀.₅; RTC; BEHAVIOR’25/’26.  
**Genealogy:** ALOHA+ACT (CVAE+TE) ∥ DP (diffusion+receding) → Mobile/ALOHA2 → π₀/π₀.₅ → RTC → BEHAVIOR’25 forks → BEHAVIOR’26.

### Visionary (11+3)
Chunking non-negotiable; CVAE+TE optional. Prefer RTC/receding over naïve full open-loop. Start OpenPI π₀.₅ / GR00T N1.7; port horizon/overlap/correlated noise — not CVAE. **Durable idea:** short generative action trajectories as motor primitive; generative family churns; **chunk + overlap execution** stays. Skip Discord/Drive.

---

## 5. Slide Maker brief

| Role | Timing | Focus |
|------|--------|-------|
| Stakeholder | 18+3 | Cheap teleop + chunking prior |
| Reviewer | 8+3 | Cite hygiene + thin real n |
| **Empiricist** | **11+3** | **k-sweep + TE + CVAE (HIGH)** |
| **Archaeologist** | **11+3** | **Chunk genealogy to BEHAVIOR (HIGH)** |
| Visionary | 11+3 | Chunk+overlap endures |

Spine: action chunking as motor contract; TE vs receding vs open-loop table. PDF for Mac.  
**GitHub title:** `[ECE 605] - ACT Making Club Pack`

---

## Paths
| Artifact | Path |
|----------|------|
| This pack | `/workspace/act-aloha-quiet/KANTA_PACK.md` |
| Senior Research | `/workspace/act-aloha-quiet/DELIVERABLE.md` |
| Summarizer | `/workspace/papers/act-aloha-2304.13705-summary.md` |
| Paper PDF | `/workspace/papers/act-aloha-2304.13705.pdf` |
