# Appendices

## Mean Fishing Exposure

\begin{figure}[htbp]
\centering
\includegraphics[height=0.48\textheight,keepaspectratio]{figures/fishing_activity_mean_2014-2023.png}
\caption{Mean fishing activity during 2014-2023 summarized as vessel-hours by H3 cell.}
\label{fig:appendix-fishing-activity-mean-2014-2023}
\end{figure}

## Seasonal Fishing Variability

\begin{figure}[htbp]
\centering
\includegraphics[width=0.92\textwidth]{figures/fishing_activity_monthly_totals_2014-2023.png}
\caption{Monthly fishing activity during 2014-2023 summarized as total fishing hours and unique vessel counts across the study area.}
\label{fig:appendix-fishing-activity-monthly-totals}
\end{figure}

\begin{figure}[htbp]
\centering
\includegraphics[height=0.76\textheight,keepaspectratio]{figures/fishing_activity_non_zero_median_monthly_matrix_2022.png}
\caption{Monthly fishing activity during 2022 summarized as non-zero median vessel-hours by H3 cell. Each panel represents one month and shows the spatial distribution of fishing exposure among cells with observed fishing activity.}
\label{fig:appendix-monthly-fishing-activity-2022}
\end{figure}

## Additional Temporal Diagnostics

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

## Environmental Predictor Correlations

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
\caption{Distributions of environmental anomaly predictors across the full 2014-2023 feature set. The upper row shows SST and wind-speed anomalies, and the lower row shows CHL and SSH anomalies.}
\label{fig:environmental-anomaly-histograms}
\end{figure}

<!-- Figure fig:daily-anomaly-timeseries: daily mean SST and wind-speed anomaly time series. -->
\begin{figure}[htbp]
\centering
\includegraphics[width=0.85\textwidth]{figures/sst_anom_daily_mean_2014-2023.png}\\[0.5em]
\includegraphics[width=0.85\textwidth]{figures/wind_speed_anom_daily_mean_2014-2023.png}
\caption{Daily mean environmental anomalies across the study area for 2014-2023. The upper panel shows SST anomalies and the lower panel shows wind-speed anomalies.}
\label{fig:daily-anomaly-timeseries}
\end{figure}

\newpage

## Seascape Support Products

\begin{figure}[htbp]
\centering
\includegraphics[width=0.82\textwidth,keepaspectratio]{figures/monthly_dominant_mbon_seascapes_mbon_8day_area_weighted_2022.png}
\caption{Monthly dominant MBON seascape classes during 2022 after area-weighted assignment to the study H3 grid. This appendix figure supports the MBON coverage assessment discussed in the Results.}
\label{fig:appendix-mbon-dominant-seascapes-2022}
\end{figure}

\begin{figure}[htbp]
\centering
\begin{tabular}{cc}
\includegraphics[width=0.48\textwidth]{figures/seascape_som_15x15_hierarchical_k30_joint_latent_risk_log_pred_non_zero_mean_BBAL_2022_monthly_matrix.png} &
\includegraphics[width=0.48\textwidth]{figures/seascape_som_15x15_hierarchical_k30_joint_latent_risk_log_pred_non_zero_mean_SAFS_2022_monthly_matrix.png} \\
\end{tabular}
\caption{Exploratory seascape-conditioned monthly latent-risk surrogate for BBAL (left) and SAFS (right) during 2022. Values were derived by summarizing final predicted species use by SOM-hierarchical k=30 class and projecting class-level values back to the H3/date grid before latent-risk calculation.}
\label{fig:appendix-seascape-risk-surrogate-2022}
\end{figure}

\begin{figure}[htbp]
\centering
\begin{tabular}{cc}
\includegraphics[width=0.48\textwidth]{figures/monthly_non_zero_mean_plausibility_BBAL_2014-2023.png} &
\includegraphics[width=0.48\textwidth]{figures/monthly_non_zero_mean_plausibility_SAFS_2014-2023.png} \\
\end{tabular}
\caption{Monthly environmental plausibility distributions for BBAL (left) and SAFS (right) across 2014-2023, summarized as non-zero mean plausibility by H3 cell and calendar month. The panels show seasonal differences in the environmental-support layer rather than species presence probability.}
\label{fig:appendix-monthly-plausibility-2014-2023}
\end{figure}

\begin{figure}[htbp]
\centering
\begin{tabular}{cc}
\includegraphics[width=0.48\textwidth]{figures/hybrid_presence_gate_extra_trees_som_hierarchical_k30_5fold_blockcv_bayesian_gmm_k30_joint_latent_risk_log_pred_non_zero_mean_BBAL_2022_monthly_matrix.png} &
\includegraphics[width=0.48\textwidth]{figures/hybrid_presence_gate_extra_trees_som_hierarchical_k30_5fold_blockcv_bayesian_gmm_k30_joint_latent_risk_log_pred_non_zero_mean_SAFS_2022_monthly_matrix.png} \\
\end{tabular}
\caption{Monthly latent-risk matrices for BBAL (left) and SAFS (right) during 2022. Latent risk applies a standardized minimum fishing exposure, so the panels show potential interaction risk independent of observed fishing activity.}
\label{fig:appendix-latent-risk-monthly-2022}
\end{figure}

\newpage

\input{tables/som_k30_class_environment_profiles.tex}

\newpage

## Environmental Monthly Matrixes

<!-- Figure app:sst-monthly-matrix: monthly SST matrix for 2022. -->
\begin{figure}[htbp]
\centering
\includegraphics[width=0.76\textwidth,keepaspectratio]{figures/sst_mean_monthly_matrix_2022.png}
\caption{Monthly mean sea surface temperature (SST) during 2022.}
\label{fig:app-sst-monthly-matrix}
\end{figure}

<!-- Figure app:ssh-monthly-matrix: monthly SSH matrix for 2022. -->
\begin{figure}[htbp]
\centering
\includegraphics[width=0.76\textwidth,keepaspectratio]{figures/ssh_mean_monthly_matrix_2022.png}
\caption{Monthly mean sea surface height (SSH) during 2022.}
\label{fig:app-ssh-monthly-matrix}
\end{figure}

<!-- Figure app:wind-speed-monthly-matrix: monthly wind-speed matrix for 2022. -->
\begin{figure}[htbp]
\centering
\includegraphics[width=0.76\textwidth,keepaspectratio]{figures/wind_speed_mean_monthly_matrix_2022.png}
\caption{Monthly mean near-surface wind speed during 2022.}
\label{fig:app-wind-speed-monthly-matrix}
\end{figure}

<!-- Figure app:chl-monthly-matrix: monthly log-CHL matrix for 2022. -->
\begin{figure}[htbp]
\centering
\includegraphics[width=0.76\textwidth,keepaspectratio]{figures/chl_log_mean_monthly_matrix_2022.png}
\caption{Monthly mean log-transformed chlorophyll-a concentration during 2022.}
\label{fig:app-chl-monthly-matrix}
\end{figure}

<!-- Figure app:sst-anom-monthly-matrix: monthly SST anomaly matrix for 2022. -->
\begin{figure}[htbp]
\centering
\includegraphics[width=0.76\textwidth,keepaspectratio]{figures/sst_anom_mean_monthly_matrix_2022.png}
\caption{Monthly mean sea surface temperature anomaly during 2022.}
\label{fig:app-sst-anom-monthly-matrix}
\end{figure}

<!-- Figure app:ssh-anom-monthly-matrix: monthly SSH anomaly matrix for 2022. -->
\begin{figure}[htbp]
\centering
\includegraphics[width=0.76\textwidth,keepaspectratio]{figures/ssh_anom_mean_monthly_matrix_2022.png}
\caption{Monthly mean sea surface height anomaly during 2022.}
\label{fig:app-ssh-anom-monthly-matrix}
\end{figure}

<!-- Figure app:wind-speed-anom-monthly-matrix: monthly wind-speed anomaly matrix for 2022. -->
\begin{figure}[htbp]
\centering
\includegraphics[width=0.76\textwidth,keepaspectratio]{figures/wind_speed_anom_mean_monthly_matrix_2022.png}
\caption{Monthly mean near-surface wind-speed anomaly during 2022.}
\label{fig:app-wind-speed-anom-monthly-matrix}
\end{figure}

<!-- Figure app:chl-anom-monthly-matrix: monthly log-CHL anomaly matrix for 2022. -->
\begin{figure}[htbp]
\centering
\includegraphics[width=0.76\textwidth,keepaspectratio]{figures/chl_log_anom_mean_monthly_matrix_2022.png}
\caption{Monthly mean log-transformed chlorophyll-a anomaly during 2022.}
\label{fig:app-chl-anom-monthly-matrix}
\end{figure}

<!-- Figure app:sst-grad-monthly-matrix: monthly SST gradient matrix for 2022. -->
\begin{figure}[htbp]
\centering
\includegraphics[width=0.76\textwidth,keepaspectratio]{figures/sst_grad_mean_monthly_matrix_2022.png}
\caption{Monthly mean sea surface temperature gradient during 2022.}
\label{fig:app-sst-grad-monthly-matrix}
\end{figure}

<!-- Figure app:ssh-grad-monthly-matrix: monthly SSH gradient matrix for 2022. -->
\begin{figure}[htbp]
\centering
\includegraphics[width=0.76\textwidth,keepaspectratio]{figures/ssh_grad_mean_monthly_matrix_2022.png}
\caption{Monthly mean sea surface height gradient during 2022.}
\label{fig:app-ssh-grad-monthly-matrix}
\end{figure}

<!-- Figure app:chl-grad-monthly-matrix: monthly log-CHL gradient matrix for 2022. -->
\begin{figure}[htbp]
\centering
\includegraphics[width=0.76\textwidth,keepaspectratio]{figures/chl_log_grad_mean_monthly_matrix_2022.png}
\caption{Monthly mean log-transformed chlorophyll-a gradient during 2022.}
\label{fig:app-chl-grad-monthly-matrix}
\end{figure}

\newpage
