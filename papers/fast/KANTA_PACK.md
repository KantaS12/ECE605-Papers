# Paper Manager pack — FAST (Oct 19)

**Paper:** FAST: Efficient Action Tokenization for Vision-Language-Action Models  
**arXiv:** https://arxiv.org/abs/2501.09747  
**Site:** https://pi.website/research/fast · HF `physical-intelligence/fast`  
**PDF:** /workspace/papers/fast-2501.09747.pdf  
**Session:** Oct 19 · due 9:30 AM · **Empiricist=2** (only filled seat); full five packs  
**Sources:** Summarizer + Senior Research (`/workspace/fast-quiet/DELIVERABLE.md`)

---

## 1. CRITICAL MYTH BUST (say first)

| Label | Meaning |
|-------|---------|
| **FAST** | Discrete DCT→BPE action tokenizer for **AR** VLAs |
| **FM** | Continuous flow-matching action generation (π₀ / π₀.₅ **deploy**) |
| **π₀-FAST** | Discrete AR **variant** using this tokenizer |
| **π₀.₅** | FAST for **pretrain**, **FM for post-train/deploy** — FAST does **NOT** replace FM |

Naming hygiene: π₀ ≈ FM; π₀-FAST ≈ discrete AR; π₀.₅ ≈ hybrid successor.

---

## 2. Paper summary

Compression-based **DCT + BPE** action tokenization unlocks high-Hz AR VLAs where naïve per-dim binning fails. Ships **FAST+** universal tokenizer (~1M 1s chunks). **π₀-FAST** matches diffusion/FM π₀ on dexterous generalist tasks at **~5× less train compute**, but infer ~**750 ms** vs FM ~**100 ms**/chunk (4090). First zero-shot DROID language generalist in unseen rooms.

---

## 3. Keywords / pipeline

Quantile norm → per-dim **DCT** → scale γ (default **10**) → round → low-freq-first flatten → **BPE** vocab **1024** → overwrite VLM vocab slots. Typical **1 s** chunks; invertible decode.

Keywords: FAST, FAST+, DCT, BPE, π₀-FAST, AR VLA, action chunks, DROID ZS, OXE

---

## 4. When FAST helps vs FM wins

| FAST helps | Continuous FM wins |
|------------|-------------------|
| High-Hz next-token prediction | Inference latency / smooth dynamics |
| Train compute / compression | Closed-loop control (fewer ms/chunk) |
| Denser chunks without token blowup | Real-time household deploy |

π₀.₅ recipe: FAST for efficient pretrain NTP → serve with continuous FM.

---

## 5. Role packs

### Stakeholder (18+3)
Ship FAST+ for AR training / cheap pretrain; keep FM for realtime deploy. Do **not** delete FM because FAST exists. Risk: AR latency ~7.5× vs FM.

### Scientific Reviewer (8+3)
Strengths: causal story; invertible compressor; honest latency; open HF. Weaknesses: figure-only scores; FAST+ mix leakage; AR vs FM not FLOP-matched; DROID 44 trials. Verdict: cite as discrete tokenizer unlock / π₀-FAST — **not** as FM retirement.

### Empiricist (11+3) — WEIGHT 2 · LONGEST
**H / smoke**
| Priority | Experiment |
|----------|------------|
| P0 Y | HF FAST+ encode/decode; compression vs naïve bins; didactic AR toy across Hz |
| P1 Partial | VLA FT FAST+ vs bins; BPE on/off; γ sweep; **AR-FAST vs continuous FM** matched task |
| P2 Partial | fit() vs universal; π₀.₅ hybrid echo |
| Defer | Full π₀-FAST 10k h; real laundry hardware |

**Reproduce:** `AutoProcessor.from_pretrained("physical-intelligence/fast", trust_remote_code=True)`; γ=10; |V|=1024.  
**Sanity Table I:** Bridge~20, DROID~29, bussing~28, shirt~53 vs naïve 35/105/140/700.  
**Latency:** ~100 ms FM vs ~750 ms AR-FAST. Env: `/workspace/fast-quiet/env_outline/`.

### Archaeologist (11+3)
Priors: JPEG/DCT, BPE, RT-2/OpenVLA naïve bins, ACT/DP/π₀ chunks, π₀ FM, VQ/FSQ.  
Forward: FAST → π₀-FAST → π₀.₅ (FAST pre→FM) → KI → RTC → BEHAVIOR. Keep labels distinct.

### Visionary (11+3)
Keep FM(+RTC) at deploy; keep FAST for pretrain/language/compression. Invest AR decode speed if discrete VLAs compete. FAST+ default for new morphologies. Skip Discord/Drive.

---

## 6. Slide Maker brief

| Role | Timing | Focus |
|------|--------|-------|
| Stakeholder | 18+3 | FAST for train / FM for deploy |
| Reviewer | 8+3 | Not FM retirement |
| **Empiricist** | **11+3** | **Tokenizer smoke + AR vs FM (HIGH)** |
| Archaeologist | 11+3 | Naming hygiene π₀ / π₀-FAST / π₀.₅ |
| Visionary | 11+3 | Hybrid for 2026 BEHAVIOR |

Spine: myth bust + DCT→BPE pipeline. PDF for Mac.  
**GitHub title:** `[ECE 605] - FAST Making Empiricist`

---

## Paths
| Artifact | Path |
|----------|------|
| This pack | `/workspace/fast-quiet/KANTA_PACK.md` |
| Senior Research | `/workspace/fast-quiet/DELIVERABLE.md` |
| Summarizer | `/workspace/papers/fast-2501.09747-summary.md` |
| Paper PDF | `/workspace/papers/fast-2501.09747.pdf` |
