# Results

## Data Summary

The final study grid contained 37,209 H3 resolution 6 cells covering the Falkland Islands fisheries grid plus a 50 km buffer. Across the 2014-2023 analysis period, this produced 3,652 daily time steps and 135,887,268 H3 cell-day records in the environmental feature grid. The environmental tables contained no duplicate `h3`/`date` keys and provided daily values for sea surface temperature, sea surface height, wind speed, log-transformed chlorophyll-a, seasonal terms, spatial gradients, and temporal anomalies.

\begin{figure}[htbp]
\centering
\includegraphics[width=0.75\textwidth]{figures/fishing_activity_mean_2014-2023.png}
\caption{Mean fishing activity (vessel-hours) across the 2014-2023 analysis period.}
\label{fig:fishing-activity}
\end{figure}

The raw Global Fishing Watch dataset included 2,297,069 manually curated AIS fishing-vessel presence records from 2,011 unique vessels, representing 3,094,974.5 fishing hours between 2014 and 2023. After spatial aggregation to the H3 grid, the processed fishing-effort table contained 849,818 active `h3`/`date` records spanning 17,218 H3 cells and all 3,652 dates in the analysis period. These records retained 3,086,036.2 fishing hours and were expanded with zero-valued fishing exposure across non-observed cell-days in the full 135,887,268-row modeling grid.

\begin{figure}[htbp]
\centering
\begin{tabular}{ccc}
\includegraphics[width=0.46\textwidth]{figures/bbal_presence_count_all_years.png} &
\includegraphics[width=0.46\textwidth]{figures/safs_presence_count_all_years.png}
\end{tabular}
\caption{Spatial distribution of aggregated telemetry presence counts for black-browed albatrosses (left) and South American fur seals (right) across the 2014-2023 analysis period.}
\label{fig:species-presence-observations}
\end{figure}

The cleaned SAERI telemetry dataset included 59,182 valid records from 42 tracked individuals and 76 trips during 2022-2023. Black-browed albatrosses (BBAL) accounted for 33,425 telemetry records from 27 individuals and 58 trips, while South American fur seals (SAFS) accounted for 25,757 records from 15 individuals and 18 trips. After aggregation to the H3 framework, the species-presence table contained 10,268 `h3`/`date`/`species` records spanning 6,763 H3 cells and 146 observed dates.

BBAL contributed 4,552 `h3`/`date`/`species` rows across 3,270 H3 cells and 16 dates, with 21,329 aggregated presence counts. SAFS contributed 5,716 rows across 4,024 H3 cells and 146 dates, with 19,495 aggregated presence counts.

The resulting modeling products were substantially larger than the raw biological observations because the workflow evaluated species use and risk across the full study grid. The species-training table contained 6,027,858 rows for observed species-date combinations, while the final joint plausibility, prediction, and cube-component tables each contained 257,916,862 species-cell-day records spanning the 2014-2023 analysis period.

## Environmental Feature Generation

### Environmental Coverage and Completeness

The environmental feature-generation workflow produced a continuous daily feature grid for all 37,209 H3 cells across the full 2014-2023 analysis period. The resulting environmental table contained 135,887,268 H3 cell-day records, with one record for each cell on each of 3,652 dates. No duplicate `h3`/`date` keys were present.

Coverage was complete for SST and exceeded 96% for all other dynamic environmental variables. Static spatial predictors, including bathymetric depth, slope, distance to coast, and encoded spatial coordinates, were complete for all H3 cells. Summary statistics for the environmental feature space are provided in Table X.

<!-- Suggested figure: Example environmental layers for a representative date showing SST, SSH, CHL, and 
wind speed aggregated to the H3 grid. -->

\begin{figure}[htbp]
\centering
\begin{tabular}{ccc}
\includegraphics[width=0.31\textwidth]{figures/sst_20221210.png} &
\includegraphics[width=0.31\textwidth]{figures/chl_log_grad_20221210.png} &
\includegraphics[width=0.31\textwidth]{figures/ssh_grad_20221210.png} \\
\includegraphics[width=0.31\textwidth]{figures/sst_anom_20221210.png} &
\includegraphics[width=0.31\textwidth]{figures/ssh_anom_20221210.png} &
\includegraphics[width=0.31\textwidth]{figures/sst_grad_20221210.png}
\end{tabular}
\caption{Example environmental feature layers aggregated to the H3 grid for 10 December 2022. The panels show base environmental conditions, anomaly fields, and local gradient structure used by the feature-generation workflow.}
\label{fig:environmental-layers-20221210}
\end{figure}

<!-- Suggested figure ends here -->

\begin{table}[htbp]
\centering
\small
\caption{Summary statistics for environmental and static predictors.}
\label{tab:environmental-predictors}
\begin{tabular}{lrrrrr}
\hline
Predictor & Completeness & Mean & Median & 95th pct. & Range \\
\hline
SST (K) & 100.0\% & 280.16 & 279.64 & 285.85 & 271.41--293.46 \\
SSH (m) & 99.5\% & 0.08 & 0.16 & 0.50 & -1.32--1.25 \\
Wind speed (m/s) & 98.4\% & 8.08 & 8.11 & 13.29 & 0.00--20.54 \\
CHL log & 96.6\% & 0.34 & 0.22 & 0.95 & 0.02--4.19 \\
SST anomaly (K) & 100.0\% & 0.00 & -0.01 & 1.37 & -6.00--7.00 \\
SSH anomaly (m) & 99.5\% & 0.00 & 0.00 & 0.18 & -0.90--1.02 \\
Wind-speed anomaly (m/s) & 98.4\% & 0.00 & 0.06 & 4.79 & -10.58--11.91 \\
CHL-log anomaly & 96.6\% & 0.00 & -0.01 & 0.28 & -1.75--3.41 \\
SST gradient & 100.0\% & 0.11 & 0.08 & 0.28 & 0.00--1.23 \\
SSH gradient & 99.5\% & 0.01 & 0.01 & 0.04 & 0.00--0.14 \\
CHL-log gradient & 96.6\% & 0.02 & 0.01 & 0.10 & 0.00--2.67 \\
Day-of-year sine & 100.0\% & 0.00 & 0.00 & 0.99 & -1.00--1.00 \\
Day-of-year cosine & 100.0\% & 0.00 & 0.00 & 0.99 & -1.00--1.00 \\
Depth (m) & 100.0\% & 2,099.91 & 1,581.68 & 6,032.38 & -440.58--6,261.56 \\
Slope & 100.0\% & 0.03 & 0.01 & 0.13 & 0.00--0.50 \\
Distance to coast (km) & 100.0\% & 308.61 & 296.25 & 598.69 & 0.01--789.50 \\
\hline
\end{tabular}
\end{table}

### Derived Environmental Features

The final feature grid expanded the raw environmental inputs into a richer spatiotemporal representation including base oceanographic variables, seasonal encodings, static spatial predictors, local spatial gradients, and temporal anomaly fields. Together, these variables described not only environmental state, but also seasonal timing, coastal and bathymetric context, local spatial heterogeneity, and departures from expected seasonal conditions.

Correlations among the base environmental predictors were generally moderate. SST showed positive correlations with chlorophyll-a and SSH, while wind speed was only weakly correlated with the other variables.

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

### Spatial Gradients and Front-Like Structure

Spatial gradient features captured local environmental heterogeneity across neighboring H3 cells. Most cell-days showed relatively smooth local conditions, while a smaller subset contained stronger spatial transitions associated with front-like structure and shelf-boundary variability. These layers therefore added information distinct from the base environmental state variables.

### Seasonal and Anomaly Features

Seasonal predictors preserved continuous cyclic annual structure across the full 10-year record. Environmental anomalies remained centered near zero, consistent with their definition relative to local seasonal climatologies.

\begin{figure}[htbp]
\centering
\includegraphics[width=0.85\textwidth]{figures/sst_anom_daily_mean_2014-2023.png}\\[0.5em]
\includegraphics[width=0.85\textwidth]{figures/wind_speed_anom_daily_mean_2014-2023.png}
\caption{Daily mean environmental anomalies across the study area for 2014--2023. The upper panel shows SST anomalies and the lower panel shows wind-speed anomalies.}
\label{fig:daily-anomaly-timeseries}
\end{figure}

Yearly mean anomaly patterns indicated that the environmental feature space retained interannual variability after seasonal adjustment. SST anomalies were generally negative during 2014-2016 and positive during 2017-2018 and 2020-2023, while wind-speed anomalies also varied substantially among years. These results indicate that the generated feature space preserved daily, seasonal, spatial, and interannual variability for downstream species-use and risk modeling.

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

Species-use models were evaluated using held-out test data after back-transformation to the original residence-index scale. Model comparison metrics included coefficient of determination ($R^2$), root mean squared error (RMSE), and mean absolute error (MAE). All candidate models were evaluated on the same joint-species train-test split, with 15,278 training rows and 5,092 held-out test rows.

\begin{table}[htbp]
\centering
\small
\caption{Performance metrics for candidate species-use models evaluated on held-out test data.}
\label{tab:species-model-performance}
\begin{tabular}{lrrr}
\hline
Candidate model & $R^2$ & RMSE & MAE \\
\hline
Histogram gradient boosting & 0.606 & 60.273 & 3.665 \\
Random forest & 0.807 & 42.186 & 3.274 \\
Extra trees & 0.916 & 27.758 & 2.379 \\
Bayesian/Gaussian mixture & 0.045 & 93.852 & 6.259 \\
\hline
\end{tabular}
\end{table}

The Extra Trees model produced the strongest predictive performance across all evaluation metrics, with the highest coefficient of determination ($R^2 = 0.916$) and the lowest RMSE and MAE values among the evaluated models. Random forest also performed well, although with substantially higher prediction error and lower explained variance than Extra Trees. Histogram gradient boosting produced moderate predictive performance but showed reduced ability to capture variation in the residence-index response.

The Bayesian/Gaussian mixture model produced substantially lower predictive accuracy when evaluated directly against the residence-index target. This result was expected because the model was designed primarily to estimate environmental plausibility rather than to optimize direct regression performance. Consequently, the Bayesian/GMM component was retained as an environmental-support filter within the hybrid workflow rather than as the primary species-use predictor.

These results indicate that tree-based ensemble methods were substantially more effective than the probabilistic environmental-likelihood approach for predicting telemetry-derived species-use intensity within the integrated H3/day environmental feature space.

\begin{figure}[htbp]
\centering
\begin{tabular}{ccc}
\includegraphics[width=0.32\textwidth]{figures/extra_trees_observed_vs_predicted.png} &
\includegraphics[width=0.32\textwidth]{figures/random_forest_observed_vs_predicted.png} &
\includegraphics[width=0.32\textwidth]{figures/hist_gradient_boosting_observed_vs_predicted.png}
\end{tabular}
\caption{Observed versus predicted residence-index values for the three tree-based species-use models. Panels show Extra Trees, random forest, and histogram gradient boosting from left to right. Points are shown as density bins for the held-out test set, and the dashed line indicates one-to-one agreement.}
\label{fig:tree-models-observed-predicted}
\end{figure}

<!-- Suggested figure: Observed-versus-predicted log-transformed residence-index values for candidate species-use models. -->

Observed-versus-predicted comparisons on the log-transformed residence-index scale showed that the Extra Trees model most closely reproduced the 1:1 relationship across the full response range. Random forest also captured the dominant structure of the response distribution but showed greater compression toward intermediate prediction values and increased underprediction at higher residence-index values. Histogram gradient boosting produced the strongest prediction compression and the weakest representation of high-use observations.

Prediction variance increased with residence-index magnitude for all models, reflecting the sparse and highly skewed distribution of high-intensity telemetry detections. The vertical banding at lower observed values resulted from the discrete count-based structure of the residence-index target after log transformation.

\begin{figure}[htbp]
\centering
\begin{tabular}{ccc}
\includegraphics[width=0.32\textwidth]{figures/extra_trees_log_residual_distribution.png} &
\includegraphics[width=0.32\textwidth]{figures/random_forest_log_residual_distribution.png} &
\includegraphics[width=0.32\textwidth]{figures/hist_gradient_boosting_log_residual_distribution.png}
\end{tabular}
\caption{Residual distributions for the three tree-based species-use models on the log-transformed residence-index scale. Panels show Extra Trees, random forest, and histogram gradient boosting from left to right. The dashed vertical line indicates zero residual.}
\label{fig:tree-models-residual-distributions}
\end{figure}

<!-- Suggested figure: Residual distributions for candidate species-use models evaluated on the log-transformed residence-index scale. -->

Residual distributions were centered near zero for all evaluated models, indicating that predictions were generally unbiased on the log-transformed response scale. However, all models showed asymmetric residual structure with broader positive tails, reflecting reduced accuracy and increased variance for higher residence-index observations.

The Extra Trees model produced the narrowest residual distribution and the strongest concentration near zero, indicating the most stable predictive performance among the evaluated models. Random forest showed broader residual spread and heavier positive tails, while histogram gradient boosting produced the widest residual distribution and the strongest asymmetry.

These residual patterns are consistent with the sparse and highly skewed structure of telemetry-derived residence-index values, where high-intensity species-use observations were relatively rare compared with low-use and zero-use cell-days.

---

\begin{figure}[H]
\centering
\includegraphics[width=0.78\textwidth]{figures/species_feature_importance_top_features.png}
\caption{Feature-importance summary for the selected species-use model. Bars show relative feature importance for the highest-ranked predictors in the Extra Trees joint species-use model.}
\label{fig:species-feature-importance}
\end{figure}

<!-- Suggested figure: Relative feature importance for the selected Extra Trees species-use model. -->

Feature-importance analysis indicated that static spatial structure and oceanographic variability contributed strongly to species-use predictions. Bathymetry and distance to coast were the two highest-ranked predictors, followed by SSH anomaly, SSH, seafloor slope, and SST. Environmental-gradient variables, including chlorophyll, SST, and SSH gradients, also contributed substantially to the fitted model.

The relatively high importance of anomaly and gradient predictors indicates that local environmental heterogeneity and departures from expected seasonal conditions provided information beyond the base environmental state variables alone. These results support the inclusion of derived environmental features within the riskscape framework.

Species-indicator variables contributed comparatively less importance than the environmental and spatial predictors, suggesting that the joint-species model captured substantial shared environmental structure across the two study species. Seasonal sine/cosine predictors also showed relatively low importance, indicating that direct environmental conditions explained more variation in species use than cyclic seasonal timing alone.

\begin{figure}[htbp]
\centering
\includegraphics[width=0.92\textwidth]{figures/extra_trees_partial_dependence.png}
\caption{Partial-dependence plots for key predictors in the selected Extra Trees species-use model. Curves show mean predicted species use on the log-transformed response scale while varying one predictor at a time across its central observed range.}
\label{fig:extra-trees-partial-dependence}
\end{figure}

<!-- Suggested figure: Partial dependence plots for selected predictors from the Extra Trees species-use model. -->

Partial dependence analysis showed strong nonlinear relationships between predicted species use and several environmental and spatial predictors. Predicted species use was highest in shallow waters and declined progressively with increasing bathymetric depth and distance from the coast, indicating strong association with shelf and coastal environments.

SST showed a pronounced nonlinear response, with predicted species use increasing rapidly between approximately 7 and 9 °C before declining at warmer temperatures. SSH also exhibited a nonlinear relationship, with highest predicted use occurring at intermediate positive SSH values and declining sharply at the upper end of the observed range.

Environmental-gradient predictors contributed additional structure beyond the base environmental variables. In particular, predicted species use increased under stronger chlorophyll-gradient conditions, suggesting association with localized environmental transitions and front-like heterogeneity.

These response patterns indicate that the selected model captured complex and non-monotonic relationships between species use and environmental conditions. However, because several predictors were moderately correlated, the partial dependence curves should be interpreted as model-response summaries rather than as independent causal ecological relationships.

## Environmental Plausibility Surfaces

## Fishing Exposure Patterns

## Realized Risk Surfaces

## Latent Risk Surfaces

## Spatial and Temporal Patterns

\newpage
