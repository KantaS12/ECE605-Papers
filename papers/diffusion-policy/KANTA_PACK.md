# Paper Manager pack — Diffusion Policy (Sep 21)

**Paper:** Diffusion Policy: Visuomotor Policy Learning via Action Diffusion  
**arXiv:** https://arxiv.org/abs/2303.04137 · RSS 2023 / IJRR extended  
**Site/code:** https://diffusion-policy.cs.columbia.edu  
**PDF:** /workspace/papers/diffusion-policy-2303.04137.pdf  
**Session:** Sep 21 · due 9:30 AM · **You = Archaeologist (seat 1)**  
**Type:** VLA Architectures  
**Sources:** Summarizer + Senior Research (`/workspace/diffusion-policy-quiet/DELIVERABLE.md`)

---

## 1. Paper summary

Conditional **DDPM in action space**: given observation `O_t`, denoise noise → action chunk `A_t` of prediction horizon `T_p`; execute `T_a` then **replan** (receding-horizon). Observations condition once (FiLM / cross-attn). Beats BC-RNN / IBC / BET by **+46.9%** avg relative across **15** tasks.

**Wins:** multimodal `p(A|O)`, high-dim action sequences, stable MSE training, synergy with **position** control. Journal adds limiting-case control analysis, vision ablations, 3 bimanual real tasks.

**Club line to BEHAVIOR:** DP chunk+replan → ACT → π₀/π₀.₅ **flow matching** chunks → RTC soft-inpaint → BEHAVIOR winners (exec26/inpaint4 + correlated noise). Denoiser changed; **chunk–commit–replan–overlap** habit stayed.

---

## 2. Keywords

Diffusion Policy, DDPM/DDIM, action chunking, receding-horizon, multimodal BC, FiLM, CNN vs Transformer, position vs velocity, flow matching (downstream), RTC, OpenPI/π₀

**Default horizons (real Push-T style):** `T_o≈2`, `T_p≈16`, `T_a≈8` (sometimes 6); DDIM 100→10–16 steps for real-time.

---

## 3. Motion / control spine (priority)

| Idea | Detail |
|------|--------|
| Action chunking | Joint `T_p×d_a` sequence → temporal consistency + idle robustness |
| Receding-horizon | Exec `T_a < T_p`, then replan; warm-start optional |
| Sweet spot | `T_a` too long = sluggish; `T_a=1` = mode flicker; paper ~8 |
| Multimodal | Langevin basins (Push-T L/R, Kitchen order) |
| NOT | Classical motion planner / Diffuser full-trajectory planning |

**→ Flow / BEHAVIOR (CLEAR):** π₀ cites Chi’23, swaps DDPM→flow on PaliGemma; RTC soft-inpaints async chunks; BEHAVIOR 1st: predict 30 / exec 26 / inpaint 4.

---

## 4. Role packs

### Stakeholder (18+3) — seat 2
Canonical continuous-action BC before VLAs. Value = **action head + control loop**, not semantics. Real-robot credible; DDIM keeps ~10 Hz. 2026 OpenPI/BEHAVIOR stacks inherit these control assumptions.

### Scientific Reviewer (8+3) — seat 5
**Strengths:** Clear problem→method; 15-task sim+real+bimanual; ablations that matter (`T_a`, latency, pos/vel).  
**Actionable:** shared action interface for all baselines; absolute + hard-task aggregates not only +46.9%; note robomimic init bug lore; mark as control-module not foundation VLM; seed-level last-10 tables.  
**Verdict:** Still the right cite for continuous action diffusion/flow + chunks.

### Empiricist (11+3) — seat 4
**H-horizon:** receding `T_a≈8` balances consistency vs reactivity  
**H-DDIM:** quality saturates ~10–16 steps  
**H-pos:** position > velocity for DP  
**H-loop:** open-loop long chunks fail under obs shift (same failure mode as long VLA chunks)

| Priority | Experiment |
|----------|------------|
| P0 | `T_a∈{1,2,4,8,16}` on Push-T/Lift state; closed-loop vs open-loop |
| P1 | DDIM steps {4..100} infer-only; position vs velocity |
| P2 | CNN vs Transformer; warm-start |
| Defer | Vision FT, Transport, full 15-task |

**Budget:** scout ≤~50 GPU-h; state Push-T first ~4–12 GPU-h. Env: `/workspace/diffusion-policy-quiet/env_outline/`.

### Archaeologist (11+3) — **YOU (seat 1)** — HIGH PRIORITY

**Influential priors**
| Prior | Role |
|-------|------|
| DDPM / score matching / Langevin | Generative denoising backbone |
| DDIM / iDDPM | Fast sampling for real-time control |
| Diffuser | Trajectory diffusion foil — DP is **actions-only + O-conditioned** |
| IBC | Implicit BC foil (unstable / slow) |
| BC-RNN / Robomimic / BET | Multimodal BC baselines |
| FiLM / minGPT | Conditioning / Transformer pieces |
| Mayne RHC | Classical receding-horizon control ancestry |
| Relay Kitchen | Long-horizon multitask benchmark |

**Contemporaries:** ACT (parallel CVAE chunking); other diffusion policies (Pearce/Reuss/IDQL).

**CLEAR subsequent**
| Work | Link |
|------|------|
| π₀ (2410.24164) | Cites Chi’23; DDPM→**flow** on PaliGemma |
| OpenPI / π₀.₅ | Same chunked generative action habit |
| RTC (2506.07339) | Soft-inpaint async chunks |
| BEHAVIOR 1st (2512.06951) | 30/26/4 + correlated noise; q~0.26 |
| Comet (2512.10071) | Parallel RH / horizon ablations in challenge solutions |

**Talk arc (one slide):** multimodal BC hacks → **DP locks generative chunks+replan** → VLAs swap flow for DDPM → RTC/BEHAVIOR soft-inpaint+correlated noise. Do **not** claim BEHAVIOR trains DP itself.

### Visionary (11+3) — seat 3
2026 BEHAVIOR: control loop > bigger VLM alone for long-horizon fails. Bets: unify RTC+correlated inpaint; adaptive/event-triggered `T_a`; latency certificates; language-conditioned basin selection + DP-style commitment.  
**One-liner:** DP is to continuous VLA motor control what early Transformers were to NLP sequences — the habit everyone still lives in after the backbone moved on. Skip Discord/Drive.

---

## 5. Slide Maker brief

| Role | Timing | Focus |
|------|--------|-------|
| Stakeholder | 18+3 | Why action diffusion + real-robot credibility |
| Reviewer | 8+3 | Strengths + actionable cite hygiene |
| Empiricist | 11+3 | `T_a` grid + DDIM + closed vs open loop |
| **Archaeologist** | **11+3** | **Priors + DP→flow→RTC→BEHAVIOR timeline (HIGH)** |
| Visionary | 11+3 | 2026 control-loop bets |

**Spine:** chunk–commit–replan (`T_p`/`T_a`) as motion/control contract. Image-first; PDF for Mac.

---

## Paths
| Artifact | Path |
|----------|------|
| This pack | `/workspace/diffusion-policy-quiet/KANTA_PACK.md` |
| Senior Research | `/workspace/diffusion-policy-quiet/DELIVERABLE.md` |
| Summarizer | `/workspace/papers/diffusion-policy-2303.04137-summary.md` |
