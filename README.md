# The Legacy Effect — Posthumous Music Consumption Analysis

## SC3021 Data Science Project

### Overview

This project investigates how an artist's death impacts their popularity, and what factors drive the magnitude and duration of posthumous popularity changes. We analyse 30 deceased musicians using data from Wikidata, Google Trends, and the Wikipedia Page Views API to quantify the "legacy effect" — the measurable surge in public attention following an artist's death.

### Research Question

> How does an artist's death impact their popularity, and what factors drive the magnitude and duration of posthumous popularity changes?

### Key Findings

- A **26× spike** in average Google search interest during the week of an artist's death, followed by a gradual decay over 12 weeks.
- **Pre-death fame does not significantly amplify the effect** (Mann-Whitney U, p = 0.287). Less-famous artists often show larger *proportional* spikes due to their lower baseline.
- The strongest predictor of spike magnitude is **pre-death search interest** (Spearman rho = −0.833, p < 0.001) — a largely mathematical artefact of ratio construction.
- A Random Forest regression model failed to predict spike magnitude from biographical features (R² = −5.39), indicating that the legacy effect is driven by factors outside our dataset (media coverage, cause of death, social media virality).

### Data Sources

| Source | Role | Coverage |
|--------|------|----------|
| **Wikidata (SPARQL)** | Biographical data (name, dates, genre, country) | 50 artists |
| **Google Trends (pytrends)** | Primary popularity signal (weekly search interest) | 29 artists |
| **Wikipedia Page Views API** | Secondary popularity signal (daily article views) | 22 artists (post-2015 deaths) |

### Methodology

The project follows the **ASK → PREPARE → PROCESS → ANALYSE → SHARE → ACT** data science framework:

1. **Ask** — Define the hypothesis and analytical goals (descriptive, diagnostic, predictive).
2. **Prepare** — Explore 6 data sources, select 3 based on coverage and quality.
3. **Process** — Query Wikidata, collect time-series data, integrate via SQL joins in SQLite, clean and engineer features.
4. **Analyse** — Four analyses: visualisation of posthumous curves, Spearman correlation, Random Forest regression, Mann-Whitney U hypothesis test.
5. **Share** — Summary dashboard and narrative for music estate managers.
6. **Act** — Actionable recommendations for estate managers, streaming platforms, and researchers.

### Project Structure

```
SC3021_Project.ipynb       — Main notebook (code + report)
legacy_effect.sqlite       — SQLite database with all integrated data
artists_checkpoint.csv     — Wikidata artist data
trends_data_checkpoint.csv — Google Trends time-series data
wiki_views_checkpoint.csv  — Wikipedia page view data
weekly_data_final.csv      — Integrated weekly time-series table
artist_summary_final.csv   — Aggregated artist-level summary table
preparation_overview.png   — Data preparation pipeline diagram
```

### Setup & Dependencies

This project runs on **Google Colab** (Python 3). Required packages:

```
pip install pytrends sparqlwrapper scikit-learn scipy matplotlib pandas numpy
```

No API keys are required — all data sources are open access. Google Trends may rate-limit requests; the notebook includes retry logic and checkpoints for resilience.

### How to Run

1. Open the notebook in Google Colab.
2. Run all cells sequentially. Data collection cells (Google Trends, Wikipedia Page Views) take approximately 15–20 minutes total due to rate-limit delays.
3. If the notebook is restarted mid-run, uncomment the checkpoint reload cells to resume without re-collecting data.

### Limitations

- Sample size is small (29–30 artists) due to Google Trends rate limiting.
- Wikipedia page views are only available from July 2015, limiting cross-source validation for older deaths.
- The spike ratio metric is sensitive to baseline levels, which confounds interpretation.
- Biographical features alone are insufficient to predict the legacy effect; future work should incorporate media coverage, cause of death, and social media data.
