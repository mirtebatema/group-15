# Public charging points in Dutch municipalities: need or affluence?

**TIL6022 Python Programming , Group 15 , TU Delft, 2026**

This project examines why the number of public EV charging points differs so much between Dutch municipalities. For all 342 municipalities, the number of regular public charging points per 1,000 households is related to indicators of need (population density, share of apartments), socioeconomic characteristics (income, education), demographics (age structure) and political preference (left-wing vote share). A multivariate regression shows which characteristics explain the most variation.

**Main research question:** To what extent do socioeconomic, demographic and political characteristics explain differences in the number of public charging points per 1,000 households between Dutch municipalities, and which characteristics have the greatest explanatory power?

## Authors

- Mirte Batema
- Merel Vos
- Famke Lemstra
- Luca Preijde
- Mats de Vries

See the contribution statement (section 3) in the main report for the tasks per member.

## Where to find what

The report is the starting point. Every result in it comes from one of the notebooks in `notebooks/`. The table below shows which notebook produces which part of the report.

| Report section | Content | Notebook | Figure in `figures/` |
| --- | --- | --- | --- |
| 6.1 Variation (SQ1) | Map, histogram and boxplot of charging point density | 2, 6, 6.2 | `map_chargingpoints_per_1000households.png`, `figure_distribution_of_public_charging_point_density.png`, `figure_boxplot_charging_point_density.png` |
| 6.2 Income and education (SQ2) | Maps of income and education level | 1.2, 5 | `map_income_per_municipality.png`, `map_education_per_municipality.png` |
| 6.3 Age structure (SQ3) | Map of young and older residents | 3 | `map_age_per_municipality.png` |
| 6.4 Political preference (SQ4) | Map of left-wing vote share (broad and narrow) | 4 | `map_left_wing_votes_per_municipality.png` |
| 6.5 Population density (SQ5) | Map of population density | 2.2 | `map_population_density_per_municipality.png` |
| 6.6 Comparison of all factors | Correlation table, scatter plots, correlation matrix | 6, 6.2 | `figure_scatter_all_factors.png`, `figure_correlationmatrix.png` |
| 7 Regression model | Main OLS model, residuals, robustness checks | 7 | `figure_regression_coefficients.png`, `figure_regression_residuals.png` |
| 7 Stepwise regression | Hierarchical, all-subsets and composite-index regression | 8 | `figure_stepwise_coefficients.png`, `figure_all_subsets.png` |

## Repository structure

```
group-15/
├── data/
│   ├── raw/          # input: the original datasets, kept unchanged
│   └── processed/    # output: tables that the notebooks create from the raw data
├── figures/          # figures created by the notebooks
├── notebooks/        # all notebooks, run in numbered order
├── final_report_group_15.ipynb # final report (nog toevoegen)
└── README.md
```

## Data

All datasets are open data. The unit of analysis is the municipality (municipal division 2024, 342 municipalities).

| File in `data/raw/` | Content | Source | Period |
| --- | --- | --- | --- |
| `laadpunten_per_gemeente.csv` | Regular public charging points per municipality | [Laadinfrastructuur databank (NAL)](https://laadinfrastructuur.databank.nl/) | July 2026 |
| `inkomens_per_gemeente.csv` | Private households, mean and median standardised income | [CBS StatLine, table 86161NED](https://www.cbs.nl/nl-nl/cijfers/detail/86161NED) | 2024 (provisional) |
| `jongeren_per_gemeente_2026.csv` | Share of residents aged 0–25 | CBS StatLine | 2026 |
| `ouderen_per_gemeente_2026.csv` | Share of residents aged 65+ | CBS StatLine | 2026 |
| `kwb2024.zip` | Full CBS download of Kerncijfers wijken en buurten 2024 | [CBS, table 85984NED](https://www.cbs.nl/nl-nl/cijfers/detail/85984NED) | 2024 |
| `kwb2024_per_gemeente.csv` | Extract of `kwb2024.zip` with one row per municipality: education level, population density, share of apartments | Created by notebook 0.5 | 2024 |
| `tk2025_uitslag.csv` | Votes per party list per municipality | [Kiesraad, Tweede Kamer election 29 October 2025](https://data.overheid.nl/dataset/verkiezingsuitslag-tweede-kamer-2025) | 2025 |
| `gemeenten_2024.gpkg` | Municipal boundaries for the maps | [CBS/PDOK gebiedsindelingen 2024](https://www.pdok.nl/introductie/-/article/cbs-gebiedsindelingen) | 2024 |

Raw data files keep their original Dutch names; all notebooks, processed data and figures have English names. Column names in the processed files are partly Dutch (for example `laadpunten_per_1000_huishoudens`), because they follow the source data.

**Missing data.** CBS does not publish income figures for a few very small municipalities. Mean income is missing for Ameland, Renswoude, Rozendaal, Schiermonnikoog, Terschelling and Vlieland; median income for Rozendaal, Schiermonnikoog and Vlieland. The regressions therefore use 339 of the 342 municipalities.

## How to run the analysis

### 1. Install the required packages

Python 3.10 or newer is recommended.

```bash
pip install pandas numpy geopandas matplotlib seaborn scipy statsmodels jupyter
```

### 2. Run the notebooks in order

Open Jupyter from the repository folder and run the notebooks in `notebooks/` from top to bottom, in numbered order. All paths are relative, so the notebooks work on any computer as long as the folder structure is kept. The notebooks fall into three groups.

**Step 0 – Data processing.** These notebooks clean the raw data and save one processed file per variable.

| Notebook | What it does | Output |
| --- | --- | --- |
| `0.1` age | Combines the shares of young (0–25) and older (65+) residents | `data/processed/age_per_municipality.csv` |
| `0.2` education | Calculates the share of highly and low educated residents | `data/processed/education_per_municipality.csv` |
| `0.3` left-wing votes | Calculates the left-wing vote share (broad and narrow definition) | `data/processed/left_wing_votes_per_municipality.csv` |
| `0.4` population density | Calculates population density and its logarithm | `data/processed/population_density_per_municipality.csv` |
| `0.5` kwb2024 extract | Builds the extract `kwb2024_per_gemeente.csv` from `kwb2024.zip` | `data/raw/kwb2024_per_gemeente.csv` |
| `0.6` income | Prepares mean standardised income per municipality | `data/processed/income_per_municipality.csv` |

**Steps 1–5 – Exploration per variable.** Each notebook builds or loads one variable and draws a map per municipality.

| Notebook | What it does | Output |
| --- | --- | --- |
| `1` charging points, households, income | Merges charging points with households and income; calculates charging points per 1,000 households (the dependent variable) | `data/processed/chargingpoints_households_income.csv` |
| `1.2` income | Map of mean standardised income | `figures/map_income_per_municipality.png` |
| `2` charging points per 1,000 households | Map of the dependent variable | `figures/map_chargingpoints_per_1000households.png` |
| `2.2` population density | Map of population density (log scale) | `figures/map_population_density_per_municipality.png` |
| `3` age | Map of young and older residents | `figures/map_age_per_municipality.png` |
| `4` left-wing votes | Map of left-wing vote share | `figures/map_left_wing_votes_per_municipality.png` |
| `5` education | Map of highly and low educated residents | `figures/map_education_per_municipality.png` |

**Steps 6–8 – Analysis.** Notebook 6 merges all variables into one table. Notebooks 6.2, 7 and 8 use that table, so run notebook 6 first.

| Notebook | What it does | Output |
| --- | --- | --- |
| `6` descriptive statistics and correlations | Merges all variables into one table; descriptive statistics, correlations with charging point density, correlation matrix | `data/processed/all_variables_per_municipality.csv`, `data/processed/table_comparison_factors.csv`, `figures/figure_distribution_of_public_charging_point_density.png`, `figures/figure_correlationmatrix.png` |
| `6.2` distribution and scatter plots | Boxplot of charging point density; scatter plots of each factor | `figures/figure_boxplot_charging_point_density.png`, `figures/figure_scatter_all_factors.png` |
| `7` regression | Multivariate OLS regression with robust (HC3) standard errors, VIF, residual diagnostics and robustness checks | `data/processed/regression_results.csv`, `figures/figure_regression_coefficients.png`, `figures/figure_regression_residuals.png` |
| `8` stepwise regression | Hierarchical, all-subsets and composite-index regression | `data/processed/stepwise_results.csv`, `data/processed/all_subsets_results.csv`, `data/processed/composite_model_results.csv`, `figures/figure_stepwise_coefficients.png`, `figures/figure_all_subsets.png` |

### 3. Read the report

The final report is `final_report_group_15.ipynb`, with an HTML export of the same notebook.

## Key definitions

- **Charging point density:** regular public charging points / private households × 1,000.
- **Income:** standardised household income (corrected for household size), in € 1,000. The median is used in the regression, because the mean is distorted by a few very rich municipalities.
- **Age structure:** share of residents aged 0–25 and aged 65 and over, as a percentage of the total population.
- **Highly educated:** persons aged 15–75 with hbo or wo as highest level, divided by all persons aged 15–75 with a known education level.
- **Left-wing vote share:** votes for GroenLinks-PvdA, SP, Partij voor de Dieren, DENK, BIJ1, D66 and Volt, divided by all valid votes for party lists. A narrow definition without D66 and Volt is used as a robustness check.
- **Population density:** inhabitants per km² of land. Because it is strongly right-skewed, its natural logarithm is used in the regression.
- **Share of apartments:** apartments as a percentage of all dwellings; a second indicator of need for public charging.
- **Municipality names:** CBS and the Kiesraad spell some names differently (for example *'s-Gravenhage* and *Den Haag*). These are aligned in the notebooks before merging.

## Use of AI

AI tools used for coding 

Claude and GitHub Copilot were occasionally used to improve code clarity, help solve programming problems, and suggest approaches for more complex calculations, such as calculating EV charging point density (Notebook 6). 

AI tools used for writing 

AI tools were used to improve the grammar, clarity, and readability of the report. 

Our own contribution 

All research questions, ideas, data analyses, interpretations, and conclusions presented in this report were developed by our group through our own reasoning, discussions, and use of course materials. AI tools were used only as supporting tools, and we reviewed the suggestions before including them in our work. 
