# Appendices

## Fishing Exposure Summary

This appendix provides additional summaries and diagnostic visualizations for the Global Fishing Watch (GFW) fishing-effort dataset used in the riskscape framework. Raw AIS-derived vessel-presence records were aggregated to the H3 grid to generate daily fishing-exposure features used in the realized-risk workflow.

### Raw Fishing-Effort Dataset

The raw GFW dataset contained 2,297,069 manually curated AIS fishing-vessel presence records from 2,011 unique vessels between 2014 and 2023, representing 3,094,974.5 fishing hours.

The dominant fishing gear types were trawlers, squid jiggers, and set longlines. Trawlers accounted for 1,497,210 fishing hours from 567 vessels, squid jiggers for 1,256,653 fishing hours from 1,207 vessels, and set longlines for 202,871 fishing hours from 41 vessels. The largest fishing-effort contributions were associated with Argentina (ARG), China (CHN), Taiwan (TWN), South Korea (KOR), Spain (ESP), and the Falkland Islands (FLK).

### Spatial Aggregation to the H3 Framework

Raw fishing-effort observations were converted to geographic points, spatially joined to the H3 study grid, and aggregated by `h3` and `date`. The processed fishing-effort table contained 849,818 active `h3`/`date` records spanning 17,218 H3 cells and all 3,652 dates in the 2014–2023 analysis period.

The final fishing-exposure grid retained 3,086,036.2 fishing hours after spatial aggregation. Zero-valued fishing-exposure rows were then added for all H3/date combinations without observed fishing activity, producing a complete 135,887,268-row fishing-exposure framework aligned to the environmental feature grid.

### Mean Fishing Exposure

\begin{figure}[htbp]
\centering
\includegraphics[height=0.48\textheight,keepaspectratio]{figures/fishing_activity_mean_2014-2023.png}
\caption{Mean fishing activity during 2014--2023 summarized as vessel-hours by H3 cell.}
\label{fig:appendix-fishing-activity-mean-2014-2023}
\end{figure}

### Seasonal Fishing Variability

\begin{figure}[htbp]
\centering
\includegraphics[width=0.92\textwidth]{figures/fishing_activity_monthly_totals_2014-2023.png}
\caption{Monthly fishing activity during 2014--2023 summarized as total fishing hours and unique vessel counts across the study area.}
\label{fig:appendix-fishing-activity-monthly-totals}
\end{figure}

\begin{figure}[htbp]
\centering
\includegraphics[height=0.76\textheight,keepaspectratio]{figures/fishing_activity_non_zero_median_monthly_matrix_2022.png}
\caption{Monthly fishing activity during 2022 summarized as non-zero median vessel-hours by H3 cell. Each panel represents one month and shows the spatial distribution of fishing exposure among cells with observed fishing activity.}
\label{fig:appendix-monthly-fishing-activity-2022}
\end{figure}

### Additional Temporal Diagnostics

\begin{figure}[htbp]
\centering
\includegraphics[width=0.85\textwidth]{figures/fishing_activity_daily_totals_2022.png}
\caption{Daily fishing activity totals during 2022 summarized as total fishing hours and unique vessels across the study area.}
\label{fig:appendix-fishing-activity-daily-2022}
\end{figure}

\begin{figure}[htbp]
\centering
\includegraphics[width=0.85\textwidth]{figures/fishing_activity_monthly_totals_2022.png}
\caption{Monthly fishing activity totals during 2022 summarized as total fishing hours and unique vessel counts across the study area.}
\label{fig:appendix-fishing-activity-monthly-2022}
\end{figure}

## Appendix X. Environmental Predictor Correlations

Spearman rank correlations among dynamic environmental predictors across the full 2014-2023 environmental feature grid are shown below.

$$
\scriptsize
\mathbf{R} =
\left[
\begin{array}{rrrrrrrrrrr}
1.00 & 0.49 & -0.15 & 0.57 & 0.25 & 0.13 & -0.02 & -0.02 & 0.08 & -0.28 & 0.47 \\
0.49 & 1.00 & -0.10 & 0.56 & 0.14 & 0.27 & 0.00 & -0.04 & -0.26 & -0.57 & 0.40 \\
-0.15 & -0.10 & 1.00 & -0.12 & -0.06 & 0.02 & 0.92 & 0.01 & 0.02 & 0.09 & -0.14 \\
0.57 & 0.56 & -0.12 & 1.00 & 0.12 & 0.06 & 0.00 & 0.35 & -0.02 & -0.38 & 0.79 \\
0.25 & 0.14 & -0.06 & 0.12 & 1.00 & 0.47 & -0.06 & 0.20 & -0.01 & 0.01 & 0.09 \\
0.13 & 0.27 & 0.02 & 0.06 & 0.47 & 1.00 & 0.03 & 0.05 & -0.01 & 0.09 & 0.04 \\
-0.02 & 0.00 & 0.92 & 0.00 & -0.06 & 0.03 & 1.00 & -0.01 & 0.02 & 0.01 & -0.01 \\
-0.02 & -0.04 & 0.01 & 0.35 & 0.20 & 0.05 & -0.01 & 1.00 & -0.01 & 0.04 & 0.28 \\
0.08 & -0.26 & 0.02 & -0.02 & -0.01 & -0.01 & 0.02 & -0.01 & 1.00 & 0.28 & 0.03 \\
-0.28 & -0.57 & 0.09 & -0.38 & 0.01 & 0.09 & 0.01 & 0.04 & 0.28 & 1.00 & -0.25 \\
0.47 & 0.40 & -0.14 & 0.79 & 0.09 & 0.04 & -0.01 & 0.28 & 0.03 & -0.25 & 1.00
\end{array}
\right]
$$

$$
\begin{aligned}
1  &= \mathrm{SST} \\
2  &= \mathrm{SSH} \\
3  &= \mathrm{WindSpeed} \\
4  &= \log(\mathrm{CHL}) \\
5  &= \mathrm{SST}_{anom} \\
6  &= \mathrm{SSH}_{anom} \\
7  &= \mathrm{Wind}_{anom} \\
8  &= \log(\mathrm{CHL})_{anom} \\
9  &= \mathrm{SST}_{grad} \\
10 &= \mathrm{SSH}_{grad} \\
11 &= \log(\mathrm{CHL})_{grad}
\end{aligned}
$$

The environmental predictor space showed moderate positive correlations among SST, SSH, and log-transformed chlorophyll-a ($\rho \approx 0.49$-$0.57$), indicating that warmer conditions were generally associated with elevated SSH and higher chlorophyll concentrations across the study region. Wind speed showed weak negative correlations with the other base environmental variables ($\rho \approx -0.10$ to $-0.15$).

Anomaly variables were generally weakly correlated with the corresponding base environmental fields, indicating that the anomaly representation captured departures from expected seasonal conditions rather than simply reproducing the original environmental gradients. The strongest anomaly relationship occurred between SST and SSH anomalies ($\rho = 0.47$), suggesting partial coupling between thermal and sea-surface-height variability.

Spatial-gradient predictors also showed largely distinct behavior relative to the base environmental variables. SST and SSH gradients had weak correlations with most base predictors, while SSH gradient exhibited moderate negative correlations with SSH ($\rho = -0.57$) and log-transformed chlorophyll-a ($\rho = -0.38$). Log-transformed chlorophyll-a and its gradient remained strongly correlated ($\rho = 0.79$), indicating that high-productivity regions were frequently associated with strong local chlorophyll contrasts.

\newpage

## Feature Correlations

<!-- Figure fig:environmental-correlation: Spearman correlation matrix for environmental predictors. -->
\begin{figure}[htbp]
\centering
\includegraphics[width=0.85\textwidth]{figures/environmental_predictors_spearman_correlation_all.png}
\caption{Spearman correlation matrix for environmental predictors across the 2014-2023 analysis period.}
\label{fig:environmental-correlation}
\end{figure}

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

\newpage

## Anomalies

<!-- Figure fig:environmental-anomaly-histograms: distributions of anomaly predictors. -->
\begin{figure}[htbp]
\centering
\begin{tabular}{cc}
\includegraphics[width=0.48\textwidth]{figures/sst_anom_histogram_2014-2023.png} &
\includegraphics[width=0.48\textwidth]{figures/wind_speed_anom_histogram_2014-2023.png} \\
\includegraphics[width=0.48\textwidth]{figures/chl_log_anom_histogram_2014-2023.png} &
\includegraphics[width=0.48\textwidth]{figures/ssh_anom_histogram_2014-2023.png}
\end{tabular}
\caption{Distributions of environmental anomaly predictors across the full 2014--2023 feature set. The upper row shows SST and wind-speed anomalies, and the lower row shows CHL and SSH anomalies.}
\label{fig:environmental-anomaly-histograms}
\end{figure}

\newpage

## Appendix A: Datasets

| Dataset                       | Provider                            | Product                                                     | Variable(s)                                          | Description                                                                                               |
|-------------------------------|-------------------------------------|-------------------------------------------------------------|------------------------------------------------------|-----------------------------------------------------------------------------------------------------------|
| Sea Surface Temperature (SST) | NASA PO.DAAC                        | MUR-JPL-L4-GLOB-v4.1                                        | `analysed_sst`                                       | Multi-scale Ultra-high Resolution (MUR) Level 4 daily sea surface temperature product.                    |
| Chlorophyll-a (CHL)           | Copernicus Marine Service           | `cmems_obs-oc_glo_bgc-plankton_my_l4-gapfree-multi-4km_P1D` | `CHL`                                                | Global daily gap-free Level 4 chlorophyll-a concentration product derived from ocean colour observations. |
| Sea Surface Height (SSH)      | Copernicus Marine Service           | `cmems_obs-sl_glo_phy-ssh_my_allsat-l4-duacs-0.125deg_P1D`  | `adt`                                                | Global daily Level 4 sea level product providing absolute dynamic topography from satellite altimetry.    |
| Wind                          | Copernicus Climate Data Store (CDS) | `derived-era5-single-levels-daily-statistics`               | `10m_u_component_of_wind`, `10m_v_component_of_wind` | Daily ERA5-derived near-surface wind components at 10 m above sea level.                                  |
| Fishing Effort                | Global Fishing Watch                | AIS-derived fishing activity products                       | —                                                    | AIS-derived fishing activity and vessel effort products used to estimate fishing exposure.                |
| Bathymetry                    | GEBCO                               | `gebco_2026`                                                | `elevation`                                          | Global bathymetric elevation model used to derive seafloor depth and bathymetric features.                |

<https://www.earthdata.nasa.gov/data/catalog/pocloud-mur-jpl-l4-glob-v4.1-4.1>

\newpage
