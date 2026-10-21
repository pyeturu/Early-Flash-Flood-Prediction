# River Flow Forecasting for Flood Early Warning

## Project Overview

Our project investigates whether machine learning can predict unusually
high river flow 7–14 days in advance, providing additional time for
flood preparedness.

We will use historical river discharge, weather, and catchment
characteristics from the Caravan dataset. Later, we will incorporate
archived weather forecasts from Caravan MultiMet.

We aim to evaluate:

- How prediction accuracy changes with forecast lead time.
- Whether forecast weather improves predictions.
- Whether models generalize to river basins excluded from training.
- How reliably models identify unusually high-flow conditions.

The 7–14-day horizon is a research objective, not an achieved capability.

## Motivation and Intended Impact

Floods can threaten lives, damage infrastructure, and disrupt essential
services. Reliable advance information could support:

- Emergency resource allocation.
- Infrastructure inspections and protective actions.
- Community preparedness.
- Water-management planning.

A 7–14-day forecast would provide an advance risk outlook that can be
updated as new information arrives.

Our model cannot prevent floods. Caravan's daily data supports
river-flow forecasting; predicting rapid flash floods would require
finer-resolution observations and additional information.

## Datasets

### Caravan

Caravan is a public hydrological dataset combining:

- Historical daily river discharge.
- Weather variables, including precipitation and temperature.
- Catchment characteristics such as area, terrain, soils, and land cover.
- Records from river basins across multiple regions.

A catchment is the land area that drains toward a particular river location.

**Source and downloads:**
https://github.com/kratzert/Caravan

**Dataset paper:**
https://doi.org/10.1038/s41597-023-01975-w

### Caravan MultiMet

MultiMet extends Caravan with additional precipitation products and
archived weather forecasts.

Its forecast products include:

- ECMWF IFS HRES.
- GraphCast.
- CHIRPS-GEFS.

Their variables, dates, and lead-time coverage differ. CHIRPS-GEFS
provides precipitation forecasts extending to 16 days, making it a
candidate input for our 14-day experiment.

We will verify coverage and forecast availability before selecting
the final experiment.

**Paper:**
https://arxiv.org/abs/2411.09459

**Forecast data:**
https://zenodo.org/records/14196772

### Initial Subset

For Lab 2, we plan to select 3–5 basins from one region with several
years of usable daily records.

After inspection, we will document:

- Dataset version and source.
- Basin identifiers.
- Selected dates.
- Variables and units.
- Missing values and processing decisions.
- Applicable data licenses.

Exact basins and dates have not yet been selected.

## Research Questions

1. How far in advance can we predict unusually high river flow while
   keeping missed events and false alarms low?

2. Does adding archived precipitation forecasts improve predictions
   compared with using historical discharge and weather alone?

3. Can a model trained in one region predict high-flow events in
   previously unseen regions, and how much local data is needed
   to improve its predictions?

These questions guide the project. We are not claiming a novel
research contribution at this stage.

## Scope for Lab 2

Lab 2 will establish the data-processing and modeling workflow using
Caravan alone.

Our initial task is:

> Can linear regression predict next-day river discharge more accurately
> than assuming discharge remains unchanged?

Longer forecast horizons and MultiMet integration are planned for the
Final Project.

## Initial Visualizations

1. **River discharge over time**
   - Examine seasonal patterns, high-flow periods, and missing records.

2. **Today's discharge versus tomorrow's discharge**
   - Examine how strongly recent river conditions predict the next day.

3. **Optional: rainfall and discharge on aligned time panels**
   - Explore how river flow changes following rainfall.

Each visualization will include a title, axis labels, units, and a
short interpretation.

## Evaluation Strategy

### Chronological Evaluation

- Train on earlier dates and evaluate on later dates.
- Use identical test dates when comparing models.
- Fit preprocessing using training data only.
- Construct lagged features separately within each basin.
- Verify consecutive dates before creating next-day targets.
- For longer horizons, exclude training examples whose target dates
  overlap the evaluation period.

### High-Flow Evaluation

Initially, we will define high flow as discharge exceeding a basin's
training-period 95th percentile.

We will evaluate:

- Recall: the proportion of observed high-flow days detected.
- Precision: the proportion of predicted high-flow days that occurred.
- False-alarm rate: the proportion of non-high-flow days flagged.
- Number of high-flow days available for evaluation.

This percentile is an experimental threshold, not an official
flood-warning level.

## Geographic Transfer Experiment

For the Final Project, we plan to:

1. Train on selected source regions.
2. Exclude an entire target region from training.
3. Evaluate predictions in the target region.
4. Compare no local training with limited local adaptation.
5. Report performance across basins and high-flow conditions.

We will document whether recent target-basin discharge is available as
an input. Having no local training data is different from having no
local observations at prediction time.


## Next Steps for Final Project

- Incorporate MultiMet forecast inputs.
- Evaluate 3-, 7-, and 14-day horizons where data permits.
- Compare stronger models.
- Investigate forecast uncertainty.
- Test transfer to unseen basins and regions.
