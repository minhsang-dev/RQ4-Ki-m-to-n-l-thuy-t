# RQ4 baseline specifications (v1, no code run)

Facts come from pages/PDFs opened in this session. **Adaptation items are choices, not facts.** Defaults not seen on a page are marked [VERIFY]. Costs labelled "estimate" are my own reasoning, not measured.

Common setting for the adaptations: daily context of k = 2,000 rows, labels arrive with delay, K label sources, held-out labelled set V_t, predictors = TabFM (frozen, context = rows) and LightGBM (retrained).

---
## 1. RONI (Reject On Negative Impact)

**Official code.** None found for the original. Closest public implementation: `RONIDefense` in IBM/Trusted-AI Adversarial Robustness Toolbox, https://github.com/Trusted-AI/adversarial-robustness-toolbox (MIT; PyPI https://pypi.org/project/adversarial-robustness-toolbox/ shows 1.20.1 released 7 Jul 2025, previous 1.20.0 on 30 Jun 2025). Docs signature from https://adversarial-robustness-toolbox.readthedocs.io/en/latest/modules/defences/detector_poisoning.html: `RONIDefense(classifier, x_train, y_train, x_val, y_val, perf_func='accuracy', pp_cal=0.2, pp_quiz=0.2, calibrated=True, eps=0.1)`; methods `detect_poison`, `evaluate_defence`, `get_calibration_info`, `is_suspicious`. The page does not explain `pp_cal`, `pp_quiz`, `eps` [VERIFY in source].

**Algorithm (original, Nelson et al. 2008, https://www.usenix.org/event/leet08/tech/full_papers/nelson/nelson_html/, section 5.1).**
1. For a candidate Q, sample a training set T (20 messages) and a validation set V (50 messages) from the initial pool.
2. Train one filter on T and one on T + Q; measure the change in misclassifications on V.
3. Repeat 5 times and average.
4. Reject Q if the average impact is "significantly negative". No numeric cutoff is given in the paper.

**Hyper-parameters.** |T| = 20, |V| = 50, repetitions = 5, threshold unspecified (paper); ART defaults above.

**Clean data needed.** Samples come from the "initial pool"; the page does not say the pool is verified clean. In practice a labelled, trusted pool for T and V.

**Cost per row.** Paper: 2 trainings x 5 repetitions = 10 fits per candidate. Estimate for a TabFM: no weight fit, so 2 forward passes over V per repetition (context with / without Q), i.e. 10 passes per candidate row if the paper's protocol is kept; for k = 2,000 that is about 20,000 forward passes per day before batching.

**Known weakness (from the paper).** Detects broad dictionary attacks (100% of attack mails, no non-attack mails flagged in the preliminary test) but not focused attacks, whose effect does not show on the training pool.

**Adaptation choices.**
- C-R1: the "initial pool" = the V_t split by time (trusted, labelled); T = random subset of the current context.
- C-R2: replace "train" by "swap row into / out of the TabFM context"; for LightGBM, warm-start refit on T + Q.
- C-R3: the impact statistic is the change in log-loss or in target-family recall on V_t (the paper uses error count); the threshold set by a calibration batch of known-clean rows (as ART's `calibrated=True` suggests) [VERIFY].
- C-R4: label delay: evaluate a row only after its label matures, otherwise hold it out of the context (a holding policy, not part of RONI).
- C-R5: V_t must contain the targeted families, otherwise RONI is blind to targeted flips (inference from the focused-attack limitation).

---
## 2. Confident learning (cleanlab)

**Official code.** https://github.com/cleanlab/cleanlab, Apache-2.0. Latest release per PyPI (https://pypi.org/project/cleanlab/): 2.9.0 on 13 Jan 2026; 2.8.0 on 8 Jan 2026; 2.7.1 on 27 Feb 2025. Docs: https://docs.cleanlab.ai/stable/cleanlab/filter.html.

**Algorithm (Northcutt et al., https://arxiv.org/pdf/1911.00068, section 3).**
1. Obtain out-of-sample predicted probabilities P (n x m), e.g. by cross-validation (the paper fixes 4 folds in its experiments).
2. Per-class threshold t_j = mean self-confidence of examples labelled j.
3. Confident joint C[i][j] = number of examples labelled i whose p(j) >= t_j (and j is the argmax among classes above threshold).
4. Calibrate C to a joint Q estimate (rows rescaled to observed label counts, total to n).
5. Prune: prune-by-class, prune-by-noise-rate, or the off-diagonal members of C (the paper's default "CL: C_{y~,y*}").

**Hyper-parameters / defaults (docs).** `find_label_issues(labels, pred_probs, *, return_indices_ranked_by=None, rank_by_kwargs=None, filter_by='prune_by_noise_rate', frac_noise=1.0, num_to_remove_per_class=None, min_examples_per_class=1, confident_joint=None, n_jobs=None, verbose=False, multi_label=False)`. `filter_by` options: prune_by_noise_rate, prune_by_class, both, confident_learning, predicted_neq_given, low_normalized_margin, low_self_confidence. Fold count of the cleanlab wrapper class: [VERIFY].

**Clean data needed.** None as a separate set, but needs a model trained on the noisy data and out-of-sample probabilities. Assumes class-conditional noise (flip rate depends only on the true class). An attacker who flips a target family chosen by features, or who flips more than the model's confidence margin, breaks this.

**Cost per row.** Paper (ImageNet, ResNet): about an hour for cross-validation, under a minute to find errors, i.e. the cost is the cross-validated prediction; the counting step is cheap. Estimate for k = 2,000: one TabFM pass per fold, with the fold's rows out of context.

**Adaptation choices.**
- C-C1: pred_probs from TabFM with K-fold context splitting of the day's rows plus the retained context (folds respect source: leave-one-source-out is an alternative).
- C-C2: run on the new batch only, with thresholds t_j estimated on the retained clean context so that a mostly poisoned batch cannot shift them.
- C-C3: flag, do not drop, then hold flagged rows for verification (matches the outline's "held pending verification").
- C-C4: report both `prune_by_noise_rate` (default) and `confident_learning`.

---
## 3. kNN label cleaning (Paudice et al.)

**Official code.** None found on the paper (no repository link in the text I searched) [VERIFY].

**Algorithm (https://arxiv.org/pdf/1803.00992, Algorithm 2).** For each training point i, find its k nearest neighbours (Euclidean) among the other points; if the share of the most common label among them, conf >= eta (0.5 <= eta <= 1), set y_i to that mode label; optionally repeat until nothing is relabelled.

**Hyper-parameters.** k chosen on a trusted validation set; eta = 0.5 in their experiments; k and eta sensitivity shown (larger k better at high poison, smaller k at low poison; larger eta relabels less). Evaluated only for binary classification.

**Clean data needed.** A small trusted validation set (not poisonable by the attacker) to choose k. The authors note it can relabel genuine points in overlapping class regions and slightly hurts clean performance.

**Cost per row.** Estimate: O(m d) per query against m points; naive all-pairs O(m^2 d), for m = 2,000 negligible; with the retained context of size m, the new batch costs O(k m d).

**Adaptation choices.**
- C-K1: feature space = standardised numeric flow features (or TabFM embeddings if available) [choice].
- C-K2: "relabel" mode and "drop/hold" mode are both run; only hold-mode is comparable to the other filters' output.
- C-K3: neighbours drawn from the retained clean context, not the poisoned batch, to prevent coordinated poisoned points from vouching for each other (the attack Koh et al. 2022 use: poisoned points placed near one another).
- C-K4: k, eta tuned on V_t.
- C-K5: expect distribution drift from filtering. In AndroVenom (https://arxiv.org/pdf/2503.11841) Deep k-NN filtering cut TPR of a gradient-boosted model by about 20% (FPR 0.1%, k = 11) while raising an NN's by 20%, and did not stop the targeted attack; the authors suspect filtering-induced drift. Report LightGBM and TabFM separately and log the share of clean rows removed.
- Related variant: Deep k-NN (Peri et al., cited by Lan et al., not opened) and the KSSD relabelling that Taheri et al. (https://arxiv.org/pdf/1908.04473) criticise for low accuracy; neither is specified here.

---
## 4. Source-level method A: M-MSR (Ma and Olshevsky)

**Official code.** https://github.com/maqqbu/MMSR; the README names `MainProgram.m` (suggests MATLAB, inferred); licence not shown on the page; last commit date not shown (8 commits) [VERIFY]. Paper: https://arxiv.org/pdf/2010.12181.

**Algorithm (Algorithm 1, M-MSR).** Workers' pairwise correlation matrix X is rank-one in expectation under single-coin Dawid-Skene. Reveal entries on a set Omega (pairs with shared tasks). Iterate T times: update u_i(t+1) as a convex combination of X_ij / v_j(t) over j in Omega_i after discarding the F largest and F smallest values; update v_j(t+1) from X_ij / u_i(t+1) likewise; output u v^T. A sign-determination post-step recovers sign, then workers' skills give weighted labels.

**Hyper-parameters.** F (max adversaries per neighbourhood), T (iterations), initial v(0) > 0. Tolerated adversary fraction on random graphs about 1/[(2+eps) log log n]; the recovery condition is (2F+1)-robustness of the revealed graph (Theorem 2).

**Clean data needed.** None. Needs overlap: each pair of workers must label common tasks.

**Cost per row.** Not reported on the pages I read [VERIFY]; per-iteration cost is linear in |Omega| (estimate).

**Adaptation choices.**
- C-M1: sources rarely label the same flow. Create overlap by matching flows across sources in feature space (nearest neighbour with same-day timestamps), then treat the match as a "common task". This changes the model (labels agree only if the same class is expected for near flows); choice, not in the paper.
- C-M2: K is small (outline: m_max < K/2), so a graph over K sources is dense and F must be < K/2 by construction.
- C-M3: mark a source "undetermined" when its overlap with others is below a threshold (outline wording: "chưa xác định").

---
## 5. Source-level method B: MWA / MIE (Fang et al.)

**Official code.** Not found in the PDF text or search [VERIFY]. Paper: https://arxiv.org/pdf/2102.09171 (WWW 2021).

**Algorithm.**
- MWA (no knowledge of attack): CRH-style worker weights; per item sort workers' values, split into L groups, take the weighted average of each group, output the median of the L averages.
- MIE (knows attack and alpha): find the floor(alpha |M|) workers whose removal changes the estimated values of all items the most (Algorithm 5), remove them, run CRH on the rest (Algorithm 6).

**Hyper-parameters.** L (number of groups); alpha (assumed malicious fraction) for MIE; initial weights equal.

**Clean data needed.** None, but MIE needs alpha and the knowledge that an attack is happening.

**Cost per row.** Not reported on the pages I read [VERIFY]; MIE solves an influence-maximisation subset search (estimate: costlier than MWA).

**Adaptation choices.**
- C-F1: "item" = a cluster of near flows (as C-M1); categorical labels, so use the CRH categorical form.
- C-F2: MIE's alpha set to m_max/K (outline's assumption) to give it the same information as the proposed method; reported as an oracle variant.
- C-F3: use MWA as the knowledge-free variant.

---
## Notes
- ICLShield (https://arxiv.org/pdf/2507.01321) is not a filtering baseline: it appends k clean demonstrations (k/2 by cosine similarity of LLM embeddings to the poisoned demonstration, k/2 by P(y|x, poisoned demo)) drawn from a clean pool D. A TabFM analogue (append trusted rows near the target region of the context) is a possible extra comparator, a choice not specified in the outline.
- FLTrust (https://arxiv.org/pdf/2012.13995) is the reference point for "a clean root dataset removes the honest-majority limit" (read abstract/intro only), and is a candidate in place of Method B if a trusted V_t is already assumed; not specified further here.
- The optional data-valuation baseline (Data Shapley, KNN-Shapley, TracIn) was read at abstract level only; no spec is given.
