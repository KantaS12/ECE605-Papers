# Paper Manager pack — DART (Nov 9 · Data)

**Paper:** DART: Noise Injection for Robust Imitation Learning  
**(Disturbances for Augmenting Robot Trajectories)**  
**arXiv:** https://arxiv.org/abs/1703.09327 · [project](http://berkeleyautomation.github.io/DART/) · [BAIR blog](https://bair.berkeley.edu/blog/2017/10/26/dart/) · CoRL 2017 · PMLR 78:143–156  
**Authors:** Michael Laskey, Jonathan Lee, Roy Fox, Anca Dragan, Ken Goldberg — UC Berkeley AUTOLAB / BAIR  
**PDF:** `/workspace/papers/dart-1703.09327.pdf` (= `/workspace/dart-quiet/paper.pdf`)  
**Session:** Nov 9 · due 9:30 AM · Type: **Data**  
**Role weights:** **Archaeologist=3 (LONGEST)** · Stakeholder / Scientific Reviewer / Empiricist / Visionary also filled (seats otherwise empty)  
**Kanta role:** unspecified — deepen lineage first; still produce **all five**  
**Themes:** noise injection / recovery demos / covariate shift / safer than DAgger  
**Sources:** Summarizer + Ablation Bot + Senior Research (`/workspace/dart-quiet/DELIVERABLE.md`) + `paper_notes.md` / `env_outline/`  
**Sibling links:** `/workspace/droid-quiet/` · `/workspace/act-aloha-quiet/` · `/workspace/diffusion-policy-quiet/` · `/workspace/oxe-quiet/` · `/workspace/pi05-quiet/` · `/workspace/openpi-comet-smoke/` · `/workspace/behavior-1st-place-summary.md`

---

## 1. One-liner

**Label recoveries by disturbing the expert, not by deploying the learner** — protocol > architecture; heed ACT’s warning that raw teleop noise can break fine manipulation.

**Contrast:** DAgger = on-policy corrections on *learner* states (unsafe / tedious); DART = *supervisor-side* optimized action noise so demos already contain boundary recoveries, while collection stays near the expert manifold.

---

## 2. Summary (exec overview + key numbers)

**DART** (CoRL 2017) is an **off-policy** Behavior Cloning variant that **injects optimized noise into the supervisor’s control stream** during demo collection. The supervisor must demonstrate **recoveries** near the ideal-policy boundary; learning remains standard supervised BC on the aggregated noisy demos. Noise magnitude/shape is iteratively fit so the noisy supervisor approximates the **error of the trained robot** (Σ̂ from residual outer-products, then α-shrinkage).

**Why Data session:** The durable artifact is a **demo-collection protocol** (noise schedule + α), not a new policy architecture.

| Axis | Number / claim |
|------|----------------|
| HSR clutter success (Fig. 3) | **BC 49%** \| **DART(α=3) 79%** \| **DART(α=6) 72%** |
| Relative lift vs BC | **~+62%** ((79−49)/49) |
| Humanoid wall-clock | DART **~3× faster** than DAgger |
| Supervisor reward @ collect | DART **−5%** vs clean; DAgger **~−80%** |
| α story | **Inverted-U** — α=3 best; α=6 still > BC but hurts humans |
| Isotropic Σ=I | Unsafe / weak — **need optimized Σ** |
| MuJoCo domains | Walker, Hopper, Half-Cheetah, Humanoid; DART ≈ DAgger reward |
| HSR protocol | 4 humans; N=10 BC + 30/method; **K=2**; 20 evals |
| Venue | CoRL 2017 · Berkeley AUTOLAB |

**Action noise, not state noise (FLAG):** Noise hits the **supervisor control** \(u\) (Gaussian \(\mathcal{N}(\pi^*(x),\Sigma)\) or discrete ε-greedy) → dynamics widen the state distribution. Distinct from obs dropout / domain rand / visual aug.

---

## 3. Keywords

DART; Disturbances for Augmenting Robot Trajectories; imitation learning; behavior cloning; covariate shift; compounding error; DAgger; noise injection; action noise; recovery demonstrations; data augmentation; supervisor-side noise; α-shrinkage; Σ^α; MuJoCo locomotion; Toyota HSR; grasping in clutter; CoRL 2017; off-policy IL robustness; persistence of excitation; ACT chunking contrast; FM sample noise distinction; BEHAVIOR recovery demos

---

## 4. Deep dive — Motion / covariate shift / recovery control (**PRIORITY**)

**Callout:** This paper is a **primary syllabus node for covariate shift in IL**, not a classical motion planner. Control implications are direct and must be distinguished from later **chunking** and **FM sample noise**.

### Three noises — keep them labeled

| Layer | What it is | When applied | Example |
|-------|------------|--------------|---------|
| **(a) DART teleop / demo-collection noise** | Optimized Gaussian (or ε-greedy) on **supervisor actions** during demos | Data collection | This paper; HSR α∈{3,6} |
| **(b) FM / diffusion training / sample noise** | Generative-head noise for score/flow matching | Train / inference sampling | π₀ / DP / BEHAVIOR’25 correlated FM |
| **(c) Chunking / receding-horizon control** | Predict action sequences; execute prefix; replan | Policy architecture | ACT (pred/exec chunks); DP obs=2/pred=16/action=8 |

**Do not collapse (a)/(b)/(c).** ACT **cites DART [36] CLEARLY**, then prefers **chunking** for fine teleop because “noise injection can directly lead to task failure.” DART attacks the *data distribution*; chunking attacks *effective horizon* — complementary, not substitutes.

### Compounding error over horizon \(T\)

Same long-horizon pathology that later motivates ACT chunking, Diffusion Policy receding-horizon, and RTC soft-inpaint. Ross & Bagnell formalize the **Shift + Loss** decomposition; BC minimizes under supervisor dist; robot executes under its own → **compounding**.

| Attack | Side | Mechanism |
|--------|------|-----------|
| **DART** | **Data** | Widen supervisor traj so recoveries are labeled in-distribution |
| **DAgger** | **Query / on-policy** | Label learner states; visit low-reward states early |
| **Chunking (ACT/DP)** | **Control** | Shorten open-loop effective horizon; reactive replan |
| **Keep-fails / scene churn (DROID)** | **Data ops** | Coverage & recovery segments without live noise schedule |

### Recovery funnel (Fig. 1 moral)

1. Off-policy BC → drift off demo manifold → no recovery labels.  
2. On-policy DAgger → learner visits dangerous states; human corrects under failure.  
3. **DART** → small optimized disturbances → supervisor shows **boundary recoveries** while stay near high-reward manifold (“funnel”).

**Empiricist hook:** Script mid-traj kicks on Hopper; log recovery SR separately from nominal return. HSR clutter = qualitative long-horizon (clear → open-loop grasp) — course smoke uses locomotion recovery instead.

### Vs DAgger (one-table)

| | **DAgger** | **DART** |
|---|---|---|
| Who acts @ collect | Mixture / learner | **Supervisor + action noise** |
| Who labels | Supervisor on learner states | Supervisor’s own noisy demos |
| Retrain cadence | Every demo (full) | Infrequent (ψ update; final BC) |
| Dangerous states? | Often yes early | Mostly near expert funnel |
| Human load | High | Lower (small disturbances) |
| Humanoid collect reward | **~−80%** | **~−5%** |
| Humanoid compute | Baseline | **~3× faster** |

---

## 5. Method deep dive — Σ^α, action vs state noise, iterative schedule

### Problem (Eq. 1) → noise-injected BC (Eq. 2)

Minimize surrogate loss of learner under **its own** traj dist. BC instead fits under supervisor dist → Shift. DART collects from \(p(\xi\mid\pi_{\theta^*},\psi)\) and does ERM on that widened distribution.

### Bound → KL objective (Lemma 4.1)

If \(l\in[0,1]\), shift \(\le T\sqrt{\tfrac12 D_{\mathrm{KL}}(p(\xi\mid\pi_{\theta^R}),\,p(\xi\mid\pi_{\theta^*},\psi))}\). Minimize KL ⇒ maximize likelihood of **learner’s actions** under noisy supervisor. Chicken-and-egg: final robot dist unknown at collection time.

### Iterative fit (Eq. 3) + α-scaling (Eq. 4)

With current noise \(\psi_k\) and current learner \(\pi_{\hat\theta}\): estimate \(\hat\psi_{k+1}\) by MLE of robot residual under noisy expert demos, then shrink toward prior expected final error \(\alpha\).

### Gaussian closed form (squared \(L_2\))

\[
\hat\Sigma_{k+1}=\frac1T\mathbb{E}\sum_t\big(\pi_{\hat\theta}(x_t)-\pi_{\theta^*}(x_t)\big)(\cdots)^\top,\quad
\Sigma^\alpha_{k+1}=\frac{\alpha}{T\,\mathrm{tr}(\hat\Sigma_{k+1})}\hat\Sigma_{k+1}.
\]

- **Shape** from empirical error outer-products.  
- **Magnitude** from \(\alpha\) (expected simulated error \(=T\,\mathrm{tr}(\Sigma)\)).  
- Estimate Σ̂ on **held-out** prior-iteration demos (App §4.3).

### Algorithm 1 checklist

1. Inputs: \(\psi_1^\alpha\), target \(\alpha\), iterations \(K\), demos/round \(N\).  
2. For \(k=1..K\): sample \(N\) demos under supervisor+noise → ERM on aggregate → update \(\hat\psi\) then \(\psi^\alpha\).  
3. Output: final \(\theta^R\) by ERM on full aggregate.

### Practical schedule knobs

| Knob | Paper default |
|------|---------------|
| Noise family | Multivariate Gaussian on continuous \(U\) (ε-greedy in app.; unused empirically) |
| MuJoCo α | \(\alpha=T\,\mathrm{tr}(\hat\Sigma_k)\) — “no extra prior” |
| HSR α | Multipliers **3** and **6** on \(T\,\mathrm{tr}(\hat\Sigma_1)\) — **3 wins** (inverted-U) |
| Isotropic baseline | \(\Sigma=I\) always — **fails** |
| K | Small domains: update @ iters **2 & 8**; Humanoid: 50 init then every **25**; HSR **K=2** |
| DAgger β | **0.5** (swept) |
| Data poverty | Subsample **50** (s,a)/traj (Ho & Ermon trick) |
| Architecture | Same 2×64 MLP as TRPO supervisor (MuJoCo); HSR = eye-in-hand CNN push + open-loop grasp |

**Moral:** Wrong scale (isotropic, α too large for humans, Wishart Tr∈{0.005,5} extremes) hurts; magnitude often dominates structure *if* simulated error is right (App Fig. 5, Tr=0.5 can match DART).

---

## 6. Role-ready sections (all five)

### 6.1 Stakeholder

**Product one-liner:** A **demo protocol** that buys DAgger-level robustness **without** putting humans/robots through early catastrophic on-policy rollouts — **49% → 79%** on HSR clutter with moderate α; Humanoid **3×** cheaper collect with only **−5%** supervisor reward drop.

**Who pays / who benefits**
- **Teleop / IL labs:** add optimized action-noise schedule to existing BC pipelines — **no new architecture**.  
- **BEHAVIOR’26 entrants:** recovery-demo card is the missing data knob beside π₀.₅ / N1.7 training noise.  
- **Safety / IRB reviewers:** quieter collect-time noise beats on-policy visits to failed household states.  
- **Non-consumers:** contact-rich fine teleop where ACT’s warning bites — use **task-gated** noise (larger in loco/nav, smaller in precision insert) or prefer chunking.

**Decision framing (Nov 9)**
1. **Have a BC pipeline + teleop?** Inject **optimized** Σ^α (not isotropic vibes) after a small clean seed; log α + supervisor return@collect.  
2. **Tempted by DAgger on humans?** Prefer DART-like off-policy recovery — paper never ran DAgger on physical HSR for a reason.  
3. **Building 2026 data mix?** Budget a **recovery fraction** of demos with noise metadata (`recovery=true`).  
4. **Fine manip?** Heed ACT — chunk first; DART noise as complementary coverage, not a substitute for horizon control.

**Risks**
- α is a **strong hyperparameter** (HSR 3 vs 6); humans degrade if noise too aggressive.  
- Still iterative (not one-shot BC); needs held-out demos for Σ̂.  
- 2017 HSR CNN stack ≠ modern VLA — steal the **idea**, not the codebase.  
- Prop 4.1: if learner can fit expert perfectly, noise adds little.

**Exam bite:** “DART labels recoveries by disturbing the **expert**, matches DAgger reward with **safer/cheaper** collection (Humanoid **3× / −5% vs −80%**), and lifts HSR **49→79%** at α=3 — protocol, not architecture.”

---

### 6.2 Scientific Reviewer

**Claim under review:** Optimized supervisor-side action noise yields DAgger-level IL robustness while staying off-policy, protecting collect-time reward/safety, and beating BC on human teleop.

**Strengths**
1. Clean BC vs DAgger positioning with a crisp **off-policy** alternative.  
2. Theory (Lemma 4.1 + Prop 4.1) — KL story for why *some* Gaussian action noise beats pure BC when error > 0.  
3. Empirics span **algorithmic** (TRPO) and **human** supervisors.  
4. Ablations vs isotropic & Wishart random Σ show **optimization of noise matters**.  
5. Honest human result: larger α not always better (inverted-U).  
6. Protocol transparency (Alg 1, closed forms, App §4.3 hyperparams).

**Weaknesses / caveats**
1. Lemma assumes bounded \(l\in[0,1]\); experiments use unnormalized L2 — bound is motivational.  
2. Assumes supervisor **unaffected** by noise — paper’s own α=6 degradation is evidence humans violate this.  
3. MuJoCo curves lack tabulated seed CIs; HSR **n=4** supervisors, **20** evals — suggestive.  
4. Half-Cheetah \(|\mathcal{X}|=117\) vs common Gym ~17 — reproducibility caveat.  
5. DAgger-B fairness: less frequent retrain hurts DAgger; modern online-update DAgger might close wall-time gap.  
6. No DAgger on physical human task — HSR claim is vs BC only.  
7. Data-poverty subsample (50 pairs/traj) may **amplify** shift vs modern large demos.  
8. ε-greedy derived but unused empirically.

**Verdict:** **Accept as methodological CoRL’17 cornerstone** for recovery-as-data-augmentation. Idea aged better than the HSR stack. Suitable **Data** syllabus node; punish overclaim of Humanoid 3× / HSR 79% from Hopper Partial smokes.

**Reviewer exam bite:** Demand α sensitivity + isotropic control before accepting any “robust IL via noise” claim; cite Prop 4.1 assumptions and ACT’s fine-teleop warning as scope limits.

---

### 6.3 Empiricist

**Hypotheses (from DELIVERABLE — feasible / falsifiable)**
1. **H-dart>bc:** DART(α≈1–3) > BC on Hopper/Walker return (matched demos/arch).  
2. **H-opt>iso:** Optimized Σ̂ > isotropic Σ=I at same intended scale.  
3. **H-alpha-sweet:** α has interior optimum; too large drops collect-return / final policy (HSR moral).  
4. **H-parity-dagger:** DART return ≈ DAgger; DART ↓ wall **or** ↑ supervisor return@collect.  
5. **H-shift:** DART shrinks rollout−demo surrogate gap faster than BC (Fig. 4).  
6. **H-recover:** Under scripted kicks, DART recovers ≥ BC.  
7. **H-K:** K=2 after small BC seed captures most gains.  
8. **H-offline-aug:** Residual-calibrated action-noise on DP/ACT demos = weak DART-lite (expect smaller lift).

**Falsifiers:** DART≈BC; isotropic≥DART; α monotone-good; no safety/wall gap vs DAgger; loss gap unchanged; kicks kill all equally; offline noise hurts SR.

**Minimal matrix:** `{method: BC | isotropic | DART_α∈{1,3} | (opt) DAgger_β=0.5} × {env: Hopper} × {N: 10–20} × {K: 2}` + Tr(Σ)∈{0.005,0.5,5} card.

| Pri | Experiment | Paper anchor | Smoke | Est. GPU-h |
|-----|------------|--------------|-------|------------|
| **P0** | α magnitude + recovery vs BC | §5.2 Fig.3 · 49/79/72 | **Y** (Hopper proxy) | 1–6 |
| **P0** | Optimized Σ vs isotropic Σ=I | Fig.2; App §4.3 | **Y** | 1–4 |
| **P0** | Wishart Tr(Σ) sweep | App Fig.5 | **Y** | 2–6 |
| **P1** | DART vs DAgger vs DAgger-B vs BC | Fig.2 · 3×/−5%/−80% | **Y** lite ≤20 iters | 4–12 |
| **P1** | Iteration + dual-loss (Fig.4) | App §4.3–4.4 | **Y** (shared) | (shared) |
| **P2** | K schedule; offline DART-lite on DP/ACT | Alg 1; siblings | **Partial** | 2–24 |
| **Defer / N** | Humanoid-200; real HSR×4 humans; fresh TRPO | Fig.2–3 | **N** | ≫40 / IRB |

**Numbers to quote (canonical):** HSR **49 / 79 / 72**; **+62%** rel; Humanoid **3× / −5% / −80%**; Tr=**0.5** can match; isotropic fails.  
**Do not** claim these from Hopper Partial.

Env outlines: `/workspace/dart-quiet/env_outline/` (see §7).

---

### 6.4 Archaeologist — WEIGHT **3** / **LONGEST**

**Role charge:** Deepen **prior → DART → subsequent** lineage for IL robustness / covariate shift / recovery. Label **CLEAR** vs **TENTATIVE**. Cross-checked against local packs (ACT, DP, DROID, OXE, Octo, OpenVLA, π₀, π₀.₅).

#### Priors (inputs this paper builds on)

| Prior | Relation to DART | Confidence |
|-------|------------------|------------|
| **Behavioral Cloning / ALVINN** (Pomerleau 1989); Bojarski et al. self-driving BC | Classic off-policy supervised IL; road-edge drift = early compounding folklore | **CLEAR** (cited) |
| **Covariate shift / compounding errors** (Ross & Bagnell 2010) | Formalizes why BC fails under own-dist execution; DART’s Shift term | **CLEAR** (cited [13]) |
| **DAgger** (Ross, Gordon, Bagnell 2011) | On-policy iterative learner→supervisor queries; DART’s main foil | **CLEAR** (cited [14]) |
| **SEARN** (Daumé III, Langford, Marcu) | Intellectual ancestor of reduction-style IL; DAgger lineage | **CLEAR** as field prior; **TENTATIVE** as direct DART citation |
| **AggreVaTe** / Deeply AggreVaTeD (Sun et al. 2017 [20]) | On-policy family siblings; cheaper on-policy updates | **CLEAR** (related work cites [20]) |
| Query-efficient / safety-aware DAgger (Zhang & Cho 2016 [24]; Laskey et al. 2016 [7]) | Document DAgger’s human + safety + compute pain — motivates DART | **CLEAR** (cited) |
| **Persistence of excitation** / white-noise in adaptive/robust control (Green & Moore; Marmarelis; Sastry & Bodson) | Classical justification for rich control noise so models stay valid off nominal | **CLEAR** (cited §2.2) |
| Noise-injection teleop **folklore** (small Gaussian on expert actions) | Practice DART turns into an **optimized** algorithm with α-shrinkage | **TENTATIVE** as named prior; **CLEAR** that DART formalizes it |
| GAIL supervisor setup (Ho & Ermon 2016 [6]) | Same TRPO deterministic-mean supervisor + 50-subsample protocol | **CLEAR** (cited) |
| Authors’ prior: hierarchy-of-supervisors grasping (CASE 2016 [8]); human- vs robot-centric sampling [7] | AUTOLAB IL / grasping context for HSR | **CLEAR** |

#### This paper’s contribution (pivot)

- **Supervisor-side optimized action noise** as an **off-policy** antidote to covariate shift.  
- Algorithmic: iterative **MLE noise fit** (Eq. 3) + **α-shrinkage** (Eq. 4), Gaussian / ε-greedy closed forms.  
- Empirical: match DAgger robustness with **safer, cheaper** collection; beat BC on real human demos (**49→79%**).  
- Syllabus slogan: **“Label recoveries by disturbing the expert, not by deploying the learner.”**

#### Genealogy

```
BC / ALVINN (Pomerleau) — off-policy drift folklore
        ↓
Covariate shift formalized (Ross & Bagnell 2010)
        ↓
DAgger family (2011) — on-policy corrections
  + AggreVaTe / query-efficient / safety-aware variants
  + Persistence-of-excitation folklore (control)
        ↓
DART (CoRL 2017): optimized supervisor-side action noise
  for recovery demos — off-policy, safer collect
        ↓
[CLEAR] ACT / ALOHA (Zhao et al. RSS 2023) cites DART [36]:
        noise yields corrective behavior, but rejects for
        fine teleop → prefers action chunking
        ↓
[TENTATIVE rhyme] Diffusion Policy — chunks + recovery demos
                  (no DART cite in local txt)
[TENTATIVE rhyme] DROID — keep-fails / scene churn coverage
[TENTATIVE rhyme] OXE / Octo / OpenVLA — scale & mixture
[TENTATIVE rhyme] π₀ / π₀.₅ — FM sample noise ≠ DART teleop noise
                  BEHAVIOR’25 correlated-FM + stage tracking
        ↓
BEHAVIOR’26: revive DART as recovery-demo + α-schedule
             data card beside π₀.₅ / N1.7 training noise
```

#### Subsequent lineage — CLEAR citations vs design rhyme

| Work / practice | Relation | Confidence |
|-----------------|----------|------------|
| **ACT / ALOHA** (`2304.13705`) | **Cites DART [36]** explicitly; noise→corrective behavior but “for fine manipulation… can directly lead to task failure”; motivates **chunking** | **CLEAR citation** |
| **Diffusion Policy** (`2303.04137`) | Compounding via chunks + receding-horizon; Push-T recovery demos. No DART cite in local txt | **TENTATIVE** design rhyme |
| **DROID** (`2403.12945`) | Diversity / scene churn / keep-failures for recovery research. No DART cite | **TENTATIVE** rhyme (coverage & recovery data) |
| **OXE / RT-X** (`2310.08864`) | Scale + mixture; failure-recovery upsampling lore — breadth not DART noise | **TENTATIVE** rhyme |
| **Octo / OpenVLA / OpenVLA-OFT** | Generalist BC/VLA; robustness from scale/co-train — not supervisor noise schedules | **TENTATIVE** (shared BC shift ancestor only) |
| **π₀ / π₀.₅** | Continuous chunks + flow matching; BEHAVIOR forks add **correlated FM noise** (execution/sample noise, **not** DART teleop noise). No DART cite in local π txts | **TENTATIVE** rhyme; **CLEAR** distinction of noise layers |
| Gaussian action noise in modern teleop / OpenPI lore | Common practical knob; rarely attributed to DART | **TENTATIVE** folklore continuity |
| Domain rand / visual aug / obs noise | Orthogonal (perception) vs DART **control-stream** | **CLEAR** distinction (not descendants) |
| BEHAVIOR Challenge recovery bottlenecks | “No recovery demos” = failure mode; DART-style forced recovery labeling = plausible intervention | **TENTATIVE** application rhyme; high pedagogical value |

**Consistency vs prior local packs**
- ACT pack: add **DART as the cited noise-injection prior** ACT explicitly discusses, then rejects for fine teleop.  
- DP / π₀ / π₀.₅: keep chunking & FM noise as **parallel anti-compounding tools**, not DART forks, unless CLEAR cite appears.  
- DROID “keep failed demos” = **complementary data ops** to DART’s *forced expert recoveries*.

**Archaeologist exam bite:** “Genealogy is **BC → Ross shift → DAgger → DART (optimized supervisor noise)**; ACT **CLEARLY cites then prefers chunking** for fine teleop; modern VLA recovery is mostly **TENTATIVE rhyme** (diversity, keep-fails, FM sample noise) — teach rhyme vs lineage, and never collapse DART teleop noise with FM training noise.”

---

### 6.5 Visionary — BEHAVIOR + follow-ups + apps (**no Discord/Drive**)

**BEHAVIOR Challenge connection**
- Long-horizon household tasks die from **unrecoverable mid-stage drift** (slip, miss grasp, occlusion) — exactly DART’s boundary-recovery moral.  
- 2025 winners leaned on **correlated FM noise inside the trainer** + System-2 stages — **not** DAgger human-query loops. DART is the missing **collection-time** knob.  
- Two data levers: (a) **DART-style teleop action noise** so experts demonstrate restabilization; (b) retain/upsample **failure & intervention** segments (DROID / π₀.₅ coaching).  
- Prefer **task-gated** noise — larger in loco/nav/push, smaller in precision insert (heed ACT).  
- Race on **π₀.₅ / N1.7** substrates; treat DART as **data recipe**, not a 2017 MLP reimplementation.

**One narrative line:** DAgger taught **query the robot’s mistakes**; DART taught **make the supervisor rehearse the robot’s likely mistakes**; 2026 should **schedule those rehearsals** into household demos beside FM training noise.

**Follow-up research**
1. Learn **state-dependent** heteroscedastic \(\Sigma(x)\) instead of global Σ.  
2. Combine **DART collection** with **chunked policies** (ACT/DP/π₀) — noise for coverage, chunks for horizon.  
3. Replace hand α with meta-learned target error from prior tasks.  
4. VLA finetune: inject noise in **proprio/action** channels during co-train replay, not only pixels.  
5. CLEAR three-way ablate: DART teleop noise vs **correlated FM sample noise** vs **RTC inpaint**.  
6. Stage-aware recovery: stronger noise near stage boundaries / grasp commits (link `commit_s` + BEHAVIOR stage votes).  
7. Publish a **BEHAVIOR recovery/noise card** (α schedule, Σ structure, per-stage flags, `recovery=true` metadata).

**New applications**
- Assistive teleop that stays high-reward under tremor.  
- Autonomous driving lane-recovery demos.  
- Surgical / robotic ultrasound where safe expert disturbances beat learner-in-the-loop.  
- Sim-to-real locomotion where persistence-of-excitation meets IL.  
- Household BEHAVIOR: slip/occlusion recovery teleop budget beside scene churn (DROID-like).

**What not to do**
- Blind isotropic action noise on dexterous bimanual demos.  
- Claiming modern VLAs are “DART descendants” without citations — teach **rhyme vs lineage**.  
- Reviving full DAgger on 50 household tasks for humans.  
- Conflating FM noise, domain rand, and live recovery teleop in writeups.

**Visionary exam bite:** “For BEHAVIOR’26, revive DART as a **recovery-demo + α-schedule data card** beside π₀.₅/N1.7 **training noise**; separate the three noises; steal protocol, not 2017 TF Humanoid numbers.”

---

## 7. Empiricist run pack (from DELIVERABLE)

### Env setup
```bash
export DART_WORK_ROOT=${DART_WORK_ROOT:-$HOME/dart-smoke}
cd /workspace/dart-quiet   # or copy env_outline to cluster
bash env_outline/setup_mamba.sh
# mamba activate dart-scout   # name in conda_env.yaml
mkdir -p logs runs
# EDIT partition/account in slurm_*.sh
sbatch env_outline/slurm_hopper_smoke.sh      # P0: BC vs isotropic vs DART(α=1)
sbatch env_outline/slurm_alpha_sweep.sh       # P0: α ∈ {0.25,1,3,6}
sbatch env_outline/slurm_tr_sigma_sweep.sh    # P0: Tr(Σ) Wishart card
sbatch env_outline/slurm_dagger_compare.sh    # P1 lite: DAgger vs DART
sbatch env_outline/slurm_recovery_kick.sh     # recovery / covariate diagnostic
```

| File | Role |
|------|------|
| `env_outline/conda_env.yaml` | mamba sketch (Torch + Gymnasium/MuJoCo + SB3 optional) |
| `setup_mamba.sh` | env create; MuJoCo EGL/OSMesa notes |
| `slurm_hopper_smoke.sh` | P0 BC vs isotropic vs DART(α=1) — Ablation #2 + #1 proxy |
| `slurm_alpha_sweep.sh` | P0 α ∈ {0.25,1,3,6} — Ablation #1 proxy |
| `slurm_tr_sigma_sweep.sh` | P0 Tr(Σ) ∈ {0.005,0.5,5}+DART — Ablation #3 |
| `slurm_dagger_compare.sh` | P1 lite DAgger bake-off ≤20 iters — Ablation #4/#5 |
| `slurm_recovery_kick.sh` | recovery kicks + Fig.4-style shift diagnostic — #6 |
| `run_checklist.md` | GPU / TIME honesty preflight |
| `README.md` | cluster usage notes |

**Outlines only** — not sbatch’d from quiet box. Prefer **frozen** PPO/SAC/MLP supervisor stand-in; do **not** retrain paper TRPO for smoke.

### GPU / RAM / TIME honesty

| Workload | VRAM | Host RAM | Disk | Wall | Smoke |
|----------|------|----------|------|------|-------|
| Hopper BC / DART (≤20 demos, K=2) | **0–8 GB** (CPU OK) | 8–16 GB | <5 GB | **30–90 min** | **Y** |
| α sweep ×4 | 0–8 GB | 16 GB | shared | **2–6 h** | **Y** |
| DAgger vs DART matched | 0–8 GB | 16 GB | shared | **2–8 h** | **Y** |
| Recovery-kick eval only | 0–4 GB | 8 GB | ckpts | minutes | **Y** |
| Walker / Half-Cheetah scale | 0–8 GB | 16–32 GB | <10 GB | **4–16 h** | **Partial** |
| Offline noise → DP/ACT FT | **12–24 GB** | 32–64 GB | + demos | **8–24 h** | **Partial** |
| Humanoid-200 paper protocol | CPU days / GPU hours | 32 GB+ | — | **≫1 day** | **N** |
| Real HSR human study | — | — | robot | — | **N** |

**Honesty rule:** if `#SBATCH --gres=gpu:N` but no device → **fail**; log **α**, **tr(Σ̂)**, **supervisor return@collect**, **#retrains**, peak VRAM/CPU-time next to every return. Never claim Humanoid 3× or HSR 79% from Hopper Partial.

### Success criteria vs paper

| Paper claim | Feasible bar | Not required |
|-------------|--------------|--------------|
| DART ↓ covariate shift vs BC | Directional ↑ return **and/or** ↓ rollout−demo L2 on Hopper | Exact Fig. 2 curves |
| DART ≈ DAgger performance | Returns within noise on **one** loco env | Humanoid parity |
| DART cheaper / safer @ collect | ↓ wall **or** ↑ supervisor collect-return vs DAgger | Exact **3× / −5% / −80%** |
| Isotropic insufficient | DART > isotropic at matched scale | Paper “unsafe” qualitative |
| α matters | Interior α beats extremes | HSR 79% / 72% |
| Recovery demos help | Higher recovery SR under kicks | Real clutter push recoveries |
| Humans can use DART | Skip — note protocol only | 4-supervisor HSR study |

**Pass:** smoke card with **H-dart>bc** **or** **H-opt>iso** **or** **H-parity-dagger** (compute/safety) directional + α/Σ̂ logs.  
**Fail / overclaim:** “we reproduced CoRL Humanoid 3× / HSR 62%” without matching protocol/hardware.

### Scout success
Working Hopper (or synthetic) card with **BC vs isotropic vs DART(α)** **and/or** **Tr(Σ) sweep** directional returns + logged **α / Σ̂ trace / supervisor collect-return / wall time**.

---

## 8. Slide Maker brief

| Role | Weight / timing guide | Focus |
|------|----------------------|-------|
| **Archaeologist** | **3 · LONGEST (~14–16)** | CLEAR vs TENTATIVE lineage; ACT cite-then-reject; three noises |
| Stakeholder | filled | Protocol product; 49→79; 3×/−5%/−80%; safer than DAgger |
| Reviewer | filled | Lemma assumptions; α inverted-U; isotropic control; HSR n=4 |
| Empiricist | filled | Hopper α / Σ / Tr / DAgger-lite smokes + GPU honesty |
| Visionary | filled | BEHAVIOR recovery card; no Discord/Drive |

**Spine (paste into Slide Maker):**
1. **Label recoveries by disturbing the expert, not by deploying the learner** — protocol > architecture.  
2. **Key numbers:** HSR **49 / 79 / 72** (+62% rel); Humanoid **3×** wall, collect **−5%** vs DAgger **−80%**; α **inverted-U**.  
3. **Motion must-hits:** compounding over \(T\); action-noise recovery funnel; **DART data-side vs chunking control-side**; distinguish DART teleop noise / FM sample noise / chunking.  
4. **ACT CLEAR cite** then prefers chunking for fine teleop — complementary, not replacement.  
5. **BEHAVIOR’26:** schedule recovery rehearsals into demos beside π₀.₅/N1.7 training noise; Empiricist Hopper smokes validate schedule intuition only.

**GitHub title sketch:** `[ECE 605] - DART Making Archaeologist` (or seat if assigned)

Full Slide Maker brief: `/workspace/dart-quiet/SLIDE_BRIEF.md`

---

## Paths

| Artifact | Path |
|----------|------|
| This pack | `/workspace/dart-quiet/KANTA_PACK.md` |
| PDF | `/workspace/dart-quiet/KANTA_PACK.pdf` |
| Slide brief | `/workspace/dart-quiet/SLIDE_BRIEF.md` |
| Senior Research | `/workspace/dart-quiet/DELIVERABLE.md` |
| Summarizer | `/workspace/papers/dart-1703.09327-summary.md` |
| Ablations | `/workspace/papers/dart-1703.09327-ablations.md` |
| Paper notes | `/workspace/dart-quiet/paper_notes.md` |
| Env outlines | `/workspace/dart-quiet/env_outline/` |
| Paper PDF | `/workspace/papers/dart-1703.09327.pdf` |

---

*Pack synthesized 2026-09-29 PT (HST) for KanBot / ECE 605 Paper Manager — Nov 9 Data · Archaeologist=3 · all five seats produced.*
