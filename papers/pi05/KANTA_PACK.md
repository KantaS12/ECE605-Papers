# Paper Manager pack — π₀.₅ (Sep 30)

**Paper:** π₀.₅: a Vision-Language-Action Model with Open-World Generalization  
**arXiv:** https://arxiv.org/abs/2504.16054  
**Blog:** https://www.physicalintelligence.company/blog/pi05  
**PDF:** /workspace/papers/pi05-2504.16054.pdf  
**Session:** Sep 30 · due 9:30 AM · **You = Visionary (seat 1)**  
**Note:** Other role seats empty on schedule — full five packs still included for club/GitHub  
**Nearby:** **Oct 2 summary proposal deadline**  
**Sources:** Summarizer + Senior Research (`/workspace/pi05-quiet/DELIVERABLE.md`)

---

## 1. Paper summary

Follow-on to π₀ for **open-world** (new homes) via heterogeneous **co-training** + **hierarchical** inference (semantic subtask → continuous FM chunk). Two-stage: **FAST discrete pretrain** (280k) → **FM action expert post-train** (80k). Same model: AR subtask then **10 FM steps**, ~50-step/~1s chunks @ **50 Hz**. ~400h mobile-manip / ~100 homes; claims long-horizon cleaning in entirely new homes.

**FM vs FAST myth:** Hybrid FAST pretrain + **FM retained** for continuous control. Blogs saying “FAST replaced FM” are **FALSE**. OpenPI ships **FM head** for π₀.₅; KI (2505.23705) formalizes stop-grad FAST+FM.

---

## 2. Keywords

π₀.₅, open-world VLA, co-training (MM/ME/CE/HL/WD/VI), hierarchical subtasks, FAST+FM hybrid, knowledge insulation, flow matching, BEHAVIOR baseline, RTC

---

## 3. FM vs FAST flag (say this aloud)

| Claim | Verdict |
|-------|---------|
| FAST replaced FM | **MYTH** |
| FAST pretrain + FM post-train | **TRUE** |
| FM head retained at infer | **TRUE** |
| OpenPI FM API for π₀.₅ | **CLEAR** |
| FAST is the deployed low-level controller | **FALSE** |

---

## 4. Role packs

### Stakeholder (18+3)
Open-world KPI = **new homes**, not one kitchen. OpenPI `pi05_*` + BEHAVIOR’26 official baseline (with GR00T N1.7). BEHAVIOR’25 winner on Pi0.5 (q≈0.260) validates substrate. Budget FM fine-tunes + optional RTC; ignore FAST-replaced-FM myths.

### Scientific Reviewer (8+3)
Strong system paper; unseen protocol + ablations. Caveats: figure-only bars; abbreviated scaling recipe; private MM data; human-HL oracle result surprising; KI clearer in follow-up. Hybrid FAST→FM must be cited accurately.

### Empiricist (11+3)
**H1:** π₀.₅ > π₀ on multi-stage/lang under matched FT  
**H2:** HL on > flat prompt→action on long chains  
**H3:** WD/semantic co-train helps OOD lang; robot diversity helps OOD scenes  
**H5:** mid horizon ~32 often beats raw H=50 open-loop on BEHAVIOR-like tasks  

| Priority | Experiment |
|----------|------------|
| P0 | π₀ vs π₀.₅ (public OpenPI/Comet); HL on/off; held-out scene proxy |
| P1 | Co-train ablate (WD/ME/CE); location scaling; H∈{8,16,32,50} |
| P2 | NFE grid; VI/HL quality proxy |
| Defer | Full private MM recipe |

Budget: eval 12–24 GB; short FT 24–48 GB. Env: `/workspace/pi05-quiet/env_outline/`.

### Archaeologist (11+3)
RT-1/2, OpenVLA, Octo, OXE → **π₀** (FM) → **FAST** → **π₀.₅** (open-world+hybrid+HL) → **KI** → **RTC** → Hi Robot. BEHAVIOR-1K →’25 (Pi0.5 winner) →’26 (π0.5 baseline). What’s new: mixture, unified HL/LL, VI, home-scale eval.

### Visionary (11+3) — **YOU (seat 1)** — HIGH PRIORITY

**2026 BEHAVIOR (CLEAR)**
- Baselines: **π0.5 + GR00T N1.7**
- 100 tasks / 7 scenes / 20k demos  
- Launch ~07/02/2026 · submit **10/16/2026** · winners ~11/04/2026  
- ’25 winner adapted Pi0.5 (correlated FM noise, System-2 stages, multi-sample FM, compression) → ~26% q — preview of ’26 bottlenecks (grasps, non-Markov stages, recovery)

**Oct 2 proposal framing:** **π0.5-baseline + targeted deltas**, not greenfield.

**High-leverage deltas to champion**
1. **Keep FM**; FAST/KI for reps/language — never “replace FM with FAST”  
2. Hierarchy / stage tracking (paper HL + ’25 System-2)  
3. **RTC** for delayed control loops  
4. Recovery / VI-style intervention data  
5. Cross-embodiment + web/HL co-training when scene diversity is limited  

**Apps:** new-home cleaning, language tidying, coachable robots, OpenPI FT on DROID/LIBERO/custom.  
**Caution:** real-home success ≠ solved BEHAVIOR (BDDL, depth, 100-task multitask). Still clearest shared substrate linking PI open-world, OpenPI, and 2026 baseline.

Skip Discord/Drive.

---

## 5. Control note

Hierarchical subtask tokens = semantic outer loop; FM action chunks = inner control. Open-world = held-out scene generalization via co-train mix — **not** a classical planner. RTC is the companion paper for async soft-inpaint.

---

## 6. Slide Maker brief

| Role | Timing | Focus |
|------|--------|-------|
| Stakeholder | 18+3 | Open-world KPI + OpenPI/BEHAVIOR’26 |
| Reviewer | 8+3 | Strengths + private-data / citation hygiene |
| Empiricist | 11+3 | π₀ vs π₀.₅ + HL + horizon |
| Archaeologist | 11+3 | π₀→FAST→π₀.₅→KI→RTC→BEHAVIOR |
| **Visionary** | **11+3** | **2026 map + Oct 2 proposal deltas (HIGH)** |

Spine: open-world + hybrid FAST→**FM retained**. PDF for Mac. All five roles for GitHub.

---

## Paths
| Artifact | Path |
|----------|------|
| This pack | `/workspace/pi05-quiet/KANTA_PACK.md` |
| Senior Research | `/workspace/pi05-quiet/DELIVERABLE.md` |
| Summarizer | `/workspace/papers/pi05-2504.16054-summary.md` |
| Paper PDF | `/workspace/papers/pi05-2504.16054.pdf` |
