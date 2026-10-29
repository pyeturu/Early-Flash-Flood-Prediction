River Flow Forecasting for Flood Early Warning
Project Overview
This project investigates whether machine learning can predict river discharge and support flood preparedness. For Lab 2, we focus on a manageable first step: predicting the next day's discharge using current and past discharge, rainfall, and temperature from selected Caravan basins.

Problem type: Supervised regression. The target is a continuous discharge value, measured in millimetres per day (mm/day) and normalized by catchment area.

Daily discharge does not directly identify confirmed flash floods. Longer-lead forecasts and transfer to new regions are Final Project goals, rather than capabilities demonstrated at this stage.
Team Members
- Pranav Yeturu
- Sai Kiran Paruchuri

Repository: https://github.com/pyeturu/Early-Flash-Flood-Prediction

Folder documentation

- [Raw data](data/raw/README.md): source files, basin details, and where the data comes from.
- [Processed data](data/processed/README.md): selected columns and how the combined table is made.
- [Results](results/README.md): the analysis workflow and the outputs currently available.
- [Analysis and training outputs](results/staged/README.md): statistics, missing values, sample counts, and expected prediction output.
- [Figures](results/staged/figures/README.md): what each plot shows and how to regenerate it.

We use a three-basin CAMELS-US subset of the Caravan CSV dataset, version 1.6.
- Dataset source: https://zenodo.org/records/15530022
- Documentation: https://github.com/kratzert/Caravan
- Dataset paper: https://doi.org/10.1038/s41597-023-01975-w

Caravan combines daily river discharge observations, meteorological data, and catchment attributes. The weather used in this initial experiment is historical ERA5-Land reanalysis, not weather forecasts.

Basin ID	Location
camels_01411300	Tuckahoe River, New Jersey
camels_05495500	Bear Creek, Illinois
camels_14182500	Little North Santiam River, Oregon


The original daily files contain 41 columns, including date, streamflow, and weather variables. Our processed table selects date, basin ID, discharge, precipitation, and mean temperature. The initial study period is 1990–2014. Calendar coverage does not imply that discharge is available on every day.
Proposed Research Questions
1. Can a machine-learning model predict next-day river discharge more accurately than assuming it remains unchanged? 
2. Can a model trained in one region predict high-flow events in previously unseen regions, and how much local data is needed to improve its predictions? 
3. How far in advance can we predict unusually high river flow while keeping missed events and false alarms low? 

Current Progress
- Data loading completed: downloaded the selected Caravan files and loaded them with pandas.
- Exploration completed: inspected dimensions, selected variables, descriptive statistics, and missing values.
- Visualization completed: created discharge time-series plots and discharge-distribution plots for the selected basins.
- Processed data available: saved the selected columns in data/processed/caravan_selected.csv for the training stage.

- Initial training notebook available: fits a separate linear regression model for each basin and compares predictions with a persistence baseline. Saved notebook output and sample counts are present; the expected test_predictions.csv is currently absent from results/staged.
- Next step: regenerate the prediction CSV and evaluate model accuracy on the later test dates.

The time-series plots help identify changes in flow and sharp peaks. The distribution plots help examine how frequently low, moderate, and unusually high discharge values occur. Missing observations must be distinguished from genuine zero flow.
Initial Training Setup

The training notebook fits separate linear regression models for the three basins using the processed Caravan data. Seven input features are constructed:

- Current day's discharge.
- Discharge one and two days earlier.
- Current day's rainfall.
- Three-day and seven-day rainfall totals ending on the current day.
- Current day's mean temperature.
The target is the next day's discharge. Dates will be aligned to a daily calendar before creating lags. Rows with missing required inputs or targets will be excluded; missing discharge will not be filled with zero.





