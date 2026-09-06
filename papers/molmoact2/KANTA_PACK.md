# Paper Manager pack — MolmoAct2 (Oct 28)

**Paper:** MolmoAct2: Action Reasoning Models for Real-World Deployment  
**arXiv:** https://arxiv.org/abs/2605.02881  
**Code:** allenai/molmoact2 (Ai2)  
**PDF:** /workspace/papers/molmoact2-2605.02881.pdf  
**Session:** Oct 28 · due 9:30 AM · **all seats empty** — full five packs  
**Sources:** Summarizer + Senior Research (`/workspace/molmoact2-quiet/DELIVERABLE.md`)

---

## 1. One-liner

Open **Action Reasoning Model (ARM)**: geometric **System-2** (adaptive depth tokens in Think) before a **per-layer-KV DiT FM** action expert — deployable open alternative to closed π-series.

---

## 2. CRITICAL System-2 distinctions (do NOT conflate)

| Outer loop | What it is | What it is NOT |
|------------|------------|----------------|
| **MolmoAct2 Think** | Adaptive **depth / geometry** codes (10×10) | π₀.₅ HL **semantic** subtask strings |
| **π₀.₅ HL** | Semantic subtask language | Depth lattice |
| **BEHAVIOR’25 System-2** | Task-progress **stage IDs + voting** | Scene geometry |

All three call an FM chunk expert (System 1). **TENTATIVE:** BEHAVIOR may want **depth + stage** complementary.

**FALSE claims:** “has π₀.₅ HL” / “has RTC” / “has BEHAVIOR S2 voting.”

---

## 3. Stack snapshot

- Backbone: Molmo2-ER (4B LLM + SigLIP2)  
- Curriculum: FAST discrete AR pretrain → FM expert post-train (KI) → FT  
- FM expert ~621M, L=36, per-layer KV; infer N=10 Euler  
- Think: 100 depth codes; regen if RGB cosine <0.996  
- Rates: ~55.8 Hz Act2 / ~12.7 Hz Think @ H100 H=10 + CUDA Graphs  
- Claims: LIBERO **97.2 / Think 98.1** (π₀.₅ 96.9); MolmoSpaces **37.7** vs **34.5**; DROID OOD **87.1%**

---

## 4. Motion / control hierarchy

| Layer | What | Interface |
|-------|------|-----------|
| L0 | SigLIP2+LLM multi-cam | tokens + per-layer K/V |
| L1 System-2 (opt Think) | AR **depth codes** | depth → VLM context → expert KV |
| L2 Motor | DiT FM; H≈Hz; N=10 Euler | continuous joints |

Closed-loop: chunked FM (open-loop within chunk, replan) — same family as π₀/π₀.₅/DP. Paper often open-loop; Comet/BEHAVIOR prefer **receding horizon** — don’t cargo-cult open-loop on long-horizon household tasks. **RTC absent** in PDF — latency via FM cache + CUDA Graphs + Think reuse.

---

## 5. Role packs

### Stakeholder (18+3)
Open ARM with weights/code/data; competitive with π₀.₅ on paper protocols; openness vs π opacity. Risks: Spatial OOD; OOTB mainly SO-100/Franka. Use as open FM-VLA+ARM baseline when π gated.

### Scientific Reviewer (8+3)
Credible systems paper; deltas = per-layer KV + adaptive depth. Caveats: Think threshold unablated; π₀.₅/GR00T not always compute-matched; “>GPT-5” is ER-VLM not robot SR. Verdict: open ARM + FM interface advances; not a theorem paper.

### Empiricist (11+3)
| Priority | Experiment |
|----------|------------|
| P0 | Think vs flat continuous; vs π₀.₅ on LIBERO/DROID proxy; log SR + ms/chunk + Hz |
| P1 | Per-layer KV; K flow samples; adaptive depth; horizon×open-loop vs receding |
| Defer | Full pre/post; real YAM 720h |

Tattoo: LIBERO 97.2/98.1; Spaces 37.7; DROID 87.1; KV 95.9>hidden 94.0; 55.79/12.71 Hz. Env: `/workspace/molmoact2-quiet/env_outline/`.

### Archaeologist (11+3)
Molmo/Molmo2 → Molmo2-ER → MolmoAct → **MolmoAct2** (FAST+per-layer KV FM) → Think. Parallel PI: π₀→FAST→π₀.₅→KI→RTC→BEHAVIOR S2. Hybrid FAST→FM curriculum like π₀.₅. New: ER; YAM data; per-layer KV; adaptive depth.

### Visionary (11+3) — 2026 BEHAVIOR
1. Keep FM low-level (discrete AR slower — aligns FAST≠FM-at-deploy)  
2. Choose S2 deliberately: semantic HL/stages vs depth ARM vs **hybrid**  
3. Open substrate when π gated; don’t assume OOTB wins BEHAVIOR without stage tracking + RTC  
4. **RTC wrap** on Molmo FM = high-leverage follow-on (**TENTATIVE**)  

Skip Discord/Drive.

---

## 6. Slide Maker brief

| Role | Timing | Focus |
|------|--------|-------|
| Stakeholder | 18+3 | Open ARM vs π opacity |
| Reviewer | 8+3 | Systems deltas + cite hygiene |
| Empiricist | 11+3 | Think on/off + Hz/latency |
| Archaeologist | 11+3 | Parallel Molmo vs PI lineages |
| Visionary | 11+3 | Three outer loops for 2026 |

Spine: geometric S2 ≠ semantic HL ≠ stage voting; FM motor. PDF for Mac.  
**GitHub title:** `[ECE 605] - MolmoAct2 Making Club Pack`

---

## Paths
| Artifact | Path |
|----------|------|
| This pack | `/workspace/molmoact2-quiet/KANTA_PACK.md` |
| Senior Research | `/workspace/molmoact2-quiet/DELIVERABLE.md` |
| Summarizer | `/workspace/papers/molmoact2-2605.02881-summary.md` |
| Paper PDF | `/workspace/papers/molmoact2-2605.02881.pdf` |
