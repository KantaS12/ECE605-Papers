# Paper Manager pack — Open X-Embodiment (Nov 2 · Data)

**Paper:** Open X-Embodiment: Robotic Learning Datasets and RT-X Models  
**arXiv:** https://arxiv.org/abs/2310.08864  
**PDF:** /workspace/papers/open-x-embodiment-2310.08864.pdf  
**Session:** Nov 2 · due 9:30 AM · **You = Visionary (seat 1)** · Type: **Data**  
**Sources:** Summarizer + Senior Research (`/workspace/oxe-quiet/DELIVERABLE.md`)

---

## 1. One-liner

**OXE = municipal water** for generalist robot policies; BEHAVIOR’26 = bottled specialty blend — steal mix science, don’t drink the river.

Shared multi-lab demo pool + RT-1-X/RT-2-X proving positive transfer without embodiment adapters — foundation for Octo/OpenVLA/π₀/BEHAVIOR data stories.

---

## 2. Exact pool vs train mix (CLEAR)

| | Pool | RT-X train mix |
|--|------|----------------|
| Scale | **60** datasets / **22** embodiments / **1M+** traj | **9** manipulators (≠ full 22) |
| Schema | RLDS | same |
| Eval | — | **3600** trials / **6** robots |

**Headline morals:** Fig.4 ~**+50%** relative small-domain lift from co-training; Table I RT-1-X **underfits** large native Google set (73% vs RT-1 92%); Table II emergent **27.3% → 75.8%** (~3×); no-Bridge **42.8%**.

---

## 3. Keywords

OXE, RT-1-X, RT-2-X, RLDS, cross-embodiment, mixture weights, Bridge, Magic Soup, BEHAVIOR data design

---

## 4. Role packs

### Stakeholder (18+3)
Open shared pool enabled community generalists without each lab building PaLI-scale data. Cite as infrastructure win; treat RT-2-X as transfer existence proof not production VLA; invest in modality coverage + long-horizon, not only more pick-place hours.

### Scientific Reviewer (8+3)
Holds: matched RT-1 vs RT-1-X isolates mix; capacity caveat honest; Bridge ablation. Punish: “generalist X-robot” ≠ zero-shot new hardware; 9≪22 blur; PaLM skill ontology; 55B unrepro. Verdict: landmark systems+dataset paper — not complete BEHAVIOR recipe.

### Empiricist (11+3)
| Priority | Experiment |
|----------|------------|
| P0 Y | RLDS shard inventory/loader smoke; mix-cardinality (Bridge-only vs +2 vs +5) on open policy FT |
| P0 Partial | Leave-Bridge-out (Table II moral) |
| P1 | Small-domain lift proxy; capacity×mix; embodiment leave-one-out; same mix → Octo/OpenVLA/π₀ |
| Defer | Full RT-1-X / RT-2-X retrain; TB-class full OXE mirror |

Env: `/workspace/oxe-quiet/env_outline/`. Separate pool vs RT-X mix vs Octo 800k vs OpenVLA 970k.

### Archaeologist (11+3)
RoboNet/lab demos/QT-Opt/Bridge/RT-1 → **OXE+RT-X** → Octo/OpenVLA soups → π₀/OpenPI/π₀.₅/GR00T → BEHAVIOR’25–’26. OXE replaced single-lab pretrain assumption; did **not** replace need for post-train demos, wrist/proprio, continuous chunks, challenge sim alignment.

### Visionary (11+3) — **YOU (seat 1)** — HIGH PRIORITY

**Steal from OXE**
1. Embodiment/scene **width** as first-class  
2. **Schema before scale** (RLDS → lock obs keys early)  
3. Language as mix **column** (push BEHAVIOR post-train **>90%** + paraphrases)  
4. Transfer **ablations** (Bridge-removal gold) + publish negative transfer  
5. **Capacity × mix** co-design (Table I)

**Anti-patterns → fixes**
| OXE-era fail | Fix for BEHAVIOR |
|--------------|------------------|
| Uniform everything | Explicit **weight table** |
| Loyalty to bad shards | **Kill switch** → weight 0 |
| Wrist poverty | Hard gate contact tasks |
| Lang poverty | Dual-condition lang+subgoals |
| 7×256 bins only | Chunked continuous (or FAST) |
| Huge shards drown gold | **Sublinear** size temperature (π₀ n^0.43) |
| Pick-place monopoly | Cap skill family ≤~20% |

**Concrete recipe (stages)**
1. **Prior:** OXE-like / vendor π₀.₅/GR00T; sublinear weights; exclude bad shards  
2. **Mid-train sim:** cover all tasks; upsample long-horizon + recovery; dense lang/subtask tokens  
3. **Post-train gold:** highest sample weight; per-task floors; wrist-complete; aim BEHAVIOR tokens **≥30–50%** of late training  
4. **Holdout mix science:** ablate no-prior / no-wrist / no-paraphrase / uniform vs n^0.43 / kill switch  

**Red lines:** no uniform raw OXE dump; no “discrete bins enough for 2026”; no “OXE alone = BEHAVIOR plan.”

Skip Discord/Drive.

---

## 5. Slide Maker brief

| Role | Timing | Focus |
|------|--------|-------|
| Stakeholder | 18+3 | Infrastructure win |
| Reviewer | 8+3 | Landmark + overclaim punish |
| Empiricist | 11+3 | Mix-cardinality smoke |
| Archaeologist | 11+3 | OXE → soups → BEHAVIOR |
| **Visionary** | **11+3** | **Mix cards + BEHAVIOR recipe (HIGH)** |

Spine: pool ≠ train mix; mix science > drinking the river. PDF for Mac.  
**GitHub title:** `[ECE 605] - Open X-Embodiment Making Visionary`

---

## Paths
| Artifact | Path |
|----------|------|
| This pack | `/workspace/oxe-quiet/KANTA_PACK.md` |
| Senior Research | `/workspace/oxe-quiet/DELIVERABLE.md` |
| Summarizer | `/workspace/papers/open-x-embodiment-2310.08864-summary.md` |
| Paper PDF | `/workspace/papers/open-x-embodiment-2310.08864.pdf` |
