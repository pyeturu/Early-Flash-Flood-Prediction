# NOAA flood reports

This supplement contains reported floods for the counties containing our three river gauges. It comes from the [NOAA Storm Events bulk CSV archive](https://www.ncei.noaa.gov/pub/data/swdi/stormevents/csvfiles/).

| River gauge | County screened | State FIPS | County FIPS |
| --- | --- | --- | --- |
| `01411300`, Tuckahoe River | Cape May County, New Jersey | 34 | 009 |
| `05495500`, Bear Creek | Adams County, Illinois | 17 | 001 |
| `14182500`, Little North Santiam River | Marion County, Oregon | 41 | 047 |

The county assignments were checked against [USGS station metadata](https://waterservices.usgs.gov/nwis/site/?format=rdb&sites=01411300,05495500,14182500&siteOutput=expanded). These counties are a first screening area, not the watershed boundaries. An upstream part of a basin can lie in another county.

`county_flood_reports_1996_2014.csv` retains NOAA's original columns for the selected records and adds `source_url`. We select `EVENT_TYPE` equal to `Flood` or `Flash Flood`, with `CZ_TYPE` equal to `C` and the matching state and county FIPS codes. Coastal floods, other event types, and forecast-zone-coded records are excluded. In older years, zone-coded reports can describe relevant floods, so their exclusion limits coverage.

`source_manifest.csv` records the 19 annual source files, download times, SHA-256 checksums, national record counts, and selected county record counts. It also contains the saved subset's checksum. The annual national archives are read during download rather than stored in this repository.

The comparison starts in 1996 because NOAA's consistently classified event archive begins then, and ends in 2014 because these Caravan discharge records stop during that year. The event records are reports, not an exhaustive list of floods. A missing report must not be treated as proof that flooding did not occur.

Run [04_crosscheck_noaa_floods.ipynb](../../../caravan_dataset_analysis/04_crosscheck_noaa_floods.ipynb) to use the saved subset and regenerate the comparison. Its helper script downloads the subset when the source CSV and manifest are absent; otherwise it checks and reuses them. The three original Caravan basin files are unchanged.
