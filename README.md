# Public charging points in Dutch municipalities: need or affluence?

**TIL6022 Python Programming – Group 15 – TU Delft, 2026**

This project examines why the number of public EV charging points differs so much between Dutch municipalities. For all 342 municipalities, the number of regular public charging points per 1,000 households is related to indicators of need (population density, share of apartments), socioeconomic characteristics (income, education), demographics (age structure) and political preference (left-wing vote share). A multivariate regression shows which characteristics explain the most variation.

**Main research question:** To what extent do socioeconomic, demographic and political characteristics explain differences in the number of public charging points per 1,000 households between Dutch municipalities, and which characteristics have the greatest explanatory power?

## Authors

- Merel Vos
- Mirte Batema
- Mats de Vries
- Famke Lemstra
- Luca Preijde

See the contribution statement in the main report for the tasks per member.

## Repository structure

```
group-15/
├── data/
│   ├── raw/          # input: the original datasets, kept unchanged
│   └── processed/    # output: tables that the notebooks create from the raw data
├── figures/          # figures created by the notebooks
├── notebooks/        # analysis notebooks, run in numbered order
├── main_report.ipynb # final report (to be added)
└── README.md
```

## Data

All datasets are open data. The unit of analysis is the municipality (gemeentelijke indeling 2024, 342 municipalities).

| File in `data/raw/` | Content | Source | Period |
| --- | --- | --- | --- |
| `laadpunten_per_gemeente.csv` | Regular public charging points per municipality | [Laadinfrastructuur databank (NAL)](https://laadinfrastructuur.databank.nl/) | July 2026 |
| `inkomens_per_gemeente.csv` | Private households, mean and median standardised income | [CBS StatLine, table 86161NED](https://opendata.cbs.nl/statline/) | 2024 (provisional) |
| `jongeren_per_gemeente_2026.csv` | Share of residents under 25 | CBS StatLine | 2026 |
| `ouderen_per_gemeente_2026.csv` | Share of residents aged 65+ | CBS StatLine | 2026 |
| `kwb2024_per_gemeente.csv` | Education level, population density, share of apartments (extract per municipality) | [CBS Kerncijfers wijken en buurten 2024, table 85984NED](https://www.cbs.nl/nl-nl/cijfers/detail/85984NED) | 2024 |
| `tk2025_uitslag.csv` | Votes per party list per municipality | [Kiesraad, Tweede Kamer election 29 October 2025](https://data.overheid.nl/dataset/verkiezingsuitslag-tweede-kamer-2025) | 2025 |
| `gemeenten_2024.gpkg` | Municipal boundaries for maps | [CBS/PDOK gebiedsindelingen 2024](https://service.pdok.nl/cbs/gebiedsindelingen/2024/wfs/v1_0) | 2024 |

`kwb2024_per_gemeente.csv` is a small extract of the full CBS download (about 72 MB), which is not included in the repository. To recreate the extract, place the CBS files `Observations.csv` and `WijkenEnBuurtenCodes.csv` in `data/raw/kwb2024/` and delete the extract; notebook 6 then rebuilds it.

## How to run the analysis

### 1. Install the required packages

Python 3.10 or newer is recommended.

```bash
pip install pandas geopandas matplotlib seaborn scipy statsmodels jupyter
```

### 2. Run the notebooks in order

Open Jupyter from the repository folder and run the notebooks in `notebooks/` from top to bottom, in this order. All paths are relative, so the notebooks work on any computer as long as the folder structure is kept.

| Notebook | What it does | Output |
| --- | --- | --- |
| `1. code_inkomens_laadpunten_per_gemeente.ipynb` | Merges charging points with households and income; calculates charging points per 1,000 households | `data/processed/laadpunten_huishoudens_inkomen_per_gemeente.csv` |
| `2. laadpalen_huishoudens.ipynb` | Map of charging points per 1,000 households | `figures/kaart_laadpunten_per_1000_huishoudens.png` |
| `6. beschrijvende_statistiek_correlaties.ipynb` | Merges all variables; descriptive statistics, correlations with charging point density and correlation matrix (report section 6.6) | `data/processed/alle_variabelen_per_gemeente.csv`, `data/processed/tabel_vergelijking_factoren.csv`, `figures/figuur_correlatiematrix.png`, `figures/figuur_verdeling_laadpunten.png` |
| *to be added* | Maps and plots per sub-question (income, education, age, vote share, density) | `figures/` |
| *to be added* | Multivariate regression (report section 7) | `figures/`, `data/processed/` |

### 3. Read the report

The final report is `main_report.ipynb`, with an HTML export of the same notebook.

## Key definitions

- **Charging point density:** regular public charging points / private households × 1,000.
- **Highly educated:** persons aged 15–75 with hbo or wo as highest level, divided by all persons aged 15–75 with a known education level.
- **Left-wing vote share:** votes for GroenLinks-PvdA, SP, Partij voor de Dieren, DENK, BIJ1, D66 and Volt, divided by all votes for party lists. A narrow definition without D66 and Volt is used as a robustness check.
- **Municipality names:** CBS and Kiesraad spell some names differently (for example *'s-Gravenhage* and *Den Haag*). These are aligned in the notebooks before merging.

## Use of AI

AI tools were used for language, structure and code suggestions. All analyses and conclusions are the authors' own.
