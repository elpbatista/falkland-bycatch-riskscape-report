# Results

## Data Summary

The final study grid contained 37,209 H3 resolution 6 cells covering the Falkland Islands fisheries grid plus a 50 km buffer. Across the 2014-2023 analysis period, this produced 3,652 daily time steps and 135,887,268 H3 cell-day records in the environmental feature grid. The environmental tables contained no duplicate `h3`/`date` keys and provided daily values for sea surface temperature, sea surface height, wind speed, log-transformed chlorophyll-a, seasonal terms, spatial gradients, and temporal anomalies.

The raw Global Fishing Watch dataset included 2,297,069 manually curated AIS fishing-vessel presence records from 2,011 unique vessels, representing 3,094,974.5 fishing hours between 2014 and 2023. After spatial aggregation to the H3 grid, the processed fishing-effort table contained 849,818 active `h3`/`date` records spanning 17,218 H3 cells and all 3,652 dates in the analysis period. These records retained 3,086,036.2 fishing hours and were expanded with zero-valued fishing exposure across non-observed cell-days in the full 135,887,268-row modeling grid.

The cleaned SAERI telemetry dataset included 59,182 valid records from 42 tracked individuals and 76 trips during 2022-2023. Black-browed albatrosses (BBAL) accounted for 33,425 telemetry records from 27 individuals and 58 trips, while South American fur seals (SAFS) accounted for 25,757 records from 15 individuals and 18 trips. After aggregation to the H3 framework, the species-presence table contained 10,268 `h3`/`date`/`species` records spanning 6,763 H3 cells and 146 observed dates.

BBAL contributed 4,552 `h3`/`date`/`species` rows across 3,270 H3 cells and 16 dates, with 21,329 aggregated presence counts. SAFS contributed 5,716 rows across 4,024 H3 cells and 146 dates, with 19,495 aggregated presence counts.

The resulting modeling products were substantially larger than the raw biological observations because the workflow evaluated species use and risk across the full study grid. The species-training table contained 6,027,858 rows for observed species-date combinations, while the final joint plausibility, prediction, and cube-component tables each contained 257,916,862 species-cell-day records spanning the 2014-2023 analysis period.

## Environmental Feature Generation

### Environmental Coverage and Completeness

The environmental feature-generation workflow produced a continuous daily feature grid for all 37,209 H3 cells across the full 2014-2023 analysis period. The resulting environmental table contained 135,887,268 H3 cell-day records, with one record for each cell on each of 3,652 dates. No duplicate `h3`/`date` keys were present.

Coverage was complete for SST and exceeded 96% for all other dynamic environmental variables. Static spatial predictors, including bathymetric depth, slope, distance to coast, and encoded spatial coordinates, were complete for all H3 cells. Summary statistics for the environmental feature space are provided in Table X.

<!-- Suggested figure: Example environmental layers for a representative date showing SST, SSH, CHL, and wind speed aggregated to the H3 grid. -->

| Predictor                | Completeness |     Mean |   Median | 95th percentile |            Range |
|--------------------------|-------------:|---------:|---------:|----------------:|-----------------:|
| SST (K)                  |       100.0% |   280.16 |   279.64 |          285.85 |    271.41-293.46 |
| SSH (m)                  |        99.5% |     0.08 |     0.16 |            0.50 |       -1.32-1.25 |
| Wind speed (m/s)         |        98.4% |     8.08 |     8.11 |           13.29 |       0.00-20.54 |
| CHL log                  |        96.6% |     0.34 |     0.22 |            0.95 |        0.02-4.19 |
| SST anomaly (K)          |       100.0% |     0.00 |    -0.01 |            1.37 |       -6.00-7.00 |
| SSH anomaly (m)          |        99.5% |     0.00 |     0.00 |            0.18 |       -0.90-1.02 |
| Wind-speed anomaly (m/s) |        98.4% |     0.00 |     0.06 |            4.79 |     -10.58-11.91 |
| CHL-log anomaly          |        96.6% |     0.00 |    -0.01 |            0.28 |       -1.75-3.41 |
| SST gradient             |       100.0% |     0.11 |     0.08 |            0.28 |        0.00-1.23 |
| SSH gradient             |        99.5% |     0.01 |     0.01 |            0.04 |        0.00-0.14 |
| CHL-log gradient         |        96.6% |     0.02 |     0.01 |            0.10 |        0.00-2.67 |
| Day-of-year sine         |       100.0% |     0.00 |     0.00 |            0.99 |       -1.00-1.00 |
| Day-of-year cosine       |       100.0% |     0.00 |     0.00 |            0.99 |       -1.00-1.00 |
| Depth (m)                |       100.0% | 2,099.91 | 1,581.68 |        6,032.38 | -440.58-6,261.56 |
| Slope                    |       100.0% |     0.03 |     0.01 |            0.13 |        0.00-0.50 |
| Distance to coast (km)   |       100.0% |   308.61 |   296.25 |          598.69 |      0.01-789.50 |

### Derived Environmental Features

The final feature grid expanded the raw environmental inputs into a richer spatiotemporal representation including base oceanographic variables, seasonal encodings, static spatial predictors, local spatial gradients, and temporal anomaly fields. Together, these variables described not only environmental state, but also seasonal timing, coastal and bathymetric context, local spatial heterogeneity, and departures from expected seasonal conditions.

Correlations among the base environmental predictors were generally moderate. SST showed positive correlations with chlorophyll-a and SSH, while wind speed was only weakly correlated with the other variables.

<!-- Suggested figure: Correlation heatmap of environmental predictors. -->

$$
\mathbf{R} =
\begin{bmatrix}
1.00 & 0.57 & 0.49 & -0.15 \\
0.57 & 1.00 & 0.56 & -0.12 \\
0.49 & 0.56 & 1.00 & -0.10 \\
-0.15 & -0.12 & -0.10 & 1.00
\end{bmatrix}
$$

$$
\begin{aligned}
1 &= \mathrm{SST} \\
2 &= \log(1 + \mathrm{CHL}) \\
3 &= \mathrm{SSH} \\
4 &= \mathrm{WindSpeed}
\end{aligned}
$$

### Spatial Gradients and Front-Like Structure

Spatial gradient features captured local environmental heterogeneity across neighboring H3 cells. Most cell-days showed relatively smooth local conditions, while a smaller subset contained stronger spatial transitions associated with front-like structure and shelf-boundary variability. These layers therefore added information distinct from the base environmental state variables.

<!-- Suggested figure: Example gradient layers for SST, SSH, and log-CHL highlighting frontal structure and shelf transitions. -->

### Seasonal and Anomaly Features

Seasonal predictors preserved continuous cyclic annual structure across the full 10-year record. Environmental anomalies remained centered near zero, consistent with their definition relative to local seasonal climatologies.

Yearly mean anomaly patterns indicated that the environmental feature space retained interannual variability after seasonal adjustment. SST anomalies were generally negative during 2014-2016 and positive during 2017-2018 and 2020-2023, while wind-speed anomalies also varied substantially among years. These results indicate that the generated feature space preserved daily, seasonal, spatial, and interannual variability for downstream species-use and risk modeling.

<!-- Suggested figure: Time series of yearly mean SST and wind-speed anomalies across the study area. -->

<!-- Suggested figure: Distribution plots or histograms of anomaly variables showing centered seasonal departures. -->

## Species-Use Modeling Performance

## Environmental Plausibility Surfaces

## Fishing Exposure Patterns

## Realized Risk Surfaces

## Latent Risk Surfaces

## Spatial and Temporal Patterns
