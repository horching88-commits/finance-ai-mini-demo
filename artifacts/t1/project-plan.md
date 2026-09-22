# T1 Project Plan

## Project Goal

Develop a clear and reproducible research workflow for comparing three familiar asset classes: SPY (US equities), TLT (long-term US Treasury bonds), and GLD (gold). The goal is to establish a bounded, verifiable analysis process that later tutorials can build on with AI-assisted and agent-based workflows.

## Available Data

`data/etf_snapshot.csv` contains one row per illustrative ETF with the following columns (described in `data/data_dictionary.md`):

- `ticker` — ETF identifier (SPY, TLT, GLD)
- `asset_class` — broad asset type
- `expected_return_pct` — illustrative annual return assumption (%)
- `volatility_pct` — illustrative annual variability assumption (%)
- `max_drawdown_pct` — illustrative largest peak-to-trough loss (%, negative = loss)
- `expense_ratio_pct` — illustrative annual fund fee (%)

The dataset is synthetic teaching data: it is deliberately small and fixed, and its values are illustrative assumptions, not live or historical market observations.

## Expected Final Deliverable

A concise written comparison of the three asset classes based on the snapshot metrics, presented as a documented analysis artifact in this repository. The deliverable will describe how the illustrative assumptions differ across the assets and note the limitations of drawing conclusions from this data. The exact analysis steps and output format will be defined in a later bounded analysis task.

## Three Project Milestones

1. **Plan the project (T1, current)** — Create this written plan and have it reviewed; the plan is committed by the student, not by the agent.
2. **Analyze the snapshot data** — Perform the planned comparison of return, volatility, drawdown, and expense assumptions across SPY, TLT, and GLD using a verifiable workflow.
3. **Review and document results** — Verify the analysis output against the data, summarize findings with stated limitations, and finalize the deliverable in the repository.

## One Data Limitation

All numeric values are synthetic teaching assumptions, not real market data. They are not current quotations, verified historical estimates, or forecasts, so the analysis results must not be used as investment advice or as the basis for a real investment decision. The dataset also omits correlations, taxes, transaction costs, liquidity, currency exposure, and investor-specific constraints.

## Next Action

Have this plan reviewed by the student and save it with Git. After the plan is finalized, proceed to define the bounded analysis task that will compare the three asset classes using `data/etf_snapshot.csv`.
