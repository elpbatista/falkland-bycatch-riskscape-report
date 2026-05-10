# Appendices

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
