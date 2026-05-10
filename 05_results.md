# Results

## Data Summary

The final study grid contained 37,209 H3 resolution 6 cells covering the Falkland Islands fisheries grid plus a 50 km buffer. Across the 2014-2023 analysis period, this produced 3,652 daily time steps and 135,887,268 H3 cell-day records in the environmental feature grid. The environmental tables contained no duplicate `h3`/`date` keys and provided daily values for sea surface temperature, sea surface height, wind speed, log-transformed chlorophyll-a, seasonal terms, spatial gradients, and temporal anomalies.

<!-- Figure fig:fishing-activity: mean fishing activity map for 2014-2023. -->
\begin{figure}[htbp]
\centering
\includegraphics[height=0.48\textwidth]{figures/fishing_activity_mean_2014-2023.png}
\caption{Mean fishing activity (vessel-hours) across the 2014-2023 analysis period.}
\label{fig:fishing-activity}
\end{figure}

The raw Global Fishing Watch dataset included 2,297,069 manually curated AIS fishing-vessel presence records from 2,011 unique vessels, representing 3,094,974.5 fishing hours between 2014 and 2023. After spatial aggregation to the H3 grid, the processed fishing-effort table contained 849,818 active `h3`/`date` records spanning 17,218 H3 cells and all 3,652 dates in the analysis period. These records retained 3,086,036.2 fishing hours and were expanded with zero-valued fishing exposure across non-observed cell-days in the full 135,887,268-row modeling grid.

<!-- Figure fig:species-presence-observations: telemetry presence count maps by species. -->
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

<!-- Figure fig:environmental-layers-20221210: example environmental feature layers for 10 December 2022. -->
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

<!-- Table tab:environmental-predictors: summary statistics and completeness for environmental predictors. -->
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

### Spatial Gradients and Front-Like Structure

Spatial gradient features captured local environmental heterogeneity across neighboring H3 cells. Most cell-days showed relatively smooth local conditions, while a smaller subset contained stronger spatial transitions associated with front-like structure and shelf-boundary variability. These layers therefore added information distinct from the base environmental state variables.

### Seasonal and Anomaly Features

Seasonal predictors preserved continuous cyclic annual structure across the full 10-year record. Environmental anomalies remained centered near zero, consistent with their definition relative to local seasonal climatologies.

<!-- Figure fig:daily-anomaly-timeseries: daily mean SST and wind-speed anomaly time series. -->
\begin{figure}[htbp]
\centering
\includegraphics[width=0.85\textwidth]{figures/sst_anom_daily_mean_2014-2023.png}\\[0.5em]
\includegraphics[width=0.85\textwidth]{figures/wind_speed_anom_daily_mean_2014-2023.png}
\caption{Daily mean environmental anomalies across the study area for 2014--2023. The upper panel shows SST anomalies and the lower panel shows wind-speed anomalies.}
\label{fig:daily-anomaly-timeseries}
\end{figure}

Yearly mean anomaly patterns indicated that the environmental feature space retained interannual variability after seasonal adjustment. SST anomalies were generally negative during 2014-2016 and positive during 2017-2018 and 2020-2023, while wind-speed anomalies also varied substantially among years. These results indicate that the generated feature space preserved daily, seasonal, spatial, and interannual variability for downstream species-use and risk modeling.

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

Species-use models were evaluated using held-out test data after back-transformation to the original residence-index scale. Model comparison metrics included coefficient of determination ($R^2$), root mean squared error (RMSE), and mean absolute error (MAE). All candidate models were evaluated on the same joint-species train-test split, with 15,278 training rows and 5,092 held-out test rows.

<!-- Table tab:species-model-performance: held-out performance metrics for species-use models. -->
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

<!-- Figure fig:tree-models-observed-predicted: observed-versus-predicted diagnostics for tree-based models. -->
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

<!-- Figure fig:tree-models-residual-distributions: residual distributions for tree-based species-use models. -->
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

<!-- Figure fig:species-feature-importance: Extra Trees feature importance summary. -->
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

<!-- Figure fig:extra-trees-partial-dependence: partial dependence plots for key predictors. -->
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

Environmental plausibility surfaces were generated using the Bayesian/Gaussian mixture model to evaluate how closely environmental conditions across the H3 study grid resembled those associated with telemetry-informed species use. Plausibility values therefore represent relative environmental support within the modeled feature space rather than direct estimates of species presence probability.

<!-- Figure fig:non-zero-median-environmental-plausibility: 2022 non-zero median plausibility maps by species. -->
\begin{figure}[htbp]
\centering
\begin{tabular}{cc}
\includegraphics[width=0.48\textwidth]{figures/bayesian_gmm_joint_plausibility_non_zero_median_BBAL_2022.png} &
\includegraphics[width=0.48\textwidth]{figures/bayesian_gmm_joint_plausibility_non_zero_median_SAFS_2022.png}
\end{tabular}
\caption{Non-zero median environmental plausibility during 2022 for black-browed albatrosses (left) and South American fur seals (right). Values summarize typical Bayesian/Gaussian mixture environmental support by H3 cell among days with non-zero plausibility.}
\label{fig:non-zero-median-environmental-plausibility}
\end{figure}

<!-- Suggested figure: Non-zero median environmental plausibility surfaces for BBAL and SAFS during 2022. -->

Non-zero median environmental plausibility surfaces showed strong spatial structure across the Falkland Islands shelf and adjacent offshore waters. High-plausibility regions generally coincided with shelf and shelf-break environments whose environmental conditions were well represented within the telemetry-informed training domain.

The BBAL plausibility surface showed concentrated environmental support west and northwest of the Falkland Islands, with sharp declines toward deep offshore waters and the northeastern portion of the study region. In contrast, the SAFS plausibility surface was broader and more spatially diffuse, with moderate environmental support extending across much of the continental shelf and surrounding coastal waters.

Both species exhibited structured transitions between high- and low-plausibility regions that followed large-scale oceanographic gradients and shelf boundaries rather than simple geographic proximity patterns. The consistently low plausibility observed across portions of the southern offshore region suggests that these environmental conditions were weakly represented within the telemetry-derived feature space and therefore correspond to areas of increased extrapolation uncertainty.

These results indicate that the Bayesian/Gaussian mixture framework captured coherent environmental-support structure across the study region and provided a useful diagnostic layer for identifying where species-use and risk predictions were environmentally well supported versus where predictions extended beyond the dominant telemetry-informed environmental domain.

### Temporal Variability in Environmental Plausibility

Environmental plausibility varied seasonally and interannually across the 2014-2023 analysis period. Seasonal changes in SST, SSH, chlorophyll-a, and wind structure produced corresponding shifts in the environmental-support surfaces, particularly along frontal and shelf-transition regions. Interannual variability in anomaly fields also altered the spatial extent of environmentally supported conditions through time.

Despite these temporal shifts, the highest plausibility values remained concentrated within recurrent shelf-associated oceanographic regimes represented within the telemetry-informed feature space. Lower-plausibility conditions were more common during periods associated with strong environmental anomalies or uncommon combinations of environmental predictors.

<!-- Figure fig:yearly-non-zero-median-plausibility: yearly plausibility time series by species. -->
\begin{figure}[htbp]
\centering
\includegraphics[width=0.78\textwidth]{figures/yearly_non_zero_median_plausibility_2014-2023.png}
\caption{Yearly non-zero median environmental plausibility across the study region for black-browed albatrosses and South American fur seals during 2014--2023. Values summarize typical non-zero Bayesian/Gaussian mixture environmental support within each year, matching the aggregation used for the mapped plausibility surfaces.}
\label{fig:yearly-non-zero-median-plausibility}
\end{figure}

Yearly non-zero median plausibility was used to summarize temporal variation in environmentally supported cell-days while reducing the influence of the large number of zero-plausibility grid cells. This metric does not represent average habitat suitability; instead, it describes the typical plausibility value among H3 cell-days with non-zero environmental support.

Non-zero median plausibility varied through time for both species. SAFS showed a gradual increase from 2014 to 2023, with moderate declines in 2019 and 2022. BBAL showed stronger interannual variability, with higher values in 2018, 2021, and 2022 and a sharp decline in 2023. These patterns indicate that the environmental conditions represented by the telemetry-informed plausibility model were not equally expressed across years.

<!-- Figure fig:monthly-plausibility: monthly plausibility matrices by species. -->
\begin{figure}[htbp]
\centering
\begin{tabular}{cc}
\includegraphics[width=0.48\textwidth]{figures/monthly_non_zero_median_plausibility_BBAL_2014-2023.png} &
\includegraphics[width=0.48\textwidth]{figures/monthly_non_zero_median_plausibility_SAFS_2014-2023.png}
\end{tabular}
\caption{Monthly non-zero median environmental plausibility surfaces across 2014–2023 for Black-browed albatrosses (left) and South American fur seals (right). Each monthly surface summarizes Bayesian/Gaussian mixture environmental support by H3 cell among modeled days with non-zero plausibility.}
\label{fig:monthly-plausibility}
\end{figure}

<!-- Suggested figure: Monthly non-zero median environmental plausibility surfaces for BBAL and SAFS across 2014-2023. -->

Monthly plausibility surfaces showed strong temporal dependence associated with the telemetry sampling windows used to construct the environmental-support models. For both species, high-plausibility regions were concentrated within months represented by telemetry-informed environmental conditions, while much of the remaining annual cycle showed weak or near-zero environmental support.

BBAL plausibility was concentrated primarily during November-January, reflecting the relatively short telemetry observation period available for this species. SAFS showed broader temporal support extending from approximately October through March, consistent with the longer and more environmentally diverse telemetry sampling period.

These results indicate that the plausibility framework captured the temporal structure of the telemetry-informed environmental domain rather than producing a generalized year-round habitat representation. Consequently, low-plausibility months should not be interpreted as species absence or unsuitable habitat. Instead, they identify periods whose environmental conditions were weakly represented within the available telemetry-derived feature space.

The monthly plausibility surfaces therefore provide an explicit diagnostic representation of where and when species-use and risk predictions remain strongly supported by observed environmental conditions versus where predictions extend into regions of greater temporal extrapolation uncertainty.

### Relationship Between Plausibility and Species-Use Predictions

Observed telemetry-derived species-use locations were generally concentrated within regions of moderate-to-high environmental plausibility, indicating that the generated feature space successfully captured much of the environmental domain associated with tracked individuals. However, some machine-learning species-use predictions extended into regions of lower environmental plausibility, particularly in environmentally uncommon or weakly sampled portions of the study area.

The plausibility surfaces therefore provided an additional diagnostic layer for interpreting species-use and risk predictions by distinguishing environmentally supported predictions from areas of potential extrapolation beyond the telemetry-informed environmental domain. This distinction was particularly important for the interpretation of latent-risk surfaces generated across the full 2014-2023 environmental record.

<!-- Suggested figure: Comparison between observed telemetry locations and environmental plausibility surfaces. -->

<!-- Suggested figure: Scatterplot of species-use prediction versus environmental plausibility. -->

## Bayesian/GMM Environmental Components

The Bayesian/Gaussian mixture model assigned each H3/date record to an environmental component based on the highest posterior component probability computed from the environmental feature vector. These component assignments summarize recurring combinations of environmental conditions within the telemetry-informed feature space. Components were used as diagnostic labels for interpreting environmental plausibility and risk surfaces, not as independently validated ecological habitat classes.

<!-- Figure fig:monthly-dominant-bayesian-gmm-components: monthly environmental component matrix for 2022. -->
\begin{figure}[htbp]
\centering
\includegraphics[height=0.76\textheight,keepaspectratio]{figures/monthly_dominant_bayesian_gmm_components_2022.png}
\caption{Monthly dominant Bayesian/Gaussian mixture environmental component assignments during 2022. Each H3 cell is colored by the component most frequently assigned within each month from the environmental feature vectors.}
\label{fig:monthly-dominant-bayesian-gmm-components}
\end{figure}

Table \ref{tab:gmm-component-summary} summarizes the environmental conditions represented by each Bayesian/Gaussian mixture component. Components differed strongly in shelf position, depth, distance from coast, wind exposure, and chlorophyll concentration, indicating that the mixture model separated broad environmental regimes rather than arbitrary spatial labels.

<!-- Table tab:gmm-component-summary: mean plus standard deviation for each Bayesian/GMM environmental component. -->
\begin{table}[htbp]
\centering
\scriptsize
\caption{Summary statistics for Bayesian/Gaussian mixture environmental components. Values are component means $\pm$ standard deviations from the fitted mixture model, transformed to report units where applicable.}

\label{tab:gmm-component-summary}
\resizebox{\textwidth}{!}{%
\begin{tabular}{rrrrrrrr}
\hline
Cmp & Weight & SST ($^\circ$C) & SSH (m) & Wind (m s$^{-1}$) & CHL (mg m$^{-3}$) & Depth (m) & Coast (km) \\
\hline
0 & 0.080 & 11.96 $\pm$ 0.75 & 0.449 $\pm$ 0.069 & 3.71 $\pm$ 1.72 & 2.51 $\pm$ 1.62 & 196 $\pm$ 158 & 271.0 $\pm$ 114.1 \\
1 & 0.123 & 9.31 $\pm$ 1.11 & 0.423 $\pm$ 0.076 & 7.91 $\pm$ 2.78 & 1.16 $\pm$ 0.60 & 306 $\pm$ 287 & 69.5 $\pm$ 60.2 \\
2 & 0.078 & 10.51 $\pm$ 1.20 & -0.189 $\pm$ 0.104 & 7.32 $\pm$ 2.62 & 0.57 $\pm$ 0.25 & 5555 $\pm$ 638 & 505.4 $\pm$ 43.9 \\
3 & 0.088 & 9.97 $\pm$ 1.02 & 0.407 $\pm$ 0.063 & 7.41 $\pm$ 2.82 & 0.90 $\pm$ 0.48 & 227 $\pm$ 221 & 64.1 $\pm$ 56.2 \\
4 & 0.194 & 10.34 $\pm$ 0.99 & 0.444 $\pm$ 0.040 & 6.05 $\pm$ 1.92 & 1.39 $\pm$ 0.78 & 139 $\pm$ 37 & 132.2 $\pm$ 84.2 \\
5 & 0.155 & 8.70 $\pm$ 0.80 & 0.424 $\pm$ 0.039 & 6.92 $\pm$ 1.92 & 1.45 $\pm$ 0.65 & 198 $\pm$ 108 & 74.0 $\pm$ 56.6 \\
6 & 0.095 & 8.80 $\pm$ 0.69 & 0.079 $\pm$ 0.129 & 7.83 $\pm$ 2.78 & 0.35 $\pm$ 0.16 & 1899 $\pm$ 588 & 264.8 $\pm$ 93.6 \\
7 & 0.082 & 8.11 $\pm$ 0.80 & 0.361 $\pm$ 0.063 & 5.93 $\pm$ 2.59 & 1.61 $\pm$ 1.34 & 195 $\pm$ 104 & 119.8 $\pm$ 83.9 \\
8 & 0.053 & 9.72 $\pm$ 1.22 & 0.280 $\pm$ 0.072 & 7.13 $\pm$ 2.38 & 1.00 $\pm$ 0.69 & 613 $\pm$ 288 & 301.4 $\pm$ 75.7 \\
9 & 0.053 & 9.36 $\pm$ 0.93 & -0.001 $\pm$ 0.173 & 6.97 $\pm$ 2.57 & 0.51 $\pm$ 0.24 & 2103 $\pm$ 1152 & 385.6 $\pm$ 76.2 \\
\hline
\end{tabular}
}
\end{table}

The component-summary table follows the reporting logic used in dynamic seascape studies, where environmental classes are summarized by their characteristic physical and biogeochemical conditions. However, the components in this study should not be interpreted as independently validated seascape classes. They represent Gaussian mixture components fitted to the telemetry-informed environmental feature space and were used as diagnostic environmental regimes for interpreting plausibility and risk surfaces.

Observed telemetry-derived species-use records occupied different subsets of the environmental component space (Tables \ref{tab:observed-bbal-components} and \ref{tab:observed-safs-components}). BBAL observations were concentrated primarily in components 4 and 5, whereas SAFS observations were distributed across a broader set of shelf, shelf-break, and offshore components.

<!-- Table tab:observed-bbal-components: observed BBAL positive-use records by environmental component. -->
\begin{table}[htbp]
\centering
\scriptsize
\caption{Observed black-browed albatross species-use records assigned to Bayesian/Gaussian mixture environmental components during telemetry observation years. Rows include positive residence-index records only.}
\label{tab:observed-bbal-components}
\begin{tabular}{rrrrr}
\hline
Cmp & Observed rows & Rows (\%) & Residence sum & Mean residence \\
\hline
0 & 784 & 17.4 & 2544 & 3.245 \\
1 & 19 & 0.4 & 120 & 6.316 \\
4 & 1927 & 42.9 & 8248 & 4.280 \\
5 & 1605 & 35.7 & 54216 & 33.779 \\
8 & 158 & 3.5 & 283 & 1.791 \\
\hline
\end{tabular}
\end{table}

<!-- Table tab:observed-safs-components: observed SAFS positive-use records by environmental component. -->
\begin{table}[htbp]
\centering
\scriptsize
\caption{Observed South American fur seal species-use records assigned to Bayesian/Gaussian mixture environmental components during telemetry observation years. Rows include positive residence-index records only.}
\label{tab:observed-safs-components}
\begin{tabular}{rrrrr}
\hline
Cmp & Observed rows & Rows (\%) & Residence sum & Mean residence \\
\hline
0 & 58 & 1.0 & 145 & 2.500 \\
1 & 1089 & 19.1 & 3240 & 2.975 \\
2 & 771 & 13.5 & 2491 & 3.231 \\
3 & 903 & 15.9 & 2640 & 2.924 \\
5 & 135 & 2.4 & 398 & 2.948 \\
6 & 976 & 17.1 & 2776 & 2.844 \\
7 & 800 & 14.1 & 31035 & 38.794 \\
8 & 427 & 7.5 & 1233 & 2.888 \\
9 & 533 & 9.4 & 1608 & 3.017 \\
\hline
\end{tabular}
\end{table}

<!-- Suggested figure: Monthly distribution of component assignments. -->

## Fishing Exposure Patterns

Fishing-effort observations were aggregated to the H3 grid to characterize the spatial and temporal distribution of fishing exposure across the Falkland Islands region. These layers were used directly in the risk-estimation workflow and provided spatial context for interpreting species-use and riskscape patterns.

<!-- Figure fig:fishing-activity-mean-2022: mean fishing activity map for 2022. -->
\begin{center}
\refstepcounter{figure}
\label{fig:fishing-activity-mean-2022}
\centering
\includegraphics[height=0.48\textheight,keepaspectratio]{figures/fishing_activity_mean_2022.png}

\small Figure~\thefigure. Mean fishing activity during 2022 summarized as vessel-hours by H3 cell.
\end{center}

Fishing activity during 2022 showed strong spatial concentration along the Falkland Islands shelf and shelf-break regions, with recurrent high-intensity activity west and north of the islands. Activity patterns varied seasonally, with several months showing expanded offshore effort and stronger concentration along major fishing corridors.

The fishing-exposure surfaces showed strong spatial structure associated with the Falkland Islands Conservation Zones (FICZ and FOCZ). Arc-shaped and circular fishing patterns visible around the islands corresponded closely to fisheries-management boundaries and associated operational fishing corridors. These structures were preserved after H3 aggregation, indicating that the spatial framework retained management-scale organization of fishing activity across the study region.

<!-- Figure fig:monthly-fishing-activity-2022: monthly non-zero median fishing activity matrix for 2022. -->
\begin{figure}[htbp]
\centering
\includegraphics[height=0.76\textheight,keepaspectratio]{figures/fishing_activity_non_zero_median_monthly_matrix_2022.png}
\caption{Monthly fishing activity during 2022 summarized as non-zero median vessel-hours by H3 cell. Each panel represents one month, showing the spatial distribution of fishing exposure among cells with observed fishing activity.}
\label{fig:monthly-fishing-activity-2022}
\end{figure}

The monthly fishing-exposure surfaces also revealed substantial temporal variability in the spatial footprint of fishing activity. Some regions exhibited persistent fishing effort throughout the year, while others showed episodic or seasonal occupation. These spatial and temporal differences were important for the resulting riskscapes because realized risk depended directly on the overlap between predicted species use and observed fishing exposure.

## Realized Risk Surfaces

Realized risk surfaces were generated by combining predicted species use with observed fishing exposure across the H3/day framework. These surfaces represent relative spatiotemporal overlap between telemetry-informed species-use predictions and recorded fishing activity rather than direct estimates of observed bycatch probability.

<!-- Suggested figure: Mean realized risk surfaces for BBAL and SAFS during 2022. -->

### Spatial Structure of Realized Risk

Realized risk showed strong spatial concentration along the Falkland Islands shelf and shelf-break regions, particularly within recurrent fishing corridors associated with the Falkland Islands Conservation Zones. High-risk regions generally emerged where elevated fishing exposure overlapped with environmentally supported species-use predictions.

The spatial structure of realized risk differed substantially between species. BBAL risk surfaces were more spatially constrained and concentrated west and northwest of the islands, reflecting the narrower environmental-support domain identified by the plausibility framework. SAFS risk surfaces were broader and more spatially diffuse across the continental shelf, consistent with the wider environmental-support patterns observed for this species.

<!-- Suggested figure: Comparison of BBAL and SAFS realized risk surfaces. -->

### Seasonal and Temporal Variability

Seasonal changes in fishing effort and environmental plausibility produced strong temporal variability in realized risk. Months with expanded shelf and offshore fishing activity generally showed broader realized-risk footprints, while periods of reduced fishing exposure produced more spatially restricted overlap patterns.

The strongest realized-risk conditions typically occurred where recurrent fishing corridors intersected environmentally supported shelf and shelf-break regions. However, substantial portions of the study area retained low realized risk despite moderate species-use predictions because observed fishing activity was absent or weak.

<!-- Suggested figure: Monthly realized risk matrix for 2022. -->

### Influence of Environmental Plausibility Filtering

Environmental plausibility filtering reduced realized-risk predictions in environmentally weakly supported regions while preserving high-risk structure within the telemetry-informed environmental domain. This effect was strongest in offshore and environmentally uncommon regions where machine-learning species-use predictions extended beyond the dominant environmental conditions represented in the telemetry data.

As a result, plausibility-filtered riskscapes provided a more conservative representation of realized overlap by distinguishing environmentally supported predictions from areas of greater extrapolation uncertainty.

<!-- Suggested figure: Comparison between ungated and plausibility-filtered realized risk surfaces. -->

## Latent Risk Surfaces

## Spatial and Temporal Patterns

\newpage
