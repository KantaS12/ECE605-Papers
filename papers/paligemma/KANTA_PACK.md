# Paper Manager pack — PaliGemma (Sep 16)

**Paper:** PaliGemma: A versatile 3B VLM for transfer  
**arXiv:** https://arxiv.org/abs/2407.07726  
**PDF:** /workspace/papers/paligemma-2407.07726.pdf  
**Code:** https://github.com/google-research/big_vision  
**Session:** Sep 16 · due 9:30 AM · **You = Scientific Reviewer (seat 1)**  
**Type:** VLA Architectures  
**Sources:** Summarizer (`/workspace/papers/paligemma-2407.07726-summary.md`) + Senior Research (`/workspace/paligemma-quiet/DELIVERABLE.md`)

---

## 1. Paper summary

Open **base** (not instruction-tuned) ~3B VLM for transfer: **SigLIP-So400m** vision + linear connector + **Gemma-2B**. Prefix-LM masking (full attn over image+prefix; causal suffix).

**Stages:** Stage0=pretrained SigLIP+Gemma → Stage1 multimodal pretrain (~1B examples @224) → Stage2 higher-res (+50M@448, +10M@896) → Stage3 per-task FT.

**Headline transfers:** COCOcap ~141.9/144.6; VQAv2 ~83.2/85.6; DocVQA **43.7→84.8** with resolution; ScienceQA ~95%@224.

**Club relevance:** Default open substrate for **π₀ / π₀.₅ / OpenPI**; BEHAVIOR’25 1st place stack = SigLIP → PaliGemma → Action Expert (~26% q).

---

## 2. Keywords

PaliGemma, SigLIP-So400m, Gemma-2B, Prefix-LM, open base VLM, transfer, loc/seg tokens, resolution 224/448/896, VLA backbone, π₀/π₀.₅/OpenPI, BEHAVIOR

---

## 3. Role packs

### Stakeholder (18+3) — seat 4
Commodity open 3B VLM for robot FMs. Prefer `paligemma-pt-{224,448}`; budget Stage3-like FT; multi-cam often stays @224. Risks: private Stage1 data; license/gating; base≠chat.

### Scientific Reviewer (8+3) — **YOU (seat 1)** — HIGH PRIORITY

**Claim under review:** A <3B open *base* VLM transfers broadly and is a better research object than chat-tuned VLMs.

**Strengths**
- Clear **base vs IT** separation
- Coherent PaLI-line scaling thesis (SigLIP+Gemma)
- Unusually deep ablations (Stage1 length, mask/loss, freeze/reset, connector, encoder, resolution, few-shot)
- Contamination hygiene; honest single-HP transfer protocol
- Structured loc/seg API; systems transparency

**Weaknesses**
- **Private Stage1 mixture** blocks independent recipe reproduction at paper scale
- Table1 mostly self-vs-resolution, not matched public bake-off
- Metric mixing / macro-average can obscure per-family performance
- Stage3 ≠ OOD robotics transfer
- Square resize may hurt embodiment cameras
- No action / memory / 3D objectives; uneven seeds
- TPU-centric; no GPU/LoRA memory tables in paper (LoRA = Empiricist extension)

**Actionable improvements (say these aloud)**
1. Public Stage1 surrogate mixture / sampling weights  
2. Fixed public baseline panel (Idefics2 / QwenVL / InternVL) under **same** transfer recipe  
3. Per-family normalized scores instead of opaque macro-average  
4. Robotics probes: egocentric VQA, affordance pointing, multi-cam correspondence  
5. Aspect-ratio / camera-model ablations  
6. loc/seg calibration + constrained decoding analysis  
7. Continuous ViT:LLM LR-ratio sweeps (not binary freeze)  
8. Preregister resolution-sensitive tasks; corrected win rates  
9. One-command public Table1 harness from released ckpts  
10. Negative-results appendix (e.g. SciCap multitask collapse)

**Score intuition:** Strong open-model + ablation contribution; landmark *if* data opacity is acknowledged; methods venues should demand public mixture + matched baselines.

### Empiricist (11+3) — seat 3

**H1:** 448pt + LoRA r=16 closes ≥70% of 224→448 OCR/VQA gain at <30% full-FT cost  
**H2:** Native Stage2-res ckpt beats cross-res FT under LoRA  
**H3:** K-frame concat (4–8 @224) helps short-horizon BEHAVIOR-style VQA vs single frame  

| Priority | Experiment |
|----------|------------|
| P0 E1 | Resolution ladder 224/448/(896) on OCR-ish |
| P0 E2 | Freeze matrix (TT / TF / LoRA-LLM / LoRA+connector) |
| P0 E3 | Default-HP vs paper recipe regret |
| P1 | LoRA rank; cross-res transfer; few-shot curves |
| P2 | RefCOCO stretch; multi-frame; encoder-free smoke |

**Budget:** LoRA@224 on ~24GB; full FT @448–896 needs 80GB+. Outlines: `/workspace/paligemma-quiet/env_outline/`.

### Archaeologist (11+3) — seat 2
**Priors:** PaLI/PaLI-X/PaLM-E/PaLI-3; SigLIP + SoViT; Gemma-2B; CLIP→Flamingo/BLIP-2; Pix2Seq/LocCa/SAM; Prefix-LM; big_vision.  
**Subsequent (clear):** π₀ (PaliGemma init); π₀.₅ / OpenPI; BEHAVIOR’25 1st (2512.06951).  
**Arrow:** SigLIP → **PaliGemma** → π₀ → π₀.₅/OpenPI → BEHAVIOR’25.

### Visionary (11+3) — seat 5
2026 BEHAVIOR: PaliGemma-class remains default open VLA init until a better open egocentric/video 3–4B appears. Upside = **adaptation** (action expert, noise, stages, data), not backbone replacement. Multi-cam 224 + selective 448 foveation > blanket 896. Winners already swap free language for task/stage embeddings — treat PG as visual tokenizer + residual LLM. Skip Discord/Drive.

---

## 4. VLA / control note

Perception/grounding backbone (loc/seg, OCR@448+), not a policy or classical planner. Prefix-LM fits observe+goal→action shaping; don’t freeze vision early (slow warmup). Open weights are load-bearing for BEHAVIOR forks.

---

## 5. Slide Maker brief

| Role | Timing | Focus |
|------|--------|-------|
| Stakeholder | 18+3 | Open 3B substrate → robot FMs |
| **Reviewer** | **8+3** | **Strengths / weaknesses / 10 actionable asks (HIGH)** |
| Empiricist | 11+3 | Res ladder + LoRA freeze matrix |
| Archaeologist | 11+3 | SigLIP→PG→π₀→BEHAVIOR timeline |
| Visionary | 11+3 | 2026 adaptation > backbone swap |

Image-first; speaker notes; **PDF for Mac**.

---

## Paths
| Artifact | Path |
|----------|------|
| This pack | `/workspace/paligemma-quiet/KANTA_PACK.md` |
| Senior Research | `/workspace/paligemma-quiet/DELIVERABLE.md` |
| Summarizer | `/workspace/papers/paligemma-2407.07726-summary.md` |
