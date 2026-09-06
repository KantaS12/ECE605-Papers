# Paper Manager pack — OpenVLA-OFT (Oct 26)

**Paper:** Fine-Tuning Vision-Language-Action Models: Optimizing Speed and Success  
**arXiv:** https://arxiv.org/abs/2502.19645  
**Site/code:** https://openvla-oft.github.io · github.com/moojink/openvla-oft  
**PDF:** /workspace/papers/openvla-oft-2502.19645.pdf  
**Session:** Oct 26 · due 9:30 AM · **Stakeholder=3** (only filled); full five packs  
**Sources:** Summarizer + Senior Research (`/workspace/openvla-oft-quiet/DELIVERABLE.md`)

---

## 1. One-liner (Stakeholder)

**OFT = speed+success FT patch** making OpenVLA viable at 25–50 Hz (26–43× throughput, LIBERO **97.1%**) — modernize OpenVLA ops, **don’t** substitute for π₀.₅ on BEHAVIOR.

**OFT = FT recipe, not a new foundation model.** Not FAST. Not FM.

---

## 2. Output signature shift (CLEAR)

| | Vanilla OpenVLA | OFT |
|--|-----------------|-----|
| Decode | Serial AR discrete bins | **Parallel** continuous decode |
| Chunk | K=1 | K=**8** LIBERO / K=**25** ALOHA |
| Head | 256-bin CE | Continuous MLP + **L1** |
| Hz | ~4.2 | ~**109.7** LIBERO (26×); ALOHA ~**77.9** (43×) |
| LIBERO SR | 76.5% | **97.1%** (π₀ 94.2%) |

**OFT+** adds **FiLM** on SigLIP+DINOv2 (multi-view language) — without FiLM, language → ~chance.

---

## 3. Exact OFT recipe

1. Parallel decoding (bidirectional attn over empty action embeds)  
2. Action chunking K=8/25  
3. Continuous MLP head  
4. L1 on [-1,1]  
5. LoRA r=32; optional wrist+proprio  
6. OFT+: FiLM on both vision towers  

---

## 4. Role packs

### Stakeholder (18+3) — WEIGHT 3 · LONGEST

**Board table:** LIBERO SR 76.5→97.1; Hz 4.2→109.7; ALOHA Hz 1.8→77.9 (π₀ still 291); scoop 100 vs π₀ 93.3; pot 51 vs 42.

**Rule of thumb**
- OpenVLA/HF/LoRA ops + high Hz → **OFT mandatory**; stock AR obsolete for 25–50 Hz  
- Max wall-clock + OpenPI/JAX → **π₀/π₀.₅** still win latency + pretrain coverage  
- Tokenizer-only AR fix → **FAST** (different lever)

**Choose OFT when:** Prismatic/HF codebase; simple L1 FT; LIBERO-style + moderate demos; 7B AR-lineage ablations.  
**Choose π₀.₅ when:** OpenPI standardized; long-horizon open-world; **2026 BEHAVIOR primary**.

**Risks:** fine-tuning 2024 OXE AR ≠ 2025 FM generalist; L1 unimodality; FiLM tax; ALOHA chunk latency 0.321s vs π₀ 0.086s; “97% LIBERO” ≠ BEHAVIOR-ready; FAST≠OFT confusion.

### Scientific Reviewer (8+3)
Strengths: factorial decode×rep×objective; throughput+latency; FiLM ablation; L1≈diffusion cheaper. Caveats: LoRA OFT vs full-FT π₀; latency abstract “ms” vs tables in seconds; pot ~50%; L1 multimodal undertested. Verdict: better OpenVLA FT default — does **not** license “OFT supersedes FM.”

### Empiricist (11+3)
| Priority | Experiment |
|----------|------------|
| P0 Y | Vanilla vs OFT Cont-L1 dry infer + signature dump |
| P1 Partial | LIBERO-Spatial LoRA; PD&AC discrete vs Cont-L1; K∈{1,4,8}; wrist+proprio; vs π₀/DP/ACT |
| P2 | FiLM; FAST AR vs OFT parallel |
| Defer | Paper 8×80GB full suites; real ALOHA |

Budget: P0 <1 GPU-h; LIBERO scout 16–40 GPU-h. Env: `/workspace/openvla-oft-quiet/env_outline/`.

### Archaeologist (11+3)
OpenVLA AR bins → **OFT** continuous parallel chunks ∥ **FAST** (discrete AR upgrade) ∥ **π₀/π₀.₅** (FM). ACT supplies chunk+L1 ancestry. Parallel improvement paths — do not collapse names.

### Visionary (11+3)
OFT = corrective hygiene for OpenVLA stacks. Primary BEHAVIOR substrate remains **π₀.₅/OpenPI**. Keep OFT in harness as open AR-family control. Skip Discord/Drive.

---

## 5. Slide Maker brief

| Role | Timing | Focus |
|------|--------|-------|
| **Stakeholder** | **18+3** | **Decision tree OFT vs π₀.₅ (HIGH)** |
| Reviewer | 8+3 | Not superseding FM |
| Empiricist | 11+3 | Signature dump + K sweep |
| Archaeologist | 11+3 | OFT ∥ FAST ∥ FM forks |
| Visionary | 11+3 | Harness vs BEHAVIOR deploy |

Spine: output signature shift; OFT ≠ FAST ≠ FM. PDF for Mac.  
**GitHub title:** `[ECE 605] - OpenVLA-OFT Making Stakeholder`

---

## Paths
| Artifact | Path |
|----------|------|
| This pack | `/workspace/openvla-oft-quiet/KANTA_PACK.md` |
| Senior Research | `/workspace/openvla-oft-quiet/DELIVERABLE.md` |
| Summarizer | `/workspace/papers/openvla-oft-2502.19645-summary.md` |
| Paper PDF | `/workspace/papers/openvla-oft-2502.19645.pdf` |
