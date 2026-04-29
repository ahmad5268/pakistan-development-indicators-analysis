# Which Sector Tracks Pakistan’s Economy Most Closely?
## A Data Story from World Bank Indicators (1990-2018)

## Intro
Economic growth is often discussed as a single number, but real development happens across sectors. In Pakistan, agriculture, health, and education have all changed significantly over the last few decades. The key question is: **which sector moves most closely with economic outcomes over time?**

In this analysis, **we** use World Bank development indicators to compare sector trends against GDP indicators and test whether the relationships are immediate or delayed. The goal is not just to show charts, but to build a clear, reproducible workflow: load, clean, validate, and interpret.

## Dataset Introduction
The dataset comes from the World Bank indicator export for Pakistan, stored in `indicators_pak.csv`.

After initial loading:
- Raw usable rows: **98,582**
- Year range: **1960 to 2026**
- Unique indicators: **4,394**

For this project, **we** narrowed the analysis to **15 indicators** across:
- Agriculture (share in GDP, employment, cereal yield, agricultural land, food imports)
- Health (life expectancy, child mortality, DPT immunization)
- Education (literacy, primary enrollment, secondary enrollment, education expenditure)
- Economy (GDP growth, GDP per capita current, GDP per capita constant)

**We** reshaped the data to year-wise format and focused on **1990-2018** for better overlap and comparability, producing a table of **29 years x 15 indicators**.

Coverage is strong for most macro and health indicators, but weaker for some education metrics:
- Literacy rate coverage: **37.9%**
- Primary enrollment: **69.0%**
- Secondary enrollment: **65.5%**

This uneven coverage directly informs the cleaning strategy.

## Data Cleaning
Before analysis, **we** performed quality checks to avoid silent errors:

1. **Removed structural noise**  
The file includes a metadata-style row (`#country+name` etc.), which was excluded.

2. **Type conversion and filtering**  
`Year` and `Value` were converted to numeric, and only Pakistan rows were kept.

3. **Duplicate integrity check**  
- Exact duplicate rows found: **18,172**
- Conflicting duplicates (same year + indicator code but different values): **0**

Since duplicates were exact copies, deduplication was safe and lossless.

4. **Missing-value handling with limits**  
**We** used interpolation only for short gaps in smoother indicators and kept sparse education series mostly as observed. Remaining missingness is concentrated in literacy and enrollment indicators, which is expected from source reporting gaps.

This cleaning approach balances completeness and realism, avoiding over-imputation.

## Exploratory Data Analysis (EDA)
### 1) Trend story over time
The multi-panel trend chart shows clear long-run structural movement:
- Health outcomes improve steadily (higher life expectancy, lower child mortality).
- Agriculture’s role in GDP is relatively weaker over time compared with social indicators.
- GDP per capita (constant) trends upward with visible fluctuations.

This suggests stronger long-run alignment between development indicators and income levels than with short-term growth volatility.

### 2) Correlation matrix: levels vs first differences
**We** computed two correlation views:
- **Levels** (long-term alignment)
- **First differences** (year-to-year movement)

Key level results with GDP per capita (constant):
- Strong positive: cereal yield (**0.959**), life expectancy (**0.956**), primary enrollment (**0.952**), secondary enrollment (**0.931**)
- Strong negative: child mortality (**-0.958**), food imports share (**-0.622**)

Key results with GDP growth (annual):
- Much weaker relationships overall
- Largest negative in levels: agriculture GDP share (**-0.444**)
- Moderate positives in differences: cereal yield (**0.310**), DPT immunization (**0.190**)

Interpretation: long-run development co-moves strongly with income levels, while short-run growth is noisier and less tightly linked.

### 3) Sector-vs-economy scatter plots
The scatter panels reinforce the same pattern:
- Health and education indicators show clearer positive structure with GDP per capita.
- Agriculture share has a weaker or mixed linear pattern.
- Time coloring shows that much of the relationship is driven by long-term progression over years.

### 4) Lag analysis (supporting question)
Lag correlation against GDP growth shows timing differences:
- `Agri_GDP_Share`: best lag **+5**, `r = 0.554`
- `Food_Imports`: best lag **-1**, `r = -0.345`
- `Life_Expectancy`: best lag **+3**, `r = 0.295`
- `Primary_Enrollment`: best lag **+2**, `r = -0.451`

This suggests some sector effects appear with delay, but signs and strength vary by indicator.

## Conclusion
Across 1990-2018, the clearest signal is that **health and education indicators align more strongly with long-run economic level (GDP per capita) than agriculture does**. Agriculture still matters, but its relationship with short-run GDP growth is less stable and more context-dependent.

The analysis also shows why one metric is not enough:
- Levels capture structural development
- First differences capture short-run co-movement
- Lag analysis captures timing dynamics

Most importantly, these are **associations, not causal claims**. But as a sector-comparison data story, the evidence points to stronger long-run economic alignment with human development indicators, especially health and schooling outcomes.

---

## Final Headline + Subtitle Options
1. **Which Sector Tracks Pakistan’s Economy Most Closely?**  
   *A data-driven comparison of agriculture, health, and education using World Bank indicators (1990-2018).*

2. **Pakistan’s Development Story in Numbers**  
   *How sector-level progress in health, education, and agriculture aligns with economic performance over time.*

3. **Beyond GDP: Mapping Pakistan’s Sectoral Development**  
   *A time-series investigation of whether agriculture, health, or education shows the strongest economic relationship.*

4. **Health, Education, or Agriculture: What Moves With Growth?**  
   *A reproducible EDA of Pakistan’s development indicators from the World Bank.*

5. **From Indicators to Insight: Pakistan’s Development-Economy Link**  
   *Comparing long-run and short-run relationships across key sectors, 1990-2018.*

**Recommended for Medium:** Option 1.

## Figure Captions (Mapped to Notebook Output Cells)
- **Cell 12 (Missingness Heatmap):** Missing-data structure after cleaning and constrained interpolation. Most macro and health indicators are complete, while literacy and enrollment variables remain sparse.
- **Cell 14 (2x2 Trends Dashboard):** Pakistan’s sectoral trajectories from 1990-2018: agriculture share dynamics, improving health outcomes, mixed education coverage, and rising GDP per capita with volatile growth.
- **Cell 16 (Correlation Heatmap + Tables):** Correlation analysis in two views: long-run level alignment and year-to-year change alignment. Strong level associations appear for health/education with GDP per capita, while GDP growth relationships are weaker.
- **Cell 18 (Sector vs Economy Scatter Panels):** Bivariate sector-economy relationships with time coloring. Health and education show clearer positive structure with GDP per capita than agriculture share.
- **Cell 20 (Lag Correlation Plot):** Lead-lag testing against GDP growth suggests timing differences across indicators, with some sector signals appearing several years before/after growth changes.

## Key Takeaways
- The strongest relationships appear between **human development indicators** (health and schooling) and **GDP per capita levels**.
- **Agriculture’s share of GDP** shows weaker and less stable alignment with short-run growth.
- Correlations in **levels** are much stronger than correlations in **first differences**, highlighting the difference between long-run structure and short-run noise.
- Data quality checks were essential: the raw file contained many exact duplicates and uneven indicator coverage.
- These findings are **associational, not causal**, but they provide a clear sector-comparison narrative for Pakistan’s 1990-2018 development path.

---

## New Modules
- **Statistical Inference:** This module adds simple hypothesis tests, confidence intervals, and a few correlation checks so we can see whether some patterns are probably meaningful and not just visual coincidence. It keeps the explanations plain and student-friendly, with cautious conclusions.
- **AI/ML Module:** This module adds a beginner-level prediction task for adjusted net savings using a small set of economic indicators. The goal is not perfect forecasting, but to see what a simple model can learn and where it still struggles.

## Additional Requirements
- Requires: `scipy`, `scikit-learn`

---

c
- **Statistical Inference:** Formal tests of trends, correlations, and uncertainty using t-tests, confidence intervals, and regression diagnostics.
- **AI/ML Extension:** Unsupervised learning (PCA + clustering) to map development regimes, plus interpretable prediction of savings behavior.

### Conclusion Section
- Synthesizes findings across EDA, inference, and ML with reflections on limitations and next steps.

### Dependencies
- Requires: `scipy`, `scikit-learn` (already in original requirements).

### How to Run
- Run `project_data_science.ipynb` from top to bottom in order.
- The new cells depend on the earlier cleaning and EDA setup, especially `df`, `wide_clean`, and the plotting imports that are already created above.
﻿# EDA Draft for Medium Post
## Pakistan Development Indicators (1990-2018)

This file contains the full Exploratory Data Analysis (EDA) write-up .

---

## 1) EDA Goal: What are we trying to see before modeling?
In this stage, we are not trying to prove causality. We are trying to understand the structure of the data and answer practical questions:
- Which sector (agriculture, health, education) visually aligns most with economic improvement?
- Are relationships only long-term trends, or do they also appear in year-to-year movement?
- Do some indicators move before GDP growth (lead) or after GDP growth (lag)?

This EDA is organized into five visual checkpoints so readers can follow the story from basic patterns to deeper relationships.

---

## 2) Data Completeness Check Before Interpretation
**Visualization to insert:** `Output Cell 12` (missingness heatmap)

### What we observe from the visual output
The missingness map clearly shows that most macroeconomic and health indicators are densely available across 1990-2018, while education indicators are patchier. Literacy has the biggest gap pattern compared with GDP, mortality, and life expectancy.

### Why this matters for interpretation
If we ignore this and directly compare all indicators equally, we can over-trust relationships coming from very few points. So we interpret education-related correlations with extra caution.

### Literacy-specific handling (important note)
For **literacy rate**, we explicitly use the **mean value** where survey reporting is inconsistent (for example, when literacy is measured through irregular survey rounds). This keeps the heatmap interpretation more stable and avoids overreacting to one-off survey noise. Missing years are still treated as missing; we do not force-fill long gaps.


> Because literacy surveys are not conducted uniformly each year, we rely on mean literacy values across available observations to reduce survey inconsistency effects in the heatmap interpretation.

---

## 3) Sector Trend Story Over Time
**Visualization to insert:** `Output Cell 14` (2x2 trends dashboard)

### What we observe from the visual output
- **Agriculture panel:** Agriculture share and employment do not show strong sustained growth; they are relatively flat or gradually weaker in share terms.
- **Health panel:** Life expectancy rises consistently while child mortality declines strongly, giving the cleanest long-run improvement trajectory.
- **Education panel:** Enrollment improves, but some indicators remain sparse and uneven across years.
- **Economy panel:** GDP per capita rises over time, while GDP growth fluctuates year to year.

### Human interpretation for the post
This figure sets the base narrative: Pakistan shows stronger long-run progress in health and steady economic level improvement, while agriculture indicators are more mixed. Education improves, but data sparsity means we should avoid overconfident statements.


> The trend dashboard suggests that the strongest structural transformation appears in health, while agriculture behaves more like a slowly shifting base sector rather than a consistent high-growth driver.

---

## 4) Correlation Heatmap: Long-Run Alignment vs Short-Run Co-Movement
**Visualization to insert:** `Output Cell 16` (correlation heatmap + printed correlation tables)

### What we observe from the visual output
From the printed results:
- With **GDP per capita (constant)**, strongest positive links are:
  - Cereal Yield: `0.959`
  - Life Expectancy: `0.956`
  - Primary Enrollment: `0.952`
  - Secondary Enrollment: `0.931`
- Strong negative links include:
  - Child Mortality: `-0.958`
  - Food Imports Share: `-0.622`
- With **GDP growth**, relationships are much weaker overall:
  - Agri GDP Share: `-0.444` (level)
  - DPT Immunization: `0.319` (level)
  - Cereal Yield: `0.310` (first-difference)

### Human interpretation for the post
The heatmap tells us two different stories depending on method:
- In **levels**, health and education move strongly with long-run economic level.
- In **first differences**, many links weaken, showing that short-run year-to-year dynamics are noisier.

This is important because it prevents a common student mistake: treating trend-driven correlation as immediate economic effect.


> The strongest signal appears in long-run alignment (levels), not short-run co-movement (differences), which indicates structural development rather than instant yearly transmission.

---

## 5) Scatter Plots: Relationship Shape, Outliers, and Time Progression
**Visualization to insert:** `Output Cell 18` (sector vs economy scatter panel)

### What we observe from the visual output
The scatter plots make the correlation story easier to trust because we can see shape and dispersion directly:
- **Health vs GDP per capita:** clearer upward structure.
- **Education vs GDP per capita:** generally positive pattern, with some spread due sparse years.
- **Agriculture share vs GDP per capita:** weaker and less linear pattern.

Color progression by year shows many points moving through time in an ordered way, which confirms that some correlations are partly driven by long-term development trajectory.

### Human interpretation for the post
The scatter view supports the same ranking as the correlation outputs: health and education visually align more strongly with economic level than agriculture share does.

Suggested sentence for Medium:
> Correlation values are not floating in isolation; the scatter geometry confirms that health and education indicators track economic level more consistently than agriculture share.

---

## 6) Lag Analysis: Do Indicators Move Before or After GDP Growth?
**Visualization to insert:** `Output Cell 20` (lag-correlation line chart + printed best lags)

### What we observe from the visual output
Best lag correlations from output:
- `Agri_GDP_Share`: best lag = `+5`, `r = 0.554`
- `Food_Imports`: best lag = `-1`, `r = -0.345`
- `Life_Expectancy`: best lag = `+3`, `r = 0.295`
- `Primary_Enrollment`: best lag = `+2`, `r = -0.451`

### Human interpretation for the post
The lag plot suggests timing is not uniform across sectors:
- Some agricultural and health indicators show delayed alignment with growth.
- Education signals are present but unstable because data points are fewer.

This supports a realistic policy interpretation: sector improvements and economic outcomes may not move in the same year.

Suggested sentence for Medium:
> The lag structure suggests that development effects are not always contemporaneous; some sector indicators align with growth only after multiple years.

---

## 7) EDA Synthesis Paragraph (Use at end of EDA section)
**No new figure here (this ties all previous outputs together).**

Across all EDA views, our strongest evidence points to **health and education as stronger long-run companions of economic level**, while agriculture shows a more mixed relationship with short-run growth changes. At the same time, the missingness map and literacy survey inconsistency remind us that data quality shapes interpretation. So our EDA conclusion is confident but careful: long-run structural alignment is visible, short-run causality is not claimed, and lag behavior suggests timing matters.

---

