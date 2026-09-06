# Paper Manager pack — GR00T N1 (Oct 7)

**Paper:** GR00T N1: An Open Foundation Model for Generalist Humanoid Robots  
**arXiv:** https://arxiv.org/abs/2503.14734  
**Short:** GR00T N1 / GR00T-N1-2B (~2.2B)  
**PDF:** /workspace/papers/gr00t-n1-2503.14734.pdf  
**Session:** Oct 7 · due 9:30 AM · **all seats empty** — full five packs for club/GitHub  
**Sources:** Summarizer + Senior Research (`/workspace/groot-n1-quiet/DELIVERABLE.md`)

---

## 1. Paper summary

Dual-system humanoid VLA: **Eagle-2** vision-language **System 2** (~10 Hz) + flow-matching **DiT System 1** (**120 Hz** actions) via **cross-attn**, jointly trained. Chunk **H=16**, Euler **K=4**; **~63.9 ms**/chunk on L40. Heterogeneous data pyramid (human/latent + DexMimicGen + neural I2V + real GR-1/OXE/AgiBot). Beats BC-Transformer & Diffusion Policy in sim/real; strong low-data efficiency. Deployed on Fourier **GR-1**.

**Lineage (CLEAR):** N1 → Isaac → N1.6-BEHAVIOR1k → **N1.7 GA** = official **BEHAVIOR’26 co-baseline with π₀.₅**. Do **not** conflate N1 paper numbers with N1.7 challenge baseline (APIs/horizons differ).

---

## 2. Keywords

GR00T N1, humanoid VLA, dual-system, Eagle-2, FM DiT, data pyramid, N1.7, BEHAVIOR 2026, π₀/π₀.₅ contrast, LAPA/IDM, DexMimicGen

---

## 3. Architecture vs π₀/π₀.₅

| Axis | GR00T N1 | π₀/π₀.₅ |
|------|----------|---------|
| Coupling | Separate DiT **cross-attn** | Action expert **shared self-attn** + block causal |
| VLM | Eagle-2 (SmolLM2+SigLIP-2) | PaliGemma |
| Chunk | H=16, K=4; 10/120 Hz | H=50 (π₀), ~10 Euler; 20–50 Hz |
| I/O | Per-embodiment MLPs; LAPA/IDM | Pad/mask shared dim |
| Successor | **N1.7** ~3B, H≤40, Apache-2.0 | π₀.₅ + RTC + BEHAVIOR’25 forks |

---

## 4. Role packs

### Stakeholder (18+3)
Open humanoid VLA path → Apache N1.7 + BEHAVIOR’26 parity with π₀.₅. Prefer finetune `nvidia/GR00T-N1.7-3B`. Risks: N1≠N1.7 API; short-horizon paper vs house-scale; forgetting; Isaac GPU needs.

### Scientific Reviewer (8+3)
**Strengths:** falsifiable dual-system thesis; data methodology; broad eval; honest forgetting fail; open formats.  
**Weaknesses:** Table2/4 DexMG mismatch (preserve both numbers); IL not VLA baselines in main; small-n partial credit; confounded ablations; short-horizon rhetoric vs “generalist humanoid.”  
**Verdict:** Strong systems paper; architectural foil to π MoE coupling.

### Empiricist (11+3)
**H1:** K=4 near-optimal; flatten by ~4–8  
**Plus:** adapter low-data lift vs scratch DP/BC (directional)

| Priority | Experiment |
|----------|------------|
| P0 Y | Released N1-2B dry infer; eval-only NFE/chunk grid (K∈{1,2,4,8}, H) |
| P0 Partial | Mid-layer vs final VL features |
| P1 Partial | Adapter-only FT (30–100 demos); data-regime curve |
| Defer N | Full pretrain (~50k H100-h), full RoboCasa×GR-1, real GR-1, BEHAVIOR’26 N1.7 from scratch |

Tattoo: sim avg ~45% vs DP 33.4% / BC-T 26.4%; real 76.8% vs DP 46.4%. Env: `/workspace/groot-n1-quiet/env_outline/`.

### Archaeologist (11+3)
**Priors:** SigLIP-2, SmolLM2, Eagle-2, Lipman FM, π₀ (cite+contrast), Diffusion Policy, ACT, DiT/Flamingo/VIMA, LAPA, IDM, DexMimicGen/OXE.  
**Pivot:** humanoid FM-VLA + dual-system cross-attn + data pyramid.  
**Genealogy:** SigLIP-2+SmolLM2 → Eagle-2 → **N1** → N1.6 → **N1.7** → BEHAVIOR’26 ‖ π₀.₅.

### Visionary (11+3) — HIGH for BEHAVIOR’26
1. **N1.7 is official substrate** — dual-track vs π₀.₅ from day one  
2. Explicit System-2 interface → hierarchy/safety hooks vs fused π expert  
3. Port neural pyramid ideas + RTC/correlated-noise hybrids from π forks  
4. Version discipline: paper H=16 vs N1.7 H≤40 — don’t mix APIs  
5. Try both starters on hard BEHAVIOR tasks; report NFE/horizon; with/without System-2 mid-layer  
6. **Durable idea:** modular VL↔motor coupling + open humanoid data pyramid — not just a bigger PaliGemma clone  

Skip Discord/Drive.

---

## 5. Control note

Dual System 2 (VLM mid features) + System 1 (DiT action CFM) — **not** a classical planner. Contrast OpenPI action-expert FM, DP CNN/Transformer, π_RL online PPO, Comet RFT for BEHAVIOR.

---

## 6. Slide Maker brief

| Role | Timing | Focus |
|------|--------|-------|
| Stakeholder | 18+3 | N1→N1.7 product path |
| Reviewer | 8+3 | Dual-system thesis + Table mismatch |
| Empiricist | 11+3 | K/H smoke grid + tattoo numbers |
| Archaeologist | 11+3 | Genealogy to N1.7 / BEHAVIOR’26 |
| **Visionary** | **11+3** | **Dual baseline with π₀.₅ (HIGH)** |

Spine: dual-system cross-attn FM; N1≠N1.7. PDF for Mac.  
**GitHub title:** `[ECE 605] - GR00T N1 Making Club Pack`

---

## Paths
| Artifact | Path |
|----------|------|
| This pack | `/workspace/groot-n1-quiet/KANTA_PACK.md` |
| Senior Research | `/workspace/groot-n1-quiet/DELIVERABLE.md` |
| Summarizer | `/workspace/papers/gr00t-n1-2503.14734-summary.md` |
| Paper PDF | `/workspace/papers/gr00t-n1-2503.14734.pdf` |
