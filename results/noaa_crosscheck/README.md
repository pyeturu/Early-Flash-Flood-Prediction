# NOAA flood-date comparison

This analysis compares observed river discharge with NOAA flood reports for Cape May County, New Jersey; Adams County, Illinois; and Marion County, Oregon. It asks whether high-flow days at our gauges coincide with reported floods in their counties.

The comparison covers 1996-2014. Run [04_crosscheck_noaa_floods.ipynb](../../caravan_dataset_analysis/04_crosscheck_noaa_floods.ipynb), or run `python caravan_dataset_analysis/crosscheck_noaa_floods.py` from the repository root. The inputs and source links are documented in [data/raw/noaa_storm_events](../../data/raw/noaa_storm_events/README.md).

## Files

- `noaa_flood_high_flow_comparison.png`: one panel per river. Orange points mark high-flow days; red points also overlap a county flood report. Separate tracks show county Flood and Flash Flood report days, including dates where discharge is missing.
- `basin_summary.csv`: the discharge threshold, counts of county reports, report records overlapping high flow, and daily overlap totals for each basin.
- `event_matches.csv`: one row per county report, including event ID, type, start and end dates, available discharge days, high-flow overlap, peak observed flow during the interval, locations, narratives, and source links.
- `daily_comparison.csv`: one row per basin-day, with observed discharge, the threshold, a nullable high-flow flag, separate Flood/Flash Flood report counts, and the combined overlap flag.

## How dates and thresholds are matched

For each basin, the high-flow threshold is the 95th percentile of nonnegative observed discharge from January 1, 1990 through December 30, 2009, matching the training origin-date window. The fixed thresholds are 3.07 mm/day for Tuckahoe River, 3.4655 mm/day for Bear Creek, and 18.548 mm/day for Little North Santiam River. A day must be strictly above the threshold to count as high flow. This is a research definition of high flow, not an official flood threshold.

A same-date match falls within a NOAA report's start and end calendar dates, inclusive. `same_date_high_flow_days` counts those days for each report. `high_flow_within_one_day` allows one day on either side as a separate timing sensitivity check. It does not replace the same-date result.

NOAA times are local, while this first comparison uses Caravan's nominal daily dates without establishing their exact hourly alignment. Dates are therefore a screening comparison, not proof that a discharge peak and a reported flash flood happened at the same hour.

Missing or invalid discharge stays unknown. A NOAA report outside the available discharge period remains in the event table with zero available flow days and no estimated peak. It must not be interpreted as a below-threshold day. Report-day counts use the union of reported intervals; record counts count separate NOAA event IDs. Several records can belong to the same storm.

## Reading the evidence

The saved first comparison contains 55 county report records:

| Gauge county | Flood records | Flash Flood records | Records overlapping at least one high-flow day |
| --- | ---: | ---: | ---: |
| Cape May, New Jersey | 5 | 11 | 6 |
| Adams, Illinois | 7 | 15 | 16 |
| Marion, Oregon | 17 | 0 | 16 |

Examples worth inspecting in `event_matches.csv`:

- **August 21, 1997:** Tuckahoe discharge reaches 28.23 mm/day during the August 20-21 Cape May County Flash Flood report, event ID `5621734`. This is a county-level date match; the individual event narrative is missing.
- **May 12, 2002:** Bear Creek discharge reaches 57.68 mm/day on the date of Adams County Flash Flood report `5291597`. The broader episode describes flooding of creeks, roads, and farmland across the county, but does not identify the study gauge.
- **November 7, 2006:** Little North Santiam discharge reaches 122.35 mm/day during Marion County Flood report `4698`. Its narrative concerns the Santiam River near Jefferson, a different location, so date overlap cannot establish flooding at our gauge.

The only individual event narrative that names one of the study rivers is Adams County Flash Flood report `123444` on June 18, 2008. It describes a Mississippi River levee failure near Meyer, using Bear Creek as a geographic reference. Bear Creek's observed discharge that day is only 0.18 mm/day. This is a useful example of why even a matching river name needs manual review.

The spatial match is explicitly `gauge_county_only_not_verified_basin`. Many reports concern other rivers or local street flooding. `river_named_in_event_narrative` flags descriptions mentioning the selected river for closer reading; a name match still needs manual review. Episode narratives describe the broader storm and should not be read as proof of flooding at every location in that episode.

County screening can miss upstream floods in neighboring counties. Forecast-zone-coded reports are excluded because zone numbers cannot safely be interpreted as county FIPS codes. Coastal floods are also outside this screening. The USGS basin-outline service was unavailable when this analysis was prepared, so catchment intersections have not been verified.

These outputs compare observed flow with reports, not model predictions. The full-period summary is descriptive and includes years used to calculate the threshold. Unmatched high-flow days are not automatically false alarms, and unmatched reports are not automatically model misses. Verified basin event labels are needed before evaluating actual flood detection.
