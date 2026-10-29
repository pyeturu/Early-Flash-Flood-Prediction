# Analysis and training outputs

This folder holds the CSV reports produced while exploring the three basins, along with the current training sample counts. The reports help us understand the data before judging a forecasting model.

## Basin statistics

The three `<basin>_statistics.csv` files come from [02_explore_dataset.ipynb](../../caravan_dataset_analysis/02_explore_dataset.ipynb). Each summarizes `streamflow`, `rainfall`, and `temperature` between January 1, 1990 and December 31, 2014.

The rows give the number of available observations, mean, standard deviation, minimum, quartiles, and maximum. `50%` is the median. Counts exclude missing values, so different columns can have different counts. Negative discharge is treated as missing before these summaries are calculated.

## Missing-value reports

Each `<basin>_missing_values.csv` counts missing entries over the same study period, after negative discharge has been marked as missing. The current reports show:

| Basin | Missing discharge | Missing rainfall | Missing temperature |
| --- | ---: | ---: | ---: |
| `camels_01411300` | 254 | 0 | 0 |
| `camels_05495500` | 31 | 0 | 0 |
| `camels_14182500` | 73 | 0 | 0 |

A missing discharge value is an unavailable measurement, not evidence of zero river flow.

## Training sample counts

`sample_counts.csv` is written by [train_caravan.ipynb](../../model/train_caravan.ipynb). Its columns record the basin, number of input features, training samples, and test samples. The distinct entries currently saved are:

| Basin | Features | Training samples | Test samples |
| --- | ---: | ---: | ---: |
| `camels_01411300` | 7 | 7,304 | 1,571 |
| `camels_05495500` | 7 | 7,304 | 1,794 |
| `camels_14182500` | 7 | 7,304 | 1,752 |

The current CSV repeats each basin entry twice. These repeated rows do not represent additional basins or independent experiments. Running the training notebook from the beginning resets its output lists and regenerates the file.

The seven inputs are today's discharge, discharge one and two days earlier, today's precipitation, three-day and seven-day precipitation totals ending today, and today's mean temperature. The target is tomorrow's discharge. Each basin is aligned to a daily calendar before features are made, and rows missing any required input or target are dropped.

Training forecast dates run from January 1, 1990 through December 30, 2009; test forecast dates run from January 1, 2010 through December 30, 2014. December 31, 2009 is excluded so that no training target falls in 2010. Sample counts measure usable rows, not model accuracy.

## Predictions and plots

The training notebook is set up to write `test_predictions.csv` here, although that file is currently absent. Its columns are `basin`, `forecast_date`, `target_date`, `actual`, `persistence`, and `linear_regression`. The persistence baseline uses today's discharge as tomorrow's prediction. Linear regression predictions are clipped at zero, and both approaches use the same test rows.

The [figures folder](figures/README.md) contains the six saved exploration plots. The exploration notebook also displays correlations and monthly average flow, but it does not save those extra views as separate files here.
