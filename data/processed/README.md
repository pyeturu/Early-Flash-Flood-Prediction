# Processed data

`caravan_selected.csv` is the smaller, combined table used by the exploration, plotting, and training notebooks. Instead of carrying all 41 columns from each raw basin file, it keeps the date, basin ID, and three variables used in our first discharge forecasting experiment.

| Column | Meaning |
| --- | --- |
| `date` | Date of the daily observation, written as `YYYY-MM-DD`. |
| `streamflow` | River discharge, expressed as catchment-area-normalized depth in mm/day. |
| `rainfall` | Daily total precipitation, renamed from `total_precipitation_sum`. |
| `temperature` | Daily mean temperature, renamed from `temperature_2m_mean`. |
| `basin` | Identifier showing which river the row belongs to. |

One row represents one day for one basin. The current file contains 79,986 rows: 26,662 for each of the three basins. It retains the full source date range, January 1, 1951 through December 30, 2023. The 1990–2014 study window is applied later in the analysis and training notebooks.

## How this table is made

[01_load_caravan_data.ipynb](../../caravan_dataset_analysis/01_load_caravan_data.ipynb) reads each basin CSV from `data/raw`, selects four columns, renames the precipitation and temperature columns, adds the basin ID, and combines the three tables. Running its save cell replaces `caravan_selected.csv`.

Here, “processed” means selected and combined. The loading step does not fill missing values, remove incomplete rows, or create training features. Blank discharge entries remain blank. Later notebooks treat negative discharge as missing and preserve real zero-flow observations.

The exploration notebook uses this table to summarize data availability. The visualization notebook uses it to draw discharge plots. The training notebook aligns each basin to a daily calendar, creates discharge lags and rainfall totals, and removes rows that lack a required input or next-day target. Those training features are created in memory; they are not additional columns in this CSV.
