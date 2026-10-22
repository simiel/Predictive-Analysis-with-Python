# Predictive Analysis with Python

Coursework by Samuel Mensah ([@simiel](https://github.com/simiel)) from an Educative.io course. The notebooks move from NumPy and pandas practice to a complete 2018 equity study — daily returns, cross-stock correlation, a risk-versus-return comparison, and a Monte Carlo simulation of future prices — and a started analysis of e-commerce brand purchases. It is a portfolio of applied analysis for anyone reviewing hands-on work in tabular data, time series, and simulation.

## Contents

- **Library exercises** for NumPy arrays, pandas Series and DataFrames, Matplotlib and Seaborn plots, and split-apply-combine wrangling.
- **Housing cleaning** on `housing.csv`: drop incomplete rows, then cap outliers in value, income, and age with the interquartile range.
- **Project 1, stock market analysis** of four 2018 daily series — Systems Ltd (SYS), NETSOL, Avanceon (AVN), and PTC — through returns, correlation, risk, and simulated price paths.
- **Project 2, brand analysis** of an e-commerce event log (views, cart adds, purchases). The notebook is started; the CSV it reads is not in this repository.

## Tech stack

The notebooks were executed with Python 3.8.8 from an Anaconda environment. They import:

| Library | Role |
| --- | --- |
| pandas | CSV loading, alignment on dates, `groupby`, rolling means, correlations |
| NumPy | Arrays, log returns, random shocks, the simulated price grid |
| Matplotlib | Price history, return series, risk-return scatter, simulation paths |
| Seaborn | Kernel density of returns, annotated correlation heatmaps, practice plots |
| SciPy (`scipy.stats.norm`) | Inverse-normal draws for the Monte Carlo shocks |

No dependency file is committed. No environment variables are referenced. No test or build command is defined.

## Repository layout

```
.
├── numpy.ipynb                      # arrays, views versus copies, reshape, row min/max
├── pandas.ipynb                     # Series and DataFrames on film scores
├── visualizing_the_data.ipynb       # line, joint, heatmap, and regression plots
├── wrangle.ipynb                    # groupby ranking on a passenger table
├── clean_the_data_challenge.ipynb   # missing values and IQR bounds on housing data
├── Project1.ipynb                   # 2018 stock analysis and Monte Carlo paths
├── Project2.ipynb                   # started brand and customer-event analysis
├── housing.csv                      # district housing table
├── test.csv                         # passenger table used by the wrangling notebook
├── fandango_score_comparison.csv    # film scores used by the pandas notebook
├── log.txt                          # short personal note from the simulation work
└── Year_2018/
    ├── SYS.csv                      # Systems Ltd daily OHLCV, 2018
    ├── NETSOL.csv
    ├── AVN.csv                      # Avanceon
    └── PTC.csv
```

Each stock file has `Time`, `Open`, `High`, `Low`, `Close`, and `Volume`, covering 1 January 2018 through 31 December 2018 (243–247 trading rows). `housing.csv` has 20,640 rows and ten columns, including `longitude`, `latitude`, `housing_median_age`, `total_rooms`, `total_bedrooms`, `population`, `households`, `median_income`, `median_house_value`, and `ocean_proximity`. The cleaning notebook records 207 missing `total_bedrooms` values before rows are dropped. `test.csv` has 418 passenger rows. `fandango_score_comparison.csv` has 146 films and both critic and audience scores.

## Project 1: stock market analysis

`Project1.ipynb` answers five questions on the 2018 series:

1. How did each closing price change over the year?
2. What were the daily returns?
3. How are the stocks related?
4. How much capital is at risk on a typical day?
5. What future price paths are plausible for Systems Ltd?

**Approach.** Closes are plotted on their own and against 10-, 20-, 30-, 50-, and 60-day rolling means, including a four-name overlay. Daily return is the percentage change of the close; the Systems Ltd distribution is shown with a kernel density estimate. Pearson correlations of closing prices and of daily returns are drawn as annotated heatmaps. Daily risk is the standard deviation of daily returns, plotted against the mean daily return. The Systems Ltd forecast is a Monte Carlo random walk on log returns: drift is the mean log return minus half the variance, each shock is `std * norm.ppf(U)` over 60 days and 10 paths, and each path is compounded from the last close by `exp(drift + shock)`.

**Results saved in the notebook.** The simulation uses unseeded `numpy` random draws, so executing the cells again produces different paths. The figures below are the committed outputs.

Closing prices of Systems Ltd, NETSOL, and Avanceon move together. PTC moves the other way. Saved correlations of the close:

|  | SYS | NETSOL | PTC | AVN |
| --- | ---: | ---: | ---: | ---: |
| SYS | 1.00 | 0.78 | −0.79 | 0.90 |
| NETSOL | 0.78 | 1.00 | −0.71 | 0.74 |
| PTC | −0.79 | −0.71 | 1.00 | −0.84 |
| AVN | 0.90 | 0.74 | −0.84 | 1.00 |

Daily-return correlations are weaker and all positive, from 0.18 (NETSOL–PTC) to 0.45 (NETSOL–Avanceon).

| Name | Average daily return | Daily risk (standard deviation) |
| --- | ---: | ---: |
| Systems Ltd (SYS) | 0.001793 | 0.020281 |
| NETSOL | 0.000641 | 0.034397 |
| PTC | −0.001030 | 0.018328 |
| Avanceon (AVN) | 0.003773 | 0.027157 |

The notebook reads that scatter as follows. NETSOL has a small positive daily return and the highest risk. Avanceon has the highest positive return, with less risk than NETSOL. Systems Ltd has a positive return and lower risk than both NETSOL and Avanceon. PTC has the lowest risk and a negative average daily return.

The saved Systems Ltd simulation reports a drift of `0.001372706735423739`, starts from the 31 December 2018 close of **109.87**, and after 60 days the ten paths span **91.21** to **159.39**.

## Project 2: brand analysis

`Project2.ipynb` documents an e-commerce activity log with `event_time`, `event_type` (`view`, `cart`, `purchase`), `product_id`, `category_id`, `category_code`, `brand`, `price`, `user_id`, and `user_session`. It begins a split of purchases into branded and unbranded products, derives weekday, day of month, and hour from `event_time`, and sketches view counts by weekday and by hour plus a purchase count by brand.

The notebook reads `2019-Oct.csv`, which is not included, and it stores no outputs. Later cells refer to `event-type` and `event` rather than the documented `event_type`, and the purchase-share arithmetic uses the column count. The hypotheses in the notebook are drafts written ahead of a finished calculation. This project is incomplete.

## Practice notebooks

- `numpy.ipynb` builds one- and two-dimensional arrays, shows that a slice shares memory unless `.copy()` is used, reshapes and transposes, and returns the minimum and maximum of each row.
- `pandas.ipynb` builds a named Series, loads the Fandango score table, selects critic columns, and reindexes with forward fill.
- `visualizing_the_data.ipynb` plots a sine wave, a Seaborn joint distribution of a correlated sample, a year-by-month heatmap of Seaborn's flights data, and a regression plot of Anscombe's quartet.
- `wrangle.ipynb` sorts the passenger table and ranks rows inside `Pclass` and `Sex` groups (split, apply, combine).
- `clean_the_data_challenge.ipynb` drops any row with a missing value, then clips `median_house_value`, `median_income`, and `housing_median_age` to the fences `Q1 − 1.5 × IQR` and `Q3 + 1.5 × IQR`. The notebook records that this updates outlier cells to the fence instead of deleting those rows.

## Getting started

Use Python 3 with `numpy`, `pandas`, `matplotlib`, `seaborn`, and `scipy`. The committed runs used Python 3.8.8 under Anaconda. Install Jupyter if you want the notebook UI, then open the files from the repository root so the relative CSV paths resolve:

```bash
jupyter notebook
```

Run each notebook from the top. `Project1.ipynb` reads `Year_2018/`. The cleaning, pandas, and wrangling notebooks read `housing.csv`, `fandango_score_comparison.csv`, and `test.csv`. The visualization notebook also calls `seaborn.load_dataset` for the flights and Anscombe examples. `Project2.ipynb` expects `2019-Oct.csv` in the working directory.

## Project status

Coursework, written in March and April 2022. The exercises and Project 1 are complete, and Project 1 keeps its executed plots and printed figures. Project 2 was started on 11 March 2022 and last edited on 19 April 2022, and it remains unfinished.

## What this demonstrates

- A full pass over aligned daily prices: rolling means at several windows, percentage returns, and a distribution check before any simulation.
- Separation of price co-movement from return co-movement, measured with Pearson correlations and annotated heatmaps across four names.
- A risk-versus-return comparison that uses the mean and standard deviation of daily returns, with a written call on where each name sits.
- A Monte Carlo price-path model built from the log-return drift (`mean − variance / 2`), inverse-normal shocks, and compounding from the last observed close across ten paths and sixty days.
- pandas practice that holds up in the projects: missing-value audits, interquartile-range capping, `groupby` ranking, date indexes, and forward fill.
