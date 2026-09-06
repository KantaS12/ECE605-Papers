# Paper Manager pack — π_RL (Oct 5)

**Paper:** π_RL: Online RL Fine-tuning for Flow-based Vision-Language-Action Models  
**arXiv:** https://arxiv.org/abs/2510.25889  
**Code:** https://github.com/RLinf/RLinf · HF: https://huggingface.co/RLinf  
**PDF:** /workspace/papers/pi-rl-2510.25889.pdf  
**Session:** Oct 5 · due 9:30 AM  
**Schedule:** Reviewer=3, Empiricist=4; Stakeholder/Archaeologist/Visionary empty; **Kanta not seated** — all five packs still included  
**Sources:** Summarizer + Senior Research (`/workspace/pi-rl-quiet/DELIVERABLE.md`)

---

## 1. Paper summary

Online RL fine-tune for **flow-matching VLAs** (π₀/π₀.₅; GR00T in appendix) so PPO can run despite intractable flow log-likelihoods and deterministic ODE sampling.

**Two methods**
- **Flow-Noise:** learnable noise → Gaussian denoise MDP; discard noise after FT  
- **Flow-SDE:** ODE→SDE + two-layer MDP + hybrid sampler (~2× faster; often preferred)

**Recipe:** few-shot SFT → freeze VLM → PPO/GAE on ~300M action expert. Table1 avg **+27–31 pp** over SFT; few-shot π₀.₅+RL LIBERO **98.3%** > full SFT **96.9%**. OOD helps visual/env more than novel tasks (ML45 fails). Real2Sim2Real: SFT fail → RL **40%**.

**BEHAVIOR bridge:** OpenPI Comet **cites** this then **rejects** online RL for BEHAVIOR (sample efficiency + GPU heterogeneity) → uses **RFT**. π_RL = when fast parallel sim exists; RFT = pragmatic default for slow BEHAVIOR sims.

---

## 2. Keywords

π_RL, Flow-Noise, Flow-SDE, PPO/GAE, RLinf, flow VLA RL, π₀/π₀.₅, RFT contrast, Real2Sim2Real, BEHAVIOR post-train

---

## 3. Table 1 headline (ID avg %)

| Model | Method | Avg | Δ vs SFT |
|-------|--------|-----|----------|
| π₀ | SFT | 51.1 | — |
| π₀ | Flow-SDE | **78.7** | **+27.6** |
| π₀ | Flow-Noise | **80.3** | **+29.2** |
| π₀.₅ | SFT | 55.6 | — |
| π₀.₅ | Flow-SDE | **86.6** | **+31.0** |
| π₀.₅ | Flow-Noise | **84.7** | **+29.1** |

Also: LIBERO Long 43.9→94.0; ManiSkill OOD ~26→49–53; CALVIN Len-5 →87; PPO>GRPO ~6pp; GR00T App 52.5→89.9.

---

## 4. Role packs

### Stakeholder (18+3)
Open adapter for OpenPI via RLinf; few-shot+RL can beat full SFT on reported %. Needs H100-class parallel sim+rewards; not true real online RL. Prefer π_RL with fast sim; **RFT** when BEHAVIOR-slow.

### Scientific Reviewer (8+3) — MEATY (seat weight 3)
**Strengths:** Real gap (flow VLAs blocked PG); two mechanisms with clear ancestry; multi-benchmark + careful OOD split; open code; honest novel-task failure.  
**Actionable**
1. Report env-steps / GPU-hours to plateau vs “beats full SFT”  
2. Ablate unfreeze last VLM layers (frozen VLM may cap visual OOD)  
3. When to pick Noise vs SDE (wins differ by suite)  
4. “Exact likelihood” is of augmented denoise MDP, not marginal ODE policy — say so  
5. Thin real n (40%) — more seeds/tasks  
6. Head-to-head vs FPO / RFT under matched budgets  
7. Don’t overclaim venue (ICML-style keywords; acceptance TENTATIVE)  
**Verdict:** Strong same-task mastery + partial env-shift; don’t overread as open-world discovery.

### Empiricist (11+3) — MEATY (seat weight 4)
**Ruthless smoke:** defer full online RL.  

| Priority | Experiment |
|----------|------------|
| P0 Y | Released ckpt eval; eval-only K×a grids; unit-test Flow-SDE transitions |
| P1 Partial | Offline/advantage-weighted CFM; critic warm-up; tiny PPO ≤8 envs ≤20–40 updates |
| Defer N | Tab.1 multi-bench, ManiSkill 4k, Real2Sim2Real, BEHAVIOR online RL |

**H1:** mid noise `a≈0.5` maximizes usable train-mode SR  
**H2:** `K∈{4,5}` operating point; diminishing returns by ~8  
**H4:** freeze VLM + expert update = ID refinement, not novel-task invention  
**H5:** mid horizon + replan > cargo-cult H=50 during RL  

**Reproduce:** OpenPI SFT → RLinf freeze VLM → `flow_sde` (default) or `flow_noise` → monitor train/eval gap, clip fraction, EV, KL.  
**One-liner:** parallel sparse-success sim → near-saturated specialists; else stay on RFT.  
Env: `/workspace/pi-rl-quiet/env_outline/`.

### Archaeologist (11+3)
**Priors:** π₀/π₀.₅, Lipman FM, ReinFlow, Flow-GRPO, DPPO, PPO/GAE, RLinf, Song SDE, 3DGS.  
**Subsequent:** Comet cites then uses RFT; BEHAVIOR’25 Pi0.5 forks different post-train.  
**Arc:** flow VLAs blocked PG → π_RL opens it → slow-sim challenges route around via RFT.

### Visionary (11+3) — 2026 BEHAVIOR
| | Online RL (π_RL) | RFT (Comet) |
|--|------------------|-------------|
| Needs | logπ + exploration | success filter only |
| Sample efficiency | low | higher for slow sims |
| Ceiling | can exceed demos | capped by reachable modes |
| BEHAVIOR’25 | cited, not used | Q 0.224→0.345 |

**2026 bets:** RFT then short π_RL burst if faster eval arrives; don’t freeze VLM forever; tune H' for advantages; dual π₀.₅+GR00T adapters.  
**One-liner:** π_RL = open online half; RFT = pragmatic offline half — compose them. Skip Discord/Drive.

---

## 5. Control note

Flow policy as stochastic MDP via Flow-SDE; hybrid SDE+ODE sampler; PPO on action expert — **not** classical planner. Chunk length = credit-assignment knob.

---

## 6. Slide Maker brief

| Role | Timing | Focus |
|------|--------|-------|
| Stakeholder | 18+3 | π_RL vs RFT product choice |
| **Reviewer** | **8+3** | **7 actionable critiques (HIGH)** |
| **Empiricist** | **11+3** | **Smoke-feasible grid + tattoo numbers (HIGH)** |
| Archaeologist | 11+3 | FM VLAs → π_RL → RFT fork |
| Visionary | 11+3 | Compose RL + RFT for 2026 |

Spine: unlock PPO on flow VLAs; BEHAVIOR still prefers RFT. PDF for Mac. All five roles.

**GitHub title (Kanta not seated):** `[ECE 605] - π_RL Making Club Pack`

---

## Paths
| Artifact | Path |
|----------|------|
| This pack | `/workspace/pi-rl-quiet/KANTA_PACK.md` |
| Senior Research | `/workspace/pi-rl-quiet/DELIVERABLE.md` |
| Summarizer | `/workspace/papers/pi-rl-2510.25889-summary.md` |
| Paper PDF | `/workspace/papers/pi-rl-2510.25889.pdf` |
