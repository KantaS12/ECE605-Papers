# Paper Manager pack — ForceVLA (Undated LAST · Alternative approaches)

**Paper:** ForceVLA: Enhancing VLA Models with a Force-aware MoE for Contact-rich Manipulation  
**arXiv:** https://arxiv.org/abs/2505.22159 · [project](https://sites.google.com/view/forcevla2025) · [code](https://github.com/ft-robotic/ForceVLA) · arXiv **2505.22159v3** (18 Sep 2025) · Preprint under review  
**Authors:** Jiawen Yu¹\*, Hairuo Liu²͵³\*, Qiaojun Yu⁴͵²†, Jieji Ren², Ce Hao⁵, Haitong Ding⁶, Guangyu Huang⁷, Guofan Huang¹, Yan Song¹, Panpan Cai²͵³, Wenqiang Zhang¹, Cewu Lu²͵³͵⁸ — Fudan / SJTU / Shanghai Innovation Institute / Shanghai AI Lab / NUS / Shanghai University / Xi’an Jiaotong / Noematrix  
**PDF:** `/workspace/papers/forcevla-2505.22159.pdf` (= `/workspace/forcevla-quiet/paper.pdf`) · **20 pp** · ~10.3 MB  
**Session:** Undated · **LAST paper on sheet** · Type: **Alternative approaches**  
**Role weights:** **ALL EMPTY** — **still all five produced at balanced depth** (~equal Archaeologist / Stakeholder / Scientific Reviewer / Empiricist / Visionary); **Club Pack** (no LONGEST seat; no weight-3 over-index)  
**Kanta role:** Produce **balanced** five-role Club Pack; deepen late 6-axis F/T + FVLMoE spine + Empiricist Ablation Bot smokes; Visionary → **BEHAVIOR contact-rich**; **skip Discord/Drive**  
**Themes:** late **6-axis F/T** token · **FVLMoE** (E=4, top-1) · augments **π₀ FM** head · contact-rich insert/wipe/peel · fusion quality ≫ naive concat · **NOT** FAST switch · contrast **MoPA-PD** (train privilege) vs **deploy-side force** · ForceVLA-Data · **no π₀.₅ bake-off**  
**Sources:** Summarizer (`/workspace/papers/forcevla-2505.22159-summary.md`) + Senior Research (`/workspace/forcevla-quiet/DELIVERABLE.md`) + Ablation Bot (`/workspace/papers/forcevla-2505.22159-ablations.md` — **AUTHORITATIVE**) + `paper_notes.md` / `env_outline/` + figs `/workspace/papers/forcevla-figs/`  
**Sibling links:** `/workspace/pi0-quiet/` · `/workspace/pi05-quiet/` · `/workspace/fast-quiet/` · `/workspace/openvla-quiet/` · `/workspace/openvla-oft-quiet/` · `/workspace/diffusion-policy-quiet/` · `/workspace/act-aloha-quiet/` · `/workspace/umi-quiet/` · `/workspace/droid-quiet/` · `/workspace/distill-mopa-quiet/` · `/workspace/dart-quiet/`

---

## 1. One-liner

**Late 6-axis F/T + FVLMoE (4 experts, top-1) augments a π₀ flow-matching action head for contact-rich manipulation** — fusion quality ≫ naive force concat; ForceVLA keeps the **FM** head (**NOT** a FAST switch); complementary Alt to **MoPA-PD** (train-time privileged MP → vision student) by adding **deploy-side force** to an already-VLA stack.

**Contrast poles (consistency triangle — CLEAR labels; do not invent ancestry):**

| Pole | What they do | Relation to ForceVLA |
|------|--------------|----------------------|
| **π₀** (Black et al. 2410.24164) | PaliGemma + FM action expert | **CLEAR parent / foil:** ForceVLA builds “upon the π₀ framework”; baselines = π₀-base / π₀-fast ± force |
| **π₀.₅** (2504.16054) | Open-world VLA | **CLEAR related cite only** — **no empirical bake-off**; **do not invent** |
| **π₀-FAST** (Pertsch et al.) | Discrete action tokenization | **CLEAR baseline family**; ForceVLA itself **keeps FM**; naive F **hurts** fast (31.0→14.2) |
| **MoPA-PD** (2111.06383) | Privileged classical MP at **train** → distill to vision | **CLEAR contrast (prior Alt row):** train privilege removed at deploy vs **force kept at deploy** |
| OpenVLA / RT-2 / Octo | VLA lineage | Cited; concrete foil for numbers is **π₀**, not OpenVLA tables |
| TLA / Tac-Man / ForceMimic / FoAR | Contact / tactile / force neighbors | Related contact literature; ForceVLA differentiator = **VLA-scale MoE + π₀ FM** |
| JEPA / LeWM / Cosmos WM | Latent / generative world models | **CLEAR contrast poles only** — ForceVLA is **not** a WM paper |
| Classical impedance / admittance | Hand-tuned compliance | Background; ForceVLA learns force-conditioned chunks end-to-end |

**Do NOT invent / claim:** π₀.₅ bake-off numbers; ForceVLA switches FM→FAST; MoPA-PD / JEPA ancestry; course smoke = Fig.5 **60.5%** absolute; stub wrench = ForceVLA-Data reproduction; Discord/Drive checklist; official BEHAVIOR baseline status.

---

## 2. Summary (exec overview + key numbers)

**Yu\* & Liu\* et al. (arXiv 2505.22159v3)** attack **contact-rich** failure modes of vision-language-action models: insertion, wipe, peel, occlusion. Vision-only VLAs inherit semantic priors but underuse **interaction force**. **ForceVLA** keeps the **π₀** stack (SigLIP/PaliGemma VLM + **conditional flow matching** action expert + action chunks) and adds **6-axis external wrench** as a **late** multimodal channel via **FVLMoE**:

1. **Late force token:** After VLM encodes vision+language, project \(f\in\mathbb{R}^6\) → force token \(E_F\); concat with VL embeddings.  
2. **Sparse MoE fusion:** Transformer encoder → **E=4 MLP experts, top-k=1** gating; residual; project to action-expert width.  
3. **Additive FM guidance:** Fused tokens **added** into the flow-matching action-expert suffix during denoising — contact-aware every chunk step.

**Why Alternative approaches / LAST sheet row:** After MoPA-PD’s “privileged MP → vision student,” ForceVLA answers the complementary question: **when the deploy stack is already a VLA, what missing modality unlocks contact-rich success?** Answer: **real-time 6-axis F/T + late MoE fusion**, measured against vision-only π₀.

| Axis | Number / claim |
|------|----------------|
| Venue / code | Preprint under review; arXiv **2505.22159v3**; project + github.com/ft-robotic/ForceVLA **CLEAR** external |
| Backbone | **π₀-base** + FVLMoE (**CLEAR**); FM head retained |
| Force | Estimated external wrench TCP/world, \(f\in\mathbb{R}^6\) (Limitation: not always high-fid direct) |
| Robot / data | Flexiv Rizon + Dahuan + RealSense pair; Quest3 teleop; **ForceVLA-Data 244 traj / ~140k steps**; ~**50**/task |
| Tasks (5) | USB insert, plug insert, bottle pump, wipe board, peel cucumber |
| Eval | Insert/pump **20**; wipe **10**; peel **15**×15 strokes |
| **ForceVLA avg** (Fig.5) | **60.5%** |
| π₀-base w/o F / w/ F | **37.3%** / **40.2%** (+2.9 naive) |
| π₀-fast w/o F / w/ F | **31.0%** / **14.2%** (force **hurts**) |
| Δ vs vision-only | **+23.2** pp (60.5 − 37.3) |
| Plug (abstract / Fig.5) | ForceVLA **80%** |
| Tab.3 ablation | baseline **45** / linear-before **55** / MoE-before **0** / concat-after **60** / ForceVLA **80** |
| Occlusion (Tab.2) | ForceVLA **90%** |
| Peel (Tab.1) | **14.12** cm / **7** strokes |
| Multi-task (Tab.5) | ForceVLA avg **67.5%**; plug **100**; USB **10%**; π₀-fast **0%** |
| Train | Single ~**9 h / 1×4090 / 10k**; multi ~**12 h / 2×4090 / 30k** |
| Scope | Real Flexiv only; estimated wrench; **no π₀.₅** comparison |

**Must-cite tattoo:** avg **60.5** vs **37.3 / 40.2**; Tab.3 early MoE **0% → ForceVLA 80%**; occlusion **90%**; USB multi-task **10%**; ForceVLA-Data **244 traj / ~140k**; **no π₀.₅ bake-off**; **refuse** FM↔FAST laundering / stub-wrench = paper 60.5%.

**Do NOT claim:** course smoke = Fig.5 60.5% absolute; Tab.3 80% without matched fusion; π₀.₅ wins/losses; ForceVLA is a FAST policy; MoPA-PD ancestry; pump bottle as FVLMoE proof (Fig.5 base w/F **83** > ForceVLA **67**).

---

## 3. Keywords

ForceVLA; FVLMoE; force-aware Mixture-of-Experts; 6-axis F/T; late fusion; π₀; flow matching; PaliGemma; SigLIP; contact-rich manipulation; ForceVLA-Data; plug / USB insertion; occlusion; naive force concat; π₀-fast foil; Alternative approaches; BEHAVIOR contact-rich (TENTATIVE recipe)

---

## 4. Deep dive — Late force MoE on π₀ (**PRIMARY SESSION CONTRIBUTION**)

**Callout:** Scientific product of *this* PDF = a **modality-augmentation recipe** that makes calibrated 6-axis wrench a first-class late token inside a π₀-class VLA. Session type = **Alternative approaches** because the LAST Alt row must teach **deploy-side force fusion** as the complement to MoPA-PD’s **train-time classical privilege** — **without** claiming ForceVLA founded π₀.₅ or switched to FAST.

### Pipeline (Fig. 3 moral)

```
SigLIP + PaliGemma VLM (vision + language → VL embeddings)
        │  force enters AFTER VLM (not early)
        ▼
Linear φ_F: R^6 → 2048  ──► force token E_F
        │  concat [E_VL; E_F]
        ▼
FVLMoE: MHSA encoder → sparse MoE (E=4, top-k=1) → residual → project 2048→1024
        │  G_FVLMoE
        ▼
Additive inject into π₀ FM action-expert suffix (proprio + noisy a^τ)
        │
        ▼
Contact-aware action chunks ──► Flexiv real policy (+ ForceVLA-Data sync demos)
```

### Components privilege / deploy table

| Component | Role | Deploy? |
|-----------|------|---------|
| PaliGemma / SigLIP VLM | Vision + language contextual embeddings | Yes (backbone) |
| Force linear proj φ_F | 6-axis F/T → force token (2048-d) | Yes (needs F/T stream) |
| FVLMoE pre-encoder | Joint self-attn over VL + force | Yes |
| MoE experts E=4 + top-1 router | Phase / modality sparse fusion | Yes |
| Residual + out proj → 1024 | Match action expert width | Yes |
| Additive G_FVLMoE → FM suffix | Contact-aware guidance per chunk step | Yes |
| Flow-matching action expert (π₀) | Continuous action chunks from noise | Yes — **not replaced by FAST** |
| ForceVLA-Data + Quest3 pipeline | Sync multimodal demos | Train |
| Estimated external wrench | F/T source (limitation) | Sense |

### What is “alternative” here — CLEAR

| Claim | Status |
|-------|--------|
| ForceVLA builds on **π₀** FM + PaliGemma | **CLEAR** |
| Experimental foil = π₀-base / π₀-fast ± force | **CLEAR** |
| Empirical comparison to **π₀.₅** | **ABSENT — do not invent** |
| ForceVLA action head = **flow matching** (not FAST) | **CLEAR** |
| π₀-fast used as **baseline**; naive force hurts it | **CLEAR** |
| Late force fusion ≫ early MoE-into-VLM (Tab.3 **0→80**) | **CLEAR** |
| +23.2 pp avg vs π₀-base w/o F; 60.5 vs 37.3 | **CLEAR** |
| Plug insertion up to **80%** | **CLEAR** |
| MoPA-PD ancestry / MP distill | **FALSE — contrast only** |
| JEPA / Cosmos ancestry | **FALSE — contrast poles** |
| ForceVLA-Data 244 traj / 140k steps | **CLEAR** |
| Estimated wrench limitation | **CLEAR** |
| BEHAVIOR official baseline | **TENTATIVE / unverified** |
| Expert-0 = generalist functional role | **TENTATIVE** (App. C hypothesis) |

### Architecture checklist (Empiricist pin — App. B / Tab.4)

| Piece | Spec (paper) |
|-------|----------------|
| Force projection | Linear 6 → \(D_{\mathrm{VLM}}=2048\) |
| State / action proj | 32 → \(D_{\mathrm{act\_e}}=1024\) |
| Pre-MoE encoder | \(D_{\mathrm{model}}=2048\), \(N_H=8\), \(D_h=256\); MLP expansion 1 |
| MoE | **4 experts, top-1**; router 2048 → 4 |
| MoE out proj | 2048 → 1024 |
| Action out | 1024 → 32 |
| Optimizer | Adam β (0.9, 0.95); LR \(2.5\times10^{-5}\to2.5\times10^{-6}\); bf16; grad clip 1.0 |
| Single-task | 1×4090, **10k** steps, ~**9 h** |
| Multi-task | 2×4090, **30k** steps, ~**12 h**; global batch 16, effective 2048 |

**Exam bite:** "ForceVLA = π₀ + late 6D force token + FVLMoE (4 experts, top-1) additively guiding FM chunks; +23.2 pts avg vs vision-only π₀-base (**60.5 vs 37.3**); early MoE-into-VLM → **0%**; naive concat only **40.2%** / Tab.3 **60%**; **NOT** a FAST switch; **no π₀.₅ bake-off**; contrast MoPA-PD = train privilege vs deploy-side force."

---

## 5. Deep dive — Ablations (Ablation Bot AUTHORITATIVE)

**Callout:** Ablation Bot extract **AUTHORITATIVE** at `/workspace/papers/forcevla-2505.22159-ablations.md` (folded after DELIVERABLE’s PDF-derived ranks). Focus order: **FVLMoE > force vs vision > π₀ foil > insertion SR**. Course Empiricist: **Y #1/#2/#4** (fusion + plug + avg bakeoff) → Partial **#3/#5** → cite **#6/#7**; **N** paper-scale 8×4090 + full Flexiv 5-task.

| Rank | Factor (Ablation Bot) | Paper numbers | Course smoke |
|------|----------------------|---------------|--------------|
| **1** | Fusion-stage ablation (Tab.3) | baseline **45**; linear-before **55**; MoE-before **0**; concat-after **60**; ForceVLA **80** | **Y — RC1 PRIMARY** |
| **2** | Insert Plug (Fig.5) | fast 25/30; base 45/60; **ForceVLA 80** | **Y — RC2** (plug column) |
| **3** | Insert USB (Fig.5) | fast 0/0; base 5/5; **ForceVLA 25** — naive F null | Partial / cite |
| **4** | Fig.5 averages + force-fusion | ForceVLA **60.5**; base **37.3→40.2**; fast **31.0→14.2**; Δ **+23.2** | **Y — cite / RC package** |
| **5** | Tab.2 Occlusion / Obj Gen.2 | Occl. ForceVLA **90** (base w/F **30**); Obj Gen.2 **40** | **Partial — RC5** |
| **6** | Tab.5 multi-task joint | ForceVLA **67.5**; base w/F **42.5**; fast **0**; USB **10** | Partial / cite — RC7 |
| **7** | Pump Bottle reversal (Fig.5) | base w/F **83** > ForceVLA **67** — **falsifier, not confirmation** | cite caveat |
| **Defer** | Paper-scale 8×4090 + full Flexiv 5-task; true high-fid FT | — | **N** |

**Honorable (off top-7 focus):** Tab.1 peel length/strokes; Fig.5 wipe (π₀-fast w/o F wins Board-1); Fig.9 router (no SR contrast).

**Min Empiricist package:** RC0 ingest → **RC1 (#1 Tab.3 fusion) + RC2 (#2 Insert Plug) + RC3 (USB null-force)** → Partial RC5/RC6 → cite #3/#4/#6/#7 (Pump falsifier). Log **SR, peel length/strokes, wrench stats, force_fusion, force_source, expert_id@top1, peak VRAM**.

**Recipe exam bite:** "Late FVLMoE is the real knife (Tab.3 **0→80**; concat **60→80**); naive force is a straw improvement (+2.9 avg) and can **hurt** (fast 31→14.2; occlusion base 60→30); pump **83>67** is the honesty caveat; USB stays hard (multi-task **10%**)."

---

## 6. Role-ready sections (all five — **BALANCED Club Pack**)

> **Balanced pack.** Equal depth across five roles. No seat weight-3. Ablation Bot focus areas (FVLMoE fusion, plug/USB insertion, force vs vision, π₀ foil) shared across Empiricist RCs + Reviewer/Archaeologist honesty.

### 6.1 Archaeologist — balanced (~12–14)

> **Seat charge:** Place ForceVLA carefully in VLA ↔ contact-force history. **CLEAR parent / foil = π₀**. Fence false π₀.₅ bake-off and FM↔FAST switch. Equal depth with other four.

#### Priors — CLEAR

| Prior | Role | Label |
|-------|------|-------|
| **π₀** Black et al. 2410.24164 | Direct architectural parent (PaliGemma + FM) | **CLEAR** |
| **PaliGemma** Beyer et al.; **SigLIP** Zhai et al. | VLM backbone | **CLEAR** |
| **Flow matching / rectified flow** Lipman; Liu | Action expert family | **CLEAR** |
| **FAST** Pertsch et al. | Baseline tokenization foil (π₀-fast) | **CLEAR** |
| OpenVLA / RT-1 / RT-2 / Octo / OXE / DROID | VLA + data lineage | **CLEAR** related |
| Force/tactile: ForceMimic, FoAR, TacDiffusion, TLA, Tac-Man | Contact neighbors | **CLEAR** related |
| MoE: Shazeer, Switch, GShard, V-MoE, LIMOE; MORE / ChatVLA | MoE priors | **CLEAR** related |
| π₀.₅ | Related VLA cite [21] | **CLEAR related; ABSENT empirically** |
| MoPA-PD | Prior Alt row — opposite mechanism | **CLEAR contrast** |

#### Genealogy (CLEAR framing — no false FM lineage)

```
PaliGemma / SigLIP VLM
   + π₀ flow-matching action expert (vision+language+proprio)
        → ForceVLA: append 6-axis force token AFTER VLM
             → FVLMoE (attn + 4 experts, top-1)
                  → additive guidance into FM suffix
                       → contact-rich real Flexiv policy + ForceVLA-Data
```

#### Subsequent / contrast — label carefully

| Item | Note | Label |
|------|------|-------|
| π₀.₅ / open-world VLA | Sibling literature; **not measured** | **ABSENT bake-off — do not invent** |
| MoPA-PD | Privileged MP **train** → vision student | **CLEAR contrast** (inverse privilege arrow) |
| ACT / Diffusion Policy | Teleop visuomotor IL | **CLEAR contrast** |
| JEPA / LeWM / Cosmos | Latent / generative WM | **CLEAR contrast poles only** |
| Soft influence on “forceful foundation models” | Pattern family; verify per paper | **TENTATIVE** |

**Archaeologist one-liner:** *Force joins the VLA token zoo **after** the pretrained VLM — FVLMoE is the association cortex; π₀’s FM head stays the motor cortex; MoPA-PD removed privilege at deploy, ForceVLA **adds** force privilege at deploy.*

**Archaeologist exam bite:** "ForceVLA = π₀ + late 6D force + FVLMoE (E=4,k=1) → FM chunks; **60.5 vs 37.3 / +23.2**; Tab.3 early MoE **0%**; **NOT** FAST switch; **no π₀.₅ bake-off**; CLEAR contrast MoPA-PD."

---

### 6.2 Stakeholder — balanced (~12–14)

> **Seat charge:** Why schedule as LAST Alternative approaches; buy the **late force-MoE pattern**, not a claim that vision-only homes are solved.

**Buy / why schedule LAST Alt row:**
- Closes Alt column with the **deploy-side** answer to contact-rich failures vision-only π₀ stacks still show.  
- After MoPA-PD’s train-time classical privilege story: “What do we buy for **real F/T robots** already running OpenPI/π₀?”  
- HIGH force/contact flag without a new foundation pretrain from scratch.  
- Hard numbers: **60.5 vs 37.3/40.2**, Tab.3 **0→80**, occlusion **90**, ForceVLA-Data **244/~140k**.

**Buy / build vs modern stacks**

| Situation | Recommendation |
|-----------|----------------|
| Robot has reliable **6-axis F/T**; tasks are insert / wipe / press / peel-like | **Buy ForceVLA pattern:** late force token + MoE fusion on π₀-class FM head; collect sync F/T demos |
| Vision-only mobile manip in open homes (π₀.₅ narrative) | Keep π₀.₅-class for open-world language; **add ForceVLA-style fusion only where F/T exists** — don’t pretend paper beat π₀.₅ |
| No F/T hardware | Pattern blocked; estimated wrench / tactile skins are research paths (paper limitation) |
| Tempted to “just concat force to state” | Paper shows **+2.9 pts** only — budget MoE / late-fusion engineering |
| FAST-only compact policies | Naive force **hurt** π₀-fast (31→14.2); don’t bolt F/T without capacity / pretrain story |

**Risks / costs:**
- Needs ForceVLA-Data-like sync demos (~50/task) — not free from OXE alone.  
- Integrated F/T cost / calibration; paper uses **estimated** wrench.  
- USB insert remains hard (multi-task USB **10%**); Unstable Socket weak (**20%**).  
- Code/data “will be released” — verify github.com/ft-robotic/ForceVLA + sites.google.com/view/forcevla2025 freshness.  
- Don’t pitch as replacing π₀.₅ open-world — pitch as **contact-rich specialist augmentation**.  
- Pump bottle Fig.5 reversal (base w/F **83** > ForceVLA **67**) — don’t oversell “force MoE always wins.”

**Decision ask:** **Required LAST Alternative-approaches classic** for force-in-VLA; pair with MoPA-PD (planner-distill) and DART (recovery) as **complementary Alt cards**, not ancestors. Buy the **late MoE fusion pattern**.

**Stakeholder exam bite:** "Schedule ForceVLA LAST Alt to lock **deploy-side force-in-VLA** after MoPA-PD’s train privilege story; buy late FVLMoE, not naive concat; police π₀.₅ bake-off invention and FM↔FAST slogans."

---

### 6.3 Scientific Reviewer — balanced (~12–14)

**Claims under review**
1. Treating 6-axis force as first-class + FVLMoE improves contact-rich VLA success vs π₀ baselines.  
2. Late fusion ≫ early fusion into pretrained VLM.  
3. Naive force concat is insufficient vs MoE fusion.  
4. Generalizes under object/height/occlusion/unstable perturbations.  
5. MoE router learns phase-aware specialization.

**What holds**
- Main avg gap is large and consistent: **60.5 vs 37.3 / 40.2** with clear baseline matrix including π₀-fast.  
- Table 3 ablation is a **causal knife**: MoE-before-VLM **0%** vs ForceVLA **80%**; late concat **60%** vs FVLMoE **80%**.  
- Occlusion **90%** is a strong multimodal story (naive force on base **hurts** occlusion 60→30).  
- Cucumber Tab.1 supports continuous contact control, not only binary insert.  
- Honest limitations: estimated wrench; expensive F/T platforms; accessibility future work.  
- Related-work correctly separates VLA / contact-rich / MoE threads; π₀-base chosen over fast for foundation.

**What is soft / threatened**
- Tab.3 task identity / trial N underspecified (Ablation Bot: numeric match to plug column, not stated).  
- Multi-task USB **10%** shows force MoE is not a universal precision-insert fix.  
- Unstable Socket: ForceVLA **not** best (20 vs 30 π₀-fast w/ F).  
- Pump Bottle Fig.5: naive base w/F **beats** ForceVLA (**83 vs 67**) — claim scope matters.  
- Wipe Board-1: π₀-fast w/o F **91** beats ForceVLA **64**.  
- Router “Expert 0 = generalist” is **interpretive**.  
- No π₀.₅ / OpenVLA-OFT / other 2025 VLA heads in the bake-off.  
- Small demo budget (~50/task); single embodiment (Flexiv); no CIs / seeds.

**Verdict:** Strong **systems/methods** preprint for **force-augmented VLA fusion**. Accept late-fusion + MoE-over-concat claims on the reported Flexiv suite. **Do not** upgrade to “solved contact-rich foundation model” or “beats π₀.₅.”

**Reviewer exam bite:** Late FVLMoE is the real contribution; naive force is a straw improvement that can hurt; grade HIGH for Alt contact-rich syllabus, narrow on embodiment / open-world / USB / pump caveat — demand `force_fusion` logged, not only “force on.”

---

### 6.4 Empiricist — balanced (~12–14; Ablation Bot AUTHORITATIVE)

> **Seat empty this session** but produce full course smoke cards for Paper Manager continuity. **Balanced** — equal depth with other roles; richest logging on RC1/RC2/RC3. Ablation Bot ranks **1–7** govern order.

#### Claim under measurement

ForceVLA / FVLMoE ≫ π₀-base w/o F and ≥ naive w/ F on **1** contact-rich task at matched FT budget; late fusion ≥ early; early MoE does not win (cite **0%**); metrics harness logs SR + wrench; ForceVLA-Data schema ingestable — **directional**, **not** Fig.5 **60.5%** absolute / Tab.3 **80%** without matched settings.

#### Hypotheses (falsifiable; Ablation Bot ranks)

1. **H-fusion (#1):** Late FVLMoE ≥ late concat ≫ early MoE; early MoE does **not** win (Tab.3 **0%** moral).  
2. **H-plug (#2):** On plug-like insert, FVLMoE ≥ naive w/F ≥ w/o F (Fig.5 plug **80/60/45** moral).  
3. **H-usb (#3 Partial):** Naive force on/off may be **null** on USB; FVLMoE still the only gain (Fig.5 USB **25 vs 5/0**).  
4. **H-avg (#4):** Matched-budget: FVLMoE ≥ concat ≥ none on SR **or** contact metrics (avg moral **60.5 / 40.2 / 37.3**).  
5. **H-occl (#5 Partial):** Under visual occlusion, force-aware FVLMoE retains higher SR (Tab.2 **90%** moral); naive force may **hurt**.  
6. **H-multitask (#6):** Joint training: FVLMoE ≫ π₀-fast (**0%**) and ≫ base w/o F (**5%**); USB stays hard (**10%**).  
7. **H-pump (#7 caveat):** Do **not** require FVLMoE > naive on pump (Fig.5 **67 vs 83**).

**Falsifiers:** naive concat ≥ FVLMoE on fusion task; early MoE ≥ late; force hurts insertion under FVLMoE; wrench-free demos match ForceVLA-Data; occlusion collapses FVLMoE to vision-only; claiming Fig.5 60.5% from 1-task stub; treating pump win as required.

#### Prioritized matrix

| # | Experiment | Ablation Bot | Smoke | Pri |
|---|------------|--------------|-------|-----|
| **RC0** | `forcevla_data_ingest_smoke` | Data §4.3 | **Y** gate | P0 |
| **RC1** | `fvlmoe_vs_pi0_bakeoff` | **#4** (+#2 plug) | **Y — PRIMARY** | **P0** |
| **RC2** | `fusion_stage_ablation` | **#1** | **Y — PRIMARY** | **P0** |
| **RC3** | `insertion_metrics_harness` | **#2** metrics | **Y — PRIMARY** | **P0** |
| **RC4** | `forcevla_data_scale_smoke` | Data scale | **Y** / Partial if no data | P1 |
| **RC5** | `generalization_occlusion` | **#5** | **Partial** | P1 |
| **RC6** | `moe_router_log` | Fig.9 | **Partial** | P1 |
| **RC7** | `multitask_joint_stub` | **#6** | Partial / cite | P2 |
| — | Paper-scale 8×4090 + full Flexiv; true high-fid FT | — | **N** | Skip |

**Min package:** RC0 → **RC1 + RC2 + RC3** → Partial RC5/RC6 → cite #3/#6/#7.

#### Exact reproduce stack (EDIT)

```bash
# Official / stand-in
# pin https://github.com/ft-robotic/ForceVLA (+ OpenPI π₀) when public; else force-token stub
export FORCEVLA_WORK_ROOT=$SCRATCH/forcevla-smoke
cd /workspace/forcevla-quiet
bash env_outline/setup_mamba.sh
# mamba activate forcevla-scout
mkdir -p logs runs checkpoints demos
# Outlines only — do NOT sbatch from quiet box
# NEVER claim Fig.5 60.5% / Tab.3 80% absolute from 1-task Partial smoke
# ALWAYS log force_fusion={none,concat,early_*,fvlmoe} and force_source={true_ft|estimated|stub_sim|none}
# Do NOT treat π₀-fast + naive F as fair FVLMoE substitute
```

#### Run cards (from DELIVERABLE; Ablation Bot-aligned)

##### RC0 — `forcevla_data_ingest_smoke`

| Field | Spec |
|-------|------|
| Goal | Load ForceVLA-Data (or stand-in) → synced `{V_base, V_wrist, s, f∈R^6, a, L}`; 480×640; check NaNs / timestamp align |
| Paper | §4.3: **244** traj, **140k** steps, ~**50**/task, 5 tasks |
| Pass | Schema OK; wrench dim=6; wall **<1 h**; log traj count |
| Script | `env_outline/slurm_data_ingest_smoke.sh` |

##### RC1 — `fvlmoe_vs_pi0_bakeoff` (**#4 — PRIMARY**)

| Field | Spec |
|-------|------|
| Paper | Fig.5 — ForceVLA **60.5%** vs π₀-base w/o F **37.3%** / w/ F **40.2%**; π₀-fast **31.0 / 14.2**; prefer **Insert Plug** column (**80/60/45**) |
| Goal | Matched-budget: **FVLMoE vs π₀-base w/o F vs naive w/ F** on **1** contact-rich task |
| Hard req | Log `force_fusion={none,concat,fvlmoe}` and `force_source` |
| Pass | Directional: FVLMoE ≥ concat ≥ none on SR **or** contact metrics; **not** required: 5-task 60.5% absolute |
| Script | `env_outline/slurm_fvlmoe_vs_pi0.sh` |

##### RC2 — `fusion_stage_ablation` (**#1 — PRIMARY**)

| Field | Spec |
|-------|------|
| Paper | Tab.3 — baseline **45**; linear-before **55**; MoE-before **0**; concat-after **60**; ForceVLA **80** |
| Goal | `{early_linear, early_moe_stub, late_concat, late_fvlmoe}` on **1** task / short FT |
| Pass | Late ≥ early; early_moe does **not** win (cite 0% if untrainable) |
| Script | `env_outline/slurm_fusion_stage_ablation.sh` |

##### RC3 — `insertion_metrics_harness` (**#2/#3 metrics — PRIMARY**)

| Field | Spec |
|-------|------|
| Paper | Abstract up to **80%** plug; Tab.1 peel **14.12** cm / **7** strokes; eval n=20 insert/pump, 10 wipe, 15 peel |
| Goal | Define + log **SR, align error, peak \|F\|, peel length/strokes**; run on smoke policy or replay |
| Pass | Metrics JSON schema filled; **no** invented paper scores |
| Script | `env_outline/slurm_insertion_metrics.sh` |

##### RC4 / RC5 / RC6 / RC7 — scale / occl / router / multitask

| Card | Spec |
|------|------|
| RC4 scale | Subset `{10,25,50}` demos/task stub **or** coverage report — `slurm_data_scale_smoke.sh` |
| RC5 occl Partial | Mask socket/plug ROI; force-on ≥ force-off under occlusion (directional) — `slurm_gen_occlusion.sh` |
| RC6 router Partial | Dump top-1 expert vs progress bins; **do not** claim Fig.9 reproduction — `slurm_moe_router_log.sh` |
| RC7 multitask | Optional 2-task short joint **or cite** Tab.5 **67.5 / USB 10 / fast 0** — `slurm_multitask_joint_stub.sh` |

#### Numbers to memorize (exam)

| Fact | Value |
|------|-------|
| ForceVLA / base w/o F / w/ F avg | **60.5 / 37.3 / 40.2** (+**23.2** / +2.9) |
| π₀-fast ± F | **31.0 / 14.2** (force hurts) |
| Tab.3 | **45 / 55 / 0 / 60 / 80** |
| Plug / USB (Fig.5) | ForceVLA **80 / 25** |
| Occlusion | ForceVLA **90%** |
| Peel | **14.12 cm / 7 strokes** |
| Multi-task | ForceVLA **67.5%**; USB **10%**; fast **0%** |
| Data | **244 traj / ~140k / ~50 per task** |
| Caveat | Pump base w/F **83 > ForceVLA 67** |
| Scope | Course = 1-task lite; **no** π₀.₅ bake-off; **no** 60.5% from stub |

#### GPU / RAM / TIME honesty (course)

| Workload | VRAM | Wall (order) | Smoke |
|----------|------|--------------|-------|
| RC0 ingest | CPU/4–12 GB | 0–1 h | **Y** |
| RC3 metrics harness | 4–12 GB | 1–4 h | **Y** |
| RC6 router log | 8–24 GB | 1–4 h | Partial |
| RC2 fusion lite | 24–48 GB | 6–20 h | **Y** |
| RC1 bakeoff 3 methods × 1 task | 24–48 GB | 8–24 h | **Y** |
| RC4/RC5 Partial | 24–48 GB | 4–16 h | Partial |
| RC7 multitask stub | 24–48 GB | 8–24 h | Partial / cite |
| Paper single-task 10k / 1×4090 | 24 GB | ~9 h | cite / N as course default |
| Paper multi-task 30k / 2×4090 + full real | 2×24 GB+ | ≫24 h + robot | **N** |

**Empiricist exam bite:** "Quote **60.5 / 37.3 / +23.2**, Tab.3 **0→80**, occlusion **90**, USB multi-task **10**, ForceVLA-Data **244/~140k**; RC1+RC2+RC3 = π₀ bakeoff + fusion stage + insertion metrics with `force_fusion`/`force_source` logged; Ablation Bot AUTHORITATIVE ranks 1–7; refuse π₀.₅ bake-off / FM↔FAST / stub=60.5% laundering."

---

### 6.5 Visionary — balanced (~12–14) — BEHAVIOR contact-rich

> **Seat charge:** Decision-oriented **BEHAVIOR contact-rich / force-tactile** hooks. Late force MoE on modern VLA substrates. Pair with MoPA-PD (clutter transit) and DART (recovery). **No Discord/Drive checklist.**

#### BEHAVIOR connection (honest)

BEHAVIOR-class household activities are dense with **contact-rich** phases: plug/USB-like inserts, drawer/cabinet seating, wiping spills, peeling/food prep, press buttons. Vision-only VLAs (π₀ / π₀.₅ line) inherit semantic open-world strength but still fail when **forces**, **occlusion**, and **compliance** dominate. ForceVLA’s durable idea:

> **Keep the modern VLA/FM controller; make calibrated 6-axis F/T a first-class late token with sparse expert fusion so contact phases can re-route computation.**

| BEHAVIOR need | ForceVLA-shaped response |
|---------------|--------------------------|
| Plug / peg / USB / charger insert | F/T-conditioned insert chunks; occlusion robustness |
| Drawers / dishwashers / lids | Contact detection + compliant push/pull via expert routing |
| Wiping / cleaning surfaces | Continuous force maintenance (wipe/peel analogs) |
| Crowded counters (visual clutter) | Lean on force when wrist cam occludes contact |
| Hybrid with π₀.₅ / RTC stacks | **TENTATIVE recipe:** HL semantic loop + low-level FM expert **augmented** with FVLMoE when wrist F/T present — **not** claimed by paper |
| Hardware | Prefer arms with integrated or retrofit F/T; budget calibration; paper’s estimated-wrench caveat |

#### Visionary bets (balanced; not monopoly)

1. **Late force adapter on π₀.₅ / GR00T / OpenVLA-OFT:** Do **not** prepend force into VLM; add FVLMoE-style late MoE (E small, k=1) on action expert — Tab.3 moral.  
2. **Mandatory wrench in contact-stage demos:** BEHAVIOR contact stages without FT/tactile → collect **estimated wrench or low-cost FT** subset.  
3. **Metric card beyond SR:** peak \|F\|, insertion depth, slip events, peel-length analogs — Tab.1 honesty.  
4. **Occlusion + force:** When vision drops (Tab.2 **90%**), force routing should dominate — log expert_id.  
5. **Do not naive-concat onto FAST/compact tokenizers** without pretrain — paper π₀-fast **31→14.2%**.  
6. **Pair with MoPA-PD / DART siblings:** Force for **contact**; planner-distill for **clutter transit**; DART for **recovery** — three cards, not one slogan.  
7. **Sim-first smoke:** OmniGibson / BEHAVIOR-sim contact with simulated wrench → then robot Partial.  
8. **Empiricist honesty:** Course validates fusion stage + metrics + schema; leaderboard needs real contact sensors — never Fig.5 60.5% from stub wrench.  
9. **USB-hard curriculum:** Force alone insufficient for fine alignment (multi-task USB **10%**) — curriculum / tactile / precision priors still needed.  
10. **One narrative line:** VLAs see; ForceVLA **feels through a routed expert**; 2026 should **late-fuse wrench into the action expert**, measure contact, and keep VL pretraining intact.

#### Follow-up research

1. Port FVLMoE onto open π₀/OpenPI with public F/T datasets.  
2. Replace estimated wrench with high-fid transducers; ablate noise.  
3. Add tactile image tokens beside F/T in the same MoE.  
4. Contact-phase rewards / eval suites inside BEHAVIOR (insert, wipe, open).  
5. Study Expert-0 generalist bias; load-balance for rare contact events.  
6. Low-cost retrofit F/T on common arms (paper’s stated direction).  
7. Multi-task USB-hard curriculum — force alone insufficient for fine alignment.

#### New apps (**beyond Discord/Drive**)

- Kitchen plug / appliance insert assist.  
- Wipe / scrub force profiles for cleaning robots.  
- Food prep peel/cut force phases.  
- Occluded cable seating behind furniture.  
- Safe contact exploration under torque limits (Height Gen. lesson).

*(No Discord / Drive checklist content.)*

**Visionary one-liner:** *For BEHAVIOR contact-rich homes, pair modern VLAs with **wrist F/T + late MoE fusion** so insertion/wipe/occlusion phases stop being pure vision failures — ForceVLA is the 2025 blueprint beside MoPA-PD (clutter transit) and DART (recovery), not a π₀.₅ replacement.*

**Visionary exam bite:** "ForceVLA = **late force-MoE recipe** for contact-rich beside **planner-distill** (MoPA-PD) and **recovery-demo** (DART); cite **60.5/37.3/+23.2**, Tab.3 **0→80**, occlusion **90**, USB **10**, ForceVLA-Data **244/~140k**; Empiricist smokes directional only; **no π₀.₅ bake-off**; **no Discord/Drive**."

---

## 7. Empiricist run pack (env / scripts / GPU — from DELIVERABLE)

### Env setup

```bash
export FORCEVLA_WORK_ROOT=$SCRATCH/forcevla-smoke
cd /workspace/forcevla-quiet
bash env_outline/setup_mamba.sh
# mamba activate forcevla-scout
mkdir -p logs runs checkpoints demos
# EDIT: pin ForceVLA / OpenPI π₀ when public; prefer force stub if data unreleased
# Outlines only — do NOT sbatch from quiet box
```

| File | Role |
|------|------|
| `env_outline/conda_env.yaml` | mamba sketch (Torch + OpenPI/π₀ deps + force IO stubs) |
| `env_outline/setup_mamba.sh` | env create; OpenPI / ForceVLA clone notes |
| `env_outline/run_checklist.md` | GPU / TIME honesty preflight |
| `env_outline/README.md` | cluster usage |
| `env_outline/slurm_data_ingest_smoke.sh` | RC0 |
| `env_outline/slurm_fvlmoe_vs_pi0.sh` | **RC1 / #4 PRIMARY** |
| `env_outline/slurm_fusion_stage_ablation.sh` | **RC2 / #1 PRIMARY** |
| `env_outline/slurm_insertion_metrics.sh` | **RC3 PRIMARY** |
| `env_outline/slurm_data_scale_smoke.sh` | RC4 |
| `env_outline/slurm_gen_occlusion.sh` | RC5 / #5 Partial |
| `env_outline/slurm_moe_router_log.sh` | RC6 Partial |
| `env_outline/slurm_multitask_joint_stub.sh` | RC7 / #6 Partial |

### Success criteria vs paper claims

| Paper claim | Minimal course criterion | Not required |
|-------------|--------------------------|--------------|
| FVLMoE ≫ vision-only / ≥ naive F | RC1: directional SR or contact metrics; log `force_fusion` | Fig.5 60.5% absolute / 5-task |
| Late fusion ≫ early MoE | RC2: late ≥ early; early_moe does not win | Tab.3 80% absolute; full early-MoE train |
| Contact-rich metrics honesty | RC3: SR + wrench + peel fields filled | Paper n=20×5 Flexiv protocol |
| ForceVLA-Data schema | RC0: dim-6 wrench sync; traj count logged | Full 244-traj public release |
| Occlusion robustness | RC5 Partial: force-on ≥ force-off under mask | Tab.2 90% absolute |
| Multi-task joint | RC7 cite / stub | Tab.5 67.5% absolute |
| Full paper protocol | **Document N** | Never course |

**Pass:** documented smoke with Ablation Bot **#1 + #4 + insertion metrics** confirmed directionally + `force_fusion`/`force_source` logged.  
**Fail / overclaim:** “reproduced Fig.5 60.5%” / “beats π₀.₅” / “ForceVLA is FAST” / stub wrench as ForceVLA-Data from Partial smoke.

---

## 8. Slide Maker brief

| Role | Weight / timing guide | Focus |
|------|----------------------|-------|
| Archaeologist | **balanced · ~12–14** | CLEAR/TENTATIVE; π₀ parent; no π₀.₅ bake-off; FM≠FAST; MoPA-PD contrast; genealogy; exam bite |
| Stakeholder | **balanced · ~12–14** | Buy late MoE pattern; risks (FT cost, USB 10%, estimated wrench); decision ask |
| Scientific Reviewer | **balanced · ~12–14** | Tab.3+Fig.5+occlusion; pump/USB caveats; verdict Accept Alt contact-rich; fence π₀.₅ / FM↔FAST |
| Empiricist | **balanced · ~12–14** | H-fusion/avg/plug; RC1+RC2+RC3; Ablation Bot AUTHORITATIVE; GPU honesty; must-cite tattoo |
| Visionary | **balanced · ~12–14** | BEHAVIOR contact-rich; late force-MoE + MoPA-PD + DART; **no Discord/Drive** |

**Spine (paste into Slide Maker) — 5 bullets:**
1. **Late 6-axis F/T + FVLMoE (4 experts, top-1) augments π₀ FM head for contact-rich**; fusion quality ≫ naive concat; **NOT** a FAST switch; contrast **MoPA-PD** (train privilege) vs **deploy-side force**.  
2. **Must-cite:** avg **60.5** vs **37.3 / 40.2**; Tab.3 early MoE **0% → ForceVLA 80%**; occlusion **90%**; USB multi-task **10%**; ForceVLA-Data **244 traj / ~140k**; **no π₀.₅ bake-off**.  
3. **Taxonomy:** π₀ FM parent \| π₀-fast baseline (naive F hurts) \| ForceVLA late MoE \| MoPA-PD (**CLEAR contrast**) \| π₀.₅ (**related, unmeasured**) \| JEPA/WM (**contrast poles only**).  
4. **Lineage (Club Pack — all seats empty, balanced):** CLEAR parent π₀; inventum = late force token + FVLMoE additive FM guidance; **NOT** FAST head swap; **NOT** MoPA-PD / JEPA child.  
5. **BEHAVIOR’26 + honesty:** ForceVLA = **late force-MoE recipe** for contact-rich beside MoPA-PD clutter transit + DART recovery; Empiricist RC1–RC3 directional; **do not invent** π₀.₅ bake-off / Fig.5 60.5% from stub — **no Discord/Drive**.

**GitHub title sketch:** `[ECE 605] - ForceVLA Making Club Pack`

Full Slide Maker brief: `/workspace/forcevla-quiet/SLIDE_BRIEF.md`

---

## Paths

| Artifact | Path |
|----------|------|
| This pack | `/workspace/forcevla-quiet/KANTA_PACK.md` |
| PDF | `/workspace/forcevla-quiet/KANTA_PACK.pdf` |
| Slide brief | `/workspace/forcevla-quiet/SLIDE_BRIEF.md` |
| Senior Research | `/workspace/forcevla-quiet/DELIVERABLE.md` |
| Summarizer | `/workspace/papers/forcevla-2505.22159-summary.md` |
| Ablations | **AUTHORITATIVE** — `/workspace/papers/forcevla-2505.22159-ablations.md` |
| Paper notes | `/workspace/forcevla-quiet/paper_notes.md` |
| Env outlines | `/workspace/forcevla-quiet/env_outline/` |
| Paper PDF | `/workspace/papers/forcevla-2505.22159.pdf` |
| Text extract | `/workspace/papers/forcevla-2505.22159.txt` · `paper_clean.txt` |
| Page figs | `/workspace/papers/forcevla-figs/page-01.png` … `page-20.png` |
| Sibling links | `/workspace/pi0-quiet/` · `/workspace/pi05-quiet/` · `/workspace/fast-quiet/` · `/workspace/distill-mopa-quiet/` · `/workspace/dart-quiet/` · `/workspace/openvla-quiet/` · `/workspace/openvla-oft-quiet/` |
| Project / code | https://sites.google.com/view/forcevla2025 · https://github.com/ft-robotic/ForceVLA |

---

*Pack synthesized 2026-09-29 PT (HST) for KanBot / ECE 605 — Undated LAST Alternative approaches · ALL seats EMPTY · balanced five-role Club Pack · Ablation Bot AUTHORITATIVE ranks 1–7 · CLEAR parent π₀ · NOT FAST switch · no π₀.₅ bake-off · CLEAR contrast MoPA-PD · must-cite 60.5 vs 37.3/40.2 · Tab.3 0→80 · occlusion 90 · USB multi-task 10 · ForceVLA-Data 244/~140k · Visionary BEHAVIOR contact-rich · no Discord/Drive · outlines only — no cluster execution.*
