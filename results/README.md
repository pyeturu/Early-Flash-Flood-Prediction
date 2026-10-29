# Results

This folder collects the outputs of our initial river discharge analysis. The files currently sit in [staged](staged/README.md), with the saved plots in [staged/figures](staged/figures/README.md).

The work starts with three Caravan basins. We select discharge, precipitation, and mean temperature, then inspect the 1990–2014 study period for missing observations, differences in flow, and unusually high discharge. The training notebook builds a separate linear regression model for each river to predict the next day's discharge.

## What is saved here

- **Statistics CSVs:** descriptive summaries of discharge, rainfall, and temperature for each basin.
- **Missing-value CSVs:** counts of unavailable observations in those three variables.
- **`sample_counts.csv`:** the number of features and usable training and test samples for each basin.
- **Figures:** a daily discharge plot and a discharge histogram for each river, six PNGs in total.

The training notebook contains saved prediction output, but its expected `test_predictions.csv` is absent from this folder. There are also no saved model accuracy metrics here, so the existing files do not establish whether linear regression beats the baseline.

## How the work fits together

Run the notebooks from top to bottom in this order:

1. [Load the data](../caravan_dataset_analysis/01_load_caravan_data.ipynb) to download the selected source files and create `data/processed/caravan_selected.csv`.
2. [Explore the data](../caravan_dataset_analysis/02_explore_dataset.ipynb) to save the statistics and missing-value reports.
3. [Visualize discharge](../caravan_dataset_analysis/03_visualize_caravan.ipynb) to save the six figures.
4. [Train the models](../model/train_caravan.ipynb) to save sample counts and test predictions.

The notebooks use pandas and matplotlib; training also uses scikit-learn. The download cell in the first notebook has a local `ROOT` path that needs updating on another computer. The later path setup supports running from the project root or a notebook subfolder.

This stage studies next-day river discharge using historical weather reanalysis. High-flow peaks help us explore the data, but they are not confirmed flash-flood labels. Flood detection, longer forecast horizons, and testing on new regions remain future work.
