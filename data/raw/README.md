# Raw data

This folder holds the original Caravan files used in our river discharge project. We keep these files as downloaded so we can trace the analysis back to its source and rebuild the processed table when needed.

We use three river basins from the CAMELS-US part of Caravan:

| File | River and location |
| --- | --- |
| `camels_01411300.csv` | Tuckahoe River at Head of River, New Jersey |
| `camels_05495500.csv` | Bear Creek near Marcelline, Illinois |
| `camels_14182500.csv` | Little North Santiam River near Mehama, Oregon |

Each basin file has 26,662 daily rows and 41 columns, covering January 1, 1951 through December 30, 2023. The columns include the date, river discharge, precipitation, temperature, and other weather and soil variables. A date being present does not mean every measurement is available. In particular, some discharge entries are blank.

`attributes_other_camels.csv` contains information about 671 CAMELS-US gauges, including their names, coordinates, and catchment areas. It covers more gauges than the three used here. The current loading notebook does not join these attributes into the model input table.

`license_camels.md` contains the attribution and license information supplied for the CAMELS-US data. Keep it with the source files and consult it when reusing the data.

## Where the files come from

The download code in [01_load_caravan_data.ipynb](../../caravan_dataset_analysis/01_load_caravan_data.ipynb) retrieves these five files from the Caravan CSV archive linked in that notebook. It skips files that are already present. The project uses Caravan version 1.6.

The same notebook reads the three daily files, selects the variables needed for this experiment, and writes a combined table to [data/processed](../processed/README.md). Selection and later cleaning happen outside this folder; the original files stay available for reference.
