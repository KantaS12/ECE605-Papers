# Paper Manager pack — Flow Matching (Sep 23)

**Paper:** Flow Matching for Generative Modeling (Lipman et al.)  
**arXiv:** https://arxiv.org/abs/2210.02747  
**PDF:** /workspace/papers/flow-matching-2210.02747.pdf  
**Session:** Sep 23 · due 9:30 AM · **You = Visionary (seat 1)**  
**Type:** VLA Architectures  
**Sources:** Summarizer + Senior Research (`/workspace/flow-matching-quiet/DELIVERABLE.md`)

---

## 1. Paper summary

Simulation-free continuous normalizing flow (CNF) training: regress a neural vector field onto a path-generating vector field. **Conditional Flow Matching (CFM)** makes the objective tractable (same gradients as marginal FM). Gaussian probability paths include VE/VP diffusion as special cases; the highlight is the **Optimal Transport (OT)** path — straight-line, roughly constant-direction flows → faster train/sample and better FID/NLL on the paper’s U-Net setups.

**Club relevance:** This is the math behind **π₀ / π₀.₅ / OpenPI** action experts. Diffusion Policy proved generative **action chunks** with DDPM (doesn’t cite Lipman). BEHAVIOR’25 specializes FM with correlated noise + multi-sample CFM + soft inpaint.

**One-liner:** FM = pick a path, regress the VF; OT makes paths straighter/faster → DP showed generative chunks → π₀ puts OT-CFM in the VLM action head → BEHAVIOR fixes noise/inference for robot stats.

---

## 2. Keywords

Flow Matching, CFM, OT displacement interpolant, CNF, probability-flow ODE, NFE, VE/VP as special cases, Rectified Flow (concurrent), π₀/π₀.₅, OpenPI, BEHAVIOR

---

## 3. FM → robotics / control map

| Link | Status |
|------|--------|
| Lipman FM foundation | CLEAR |
| DP = action-space DDPM chunks | CLEAR |
| DP cites Lipman | likely FALSE |
| π₀ cites Lipman; OT-style CFM action expert | CLEAR |
| π₀.₅/OpenPI keep FM head | CLEAR |
| BEHAVIOR’25: correlated noise, N=15, soft inpaint on Pi0.5 | CLEAR |

Same **receding-horizon chunk** tradeoff as DP applies at policy level; FM usually fewer NFEs / lower latency for comparable multimodality.

---

## 4. Role packs

### Stakeholder (18+3) — seat 4
Scalable recipe: regress VF on straight paths; better NLL/FID/NFE same U-Net. ROI is downstream (π₀/OpenPI/BEHAVIOR), not ImageNet tables. Required literacy for flow-head work; pair with DP + π₀.

### Scientific Reviewer (8+3) — seat 3
**Strengths:** Unbiased CFM grads; unifies diffusion PF-ODEs; OT principled; consistent multi-metric gains.  
**Actionable:** verify vs EDM-class recipes; caveat CIFAR FID; gate IN-128 SOTA claim; robotics correlation not in this paper; FID≠accuracy (SR PSNR); cite Rectified Flow concurrency.  
**Checklist:** toy OT-CFM; confirm π₀ path=OT not VP; separate “FM>SM same U-Net” from “FM solves robot multimodality.”

### Empiricist (11+3) — seat 2
**H1:** OT-CFM ≥ quality at lower NFE than VP-CFM/SM  
**H2:** OT reaches target metric in fewer wall-clock hours  
**H3:** Cond CFM > pure regression on toy cond task  
**H4:** Action-chunk OT-CFM needs fewer steps than DP-style head for similar success  

| Priority | Experiment |
|----------|------------|
| P0 | OT-CFM vs VP-CFM vs score-matching on 2D + CIFAR-lite; NFE sweep |
| P1 | Conditional FM toy; CNF likelihood (Hutchinson) |
| P2 | σ_min; action-space OT-CFM vs DP toy |
| Defer | ImageNet-64/128 |

Budget: 1×GPU hours–1 day for P0 toys. Env: `/workspace/flow-matching-quiet/env_outline/`.

### Archaeologist (11+3) — seat 5
CNF/Neural ODE → sim-free attempts → diffusion/score → **★ FM/CFM (this)** (+ Rectified Flow / interpolants) → DP action DDPM → **π₀ (CLEAR Lipman cite)** → OpenPI/π₀.₅ → BEHAVIOR correlated FM + N=15 + soft inpaint.

### Visionary (11+3) — **YOU (seat 1)** — HIGH PRIORITY

**Internalize from THIS paper**
- OpenPI loss = **CFM**, not score matching  
- OT target ≈ `a − ε` along interpolant `a_t = (1−t)ε + t a`  
- Straight paths ⇒ fewer NFEs ≈ permission for ~10 Euler steps (Table1 / Fig7)  
- Diffusion paths ⊂ FM — “back to DDPM” needs a reason  

**FM → BEHAVIOR map**
- isotropic `x₀` → correlated `ε ~ N(0, 0.5 I + 0.5 Σ)`  
- single CFM MC → `N=15`  
- ODE 0→1 → soft inpaint early `t` (keep4 / exec26)  
- low NFE → predict30/exec26 + 1.3× compress  

**Follow-ups to champion (2026 BEHAVIOR)**
1. Theory: CFM with full SPD `Σ_t` (closed-form `u`) — publishable  
2. OT vs VP vs DDPM ablation inside OpenPI (freeze VLM)  
3. Adaptive NFE / early-exit flow  
4. Measure π₀ trajectory curvature; try reflow if curved  
5. Action-chunk NLL via change-of-variables as health metric  
6. Per-embodiment `Σ` paths in one FM expert  

**New apps:** contact-rich soft schedules; multi-sample test-time selection; FAST+FM gated by likelihood; sim-to-real as second path.

**Don’t overclaim:** DP≠FM; correlated noise/N=15 ≠ Lipman; IN FID ≠ robot KPI.

**One-slide Visionary:** Lipman’22 = pick path, regress VF → DP showed generative action chunks → π₀/OpenPI put OT-CFM in VLM action expert → BEHAVIOR’25 kept it and fixed correlated noise, multi-sample CFM, soft inpaint → next wins = **path design for action statistics**, not another ImageNet U-Net.

Skip Discord/Drive.

---

## 5. Slide Maker brief

| Role | Timing | Focus |
|------|--------|-------|
| Stakeholder | 18+3 | Why FM matters for robot stacks |
| Reviewer | 8+3 | Strengths + cite hygiene |
| Empiricist | 11+3 | OT vs VP vs SM toys + NFE |
| Archaeologist | 11+3 | CNF→FM→DP→π₀→BEHAVIOR |
| **Visionary** | **11+3** | **FM→BEHAVIOR map + 6 follow-ups (HIGH)** |

Spine: OT-CFM objective + path design; PDF for Mac.

---

## Paths
| Artifact | Path |
|----------|------|
| This pack | `/workspace/flow-matching-quiet/KANTA_PACK.md` |
| Senior Research | `/workspace/flow-matching-quiet/DELIVERABLE.md` |
| Summarizer | `/workspace/papers/flow-matching-2210.02747-summary.md` |
