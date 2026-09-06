# Paper Manager pack — Octo (Oct 14)

**Paper:** Octo: An Open-Source Generalist Robot Policy  
**arXiv:** https://arxiv.org/abs/2405.12213 · RSS 2024  
**Site/code:** https://octo-models.github.io · https://github.com/octo-models/octo  
**HF:** rail-berkeley/octo-base | octo-small  
**PDF:** /workspace/papers/octo-2405.12213.pdf  
**Session:** Oct 14 · due 9:30 AM · **You = Scientific Reviewer (seat 1)** · Stakeholder=3  
**Nearby:** Oct 16 BEHAVIOR deadline  
**Sources:** Summarizer + Senior Research (`/workspace/octo-quiet/DELIVERABLE.md`)

---

## 1. Paper summary

First widely used fully **open** (weights+train+loaders) generalist robot policy: ViT + frozen T5 + **DDPM chunk head** on curated OXE **~800k** / 25 datasets. **Octo-Small 27M** / **Base 93M**. Zero-shot **~+29%** vs RT-1-X; finetune avg **72%** vs scratch **20%** / VC-1 **15%** (~100 demos, ~5h A5000).

**Club line:** ACT∥DP → **Octo** (open OXE GRP) → π₀/OpenVLA → OpenPI/BEHAVIOR. π₀ (**CLEAR**) beats Octo-93M on hard dexterous tasks with VLM+FM — Octo = infrastructure history + eval baseline, **not** 2026 winner weights.

---

## 2. Keywords

Octo, OXE, generalist robot policy, diffusion action chunks, transformer policy, RT-1-X, π₀ baseline, open-source, receding-horizon chunks

**Stack:** ViT Small/Base; frozen T5; CNN patches; readout tokens; 3-layer MLP DDPM head (~20 steps); history H=2; chunked actions + **receding** (TE not helpful here).

---

## 3. Motion / control

Cross-embodiment **receding-horizon diffusion-chunk** controller (no ACT TE by default). Log `commit_seconds = H_exec/Hz`. Bimanual example: chunk 64 / exec 12. Lineage: ACT→DP→Octo→π₀/Comet/N1.

---

## 4. Role packs

### Stakeholder (18+3) — weight 3
Open usable 27M/93M GRP; ~100 demos + ~5h A5000 → 72%. Not 2026 SOTA — keep as eval baseline; ship π₀.₅/GR00T for BEHAVIOR. One-liner: Octo proved open cross-embodiment pretrain+cheap FT; π₀ proved you still need VLM scale + better generative actions.

### Scientific Reviewer (8+3) — **YOU (seat 1)** — HIGH PRIORITY

**Steelman:** open GRP stack + compositional I/O + diffusion chunks + operationalizable FT recipe.

**Actionable critiques (say these aloud)**
1. Absolute “first” priority **TENTATIVE** — safer: first widely released open GRP with demonstrated adapter FT on OXE-scale  
2. RT-2-X parity is task-limited / mixed provenance — not “Octo≈55B VLM”  
3. Thin trials (~10 zero-shot / 20 FT) — demand CIs/seeds  
4. Scratch/VC-1 not fully matched to diffusion+chunks — part of 52pp gap may be objective  
5. VC-1 uses MSE MLP — unfair vs multimodal diffusion  
6. Chunk T_p/T_a underspecified in main (“several”); only bimanual 64/12 — mushy vs DP/π₀  
7. Manual data curation under-ablated (which datasets drive 43→83%)  
8. Frozen T5 + lang poverty; goals +25% = visual goals carry more bits  
9. Wrist weakness admitted (27% coverage) — undermines multi-cam marketing  
10. No mobile/nav — cross-embodiment ≠ universal FM  
11. 93M vs 55B compute rhetoric ≠ equal representation  
12. Optimal demos only — no offline RL on suboptimal OXE  

**Reporting checklist:** pair 72% with ~100 demos/50k/A5000; 29% with “vs RT-1-X, selected lang tasks”; 83% with WidowX/Small; cite **5% novel skill** when someone says generalist zero-shot.

**Citation guidance:** open FT ancestor / mixture case study — **NOT** proof open≈RT-2-X or 2026 BEHAVIOR drop-in (prefer π₀.₅/N1.7).

**Verdict:** Strong RSS 2024 open systems paper between ACT/DP and π₀/OpenVLA. Praise openness+FT protocol; punish zero-shot overclaim and underspecified horizons.

### Empiricist (11+3)
| Priority | Experiment |
|----------|------------|
| P0 Y | HF ckpt dry infer; diffusion steps / chunk exec grid |
| P0 Partial | Language vs goal-image |
| P1 | FT vs scratch vs VC-1-style; new action head; wrist on/off; Bridge-only vs Octo mix |
| Defer | OXE TPU re-pretrain; 9-robot hardware suite |

Budget: FT ~24 GB; P0–P1 hours–1–2 days on 1 GPU. Env: `/workspace/octo-quiet/env_outline/`.  
Tattoo: 800k/25; +29%; 72%; 83/35/18; 27M/93M; novel skill **5%**.

### Archaeologist (11+3)
**Priors:** OXE/RT-X, DP, ACT, ViT, T5, DDPM, HER goals, RT-1/RoboCat, Bridge.  
**Pivot:** open ViT+diffusion-chunk GRP on 800k OXE with block-wise adapters.  
**Genealogy:** ACT∥DP → **Octo** → OpenVLA/π₀ → OpenPI/π₀.₅/RTC → BEHAVIOR’25–’26.

### Visionary (11+3)
Teach Octo, run π₀.₅/GR00T for Oct 16 push. Keep Octo’s eval virtues (shared FT HPs, adapter tests, App.E negatives). Steal: chunk+receding; backbone-once+light head. Ablate FM vs DDPM on same VLM. Durable idea: compositional multi-robot tokenization + generative chunk head. Skip Discord/Drive.

---

## 5. Slide Maker brief

| Role | Timing | Focus |
|------|--------|-------|
| Stakeholder | 18+3 | Open FT ancestor, not 2026 SOTA |
| **Reviewer** | **8+3** | **12 critiques + reporting checklist (HIGH)** |
| Empiricist | 11+3 | Smoke grids + tattoo numbers |
| Archaeologist | 11+3 | ACT∥DP→Octo→π₀→BEHAVIOR |
| Visionary | 11+3 | Teach Octo / run π₀.₅/N1.7 |

Spine: open OXE GRP + diffusion chunks; underspecified horizons. PDF for Mac.  
**GitHub title:** `[ECE 605] - Octo Making Scientific Reviewer`

---

## Paths
| Artifact | Path |
|----------|------|
| This pack | `/workspace/octo-quiet/KANTA_PACK.md` |
| Senior Research | `/workspace/octo-quiet/DELIVERABLE.md` |
| Summarizer | `/workspace/papers/octo-2405.12213-summary.md` |
| Paper PDF | `/workspace/papers/octo-2405.12213.pdf` |
