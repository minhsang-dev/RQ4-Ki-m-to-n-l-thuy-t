# RQ4 gap check (v3)

**Coverage:** 40 works, pages opened this session; web search only, so absence is not proof.  [34] Vejling and [35] Xu are in Group 4.

**Source tier + row tier for context/ICL models?** Not found. Row-level only: Jhuma 2025, Dolejš 2026, PriorPoison. Source-level only: WeiDetect, FLTrust, Ma–Olshevsky, Fang et al., **Vejling et al. 2025** (conformal contamination tests with Storey-BH FDR control across data agents, then BH row selection), **Xu et al. 2016** (knockoff FDR control for biased annotators). Vejling is the only work found with both an agent test and a row selection, but for acquisition before training, not for a model's context.

**Bound like Proposition 2?** Not found. Nearest: kNN certification (Jia et al.), DPA/Rosenfeld (abstract only). Vejling and Xu bound the false discovery rate, not the poison share; no power result located, which Proposition 2's f*, β need.

**Adaptive attackers who know the defence?** Koh et al., Miao et al., FLTrust, Fang et al. Never against a source+row pairing. Vejling lists advanced adversarial attacks as future work; Xu models a non-strategic bias.

**Threats to H4a/H4b**
1. Defence-aware poison evades row filters (Koh uses influence functions on the attacker side).
2. Peer audits can be disguised (Miao) or beaten by cheap-signal coordination (Zhang et al., abstract-level).
3. RONI misses focused attacks (Nelson); V_t must contain target families.
4. Real and adversarial drift look alike (Korycki).
5. Honest-but-different sources look like outliers (WeiDetect, Paudice). Vejling flags such agents by design (category "honest agents with unintentionally biased data").
6. Peer methods need item overlap (Ma–Olshevsky).
7. Confident learning assumes class-conditional noise.
8. Colluding duplicates bypass a cap (PriorPoison, one dataset).
9. Trusted references are assumed clean everywhere: Vejling needs clean local data, FLTrust a root set.
10. Upstream label sources can be manipulated (AndroVenom); Deep k-NN was inconsistent.
11. **Exchangeability:** Vejling's validity needs calibration data exchangeable with inliers and states this limits time-series use; daily drift breaks it. Xu needs sparse bias.
12. Hub-poison damage to TabFMs is small (−3.7/−3.3 pp); H4a needs a strong attack.

**GAP_NARROW** — error-controlled source tests exist (Vejling, Xu), so novelty cannot rest on a statistical source audit alone. What was not found: a peer-based (reference-free), non-exchangeable same-day audit that keeps honest-but-different sources, combined with a row-influence filter and per-source cap for tabular in-context models, plus a bound linking cap and audit power to poison share.
