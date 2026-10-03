# Paper Manager pack — World Simulation / Cosmos WFM (Dec 7 · Accompanying Models)

**Paper:** World Simulation with Video Foundation Models for Physical AI  
**arXiv:** https://arxiv.org/abs/2511.00062 · [predict2.5](https://github.com/nvidia-cosmos/cosmos-predict2.5) · [transfer2.5](https://github.com/nvidia-cosmos/cosmos-transfer2.5) · arXiv **2511.00062v2** (24 Feb 2026) · NVIDIA Open Model License  
**Authors:** NVIDIA Cosmos / Physical AI (Arslan Ali … Yuke Zhu; large author list — Sec. A)  
**PDF:** `/workspace/papers/world-simulation-2511.00062.pdf` (= `/workspace/world-simulation-quiet/paper.pdf`) · **44 pp** · ~35 MB  
**Session:** Dec 7 · due 9:30 AM · Type: **Accompanying Models (World Models, Reasoning, Planning)**  
**Role weights:** **Archaeologist=3 (Nykolas Rekasius) LONGEST PRIMARY**; **Visionary=1 (Kanta Saito)**; Stakeholder / Scientific Reviewer / Empiricist seats empty — **still all five produced**; Archaeologist heaviest  
**Kanta role:** **Visionary=1 seated** — BEHAVIOR / closed-loop / photoreal synth hooks; dual-WM fork (generative video WFM vs latent LeWM/JEPA); **skip Discord/Drive**  
**Themes:** generative video **WFM as physics/world simulator** (alt to latent WMs) · **Cosmos-Predict2.5** flow matching unified **T2W/I2W/V2W** · **Cosmos-Reason1** text · **Transfer2.5** Sim2Real/Real2Real (**3.5×** smaller than Transfer1-7B) · synth data / policy eval / closed-loop robotics+AV · **PARALLEL** to Ha/Dreamer / I-JEPA→V-JEPA→LeWM (latent) — **not** a JEPA child  
**Sources:** Summarizer (`/workspace/papers/world-simulation-2511.00062-summary.md`) + Senior Research (`/workspace/world-simulation-quiet/DELIVERABLE.md`) + Ablation Bot (**GAP** — paper-extract Tabs. 10–20 / §6–7) + `paper_notes.md` / `env_outline/`  
**Sibling links:** `/workspace/leworldmodel-quiet/` · `/workspace/vjepa21-quiet/` · `/workspace/ijepa-quiet/` · `/workspace/robogen-quiet/` (generative *curriculum* foil)

---

## 1. One-liner

**Open Physical-AI generative-video world foundation models** — **Cosmos-Predict2.5** (flow-matching DiT, unified **Text2World / Image2World / Video2World**, **Reason1** grounding, **2B / 14B**, ~**200M** curated clips + RL post-train) + **Cosmos-Transfer2.5** (control-net Sim2Real/Real2Real; **3.5×** smaller than Transfer1-7B yet higher fidelity + better long-horizon RNDS). Enables synthetic data, policy evaluation, and closed-loop robotics/AV simulation — **pixel-space world simulator** pole, **PARALLEL** to latent JEPA / Dreamer planners (LeWM / V-JEPA 2-AC), **not** their child.

**Contrast poles (consistency triangle — extend prior packs; do not contradict):**

| Pole | What they do | Primary claim / use |
|------|--------------|---------------------|
| **DINOv2 / SigLIP** | Discriminative JEA / dense towers | VLA vision towers; DINO-WM freeze |
| **MAE / Dreamer / IRIS** | Generative / reward-tied WMs | FT or RL imagination (latent/pixel) |
| **I-JEPA → V-JEPA → 2 → 2.1 → 2-AC → LeWM** | Predictive **embedding** / compact latent plan | Fast CEM / MPC in latent — **PARALLEL foil** |
| **Cosmos-Predict1 → Predict2.5 + Transfer2.5** | **Generative video WFM** as Physical AI simulator | Synth data, photoreal eval, AC video, Sim2Real — **THIS** |

**Do NOT invent / claim:** total GPU-hours as a single headline (paper gives **4096 H100 MFU**, not aggregate GPU-h); course smoke = PAI **0.768** / Tab. 13 **24/30** / DreamGen VLA task SR; Cosmos as JEPA descendant; pixel CEM as tabulated Franka/BEHAVIOR result; π₀ / OpenVLA as paper-cited consumers (absent); Discord/Drive checklist.

---

## 2. Summary (exec overview + key numbers)

**NVIDIA Cosmos (arXiv 2511.00062v2)** ships open **World Foundation Models** for Physical AI. Core inventum: treat **large generative video** as a **physics / world simulator** — complementary to latent Dreamer/JEPA planners. **Predict2.5** replaces Predict1’s EDM diffusion with **flow matching**; unifies T2W/I2W/V2W via frame-replacement conditioning; swaps T5 for **Cosmos-Reason1** (Physical AI VLM → 1024-d multi-block); progressive pretrain 256→480→720p on **~200M** curated clips (from **35M hours** raw, **~4%** survival) + domain SFT soup + **VideoAlign RL** + optional **4-step** rCM distillation. **Transfer2.5** is a control-net (edge/blur/depth/seg) on Predict2.5-**2B** — **3.5×** smaller than Transfer1-**7B**, better adherence/quality, less long-horizon RNDS drop.

**Why Accompanying Models / Dec 7 after LeWM:** Students just saw **compact latent JEPA-WM** (LeWM ~15M, 0.98 s CEM). Now see the **opposite compute contract**: foundation-scale **pixel/video** imagination for synth data, photoreal policy eval, Sim2Real/Real2Real. Syllabus lesson = **the fork**, not flattening both into one “world model.”

| Axis | Number / claim |
|------|----------------|
| Venue / code | arXiv **2511.00062v2**; `nvidia-cosmos/cosmos-predict2.5` + `cosmos-transfer2.5` **CLEAR** |
| Scales | Predict **2B / 14B**; Transfer **2B** (vs Transfer1 **7B**) |
| Frames | **93** px / **24** latent @ **16 fps** (~**5.8 s**); WAN2.1 VAE **4×8×8** + **1×2×2** patch |
| Data | **~200M** curated clips; robotics Tab. 2 (AgiBot/Bridge/DROID/GR00T/…) |
| PAI T2W Overall post (Tab. 10) | 2B/14B **0.768** (pre 2B **0.751**) |
| PAI I2W Overall post (Tab. 11) | 2B/14B **0.810** (pre 2B **0.799**) |
| Human pref 14B vs Wan2.1-14B (Fig. 7) | **48.6%** vs **31.8%** |
| Distill (Tab. 7–8) | T2W **0.768→0.764** (4 steps); I2W **0.810→0.816** |
| Transfer size / quality (Tab. 12) | **3.5×** smaller; Uniform Quality **9.31** vs Transfer1 **9.24**; Edge F1 **0.49** vs **0.38** |
| Robot Real2Real aug (Tab. 13) | Proposed **24/30** · Baseline **5/30** · Base **1/30** |
| AV FVD / LET-AP (Tab. 14–15) | FVD **23.06** vs Predict1 **63.69**; LET-AP **0.394** vs **0.243** |
| DreamGen Object GPT (Tab. 18) | Predict2.5-14B/gr00tdream-gr1 **91.8** (video IF — **not** VLA SR) |
| Bridge AC (Tab. 19–20) | PSNR **24.95** / FVD **146**; TimeEmb ≫ CrossAtten ≫ ChannelConcat |
| Infra (Tab. 9) | **4096 H100**; MFU 2B **36.49%** / 14B **33.08%** — course train = **N** |

**Must-cite tattoo:** PAI T2W/I2W **0.768 / 0.810** · Transfer **3.5×** · robot **24/30 vs 5/30** · Bridge PSNR **24.95** · TimeEmb Tab. 20 · DreamGen **91.8** (IF only) · AV LET-AP **0.394** · **latent vs pixel fork** · **infer-only** course smokes.

**Do NOT claim:** course = paper PAI / 24/30 / 4096-H100 train; DreamGen IF = VLA task success; closed-loop = Dreamer-style actor trained purely in Cosmos imagination (paper: Transfer aug + AC video eval + long-horizon Transfer); Cosmos ∈ JEPA genealogy.

---

## 3. Keywords

Cosmos-Predict2.5; Cosmos-Transfer2.5; Cosmos-Reason1; world foundation model; flow matching; Text2World; Image2World; Video2World; Physical AI; synthetic data; policy evaluation; closed-loop simulation; Sim2Real; Real2Real; control-net; multi-view driving; action-conditioned video; DreamGen; Bridge; AgiBot; GR00T GR1; PAI-Bench; WAN2.1 VAE; latent vs generative WM; Accompanying Models; BEHAVIOR synth/eval (TENTATIVE)

---

## 4. Deep dive — World-model / planning + lineage triangle (**PRIMARY SESSION CONTRIBUTION**)

**Callout:** Scientific product of *this* PDF = open **generative-video Physical AI world simulator** (Predict + Transfer + Reason1 + apps). Session type = Accompanying Models because Dec 7 must teach **why latent planners and pixel WFMs both exist** — opposite compute contracts under the same “world model” phrase.

### Taxonomy — CLEAR vs Ha/Dreamer, JEPA/LeWM, this paper

| Family | State / objective | Control use | Course smoke | Foil role |
|--------|-------------------|-------------|--------------|-----------|
| **Ha & Schmidhuber / Dreamer** | Latent VAE+RNN / RSSM; imagine + reward | RL in imagination | Trainable small | Latent-paradigm **parent** (§7 cite) |
| **I-JEPA → V-JEPA → 2.1 → 2-AC** | Predict **embeddings**; AC CEM | Real-robot latent plan (Franka) | Sibling packs | **PARALLEL** foil (§7 cites V-JEPA 2) |
| **LeWM** | Compact E2E JEPA + SIGReg; ~15M | Sim CEM **0.98 s** (~**48×** vs DINO-WM) | Trainable 1 GPU | Compact latent foil |
| **Classic physics sims** (IsaacSim…) | Engineered dynamics | Train / closed-loop | — | Transfer **photorealizes** Sim2Real |
| **Sora-class / Wan / Hunyuan / Cog** | Generative video | Mostly content | — | PAI / DreamGen foils |
| **Cosmos-Predict1 / Transfer1** | Prior NVIDIA Physical AI WFM (EDM) | Synth / AV | — | Direct predecessor |
| **THIS (Predict2.5 + Transfer2.5)** | Flow DiT **pixels** + control-net | Synth data, AC eval, Real2Real, AV | **Infer-only** probes | **Inventum** |

### Consistency / lineage triangle (extend; do **not** fold into JEPA)

```
DINOv2 / SigLIP          —— discriminative JEA / dense towers —— VLA vision towers / DINO-WM freeze
MAE / Dreamer / IRIS     —— generative / reward-tied WMs      —— FT or RL imagination
I-JEPA → V-JEPA → 2 → 2.1 → 2-AC —— Meta predictive SSL → AC latent planning (Franka)
         ↘ PLDM → LeWM            —— compact E2E JEPA-WM from pixels (sim CEM)
Cosmos-Predict1 → Predict2.5 (+ Reason1) + Transfer2.5 —— GENERATIVE VIDEO WFM as Physical AI simulator
         (PARALLEL pole: pixel/video imagination, synth data, closed-loop / policy eval)
RoboGen (side) —— generative *task/scene/reward* curriculum in classic sim — not pixels
```

**One-liner (fork):** *Latent WMs (Dreamer/JEPA/LeWM) compress to **plan**; Cosmos WFMs decompress to **simulate and synthesize** — same phrase, opposite compute contracts.*

### What is “world-model-ish” here — CLEAR (in-paper)

| Piece | In-paper role | World-model reading |
|-------|---------------|---------------------|
| Flow DiT Predict2.5 | Velocity net; unified T2W/I2W/V2W | Pixel/video dynamics \(P(\text{video}\mid\text{cond})\) |
| Reason1 text | Physical AI VLM → 1024-d cross-attn | Language-grounded world prompts |
| Transfer2.5 | Edge/blur/depth/seg control-net | Controllable Sim2Real / Real2Real translation |
| Action-cond (Tab. 19–20) | TimeEmb best; Bridge AR chunks | Action-conditioned video for **policy evaluation** |
| Real-robot Tab. 13 | Transfer Real2Real → DP **24/30** | Synth **observation** factory (offline aug) |
| AV multiview Tab. 14–15 | Map-conditioned Transfer; FVD / LET-AP | Closed-loop AV **scenario** simulation |
| DreamGen Tab. 18 | Text→robot video IF **91.8** Object GPT | VLA **demo** synth pipeline (IF ≠ task SR) |
| Long-horizon RNDS Fig. 10 | Transfer2.5 less drop vs Transfer1 | Stability of chunked pixel imagination |

### What is **not** overclaimable

- Full **online pixel CEM / Dreamer-in-Cosmos** agent SR on Franka/BEHAVIOR — **not tabulated**; Transfer robot result is **offline synth-data aug**; AC model = **video prediction / eval** quality.  
- DreamGen Tab. 18 = **video instruction-following** (VLM judges), **not** hardware VLA success.  
- Equivalence to Newton-solver physics accuracy — qualitative coherence; Domain/VideoPhy still needed.  
- Course students train 200M / 4096 H100 — **N**; smoke = frozen **2B** infer + stubs.  
- π₀ / OpenVLA citations — **absent** (GR00T / DreamGen / Diffusion Policy **are** cited).  
- Collapsing Cosmos into JEPA genealogy — **FALSE**; paper §7 explicitly adopts generative-video pole **against** latent WMs.

### Architecture checklist (Empiricist / Archaeologist pin)

| Piece | Spec (paper) |
|-------|----------------|
| Paradigm | Pixel-space generative video WFM |
| Dynamics | **Flow matching** \(v_t=\varepsilon-x\); logit-normal + β shift 1→5; 5% high-noise oversample |
| Backbone | DiT velocity; **3D RoPE only** (abs PE removed vs Predict1) |
| Scales | **2B:** 32L / d=2048 / 16H; **14B:** 36L / d=5120 / 40H (Tab. 3) |
| VAE | WAN2.1 causal; **4×8×8**; patch **1×2×2**; **93** frames @16 fps |
| Text | Cosmos-Reason1 multi-block → **1024-d** (replaces T5) |
| Modes | Unified T2W / I2W / V2W via **frame-replacement** + mask tokens |
| Post-train | Domain SFT (Tab. 5) → soup merge → VideoAlign RL → optional rCM **4-step** distill |
| Transfer | 4 control branches; blocks **every 7** main blocks (vs Transfer1 prefix stack) |
| Action inject | MLP → **time** embeddings best (Tab. 20) |
| Infra | Tab. 9 **4096 H100** MFU ~33–36% — **course train N** |

**World-model exam bite:** "Cosmos-Predict2.5 = open Physical-AI **generative-video world simulator** (flow DiT + Reason1; T2W/I2W/V2W; PAI **0.768/0.810**); Transfer2.5 = **3.5×** smaller control-net with robot **24/30** Real2Real aug; Bridge AC **24.95** PSNR; **PARALLEL** to latent Dreamer/JEPA/LeWM — plan-fast vs synth-rich; course = **infer-only** probes, never 4096-H100 train."

---

## 5. Deep dive — Cosmos family evolution + ablation honesty (Archaeologist spine methods)

**Callout:** Ablation Bot extract **AUTHORITATIVE** at `/workspace/papers/world-simulation-2511.00062-ablations.md`. P0 #1 = **Tab. 20** action inject (only labeled ablation). Most “ablations” = system leaps (Predict1→2.5, Transfer1→2.5, Wan foils, Real2Real vs image aug). Empiricist must **not** pretend RC1 reproduces Tab. 10.

### Family evolution diagram

```
Cosmos-Predict1 (diffusion DiT + T5)
        │  data↑ · unify T2W/I2W/V2W · drop abs PE
        │  flow matching · Reason1 · SFT domains · soup · RL · distill
        ▼
Cosmos-Predict2.5 (2B / 14B) ──── specialties ──► auto/multiview
        │                                    ├─ robot/action-cond
        │                                    ├─ robot/multiview-agibot
        │                                    └─ robot/gr00tdream-gr1 (14B)
        │ control-net redistribute (every 7 blocks)
        ▼
Cosmos-Transfer2.5-2B  (3.5× smaller than Transfer1-7B; better Tab.12 / RNDS)
        ├─ general (edge/blur/seg/depth)
        ├─ auto/multiview (HD map / scenario)
        └─ robot/multiview (+ agibot)

Text stack: T5 ──► Cosmos-Reason1 (Physical AI VLM; multi-block 1024-d)
```

### Ablation priority (paper-extract — Bot GAP)

| Rank | Factor | Locus | Course smoke |
|------|--------|-------|--------------|
| **1** | Frozen Predict2.5-2B short-clip probe | Tabs. 10–11; release | **Y — RC1 PRIMARY** |
| **2** | Unified modes T2W/I2W/V2W same ckpt | §3.2; Tab. 1 | **Partial — RC2** |
| **3** | Action inject TimeEmb / CrossAtten / ChannelConcat | **Tab. 20** | **Partial — RC3** or cite |
| **4** | Closed-loop stub (policy ↔ WM) | §6.2 / §6.6 | **Partial — RC4** |
| **5** | Transfer Edge vs Blur infer | Tab. 12 | **Partial** if ckpt |
| **6–11** | Pre/post PAI; Transfer1 bake-off; Tab. 13; AV; DreamGen; distill | Tabs. 7–15, 18 | **N** / cite |
| **12** | vs latent foils Dreamer/JEPA/LeWM | §7 | Conceptual + tiny foil stub |
| **13** | Full Progressive / 200M / RL / 4096 H100 | Tabs. 4–6, 9 | **N** forever |
| **GAP** | Ablation Bot file | — | Fold when present |

**Taxonomy / recipe exam bite:** "Predict1→2.5 is multi-factor (data+arch+Reason1+RL) — not a single course ablate; Tab. 20 TimeEmb **24.95** > Cross **24.41** > Concat **23.11** is the clean factorial; Transfer **3.5×** smaller yet better confounds base upgrade + block placement; course = frozen **2B** infer, never paper-scale train."

---

## 6. Role-ready sections (all five — Archaeologist RICHEST; Visionary=1 Kanta)

### 6.1 Archaeologist — WEIGHT 3 LONGEST PRIMARY (Nykolas Rekasius)

> **Seat charge:** Deepen **history, foil framing, Cosmos family evolution, CLEAR/TENTATIVE discipline**. Place generative-video WFM pole carefully vs Ha/Dreamer and I-JEPA→V-JEPA→LeWM — **PARALLEL, not child**. Longest talk block (**~16–18** slides). Empiricist empty → invest in lineage richness; do **not** overclaim smokes. Ablation Bot **GAP** → paper-extract honesty.

#### Priors — CLEAR (cited / direct ancestry)

| Prior | Role relative to Cosmos 2.5 | Label |
|-------|----------------------------|-------|
| **Cosmos-Predict1 / Transfer1** (NVIDIA 2025) | Immediate WFM predecessor (EDM DiT; T5; Transfer1-7B) | **CLEAR** |
| **Flow matching** Lipman’22; Esser SD3-style shifts | Training objective | **CLEAR** |
| **EDM** Karras’22 | Predict1 foil parameterization | **CLEAR** |
| **DiT** + WAN2.1 VAE | Backbone + tokenizer | **CLEAR** |
| **Cosmos-Reason1** (NVIDIA 2025) | Physical AI VLM text encoder (replaces T5) | **CLEAR** |
| **Sora / Kling / Veo / MovieGen / Seedance** | Closed Sora-class WFMs | **CLEAR** related |
| **Wan / Hunyuan / CogVideoX / LTX** | Open video gen foils (PAI / DreamGen) | **CLEAR** |
| **Ha & Schmidhuber 2018; Dreamer (Hafner)** | Latent WM paradigm foil | **CLEAR** (§7) |
| **V-JEPA 2** Assran’25 | Cited latent predictive WM example | **CLEAR cited as foil** — **not** ancestor |
| **ControlNet-style / Transfer1** | Transfer2.5 parent design | **CLEAR** |
| **Diffusion Policy** Chi; **DreamGen** Jang’25 | Policy / synth-data consumers | **CLEAR** |
| **Bridge / DROID / AgiBot / GR00T** datasets | Robotics data priors | **CLEAR** |
| **IsaacSim / classic physics engines** | Sim2Real photorealization partner | **CLEAR** (app context) |

#### Priors — TENTATIVE / design rhyme (do **not** overclaim)

- Treating Cosmos as “next I-JEPA / V-JEPA child”: **FALSE** — generate pixels ≠ predict embeddings.  
- Equating Transfer Real2Real aug with V-JEPA 2-AC zero-shot Franka CEM: **different mechanism** (data aug vs latent plan).  
- π₀ / OpenVLA as paper-cited consumers: **not cited** (rhyme ≠ citation).  
- Pixel-space CEM with Predict2.5 as dynamics: research fork, compute-hostile vs LeWM/2-AC.

#### This paper — CLEAR inventum

1. Unified **flow-matching** Predict2.5 (**2B/14B**) Text/Image/Video2World with **Reason1** grounding.  
2. Physical-AI-heavy **~200M**-clip curation (35M h → ~4% keep) + domain SFT soup + VideoAlign **RL** + optional 4-step distill.  
3. Transfer2.5 at **2B** beating Transfer1-**7B** on adherence/quality/long-horizon RNDS (**3.5×** smaller).  
4. Application suite: robot Real2Real policy aug (**24/30**), AV multiview world-scenario maps, camera-controlled robot views, DreamGen GR1, Bridge AC policy-eval WM.  
5. Open release under NVIDIA Open Model License (`cosmos-predict2.5` + `cosmos-transfer2.5`).

#### Subsequent — CLEAR

| Item | Note | Label |
|------|------|-------|
| github.com/nvidia-cosmos/cosmos-predict2.5 | Code + checkpoints | **CLEAR** |
| github.com/nvidia-cosmos/cosmos-transfer2.5 | Transfer stack | **CLEAR** |
| Specialty ckpts (Tab. 1) | auto/mv, action-cond, agibot, gr00tdream-gr1 | **CLEAR** |
| Prior-pack JEPA chain + LeWM | Remain valid **parallel** tracks | **CLEAR unchanged** |

#### Subsequent / parallel — TENTATIVE

| Item | Note | Label |
|------|------|-------|
| BEHAVIOR-1K boards driven by Cosmos-aug VLAs | Plausible, **not measured here** | **TENTATIVE** |
| Pixel CEM planning with Predict2.5 dynamics | Research fork; compute-hostile | **TENTATIVE** |
| Distilled 4-step as real-time closed-loop renderer | PAI near-parity; robot Hz **not** reported | **TENTATIVE** |
| Dual-WM stack (LeWM plan + Cosmos synth/eval) | Visionary composition | **TENTATIVE** — **not in-paper as stacked system** |
| RoboGen × Cosmos | Task factory × pixel factory | **TENTATIVE** composition |

#### Genealogy diagram (CLEAR framing — Dec 7 teaching map)

```
Pole A — Discriminative towers: SigLIP/DINO → VLA perception
Pole B — Predictive embeddings: I/V-JEPA → LeWM / 2-AC latent plan
         [packs: ijepa-quiet / vjepa21-quiet / leworldmodel-quiet]
Pole C — Generative video WFM: Sora-class → Cosmos-Predict1 → **Predict2.5+Transfer2.5 (THIS)**
         as simulator / synth / eval
Side   — RoboGen: generative curriculum in classic sim
         [pack: robogen-quiet]
```

Syllabus after LeWM: teach **why B and C both exist** — B for **fast** imagination/planning; C for **rich** observation synthesis and visual domain randomization. **Do not collapse C into B.**

#### Archaeologist depth — paradigm foil card (§7)

| Axis | **Latent WM** (Dreamer / JEPA / LeWM) | **Generative video WFM** (Cosmos) |
|------|----------------------------------------|-----------------------------------|
| State | Compact embedding / RSSM / CLS | VAE latent **decoded to pixels** |
| Objective | Predict next **repr** (+ anti-collapse) | Flow/diffusion match **video** distribution |
| Control use | Fast **MPC/CEM** in latent (LeWM 0.98s) | Action-cond video; policy **eval** / synth demos |
| Synth data | Weak for appearance diversity | **Strength** — Transfer Real2Real / DreamGen |
| Course smoke | Trainable on 1 GPU (sibling packs) | **Infer-only** probes (this pack) |
| Failure mode | Collapse / poor visuals | Drift / hallucination / huge cost / imperfect physics |

#### Reviewer notes (Archaeologist lens)

1. **Paradigm naming trap:** Always tag **latent-plan** vs **pixel-sim**.  
2. **Multi-factor leaps:** Predict2.5 gains ≠ single ablate; RC1 ≠ Tab. 10.  
3. **Human vs auto:** 2B≈14B on PAI Overall; human pref still jumps with size (33%→48.6% vs Wan2.1).  
4. **Transfer “3.5× smaller yet better”** confounds Predict2.5 base upgrade + Transfer redesign.  
5. **Physics humility:** Photoreal ≠ guaranteed physics.  
6. **Closed-loop:** Transfer long-horizon + robot BC aug + AC AR — **not** full Dreamer-in-Cosmos SR curves.  
7. **Open weights ≠ course trainability** — 4096 H100 MFU is a **blocker**.  
8. **Ablation Bot GAP** weakens Empiricist authority vs LeWM/V-JEPA packs — fold when present.

**Archaeologist one-liner:** *From Ha’s dreams → Dreamer’s latent imagination → JEPA’s representation prediction → LeWM’s stable compact planner → Cosmos WFM’s photoreal Physical AI simulator: Dec 7 should teach the fork, not flatten it.*

**Archaeologist exam bite:** "Cosmos-Predict2.5 = open Physical-AI **generative-video WFM** pole — successor to Predict1, **PARALLEL** to JEPA/LeWM (not child); inventum = flow+Reason1+unified modes+Transfer2.5@2B + apps (**24/30**, AV, DreamGen, Bridge AC); always tag latent-plan vs pixel-sim; Ablation Bot GAP → Tab. 20 + infer probes; never claim 4096-H100 course train."

---

### 6.2 Visionary — WEIGHT 1 (Kanta Saito) — BEHAVIOR’26 + closed-loop / photoreal synth

> **Seat charge:** Decision-oriented BEHAVIOR / motion-planning / long-horizon / closed-loop hooks. Empiricist empty → **do not demand Tab. 13 replication**. Dual-WM stack framing. **No Discord/Drive checklist.**

#### BEHAVIOR connection (honest)

- Winning BEHAVIOR stacks lean **π₀.₅ / FM action experts + strong VLM towers** — Cosmos enters as **observation factory** and **eval harness**, not as the motor policy.  
- Paper **CLEAR** path: Transfer2.5 Real2Real visual randomization (colors, lighting, distractors, backgrounds) — maps to BEHAVIOR visual/domain shift. In-paper evidence: **24/30** vs **5/30** classic augs on one bimanual Kinova task (n=3×10 scenarios).  
- DreamGen-style text→robot video→pseudo-action (IDM) for VLA data: aligned with scarce BEHAVIOR demos — but Tab. 18 is **video IF**, not BEHAVIOR SR.  
- Multi-view head→gripper synthesis (AgiBot): egocentric household robots with wrist cams / occlusions.  
- **Do not invent** BEHAVIOR-1K scores for Cosmos.

#### When to use generative video WFM vs latent LeWM/JEPA

| Need | Prefer | Defer |
|------|--------|-------|
| Novel visual generalization for BC/VLA | **Transfer2.5 Real2Real** (+ real teleop mix) | Training Cosmos from scratch |
| Fast stage-level planning | **LeWM / V-JEPA CEM** | DiT video inside MPC |
| Task/scene diversity in classic sim | **RoboGen**-style propose→verify | Pixel WFM as task author |
| Photoreal eval videos / human prefs | **Predict2.5** infer (**2B** first) | 14B until needed |
| AV multiview what-if | Transfer/Predict auto/multiview | Course smoke |
| Course proof | Sibling latent smokes + this pack **RC1/RC2/RC4** | PAI absolute / Tab. 13 |

#### Motion-planning / long-horizon / closed-loop hooks

1. **Generative eval harness:** rollout policies into Predict2.5/Transfer worlds; score visual success / VideoAlign / detectors before real BEHAVIOR trials.  
2. **Long-horizon Transfer:** RNDS-stable chunked video as backdrop for multi-step household scripts (still need a planner/policy).  
3. **AC Predict2.5:** action-conditioned imagination for short-horizon **what-if** (Bridge metrics); complementary to V-JEPA 2-AC latent CEM — richer but slower.  
4. **Hybrid (TENTATIVE research, not in-paper):** π₀.₅/FM executes motor chunks; **LeWM / V-JEPA 2-AC** does fast latent subgoal search; **Cosmos** supplies (a) synth demos, (b) photoreal eval, (c) optional slow generative imagination for rare events.  
5. Camera-controllable multiview: fill occlusions for long-horizon rearrange.

#### Synthetic data for π₀.₅ / FM stacks (careful)

Paper cites DreamGen / GR00T GR1 / Diffusion Policy — **not** π₀ by name. Legitimate Visionary rhyme: same **synth observation → policy** pattern generalizes to FM VLAs **if** IDM/action labels align — label **TENTATIVE transfer of idea**, not paper claim.

#### Visionary bets (weight 1)

1. **Dual-WM stack:** latent planner **plus** generative synth/eval — don’t pick one religion.  
2. **Treat Cosmos as data/eval appliance** in BEHAVIOR; treat LeWM/JEPA as **trainable** curriculum.  
3. **Mix card:** real teleop : Transfer-aug : RoboGen-sim — Empiricist leave-one-out later (other packs).  
4. **Non-goals:** course students won’t reproduce 4096-H100 pretrain; success = correct **tool choice** + honest probes.

#### Follow-up research agenda (Kanta)

1. BEHAVIOR visual OOD suite with Transfer2.5 prompts → measure VLA SR lift.  
2. Closed-loop: policy ↔ Transfer world-scenario / robot multiview at fixed Hz with distilled 4-step model.  
3. Hybrid generative-eval + latent-plan tournament (Cosmos judge, LeWM/2-AC propose).  
4. Physics-stress prompts as BEHAVIOR safety scenarios.  
5. Compare AC Cosmos rollouts vs V-JEPA 2-AC CEM on shared Bridge/DROID episodes (video metrics vs control success).

#### New apps (**beyond Discord/Drive**)

- Photoreal Sim2Real overlay for Isaac/BEHAVIOR scenes before robot time.  
- Multi-cam synthetic wrists for single-head demo expansion.  
- Policy regression testing in generated kitchens (visual unit tests).  
- AV-style world-scenario maps adapted to indoor semantic floorplans.

**Composition sketch (not a job):**  
`teleop/OXE demos` → optional **RoboGen** task expand → **Transfer2.5** appearance expand → train **DP/π₀.₅** → **Predict action-cond** for imagination eval → real deploy. Latent **LeWM/JEPA** optional for high-frequency planning where pixels are too slow.

**Visionary one-liner:** *For 2026 BEHAVIOR, use Cosmos WFM when you need **photoreal synthetic eyes**; use LeWM/JEPA when you need **fast trainable imagination** — Dec 7’s lesson is the fork, with Archaeologist richness and Empiricist infer-only humility.*

**Visionary exam bite:** "Cosmos = BEHAVIOR’s **generative synth/eval layer** beside π₀.₅ motors and JEPA latent planners; cite Tab. 13 **24/30** as Real2Real moral (one task); DreamGen **91.8** = video IF not VLA SR; hybrid dual-WM = **TENTATIVE proposal**; **no Discord/Drive**."

*(No Discord / Drive checklist content.)*

---

### 6.3 Empiricist — produced despite empty seat (RCs from DELIVERABLE)

> **Seat empty this session.** Produce **course smoke cards** anyway for Paper Manager continuity. **Tiny proxies only:** frozen API/ckpt probe, short clip generation, closed-loop stub. **Not** full WFM train. Bar = honesty vs LeWM pack (which *can* train tiny WMs) — Cosmos cannot. Ablation Bot **GAP**.

#### Claim under measurement

Frozen Predict2.5-**2B** generates short clips without crash; unified modes run; optional closed-loop stub finishes with finite loss — **directional**, **not** PAI **0.768** / Tab. 13 **24/30**.

#### Hypotheses (falsifiable at smoke scale)

1. **H-gate (#1):** Released Predict2.5-2B generates ≥1 short clip (≤93f or truncated) without OOM/crash on **1×24–80 GB** GPU; optional quality proxy non-degenerate.  
2. **H-modes (#2):** Same ckpt accepts T2W, I2W, V2W; I2W/V2W preserve first-frame identity better than T2W (qualitative).  
3. **H-inject (#3):** If action-cond code+ckpt: TimeEmbedding ≥ CrossAtten ≥ ChannelConcat on Bridge-lite **directional** (Tab. 20). Else: **cite-only**.  
4. **H-loop (#4):** Closed-loop stub finishes *K* steps with finite loss; open-loop MSE rises with *H*. **Not** Tab. 13 SR.  
5. **H-transfer (#5 Partial):** Edge/Blur Transfer infer respects control more than unconditional Predict.  
6. **H-foil (#12):** On tiny control proxy, LeWM/JEPA latent plan is **orders of magnitude** cheaper than DiT video roll — generative WFM reserved for synth/eval, not inner MPC (Archaeologist framing; **not** a paper claim).

**Falsifiers:** ckpt unloadable; modes missing; TimeEmb worse than ChannelConcat on matched Bridge-lite; stub NaN in <K steps; Transfer ignores edge; claiming train ablate / PAI absolute from smoke.

#### Prioritized matrix (paper-extract + DELIVERABLE)

| # | Experiment | Paper hook | Smoke | Pri |
|---|------------|------------|-------|-----|
| **RC1** | Frozen Predict2.5-2B shortclip | Tabs. 10–11 | **Y — PRIMARY** | **P0** |
| **RC2** | Modes T2W/I2W/V2W | §3.2; Tab. 1 | **Partial** | **P0** |
| **RC4** | Closed-loop stub | §6.2 / §6.6 | **Partial** | **P0** |
| **RC3** | Action inject Tab. 20 | Tab. 20 | Partial / cite | P1 |
| **RC5** | Transfer Edge/Blur infer | Tab. 12 | Partial | P1 |
| — | Pre/post PAI train; Transfer1 bake-off; Tab. 13; AV; DreamGen; 14B; 4096-H100 | — | **N** | Skip |

**Min package:** **RC0 → RC1 → RC2 → RC4** → optional RC3/RC5 → cite #6–#11 → conceptual foil #12.

#### Exact reproduce stack (EDIT)

```bash
# Official
# git clone https://github.com/nvidia-cosmos/cosmos-predict2.5
# optional: cosmos-transfer2.5
# Pin commit → logs/pins.txt ; NVIDIA Open Model License

# Course scout env (quiet outlines — preferred)
export COSMOS_WORK_ROOT=$SCRATCH/cosmos-wfm-smoke
cd /workspace/world-simulation-quiet
bash env_outline/setup_mamba.sh
# mamba activate cosmos-wfm-scout
mkdir -p logs runs checkpoints
# EDIT: download Predict2.5-2B (+ Reason1 / VAE) per upstream README
# Outlines only — do NOT sbatch from quiet box
# NEVER claim paper PAI 0.768 / Tab.13 24/30 / DreamGen VLA SR from smoke
# NEVER launch full WFM / Transfer / 14B / 4096-H100 train
```

#### Run cards (from DELIVERABLE)

##### RC0 — `env_smoke` (CPU/GPU gate)

| Field | Spec |
|-------|------|
| Goal | Clone predict2.5 (pin); mamba env; import + CUDA; list ckpts |
| Pass | Env builds; `logs/pins.txt`; GPU seen; **no** train |
| Budget | **<0.5 h** |
| Script | `env_outline/slurm_env_smoke.sh` |

##### RC1 — `ckpt_probe_shortclip` (**#1 — PRIMARY**)

| Field | Spec |
|-------|------|
| Paper | Tabs. 10–11 PAI; release 2B post-trained |
| Goal | Load **Predict2.5-2B**; generate **1–4** short clips (≤16–93f; downscale 480p/256p if needed) |
| Metrics | Crash-free; wall-time/clip; VRAM peak; optional CLIP/aesthetic; save mp4 + prompt json |
| Pass | ≥1 clip written; no NaN; VRAM documented — **not** required: PAI 0.768 |
| Budget | **~0.5–4 GPU-h**; VRAM **24–80 GB**; **14B = N** |
| Script | `env_outline/slurm_ckpt_probe_shortclip.sh` |

##### RC2 — `mode_t2w_i2w_v2w` (**#2 Partial**)

| Field | Spec |
|-------|------|
| Paper | §3.2 unified modes |
| Goal | Same seed/steps: T2W vs I2W (1 frame) vs V2W (2–8 cond frames) |
| Pass | All three paths run; I/V identity ≫ T2W to cond frame |
| Budget | **~1–6 GPU-h** |
| Script | `env_outline/slurm_mode_t2w_i2w_v2w.sh` |

##### RC3 — `action_inject_ablation` (**#3 Partial / cite**)

| Field | Spec |
|-------|------|
| Paper | Tab. 20 Bridge PSNR/SSIM/LatentL2/FVD |
| Goal | Reproduce TimeEmb > CrossAtten > ChannelConcat **or** document cite-only |
| Pass | Directional ranking **or** explicit “cite-only — no inject swap in release” |
| Budget | Cite **0**; lite FT **~2–12 GPU-h** hard cap |
| Script | `env_outline/slurm_action_inject_ablation.sh` |

##### RC4 — `closed_loop_stub` (**#4 Partial**)

| Field | Spec |
|-------|------|
| Paper | §6.2 policy+Transfer; §6.6 action-cond AR |
| Goal | `obs → (optional policy) → action → WM predict chunk → next obs` for *K*=5–20 |
| Env | Bridge-lite replay **or** toy arm; **no** real robot |
| Pass | Loop completes; log foil cost note vs LeWM/JEPA |
| Budget | **~1–8 GPU-h** |
| Script | `env_outline/slurm_closed_loop_stub.sh` |
| Cross-link | LeWM RC3/RC5 planning speed; V-JEPA robot CEM |

##### RC5 — `transfer_infer_edge_blur` (**#5 Partial**, optional)

| Field | Spec |
|-------|------|
| Paper | Tab. 12; Fig. 9–10 |
| Goal | Infer Transfer2.5 Edge vs Blur on 1–5 clips; log adherence proxies |
| Budget | **~1–6 GPU-h** |
| Script | `env_outline/slurm_transfer_infer_edge_blur.sh` |

#### Numbers to memorize (exam)

| Fact | Value |
|------|-------|
| PAI T2W / I2W post | **0.768** / **0.810** |
| Human 14B vs Wan2.1-14B | **48.6%** vs **31.8%** |
| Transfer shrink | **3.5×** (2B vs 7B) |
| Robot Tab. 13 | **24/30** vs **5/30** vs **1/30** |
| Bridge AC PSNR / FVD | **24.95** / **146** vs Predict1 **21.14** / **190** |
| Action inject Tab. 20 | TimeEmb **24.95** > Cross **24.41** > Concat **23.11** |
| DreamGen Object GPT | **91.8** (**IF only**) |
| AV LET-AP / FVD | **0.394** vs **0.243** · FVD **23** vs **63** |
| Distill | 4 steps; T2W **0.768→0.764** |
| MFU Tab. 9 | **4096 H100**; 2B **36.49%** / 14B **33.08%** |
| Scope | Infer-only course · **no** full train · Ablation Bot **GAP** |

#### Failure modes cheat-sheet

| Symptom | Likely cause | Next action |
|---------|--------------|-------------|
| OOM 2B | 720p × 93f × full steps | Drop res/frames/steps; enable offload |
| Reason1 missing | Aux ckpt not fetched | Fetch text encoder; pin path |
| Modes fail | Wrong cond packing | Match frame-replacement API |
| Tab. 20 unreproducible | Inject head not exposed | Mark cite-only |
| Loop NaN | AR latent drift | Teacher-force first frames; shorten *H* |
| Claiming train ablate | Seat empty + huge model | Relabel **N**; stick to infer smokes |
| Claimed PAI 0.768 / 24/30 from smoke | Overclaim | Report directional / crash-free only |

#### What not to invent / claim

- **Do not** claim course smoke = PAI **0.768** / Tab. 13 **24/30** / DreamGen VLA SR.  
- **Do not** invent aggregate GPU-hours beyond Tab. 9 MFU snapshot.  
- **Do not** launch full Progressive / RL / 14B / 7-cam AV / real-robot from course queue.  
- **Do not** present Cosmos as JEPA descendant or as drop-in CEM replacement for LeWM/2-AC.  
- **Do not** launder DreamGen IF into hardware VLA success.

**Empiricist exam bite:** "Quote **0.768/0.810**, **24/30**, **24.95** PSNR, **3.5×** Transfer, Tab. 20 TimeEmb; Empiricist RC1+RC2+RC4 = frozen **2B** infer + stubs — never claim paper absolutes / 4096-H100 train / DreamGen=VLA SR; Ablation Bot AUTHORITATIVE (Tab.20 P0)."

---

### 6.4 Stakeholder — produced despite empty seat

**Buy / why schedule Dec 7 after LeWM:**
- Completes the WM triangle: compact latent JEPA-WM (LeWM) → foundation-scale **generative video** WFM.  
- Direct Physical AI product relevance (NVIDIA open Cosmos stack) — robotics + AV demos in one paper.  
- Hard numbers: **24/30** robot, PAI vs Wan, Transfer **3.5×**, DreamGen GR1, Bridge AC.  
- Open weights → reproducible homework (generate / transfer / AC Bridge infer).

**Risks / costs / limits:**
- **44-page systems** paper — Archaeologist must curate; not a clean 8-page idea paper.  
- Compute inaccessible for from-scratch pretrain (**4096 H100** MFU); students consume checkpoints.  
- Easy category error: calling it “a JEPA” or “replaces V-JEPA 2-AC planning.”  
- Some “closed-loop” language is capability framing; primary quantified robot result is **offline aug**, not online MPC.  
- Author list / Cosmos brand can overshadow scientific claims — keep Empiricist numbers front.  
- Empiricist seat empty — RCs are proxies; Visionary=1 must not demand Tab. 13 lab.

**Decision ask:** **Required companion** after LeWM / V-JEPA for “generative simulator counterweight”; pair slides: LeWM (latent, fast, sim) | V-JEPA 2-AC (latent, real Franka) | **Cosmos (pixel WFM, synth+eval)**. Optional Dreamer as historical latent RL imagination bridge.

**Stakeholder exam bite:** "Buy Cosmos as the **generative-simulator counterweight** to LeWM — high syllabus value; police the JEPA-category error; students infer checkpoints, never pretend 4096-H100 course train."

---

### 6.5 Scientific Reviewer — produced despite empty seat

**Claims under review**
1. Flow-matching unified Predict2.5 + Reason1 yields a usable Physical AI video WFM at 2B/14B.  
2. Transfer2.5 at 2B beats Transfer1-7B on adherence/quality/long-horizon.  
3. Applications: Real2Real policy aug, AV multiview, DreamGen, Bridge AC policy-eval.  
4. Generative video WFM is a coherent **paradigm alternative** to latent WMs (§7).

**Strengths**
1. Coherent Physical AI WFM narrative with open release.  
2. Method upgrades concrete: FM, unified conditioning, Reason1, domain soup, RL, distillation.  
3. Broad eval: PAI-Bench, human prefs, Transfer metrics, robot hardware, AV detection, DreamGen, Bridge AC.  
4. Honest related-work split of latent vs pixel WM paradigms (cites V-JEPA 2 correctly as other pole).  
5. Tab. 20 clean within-family action-inject factorial.

**Weaknesses / ask-for-revision**
1. Systems-paper density; FM vs EDM without matched retrain table.  
2. Robot n=3 trials × 10 scenarios (30) — suggestive but small; single apple-bowl task.  
3. DreamGen table ≠ VLA success; risk of over-reading.  
4. “Closed-loop simulation” more Transfer stability + AC rollouts than full agent-in-loop SR curves.  
5. Heavy proprietary curation (3.1M driving clips) — data reproducibility limited even if models open.  
6. Comparison fairness: Wan sizes differ; human votes help but don’t replace matched compute.  
7. Ablation Bot GAP for course packs; multi-factor Predict1→2.5 leap.

**Verdict:** Strong **systems + applications** contribution for Physical AI video WFMs; scientifically place as **generative world simulator**, not as latent planner SOTA. Accept for Accompanying Models as **required counterweight** after LeWM; demand precise claims on synth-data vs online planning.

**Reviewer exam bite:** Demand Tabs. 10–13 + 19–20 + Fig. 7 before slogan; cite **24/30** not bare “robot works”; punish DreamGen-IF-as-VLA-SR and JEPA-as-parent; fence closed-loop language to Transfer/AC evidence.

---

## 7. Empiricist run pack (env / scripts / GPU — from DELIVERABLE)

### Env setup

```bash
export COSMOS_WORK_ROOT=$SCRATCH/cosmos-wfm-smoke
cd /workspace/world-simulation-quiet
bash env_outline/setup_mamba.sh
# mamba activate cosmos-wfm-scout
mkdir -p logs runs checkpoints
# EDIT: clone nvidia-cosmos/cosmos-predict2.5 (+ optional transfer2.5)
# Pin commit → logs/pins.txt
# Download Predict2.5-2B (+ Reason1 / VAE) per upstream README
# Outlines only — do NOT sbatch from quiet box
```

| File | Role |
|------|------|
| `env_outline/conda_env.yaml` | `cosmos-wfm-scout` sketch |
| `env_outline/setup_mamba.sh` | Create/update env |
| `env_outline/run_checklist.md` | RC0→RC5 order |
| `env_outline/README.md` | Cluster usage, Ablation Bot GAP, siblings |
| `env_outline/slurm_env_smoke.sh` | RC0 |
| `env_outline/slurm_ckpt_probe_shortclip.sh` | **#1 PRIMARY** |
| `env_outline/slurm_mode_t2w_i2w_v2w.sh` | **#2 Partial** |
| `env_outline/slurm_action_inject_ablation.sh` | **#3 Partial/cite** |
| `env_outline/slurm_closed_loop_stub.sh` | **#4 Partial** |
| `env_outline/slurm_transfer_infer_edge_blur.sh` | **#5 Partial** |

### GPU / RAM / TIME budget honesty

Assumptions: **infer / tiny stub** on Predict2.5-**2B** (or Transfer2.5-2B). Paper-scale video WFM train = **N**.

| Workload | GPU VRAM | Host RAM | Wall time (order) | Smoke |
|----------|----------|----------|-------------------|-------|
| RC0 env | — / 1 GPU import | 16–32 GB | **<0.5 h** | **Y** |
| RC1 shortclip 2B | **24–80 GB** | 64–128 GB | **0.5–4 h** | **Y PRIMARY** |
| RC2 modes ×3 | same | 64 GB | **1–6 h** | Partial |
| RC3 cite | — | — | **0** | cite |
| RC3 lite FT (only if forced) | **40–80 GB** | 128 GB | **2–12 h** hard cap | Partial |
| RC4 closed-loop stub | **24–80 GB** | 64 GB | **1–8 h** | Partial |
| RC5 Transfer infer | **24–80 GB** | 64 GB | **1–6 h** | Partial |
| Paper 2B/14B pretrain + SFT + RL | **4096×H100** class (Tab. 9) | cluster | weeks / **N** | **N** |
| Tab. 13 real robot | lab | — | **N** | **N** |
| Latent foil LeWM smoke (sibling) | **10–20 GB** | 32 GB | hours (other pack) | sibling |

**Course envelope:** **≤ ~8–20 GPU-h infer** for RC1+RC2+RC4 (+ optional RC5). Document aggressive downscale — don’t pretend 720p/93f on consumer GPU.

**Ruthless N list:** full Progressive Tab. 4, domain SFT 30k×256, RL, 14B any train, 7-cam AV, DreamGen 14B, Transfer1 bake-off train, real-robot 24/30, 4096-H100 pretrain.

### Success criteria vs paper claims

| Paper claim | Minimal course criterion | Not required |
|-------------|--------------------------|--------------|
| Predict2.5 ships usable WFM | RC1: shortclip OK on 2B | Match PAI Overall ~0.77 |
| Unified T2W/I2W/V2W | RC2: three paths run | Identity metrics |
| Reason1 text grounding | Prompt follow qualitative | Human/VQA stub |
| Transfer 3.5× smaller + better | Cite Tab. 12 / Fig. 10 | Infer Edge/Blur RC5 |
| Robot synth aug helps | Cite Tab. 13 **24/30** | Lab replication |
| AV multiview quality | Cite Tab. 14–15 | — |
| Action-cond > Predict1 | Cite Tab. 19; RC3 Tab. 20 | Lite Bridge PSNR |
| DreamGen VLA synth | Cite Tab. 18 (**IF**) | Hardware VLA SR |
| Pixel WFM vs latent WM | Archaeologist §7 + foil note | Bake-off |
| Distill retains quality | Cite Tab. 7 **0.764≈0.768** | — |
| Paper-scale train | **Document N** | Never course |

**Pass:** RC0+RC1 outlines runnable when cluster+ckpt available; RC2+RC4 Partial; rich Archaeologist §6.1; Visionary §6.2; no jobs launched in quiet.  
**Fail / overclaim:** "reproduced Cosmos / matched PAI 0.768 / verified 24/30 / DreamGen VLA win / JEPA child" from smoke alone.

---

## 8. Slide Maker brief

| Role | Weight / timing guide | Focus |
|------|----------------------|-------|
| **Archaeologist** | **3 · LONGEST · ~16–18** | CLEAR/TENTATIVE; lineage fork Ha/Dreamer/JEPA/LeWM vs Cosmos; family evolution; paradigm foil card; Ablation Bot GAP; exam bite |
| **Visionary** | **1 · seated Kanta · ~14–15** | BEHAVIOR dual-WM; when WFM vs latent; closed-loop/photoreal hooks; hybrid=TENTATIVE; **no Discord/Drive** |
| Empiricist | empty seat · ~12–14 | H-gate/modes/loop; RC1+RC2+RC4; GPU honesty; must-cite tattoo; exam bite |
| Stakeholder | empty seat · ~12–14 | Buy generative counterweight; risks (44pp, compute, category error); decision ask |
| Reviewer | empty seat · ~12–14 | Tabs.10–13+19–20; verdict Accept systems-WFM; fence closed-loop / DreamGen IF |

**Spine (paste into Slide Maker) — 5 bullets:**
1. **Generative video WFM as Physical AI world simulator** — Cosmos-Predict2.5 flow T2W/I2W/V2W + Reason1; Transfer2.5 Sim2Real/Real2Real — **PARALLEL** to latent Dreamer/JEPA/LeWM, not their child.  
2. **Must-cite:** PAI **0.768 / 0.810**; Transfer **3.5×**; robot **24/30 vs 5/30**; Bridge PSNR **24.95**; Tab. 20 TimeEmb; DreamGen **91.8** (IF only); AV LET-AP **0.394**; Tab. 9 **4096 H100** MFU — course train **N**.  
3. **Taxonomy fork:** Ha/Dreamer + I-JEPA→V-JEPA→LeWM (**latent plan**) \| **Cosmos Predict/Transfer** (**pixel synth/sim**) \| RoboGen (task curriculum).  
4. **Lineage (Archaeologist=3 Nykolas LONGEST):** Predict1→2.5 + Transfer1→2.5 family evolution; inventum = flow+Reason1+unified modes+apps; always tag latent-plan vs pixel-sim.  
5. **BEHAVIOR’26 + honesty (Visionary=1 Kanta):** Cosmos = photoreal synth/eval appliance; LeWM/JEPA = fast trainable imagination; Empiricist RC1–RC4 infer-only; **do not invent** dual-WM hybrid / DreamGen=VLA-SR as in-paper — **no Discord/Drive**.

**GitHub title sketch:** `[ECE 605] - Cosmos World Simulation Archaeologist Making Three`

Full Slide Maker brief: `/workspace/world-simulation-quiet/SLIDE_BRIEF.md`

---

## Paths

| Artifact | Path |
|----------|------|
| This pack | `/workspace/world-simulation-quiet/KANTA_PACK.md` |
| PDF | `/workspace/world-simulation-quiet/KANTA_PACK.pdf` |
| Slide brief | `/workspace/world-simulation-quiet/SLIDE_BRIEF.md` |
| Senior Research | `/workspace/world-simulation-quiet/DELIVERABLE.md` |
| Summarizer | `/workspace/papers/world-simulation-2511.00062-summary.md` |
| Ablations | **AUTHORITATIVE** — `/workspace/papers/world-simulation-2511.00062-ablations.md` (P0: Tab.20 → Tab.17 → Tab.12) |
| Paper notes | `/workspace/world-simulation-quiet/paper_notes.md` |
| Env outlines | `/workspace/world-simulation-quiet/env_outline/` |
| Paper PDF | `/workspace/papers/world-simulation-2511.00062.pdf` |
| Text extract | `/workspace/papers/world-simulation-2511.00062.txt` · `paper_clean.txt` |
| Page figs | `/workspace/papers/cosmos-figs/page-*.png` (if present) |
| Lineage siblings | `/workspace/leworldmodel-quiet/` · `/workspace/vjepa21-quiet/` · `/workspace/ijepa-quiet/` · `/workspace/robogen-quiet/` |
| Code | https://github.com/nvidia-cosmos/cosmos-predict2.5 · https://github.com/nvidia-cosmos/cosmos-transfer2.5 |

---

*Pack synthesized 2026-09-29 PT (HST) for KanBot / ECE 605 — Dec 7 Accompanying Models · Archaeologist=3 (Nykolas) LONGEST · Visionary=1 (Kanta) seated · Empiricist/Stakeholder/Reviewer empty but all five produced · Ablation Bot GAP (paper-extract) · generative video WFM PARALLEL to latent JEPA/LeWM · infer-only course smokes · no Discord/Drive · no invented GPU-hours / DreamGen=VLA-SR / BEHAVIOR hybrid as in-paper.*
