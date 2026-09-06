# Paper Manager pack — OpenVLA (Oct 21)

**Paper:** OpenVLA: An Open-Source Vision-Language-Action Model  
**arXiv:** https://arxiv.org/abs/2406.09246 · CoRL 2024  
**HF:** `openvla/openvla-7b` · https://openvla.github.io  
**PDF:** /workspace/papers/openvla-2406.09246.pdf  
**Session:** Oct 21 · due 9:30 AM · **You = Empiricist (seat 1)** · Archaeologist=5  
**Sources:** Summarizer + Senior Research (`/workspace/openvla-quiet/DELIVERABLE.md`)

---

## 1. Paper summary

Open Llama/Prismatic **discrete-AR VLA** on OXE ~**970k** with **256-bin** EE actions, **no chunking**. Prismatic (fused **DINOv2+SigLIP** + **Llama-2 7B**). Beats RT-2-X on WidowX/Google at **7× smaller**; LoRA/int4 make consumer FT/serve practical.

**Syllabus role:** historical open AR baseline — BEHAVIOR deploy belongs to π₀.₅/OpenPI/GR00T.

---

## 2. Action-family taxonomy (CLEAR)

| Family | Model | Signature |
|--------|-------|-----------|
| Naïve discrete AR bins | **OpenVLA / RT-2** | Lang grounding @ ~5–6 Hz; weak high-Hz |
| DDPM chunks | **Octo / DP** | Smoother narrow skills |
| FM chunks | **π₀ / π₀.₅** | 2025–26 deploy winners |
| Compressed discrete AR | **FAST / OpenVLA+FAST / π₀-FAST** | AR recovers high-Hz |

---

## 3. Keywords / stack

DINOv2∥SigLIP + Llama-2 7B @ 224²; vision unfrozen; 7D EE; 256 bins; CE; single RGB; **no** wrist/proprio/history/chunks. Train ~21.5k A100-h. Infer ~6 Hz / 4090.

---

## 4. Role packs

### Stakeholder (18+3)
Open HF 7B for lang manip + LoRA on 1×A100 / int4 ~7GB. 2026: teaching + ablation harness, not BEHAVIOR entrant — ship π₀.₅/GR00T.

### Scientific Reviewer (8+3)
Strengths: open 7B+OXE; PEFT/quant tables; honest limits. Weaknesses: RT-2-X comparison quirks; DP still better narrow dexterity; LIBERO deltas small. Verdict: strong CoRL’24 open-science systems paper — not 2026 substrate.

### Empiricist (11+3) — **YOU (seat 1)** — HIGH PRIORITY

| Priority | Experiment |
|----------|------------|
| P0 Y | HF ckpt dry infer (~15–17 GB bf16); int4/int8 serve (~7 GB int4) |
| P0 Partial | LIBERO-Spatial LoRA r=32 scout |
| P1 | LoRA r sweep; full FT vs LoRA; vision FT vs freeze; **AR discrete vs DP**; OpenVLA vs π₀ FM vs Octo @ matched commit_s |
| P2 | no-op filter; FAST drop-in vs 256 bins |
| Defer | 21.5k A100-h OpenX re-pretrain; multi-robot real suites |

**H:** LoRA r=32 ≈ full FT within ~2 pp; unfreeze vision helps; OpenVLA AR wins multi-object lang; DP/FM win precision/smooth Hz.

**Tattoo:** WidowX **70.6%** vs RT-2-X 50.6 / Octo 20; Google **85.0%**; LoRA **68.2** ≈ full **69.7**; LIBERO FT **76.5**.  
**Ladder:** HF smoke → LoRA r=32 → DP-matched side-by-side → optional FAST+ before high-Hz claims.  
Env: `/workspace/openvla-quiet/env_outline/`.

### Archaeologist (11+3) — weight 5
**Priors:** RT-1/RT-2; Prismatic; Llama-2; SigLIP+DINOv2; OXE; Octo peer; DP foil.  
**Genealogy:** RT-2-style discrete AR → **OpenVLA** (open) ∥ Octo (diffusion) → π₀ (FM) → FAST upgrades naïve bins → π₀.₅/OpenPI/BEHAVIOR.

### Visionary (11+3)
Museum/syllabus AR node. Keep in harness as bin-AR control; ship π₀.₅/GR00T. Steal LoRA recipe. If AR research continues → FAST/chunks before claiming high-Hz. Skip Discord/Drive.

---

## 5. Slide Maker brief

| Role | Timing | Focus |
|------|--------|-------|
| Stakeholder | 18+3 | Open HF + LoRA, not 2026 entrant |
| Reviewer | 8+3 | Open-science strengths + comparison caveats |
| **Empiricist** | **11+3** | **LoRA ladder + AR vs FM/DP (HIGH)** |
| Archaeologist | 11+3 | Discrete AR peer era + FAST upgrade |
| Visionary | 11+3 | Harness vs deploy split |

Spine: action-family taxonomy; no-chunk AR latency. PDF for Mac.  
**GitHub title:** `[ECE 605] - OpenVLA Making Empiricist`

---

## Paths
| Artifact | Path |
|----------|------|
| This pack | `/workspace/openvla-quiet/KANTA_PACK.md` |
| Senior Research | `/workspace/openvla-quiet/DELIVERABLE.md` |
| Summarizer | `/workspace/papers/openvla-2406.09246-summary.md` |
| Paper PDF | `/workspace/papers/openvla-2406.09246.pdf` |
