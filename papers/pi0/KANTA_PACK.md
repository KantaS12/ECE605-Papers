# Paper Manager pack — π₀ (Sep 28)

**Paper:** π₀: A Vision-Language-Action Flow Model for General Robot Control  
**arXiv:** https://arxiv.org/abs/2410.24164 · RSS 2025  
**Blog:** https://physicalintelligence.company/blog/pi0  
**PDF:** /workspace/papers/pi0-2410.24164.pdf  
**Session:** Sep 28 · due 9:30 AM · **You = Archaeologist (seat 1)**  
**Type:** VLA Architectures  
**Sources:** Summarizer + Senior Research (`/workspace/pi0-quiet/DELIVERABLE.md`)

---

## 1. Paper summary

Robot foundation VLA: **PaliGemma (~3B)** + **~300M action expert** trained with **OT-CFM** to emit continuous action chunks **H=50** at up to **50 Hz**. Pretrain→post-train on ~**10k hours** across **7 robots / 68 tasks** (+ OXE/Bridge/DROID). Beats OpenVLA/Octo out-of-box; FT beats ACT/DP; pretrain+FT unlocks multi-minute household skills.

**Club bridge:** SigLIP → PaliGemma → **π₀** → OpenPI → π₀.₅ → RTC → BEHAVIOR’25 → ’26 baselines.

---

## 2. Keywords / stack snapshot

| Item | Value |
|------|--------|
| Backbone | PaliGemma 3B + ~300M FM action expert (total ~3.3B) |
| Objective | OT-CFM; τ~shifted Beta(1.5,1); 10 Euler steps |
| Chunk | H=50 train; exec 16@20Hz / 25@50Hz (no temporal ensembling) |
| Latency | ~73 ms on-board RTX4090 (Table I) |
| Data | ~10k h proprietary teleop; mixture weight n^0.43 |

---

## 3. Motion / control spine

DP/ACT **chunk pattern** with **DDPM → OT-FM** (n=10). OpenVLA fails partly from no chunking. Open-loop seam → later **RTC** soft-inpaint. BEHAVIOR keeps this substrate with correlated ε, N=15, exec26/keep4.

**Orthogonal knobs:** H (train horizon) · Ta (open-loop commit) · NFE (Euler steps) · async overlap · Σ (noise correlation).

**NOT a classical planner** — generative continuous chunk controller.

---

## 4. Role packs

### Stakeholder (18+3) — seat 2
Reference 2025–26 VLA template. Start from OpenPI `pi0` / `pi05_base` + FT. Budget multi-cam + ~73–86ms class GPU. Risks: unreproducible 10k h; open-loop delay; not locomotion-universal.

### Scientific Reviewer (8+3) — seat 4
**Strengths:** integrative architecture+data+systems; right inductive bias vs AR VLAs; ~10k h scale; ablations (small/parity/scratch); honest difficulty.  
**Weaknesses:** figure-only floats; π₀-small confounds; baseline compute unmatched; n=10 little variance reported; external validity limited (PI robots).  
**Verdict:** Strong systems+empirical RSS; shifted field toward flow/diffusion chunks.

### Empiricist (11+3) — seat 5
**H1:** mid commit (~0.5–1s / H≈16–32) beats too-short and full open-loop H=50  
**H2:** success saturates ~5–10 NFE  
**H3:** FT from π ≫ zero-shot/scratch on multi-stage tasks  
**H5:** FM lower NFE/latency than DP at matched backbone  

| Priority | Experiment |
|----------|------------|
| P0 | H∈{8,16,25,50}; NFE∈{1,2,4,5,10,20}; FT vs zero-shot/scratch |
| P1 | Cross-embodiment; FM vs DP head |
| P2 | τ-schedule; open-loop commit; VLM init vs scratch expert |
| Defer | Full ~10k-h pretrain; full laundry/box suite |

Budget: P0 on public OpenPI ckpts = 1–4×GPU days (not weeks). Full pretrain irreproducible. Env: `/workspace/pi0-quiet/env_outline/`.

### Archaeologist (11+3) — **YOU (seat 1)** — HIGH PRIORITY

**Influential priors (CLEAR)**
| Prior | Role |
|-------|------|
| SigLIP | Vision tower lineage |
| PaliGemma (2407.07726) | VLM init |
| Lipman FM (2210.02747) | OT-CFM objective (π₀ cites) |
| Rectified Flow | Concurrent straight-path flows |
| Diffusion Policy (2303.04137) | Generative action chunks (DDPM) |
| ACT | Parallel chunking / idle robustness |
| Transfusion | MoE-style dual-stream inspiration |
| RT-2 / OpenVLA | AR VLA foils |
| OXE / Bridge / DROID | Open data mixture ingredients |
| SayCan | Language→robot planning foil |

**Pivot this paper locks:** FM VLA + VLM init + action expert + 50 Hz chunks at ~10k h scale.

**CLEAR subsequent**
| Work | Link |
|------|------|
| OpenPI (2025-02) | Open release / ecosystem |
| FAST / π₀-FAST (2501.09747) | Discrete tokenization branch |
| **π₀.₅** (2504.16054) | FM **retained** at post-train (blogs saying FAST replaced FM = **false**) |
| **RTC** (2506.07339) | Soft-inpaint async chunks |
| BEHAVIOR’25 1st (2512.06951) | On Pi0.5; q~0.26 |
| BEHAVIOR’26 | Baselines include π₀.₅ |

**Genealogy slide:**  
SigLIP → PaliGemma → **π₀** → OpenPI → (FAST ∥ π₀.₅) → RTC → BEHAVIOR’25 forks → BEHAVIOR’26 baselines.

### Visionary (11+3) — seat 3
2026 BEHAVIOR: FM+chunk = default control prior; 2025 winners specialized noise/overlap not the head. Hierarchy/stage memory next bottleneck. Deploy with RTC. Start `pi05_base`; port correlated Σ, multi-sample FM, soft inpaint, stages. Durable idea: **VLM semantic prior + generative continuous chunks**. Skip Discord/Drive.

---

## 5. Slide Maker brief

| Role | Timing | Focus |
|------|--------|-------|
| Stakeholder | 18+3 | Template VLA + OpenPI start |
| Reviewer | 8+3 | Strengths / private-data limits |
| Empiricist | 11+3 | H / NFE / FT grid |
| **Archaeologist** | **11+3** | **Priors + genealogy timeline (HIGH)** |
| Visionary | 11+3 | 2026 BEHAVIOR defaults |

Spine: PaliGemma + OT-CFM chunks (DDPM→flow). PDF for Mac. Include all five roles for GitHub.

---

## Paths
| Artifact | Path |
|----------|------|
| This pack | `/workspace/pi0-quiet/KANTA_PACK.md` |
| Senior Research | `/workspace/pi0-quiet/DELIVERABLE.md` |
| Summarizer | `/workspace/papers/pi0-2410.24164-summary.md` |
| Paper PDF | `/workspace/papers/pi0-2410.24164.pdf` |
