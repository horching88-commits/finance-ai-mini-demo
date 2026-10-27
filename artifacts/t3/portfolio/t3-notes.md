# T3 notes — reviewed ETF portfolio experiment

## Agent check summary

Written by the independent Reviewer after the page was completed. The Reviewer's separate code, detailed results and reproduction commands are kept in `artifacts/t3/portfolio/review/` (`independent_review.py`, `review_results.txt`, `reproduction_commands.md`).

**Method.** The Reviewer wrote `artifacts/t3/portfolio/review/independent_review.py` from scratch using only the standard library; it does not import `scripts/t3_portfolio.py`, so it is not a repeat of the same starter command. It independently recomputes each window's portfolio value as 100 × Σ(initial weight × adjusted-close ratio to that window's first close), with drawdown = value / running peak − 1 and maximum drawdown as its minimum. It independently re-enumerates the full candidate set, re-applies the return-eligibility, selection, tie and fallback rules, and re-derives all four return/drawdown pairs.

**Actual checks performed (all passed):**

- Input integrity: manifest hashes match `design.json` and `test.json` bytes; all three share one source SHA-256 (`5d3f12cd…a711f`); design window 2021-09-01 → 2024-08-30 (754 closes), test window 2024-09-03 → 2026-08-31 (500 closes); price arrays align with the observation counts; manifest records the unchanged T2 CSV as source.
- Four independently computed pairs (decimal fractions, 12dp):
  - design baseline: ret `0.124258185581`, dd `-0.228129712772`
  - design candidate: ret `0.339369495164`, dd `-0.178135549858`
  - test baseline: ret `0.371293304600`, dd `-0.116445298514`
  - test candidate: ret `0.633011172263`, dd `-0.170091855877`
  - Each matches the frozen files (`baseline-design.json`, `candidate.json`, `evaluation.json`) within a 1e-9 float tolerance (the only discrepancy is one ULP from `sum` vs `math.fsum`, far below the brief's 1e-12 tolerance and invisible at display precision).
- Design selection: 67 candidates enumerated with no duplicates and the exact equal-thirds allocation last; independent eligible count 39 matches `candidate.json`; the independent selection reproduces the frozen weights SPY 0.4 / TLT 0.0 / GLD 0.6; candidate design return ≥ baseline within 1e-12; drawdown loss strictly smaller beyond 1e-12; fallback (retain baseline) correctly not applied.
- Freeze/evaluation handoff: `candidate.json` byte hash equals the hash approved in chat (`03a0ef1a…cf2095`); `evaluation.json` records the same candidate SHA-256; `test_sha256` matches `test.json` bytes; evaluation reused the frozen initial weights; the evaluation window is the test window; rules in `candidate.json` match the fixed brief rules.
- Page consistency: the page contains the frozen allocation (SPY 40% / TLT 0% / GLD 60%), all four metric pairs at display precision, the frozen candidate hash, and exactly two SVG charts with four polylines (baseline + candidate × nav + drawdown).

**Key findings.** The frozen candidate reduced the design-period maximum-drawdown loss (−17.81% vs −22.81% for equal thirds) while keeping design return at least equal (33.94% vs 12.43%). In the test window the candidate again returned more (63.30% vs 37.13%) but had a deeper maximum drawdown (−17.01% vs −11.64%), so the design-period drawdown advantage did not persist. The page reports this honestly rather than claiming a lasting improvement.

**Unresolved issue.** Rendered-page check: the Reviewer's browser sandbox blocks the `file://` protocol and a local static server does not persist in this environment, so the Agent could not complete an automated in-browser render check. The student opened `docs/t3/portfolio/index.html` in their own browser and confirmed it renders correctly. Source-level page checks (values, charts, hashes) all pass; the automated browser verification remains an environment limitation, not a page defect.

## My three decisions

Recorded by the Leader in the student's exact words from chat.

1. **Plan approval** — "I approve the plan. METHOD has clear owners, a separate Reviewer and a real freeze gate; OUTPUT includes my drawdown explainer; CHECKS will provide independent evidence. I approve only the design stage — the test evaluation still needs my separate candidate-freeze approval."

2. **Dependency comparison and candidate freeze** — The student's waiting prediction was checked against actual evidence: the test evaluation ran only after the candidate-freeze approval, and the `evaluate` command verified the approved hash before opening the test input, matching the expectation. Freeze decision — "I approve freezing this candidate — SPY 40% / TLT 0% / GLD 60%, SHA-256 03a0ef1a…cf2095. Reason: it satisfies the fixed rule (design return 33.94% ≥ benchmark, and a strictly smaller design drawdown loss, −17.81% vs −22.81%), which I can explain by the move away from TLT and toward GLD. Please bind this approval to this exact file, run the test evaluation, then build the page because design, test and review are now on track, so you may continue to the test stage."

3. **Acceptance or remaining concern** — "I accept the page as final. The four common checks and my reader requirement pass with no important unexplained discrepancy, and I can explain the trade-off myself. Reason for accepting despite underperformance: in the test period the candidate kept a higher return (63.30%) but did not retain the smaller drawdown (−17.01% vs the benchmark's −11.64%), so the design advantage did not persist. Underperformance is valid, so I accept. Remaining concern (disclosed, not blocking): the rendered-page check could not run in the Reviewer sandbox and was verified manually instead."
