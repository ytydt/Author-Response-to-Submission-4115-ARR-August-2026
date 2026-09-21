# Author Response — Submission 4115 (ARR August 2026)

*Decompose and Debate: Local Premise Interception for Multi-Agent Reasoning*

We thank all three reviewers. The reviews converge on one substantive question—whether DD's design is a principled instantiation of a stated formulation or a fortunate configuration—and on several concrete requests for missing controls and statistical clarification. During the discussion period we ran the requested controls and re-audited the statistics. We summarize the new evidence first, then answer each reviewer point by point. Unless stated otherwise, new experiments use the published pool (defined below), prompts, and scoring; every number below is item-level and will be released with per-question outputs.

---

## General Response

### What DD contributes, restated

The submission described DD through its role labels (guide, advocate, opponent, provers, judge), which invited the reading that the contribution is a persona workflow. It is not. DD's contribution is a **local state-transition interface** for multi-agent reasoning: it (i) exposes an intermediate commitment rather than a complete answer, (ii) requires at least one *incompatible* local candidate, (iii) compares bounded candidates under shared evidence, and (iv) makes the selected state consequential for downstream reasoning. Each operation targets a failure pathway that has since been independently documented for unguided whole-answer debate—sycophantic conformity, contextual fragility, and consensus collapse in which a correct answer already present in the pool is discarded by voting (arXiv:2605.00914). The operations, not the role names, are the design; the five debate functions plus a guide are one factoring of these four operations, multiplexed over three model-backed agent slots. We will fix four terms in the revision—*role/function*, *agent slot*, *backbone*, *protocol*—so that constraints of the formulation are never conflated with implementation choices.

The table below is the organizing device for our response and will be added to the Method section. It separates what the formulation **requires** (N), what is a **configuration variable** whose effect must be measured (T), and what remains a **free** design choice (F).

| Failure pathway (Fig. 1) | Required operation | Current function(s) | Existing ablation signature | Freedom class | New test in this response |
|---|---|---|---|---|---|
| Missed upstream commitment | Expose intermediate commitments | Guide / decomposition | −Decomposition: −17.6 pp acc., all error modes rise | **N** localization; **F** granularity, debatable decomposition | HD-style control (DjmY W1, VLJm b) |
| No incompatible alternative | Force an incompatible local candidate | Consultant + Opponent (medical: 2 opponents; math: 1 opponent, ≤2 candidates) | −Alternative: −18.8 pp recovery | **N** existence; **T** opponent count (task-conditioned) | Arity varied in both directions (VLJm b) |
| Wrong candidate selected | Bounded comparison under shared evidence | Advocate + Prover + Judge | −Symmetric proof: −22.2 pp recovery; −Incumbent refining: +4.4 pp harm | **N** comparable selection; **T** which backbone holds which slot | Homogeneous grid, role–model swaps, guide-assignment rule; role-free, self-assigned-role, and single-call inline-role topologies (VLJm a/W2, DjmY W2) |
| Verdict without consequence | Write the verdict into downstream state | Guide refresh | −Refresh: −8.1 pp | **N** state consequence; restoration vs. retention is an empirical outcome | Local repair audit (DjmY W3) |

### Model pools used in this response

Three backbone pools appear below. We name them by their role in the argument and their capability tier, and use these names throughout.

| Pool | Backbones (guide listed first) | LMArena text score, Sep 2026 | Position on the board | Used for |
|---|---|---|---|---|
| **Published pool** | Gemma-3-27B (medicine: Gemini-2.0-Flash-Lite) / Qwen2.5-VL-72B / Llama-3.3-70B | 1376 (1357) / 1316† / 1344 | 140–200 points below the current top (1514) | All main-table results; homogeneous grid, role–model swaps, HD-style control, opponent arity, repair audit, DD-open |
| **Mid-tier successor pool** | Qwen3.5-27B / Gemini 3.1 Flash-Lite / DeepSeek V3.2 | 1426 / 1433 / 1438 | 50–120 points above the published pool; still below the fast variants of current frontier families (e.g., Gemini 3.5 Flash-Lite, 1460) | SuperGPQA-Calc: prospective test of the guide rule; DD-lite transfer |
| **Current fast-tier pool** | Gemini 3.8 Flash / GLM 5.3 Flash / DeepSeek V4.1 Flash | 1504 / 1475 / 1445‡ | Fast variants of three 2026 frontier families: Gemini 3.8 Flash ranks 6th of 400, above GPT-5.5 (1485) and Gemini 3.1 Pro (1489); GLM 5.3 Flash sits at the level of GPT-5.5-Instant (1474) | SuperGPQA-Calc-Hard: frontier probe, topology controls, DD-lite transfer |

Scores are from the arena.ai English text leaderboard (2 Sep 2026); leaderboard entries reflect the models' default or high reasoning effort, whereas we ran the fast-tier pool at minimal effort (DeepSeek with reasoning off) to keep the probe inexpensive. †Qwen2.5-VL-72B is not listed; its text sibling Qwen2.5-72B-Instruct is shown. ‡DeepSeek V4.1 Flash is not yet listed; its predecessor V4 Flash is shown.

### New evidence added during the discussion period

1. **Homogeneous controls for DD, SoM-3, and an operational Hierarchical-Debate-style baseline** on Math Pro and College Math (all-Gemma vs. the published pool). Result: in the published pool, the same diversity that is worth +7.7 / +9.7 pp to DD is worth ≈ +1–2 pp to whole-answer SoM and ≈ 0 to HD-style refinement; homogeneous DD has no advantage over homogeneous SoM.
2. **Role–model swaps inside the published pool** (Math Pro, College Math) and a **guide-assignment rule** that follows from the protocol's single-guide structure, is checkable against Table 1, and was used prospectively on the mid-tier successor pool, with same-pool swaps confirming its prediction.
3. **Operational HD-style control** (debated decomposition + additive subtask refinement, no forced local alternatives / bounded adjudication / dependency-aware refresh), 3 runs × 2 datasets: −11.7 / −12.0 pp relative to DD, CIs exclude zero.
4. **Opponent count varied in both directions** (medical 2→1; Math Pro 1→2): neither direction changes accuracy significantly; existence of the counter-candidate remains load-bearing (−18.8 pp recovery).
5. **Local repair audit** of the fault-injection study, separating final-answer tolerance from restoration of the injected step (3×2 table).
6. **Per-module attribution for DD-open's reliability stack** (tool verification, flip guard, canonicalization), on DD and when attached to SoM.
7. **DD-lite hold-out**: gates frozen on Math Pro and applied without retuning to two held-out SuperGPQA calculation sets, one with the mid-tier successor pool and one with the current fast-tier pool.
8. **Role-free, self-assigned-role, and single-call inline-role topology controls on the current fast-tier pool**, with decomposition, write-back, and refresh held fixed: a symmetric role-free subtask debate and a self-assigned-role variant each score 6.7 pp below DD; inlining the role sequence into one call scores 8.9 pp below DD and reproduces the guide's Round-0 answers item for item; agents disagree spontaneously on 99% of subtasks but repair 2.8% of initially wrong items where DD repairs 11.1%.
9. **Statistical protocol corrections**: the Appendix C.2.3 interval, the per-dataset McNemar entry, and the Table F.2 tok/pp value; the primary inferential claim is placed at the cross-dataset layer, where it is fully reproducible.
10. **Terminology, naming**: a single name per dataset/setting, MedxpertQA-R introduced as a control-only set.

---

## Response to Reviewer VLJm

We appreciate the careful reading and the recognition that the accuracy, repair, and ablation results "all point at the proposed improvements." Your weakness concerns two things: whether the model composition/routing and the system design are principled, and whether the method survives stronger backbones. We address each.

### W1(a). Model composition and routing — "whether using the same model really produces worse results" and "why these models for these roles"

**Does the same model produce worse results?** Yes, and the new grid shows *where* the loss appears. We ran all-Gemma-3-27B versions of DD, SoM-3, and an HD-style baseline and compared them with the published pool.

| Protocol | Math Pro (N=74): homo → hetero | Δ | College Math (N=100): homo → hetero | Δ |
|---|---|---|---|---|
| **DD** | 76.6 → 84.2 | **+7.7 [+1.8, +14.0]** | 83.7 → 93.3 | **+9.7 [+4.7, +15.0]** |
| SoM-3 | 78.4 → 79.7 | +1.3 [−6.8, +9.5] | 87.0 → 88.3 | +1.3 [−3.7, +6.7] |
| HD-style | 71.6 → 72.5 | +0.9 [−4.5, +6.8] | 82.0 → 81.3 | −0.7 [−8.0, +6.7] |

Brackets are item-clustered 95% bootstrap intervals. The DD-minus-SoM *interaction* (how much more DD gains from the same diversity than SoM does) is +8.3 pp [+1.0, +15.3] on College Math and +7.0 pp [+1.4, +12.8] in an equal-weight two-dataset aggregate; DD-minus-HD is +8.6 pp [+2.7, +14.4]. A contemporaneous published-pool DD run independently reproduces a +7.2 pp homogeneous-to-heterogeneous shift. Conversely, **homogeneous DD has no measurable advantage over homogeneous SoM** (−1.8 / −3.3 pp) or, on College Math, over a contemporaneous single Gemma pass.

We read this as **complementarity in the published pool**: diversity supplies candidate variation; local candidate construction and adjudication convert it into consequential state. Giving the same three models to a whole-answer protocol does not reproduce DD's result, and giving DD a single model removes the variation it is designed to convert. This also answers the ensemble reading of DD: pool-matched SoM and pool-matched DT×2 (same three families, 45K tok/q) remain below DD by +4.5 / +5.0 and +6.8 / +9.0 pp respectively; on items that all three backbones solve incorrectly in Round 0, DD still recovers 30% / 67% versus 15% / 0% for SoM; and DD's per-item correctness is far less coupled to the strongest backbone than SoM's (φ = 0.51 / 0.32 vs. 0.84 / 0.67); the SoM coupling recurs, with a different strongest model, in a newer pool with a different strongest model (φ = 0.75–0.82). We do not claim DD exceeds the OR-of-backbones upper bound; the claim is that DD uses diversity differently.

The pattern is consistent with independent evidence on unguided whole-answer debate (arXiv:2605.00914, 7–8B models, plurality voting, no roles): homogeneous teams do not benefit from peer exchange, and, in that study's appendix, role-free heterogeneous teams show *negative* synergy under voting. That work explicitly lists structured-debate baselines as untested; our grid supplies them: adding roles without diversity does not help (homogeneous DD ≤ SoM), and adding diversity without local adjudication converts little of it (SoM, HD). We cite this only as consistent-with; our claims rest on our own controls.

**Why these models hold these roles.** The guide assignment follows from a structural property of the protocol rather than from tuning. In SoM all three models solve independently and the outcome tracks the strongest one (per-item φ with Gemma 0.84 / 0.67). In DD the Round-0 plan is written by the guide alone and the other models enter only through local counter-candidates and adjudication; hence **the pool's strongest independent solver must hold the guide slot**, or its capability reaches the debate only piecemeal. Which model that is can be read from Table 1: on Math, Physics, and Math Pro the CoT+Gemma row is the highest single-model row (tied on Logic), and on MedBullets CoT+Gemini is; on MedQA CoT+Llama is 2 pp above the Gemini guide, which we report as the one exception. The submitted text motivated routing only by cross-family diversity and did not state this constraint explicitly; we will.

We then used the rule prospectively. On **SuperGPQA-Calc**—a held-out set of 50 hard calculation items from SuperGPQA mathematics, disjoint from every other set we use—with the mid-tier successor pool, whose independent Round-0 accuracies are Gemini 66%, DeepSeek 74%, Qwen 80%, we chose Qwen as guide *before* running the swaps. Same-pool swaps confirm the prediction: Gemini as guide gives 76%, below the strongest single model; swapping only the opponents does not recover it (70%, 66%); Qwen as guide gives 84%, with SoM-3 at 78–80%. In the published pool, assigning a weaker model to the guide slot likewise costs −8.1 pp on Math Pro (the two other swaps: −2.7 and −5.4 pp), and the same assignment is the worst of three permutations on College Math. The rule is slot-relative, not a claim that a named backbone is required; cross-family routing between the incumbent and counter-candidate generators remains a stated design prior; and we do not claim a global optimum over assignments.

*Limitation we will add.* The evaluated DD advantage is scoped to pools in which the opponent and judge models trail the strongest model by a bounded margin (≤10 pp across Table 1 rows; 5–8 pp in same-period Round 0). In a lineup whose opponent trails by 26–37 pp, DD begins to overturn items all three models solve independently and falls below SoM. This is a scope condition supported by cross-pool comparison; it has not yet been tested by a within-pool intervention.

### W1(b). System design — multiple opponents; debatable decomposition

**Opponent count.** The premise "one can also have multiple opponents" is already part of the evaluated protocol: the medical configuration uses **two opponents** (two model families, each producing a local counter-candidate, with a three-way judge), while closed-form mathematics uses one opponent returning up to two candidates. The count is task-conditioned, not a fixed constant. Existing ablations show that the *existence* of an incompatible local candidate is load-bearing (removing it costs −18.8 pp recovery). We have now varied the *number* in both directions:

- **Removing** the second medical opponent (MedQA-hard and MedxpertQA-R controls, not main-table MedQA): **0.00 pp** (12 items flip each way) and **−4.00 pp** (McNemar p = 1.00 / 0.42). Logs confirm the second opponent never fired, while the remaining one was still selected on 87–89% of adjudicated subtasks.
- **Adding** an independent second opponent to closed-form Math Pro: **−6.8 pp** against a contemporaneous single-opponent run (p = 0.27).

Neither change is significant, so we retain the task-specific configurations; the ablations quantify sensitivity to arity rather than license a change. This is how we now treat every configuration variable in the table above: state the selection basis and measure sensitivity.

**Debating the guide's decomposition.** This is exactly the first stage of Hierarchical Debate, so we implemented an operational HD-style control (details under DjmY W1): the plan is debated and revised, subtasks are refined additively and concatenated, with no forced local alternatives, bounded adjudication, or dependency-aware refresh. It reaches 72.5% / 81.3% on Math Pro / College Math, **−11.7 / −12.0 pp** relative to DD, with item-level CIs excluding zero on both sets. Debating the decomposition alone does not recover DD's result; the gap is carried by the local interception loop. We will additionally report an additive variant that debates the decomposition *before* running DD, as a free design choice rather than a requirement. Whether the debate roles themselves can be dropped or left to the agents to self-assign is the remaining design question; we test it directly under W2 below.

### W2. Model capability and "hardcoded" roles — will this become obsolete with frontier models?

We separate two things that the question runs together. The **role–model mapping** (which backbone sits in which slot) is a configuration whose optimum can and will shift with model generation; we do not claim otherwise. The **local commitment interface** (expose → force incompatible candidate → bounded comparison → consequential write-back) is a claim about *where* inference intervenes, and its test is whether it still adds accuracy on tasks where a strong backbone still makes localized, correlated errors. Two observations bear on this directly.

First, the guide rule above is relative, not tied to a named model: when the pool changes, "the strongest independent solver" changes and the rule still applies (Qwen in the mid-tier successor pool; Gemini in the current fast-tier pool). Second, we ran two modernization probes:

- **Mid-tier successor pool**, SuperGPQA-Calc (n=50), gates frozen from Math Pro: full DD 84%, above the strongest single-model Round 0 (80%) and SoM-3 (80%).
- **Current fast-tier pool**, on **SuperGPQA-Calc-Hard**—45 further held-out SuperGPQA calculation items, disjoint from SuperGPQA-Calc, on which all three models of the mid-tier successor pool fail when solving independently: single-model CoT 20.0 / 17.8 / 17.8%; **DD 28.9%** vs. the guide's Round-0 20.0% and SoM-3 24.4%; DD-lite with the frozen Math Pro gate 26.7% at **0.19× SoM's tokens**.

Third, on the same fast-tier pool and set we tested directly whether the roles themselves are dispensable once agents are strong enough to take positions on their own—the reviewer's hypothesis—by holding the decomposition, the write-back, and the dependency refresh fixed and changing only the debate topology on each subtask. A **symmetric role-free debate** (three agents, identical neutral prompts, a second round after seeing each other's analyses, majority aggregation) scores 22.2%; a **self-assigned-role** variant, in which each agent picks one of DD's five functions before acting, also scores 22.2%; a **single-call inline-role** variant, in which one response must restate the incumbent, produce an incompatible candidate, argue both sides, and choose, scores 20.0%, identical item for item to the guide's frozen Round-0 plan; DD scores 28.9% and pool-matched SoM-3 24.4%. Spontaneous disagreement was not the bottleneck: in the role-free arm at least one agent departed from the incumbent on 99% of subtasks, yet the arm repaired 2.8% of the initially wrong items where DD repaired 11.1%, with no harm in either. Inlining the sequence recovered none of them. When agents chose their own function they opposed the incumbent only 7% of the time and mostly elected to prove or judge. On this set, therefore, what carries DD's gain is the *form* of the disagreement—forced incompatibility, symmetric evidence, and a verdict with downstream authority—rather than its occurrence, and collapsing that form into one call does not preserve it. The role-free arm is also cheaper (0.57× DD tokens; inline 0.18×), which we report rather than trade away by shortening DD.

These probes are mathematics-only; the preregistered medical study ({homogeneous, heterogeneous} × {reasoning on, off} on MedxpertQA-R and MedBullets-Hard) is planned for the revision and will repeat the topology comparison. What the probes show is that, with the current fast-tier pool, the protocol does not fall below the guide's own first pass or below whole-answer debate, that removing, un-assigning, or inlining the roles gives back part of the gain, and that the adaptive gate keeps most of the saving. Two further points frame the question. The paper's absolute numbers are from 2024–25 backbones; at the current frontier tier, single-agent CoT already saturates College Math Pro, so frontier evaluations of any debate method must be run on tasks with remaining headroom—which is why we exclude Math Pro from the preregistered study. And the predicted direction of frontier behaviour is not obviously toward "models figure out their own positions": preliminary evidence from the study cited above suggests conformity in unguided debate *increases* with scale, which, if it holds, makes a forced incompatible candidate more rather than less relevant. We will state in Limitations that DD's advantage is conditional on heterogeneous composition, on the strongest model holding the guide slot, and on unsaturated tasks, and that the cross-generation study is future work.

### Naming and presentation

We will adopt one name per object throughout: **Math Pro**, **College Math**, **MedBullets-Hard**, **MedxpertQA-R**, **SuperGPQA-Calc** and **SuperGPQA-Calc-Hard** (the two held-out sets introduced above), **DT** (Divergent Thinking), **true-open** (no-options). §4.1 will introduce **MedxpertQA-R** as a control-only set used for budget-matched comparisons and medical audits, with source and size, and a glossary will define *repair*, *recovery*, *mis-correction*, and *harm* once, with denominators stated in each table caption.

---

## Response to Reviewer DjmY

We thank the reviewer for a review whose every point turned into a concrete experiment or correction. We take them in order.

### W1. Comparison with Hierarchical Debate

We implemented an **operational HD-style control** that captures the operations the paper attributes to Hierarchical Debate—debated decomposition, one critic-and-revise pass on the plan, additive per-subtask refinement, concatenation into a final answer—while deliberately omitting DD's three distinguishing operations: forced incompatible local candidates, bounded local adjudication, and dependency-aware refresh. It uses the published pool and decomposition schema, three runs per dataset, zero missing items. It is an operational reconstruction, not a line-by-line reproduction of the cited system.

| | Math Pro (N=74) | College Math (N=100) |
|---|---|---|
| HD-style control (3 runs) | 72.5% | 81.3% |
| DD (paper, 3 runs) | 84.2% | 93.3% |
| Δ (HD − DD), item-level 95% CI | **−11.7 pp [−18.5, −5.0]** | **−12.0 pp [−17.7, −6.7]** |
| Majority-vote McNemar | p = 0.013 | p = 0.003 |

The gap is ≈12 pp on both sets with the same sign. The control also does not exceed SoM-3 (−4.1 / −8.0 pp). Since it retains decomposition and a debated plan, the missing 12 pp is attributable to the local interception loop itself. We will add this comparison to the main results and to the closest-work discussion; we have not run a homogeneous HD arm on the medical sets, which we will add.

### W2. Changing the models assigned to DD's roles

Two new experiments address this, and together they give actionable guidance for pools that differ from ours.

*Homogeneous vs. heterogeneous, protocol by protocol.* See the grid in our response to VLJm W1(a): replacing the pool with a single model costs DD 7.7 / 9.7 pp on Math Pro / College Math while costing whole-answer SoM ≈1–2 pp and HD-style refinement ≈0; homogeneous DD has no advantage over homogeneous SoM.

*Role–model swaps within the published pool* (Math Pro, N=74, contemporaneous default 83.8%, at the paper's level): Llama as guide −2.7 pp; judge slot moved to another family −5.4 pp; Qwen2.5-VL as guide **−8.1 pp**. On College Math (three permutations: 84%, 78%, 87%) the same assignment is again the worst. The ordering is consistent across both sets and matches the guide rule we derive in VLJm W1(a): the Round-0 plan has a single author, the guide, so the pool's strongest independent solver must occupy that slot. The rule was then used prospectively on the mid-tier successor pool and confirmed by same-pool swaps (Gemini as guide 76% < strongest single model 80%; Qwen as guide 84%).

*Guidance when the available models differ.* (1) Put the strongest independent solver in the guide slot—identifiable from a single-model CoT pass, as in Table 1. (2) Draw the counter-candidate generator from a different model family than the incumbent generator. (3) Keep opponent and judge models within a bounded margin of the guide (≤10 pp single-model accuracy in the pools we evaluated); with an opponent 26–37 pp weaker, DD starts to overturn items all models solve. (4) With a single available model, expect no advantage over whole-answer debate in the settings we tested. We will state (1)–(3) in the Method section and (3)–(4) in Limitations, and we will report the swaps with additional runs in the revision. Finally, the ensemble reading of these results—that DD merely aggregates three models' answers—is tested most directly by a control in which the three fast-tier models solve each subtask independently from a fixed decomposition and are aggregated by majority: it scores 22.2% against DD's 28.9% on the same set (details in our response to VLJm W2).

### W3. Are the injected errors themselves corrected?

The reviewer is right that final-answer tolerance does not establish that the corrupted step was repaired. We re-audited all 54 injected items per condition against the pre-injection, post-injection, and post-debate decompositions and now report three outcomes per condition: the injected step is **restored**, the step is **deleted** from the final plan, or the fault **remains in the plan** while the final answer is scored.

| Injection | Final answer correct (tolerance) | Injected step restored | Step deleted | Fault retained, answer correct |
|---|---|---|---|---|
| B1 drop key step | 90.7% | 50.0% (threshold-sensitive; 27.8–83.3% across similarity cutoffs) | — | 46.3% |
| B2 flip step result | 92.6% | **29.6%** (stable, 25.9–31.5%) | 13.0% | **53.7%** |
| B3 hallucinated dependency | 94.4% | **70.4%** (exact criterion) | — | 29.6% |

Three conclusions follow. First, the tolerance figures stand—over 90% of originally correct answers survive each injection. Second, restoration depends strongly on fault type: **dependency-graph corruption is mostly removed** (70.4%, exact match criterion), whereas **flipped step results are mostly not restored**—in 53.7% of B2 cases the final answer is correct while the flipped value remains in the plan, and in 13.0% the refresh removes the corrupted step altogether. Third, the B1 estimate is sensitive to how a rewritten step is matched to the deleted one and should be read as a range. We will report tolerance and restoration side by side, keep the "retained" and "deleted" columns, and revise the Method/Exp B wording: write-back remains a protocol step whose observed consequence we now measure, rather than a guaranteed repair of the injected step. We will also add a sidecar that stores the pre-injection content so that restoration can be audited directly in the release.

### W4. Separating local debate from DD-open's added features

DD-open adds three protocol-conditioned reliability modules—tool verification, a flip guard, and answer canonicalization—on top of the same local-interception loop. We now report the **within-protocol net effect of that stack** and a **per-module attribution**, on DD and when the identical modules are attached to SoM-3. The estimand is the stack's effect on a fixed host protocol, not a DD-vs-SoM comparison.

*Joint stack (N=61 true-open items per set).* On Physics-Pro, switching the stack off in DD while keeping the answer contract and failure hints fixed costs **−6.56 pp** (95.1% → 88.5%); the failure modes the stack was introduced to stop return (aspect drift on Q2, mean-without-rms on Q42). Attached to SoM-3, the same stack yields **0.00 pp** on Physics-Pro (one item gained, one lost) and **−1.64 pp** on Math-Pro (a correct symbolic answer overwritten by the guard). On Math-Pro, DD without and with the stack differs by +3.33 pp (91.7% → 95.0%).

*Per-module attribution* (reconstructed sequentially along debate → tool reconcile → flip-guard block → canonicalization, on the same trajectories):

| Host | tool_verify | flip_guard | canonical | net |
|---|---|---|---|---|
| SoM-3 Math | 0.00 | −1.64 | 0.00 | −1.64 |
| SoM-3 Physics | 0.00 | 0.00 | 0.00 | 0.00 |
| DD-open Math | −3.28 | +3.28 | −1.64 | −1.64 |
| DD-open Physics | 0.00 | 0.00 | 0.00 | 0.00 |

A 0.00 net is not a no-op: on SoM Physics the flip guard gains one item and loses one; on DD Math the tool reconcile step damages two items and the guard recovers one of them. Tool verification fires on every item but changes the final SoM answer on none; almost all accuracy changes come from flip-guard *rejections*; canonicalization is mostly notational, with one confirmed harmful value change (DD Math Q0). In DD, tool evidence also enters the judge inside the loop, which terminal reconstruction cannot undo; the joint −6.56 pp on Physics therefore includes in-loop effects that the terminal columns do not sum to. The conclusion the reviewer asked for is: the open-domain gain is carried by the interception loop with the stack acting as a signed, protocol-conditioned safety layer whose net contribution is positive on DD and zero-to-negative when bolted onto whole-answer debate. A per-flag switch-off rerun is scheduled for the revision.

### C1. DD-lite: were the gating rules evaluated on held-out Math Pro questions?

No—and we will say so explicitly. The mathematics gates (an easy-end conjunction on subtask count and confidence, and a hard-end conjunction on hedging and confidence) were selected on the full Math Pro set (N=74) from DD's own decomposition diagnostics, and the medical gate was selected on MedxpertQA-R; the Math Pro and MedxpertQA-R rows of the DD-lite table are therefore in-sample results. The evidence that the gates are not overfit is the transfer: (i) in the paper, the medical gate applied to MedBullets-Hard without retuning (+2 pp) and the mathematics gate applied to College Math (no trigger, no loss); (ii) newly, **frozen gates applied with zero recalibration** to SuperGPQA-Calc (n=50) with the mid-tier successor pool: the easy gate skips 22% of items with no change in accuracy (84%, above SoM-3's 80%); the union gate skips 60% of items at **0.66× SoM's tokens** and stays at 82%; and (iii) on SuperGPQA-Calc-Hard (n=45) with the current fast-tier pool, the union gate skips 78% of items and holds 26.7% vs. 28.9% for full DD and 24.4% for SoM-3 at **0.19× SoM's tokens**. In the revision we will (a) label the Math Pro and MedxpertQA-R rows as in-sample, (b) add a pre-registered two-fold split on Math Pro, and (c) report the cross-task transfers above.

### C2. Table F.2 tok/pp for DD

The reviewer's arithmetic is correct: 40,570 tok/q ÷ 5.3 pp = **7,655 tok/pp**. The printed 8,452 divided DD's token count by DD-lite's gain (4.8 pp) instead of DD's own. The DD-lite entry (5,170) is unaffected; the DD row, the Pareto caption, and the accompanying sentence will be corrected (DD is then 4.0× more token-efficient than SoM per point gained; DD-lite's 6× is unchanged). We have re-derived every ratio in Appendix F from the underlying token logs.

### C3. Appendix C.2.3: how runs were combined; how CIs and significance tests were computed

The reviewer's diagnosis is exactly right, and we correct the appendix as follows.

*The interval.* The printed 83.8%–85.1% is the **range of the three run-level Math Pro accuracies**, not a per-question bootstrap; it was mislabeled. The correct procedure—average each item's correctness over the three runs, then bootstrap over items (10,000 resamples, percentile)—gives DD **[77.0%, 90.5%]** and SoM **[71.6%, 87.8%]** on Math Pro, and a paired difference of **+4.5 pp [−1.8, +11.3]**. The intervals overlap; the "well separated" sentence will be removed. Repeated runs are combined at the item level, never by pooling 3×74 observations as independent.

*The per-dataset significance tests.* The Math Pro McNemar entry (χ² = 8.33) is arithmetically incompatible with the reported accuracies: for paired binary outcomes χ² ≤ |d|, where d is the difference in correct counts, and d ≈ 3–4 items on Math Pro. We could not reproduce the entry from any pairing of the stored runs and **withdraw it**. The valid item-level statistic for Math Pro is descriptive: majority-vote McNemar p = 0.11. The same bound explains why, at N = 74 and a 3–4 item gap, no per-dataset paired test could be decisive even in the most favourable cell; a single closed-form set is not where the claim should rest.

*Where the claim rests.* The primary inferential layer is the **cross-dataset paired analysis**, which reproduces exactly from the item-level vectors and does not depend on the scoring choices above: six paired DD−SoM differences of +4, +5, +2, +4, +7, +12 pp (mean +5.67, median +4.5), paired t = 3.96, **p = 0.011**, Cohen's d = 1.62, sign test 6/6 (p = 0.031). It is stable under either uniform SoM baseline (all SoM-3: p = 0.008; all SoM-8: p = 0.012) and, with the +12 pp MedBullets point held out, the t-test remains significant (p = 0.006, 5/5 direction) although the sign test no longer can (p = 0.0625 with five sets). Its unit is the dataset (n = 6), so it supports the cross-task claim, not any single-dataset claim. We will rewrite C.2.3 around this layer, report item-clustered intervals per dataset, keep the run-level Welch tests as supporting evidence with n = 3 stated, and report all runs on fixed denominators with unparsable or abstained outputs counted as errors. The ± column of Table 1 will be unified to a single stated convention (item-level SE or interval).

---

## Response to Reviewer Xcv6

We are grateful for the candid review and take the difficulty of the paper seriously; several of the changes below were prompted by it.

### What the paper does, in plain terms

Existing multi-agent debate methods let several language models each produce a complete answer and then argue about, or vote on, those complete answers. Many wrong answers, however, come from a single wrong intermediate step that all agents share, so complete-answer debate rarely finds it. Our method, Decompose and Debate (DD), first breaks a problem into a sequence of small steps with stated dependencies. For each step it forces one agent to propose a *different* answer to that step, has both sides justify their step-answer with the same evidence, lets a judge pick one, and then writes the chosen step-answer back into the plan so that later steps are recomputed from it. In experiments on six expert-level benchmarks in mathematics, logic, physics, and medicine, this raises accuracy over the strongest debate baseline by 5.7 points on average, fixes 58% of the errors that survive ordinary debate, and changes an initially correct answer to a wrong one only 11% of the time (versus 25–31% for prior adversarial debate methods).

### Clarity changes in the revision

- **One name per object**: Math Pro, College Math, MedBullets-Hard, MedxpertQA-R, SuperGPQA-Calc, SuperGPQA-Calc-Hard, DT, true-open; MedxpertQA-R introduced where the datasets are listed.
- **A four-term vocabulary** stated once and used throughout: *role/function* (what the protocol requires), *agent slot* (which conversation executes it), *backbone* (which model), *protocol* (the control flow).
- **A one-page overview** at the start of the Method: a worked example of one subtask passing through expose → counter-candidate → adjudication → write-back, followed by the design table shown in the General Response.
- **A metric glossary** for repair, recovery, mis-correction, and harm, with denominators in each caption.
- **Plain-language summaries** at the head of each experimental section stating what is compared and what would count as a negative result.

---

## Summary of revisions we commit to

1. Method: the design table (failure pathway → operation → function → freedom class), the four-term vocabulary, the guide-assignment rule with its Table 1 check and MedQA exception, and the statement that DD is option-blind at the subtask stage.
2. Results: homogeneous rows for DD, SoM-3, and HD-style in the main table and Appendix C; the HD-style comparison in the closest-work discussion; opponent-arity sensitivity in both directions; role–model swaps with additional runs; role-free, self-assigned-role, and single-call inline-role topology controls on the medical sets.
3. Exp B: tolerance and restoration reported side by side with retained/deleted columns; write-back described as a measured consequence.
4. DD-open: within-protocol net effect and per-module attribution; per-flag switch-off rerun.
5. DD-lite: in-sample label on Math Pro, pre-registered two-fold split, and zero-recalibration transfers.
6. Statistics: Appendix C.2.3 rewritten around the cross-dataset layer with item-clustered intervals; McNemar entry withdrawn; tok/pp corrected; unified ± convention; fixed denominators.
7. Limitations: DD's advantage is conditional on heterogeneous composition, on the strongest model holding the guide slot, on a bounded opponent/judge gap, and on unsaturated tasks; absolute numbers are from 2024–25 backbones; the preregistered frontier 2×2 study on medical hard sets is future work.
8. Naming, MedxpertQA-R introduction, glossary.
