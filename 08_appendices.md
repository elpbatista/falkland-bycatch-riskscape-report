# Appendices

These appendices provide supporting diagnostics, extended figures, and complete class summaries referenced from the Methods and Results.

## Environmental Monthly Matrices

\begin{figure}[htbp]
\centering
\begin{tabular}{cc}
\includegraphics[width=0.36\textwidth]{figures/sst_mean_monthly_matrix_2022.png} &
\includegraphics[width=0.36\textwidth]{figures/chl_log_mean_monthly_matrix_2022.png} \\
\includegraphics[width=0.36\textwidth]{figures/ssh_mean_monthly_matrix_2022.png} &
\includegraphics[width=0.36\textwidth]{figures/wind_speed_mean_monthly_matrix_2022.png}
\end{tabular}
\caption{Monthly mean base environmental fields during 2022. Panels show sea surface temperature (upper left), log-transformed chlorophyll-a concentration (upper right), sea surface height (lower left), and near-surface wind speed (lower right).}
\label{fig:app-base-environmental-monthly-matrices}
\end{figure}

\begin{figure}[htbp]
\centering
\begin{tabular}{cc}
\includegraphics[width=0.36\textwidth]{figures/sst_anom_mean_monthly_matrix_2022.png} &
\includegraphics[width=0.36\textwidth]{figures/chl_log_anom_mean_monthly_matrix_2022.png} \\
\includegraphics[width=0.36\textwidth]{figures/ssh_anom_mean_monthly_matrix_2022.png} &
\includegraphics[width=0.36\textwidth]{figures/wind_speed_anom_mean_monthly_matrix_2022.png}
\end{tabular}
\caption{Monthly mean environmental anomaly fields during 2022. Panels show sea surface temperature anomaly (upper left), log-transformed chlorophyll-a anomaly (upper right), sea surface height anomaly (lower left), and near-surface wind-speed anomaly (lower right).}
\label{fig:app-environmental-anomaly-monthly-matrices}
\end{figure}

\begin{figure}[htbp]
\centering
\begin{tabular}{cc}
\includegraphics[width=0.36\textwidth]{figures/sst_grad_mean_monthly_matrix_2022.png} &
\includegraphics[width=0.36\textwidth]{figures/chl_log_grad_mean_monthly_matrix_2022.png} \\
\includegraphics[width=0.36\textwidth]{figures/ssh_grad_mean_monthly_matrix_2022.png} &
\phantom{\includegraphics[width=0.36\textwidth]{figures/chl_log_grad_mean_monthly_matrix_2022.png}}
\end{tabular}
\caption{Monthly mean environmental-gradient fields during 2022. Panels show sea surface temperature gradient (upper left), log-transformed chlorophyll-a gradient (upper right), and sea surface height gradient (lower left).}
\label{fig:app-environmental-gradient-monthly-matrices}
\end{figure}

\newpage

## Environmental Predictor Correlations

Spearman rank correlations among dynamic environmental predictors across the full 2014-2023 environmental feature grid are shown below.

\begin{figure}[htbp]
\centering
\includegraphics[width=0.65\textwidth]{figures/environmental_predictors_spearman_correlation_all.png}
\caption{Spearman correlation matrix for environmental predictors across the 2014-2023 analysis period.}
\label{fig:environmental-correlation}
\end{figure}

The environmental predictor space showed moderate positive correlations among SST, SSH, and log-transformed chlorophyll-a ($\rho \approx 0.49$-$0.57$), indicating that warmer conditions were generally associated with elevated SSH and higher chlorophyll concentrations across the study region. Wind speed showed weak negative correlations with the other base environmental variables ($\rho \approx -0.10$ to $-0.15$).

Anomaly variables were generally weakly correlated with the corresponding base environmental fields, indicating that the anomaly representation captured departures from expected seasonal conditions rather than simply reproducing the original environmental gradients. The strongest anomaly relationship occurred between SST and SSH anomalies ($\rho = 0.47$), suggesting partial coupling between thermal and sea-surface-height variability.

Spatial-gradient predictors also showed largely distinct behavior relative to the base environmental variables. SST and SSH gradients had weak correlations with most base predictors, while SSH gradient exhibited moderate negative correlations with SSH ($\rho = -0.57$) and log-transformed chlorophyll-a ($\rho = -0.38$). Log-transformed chlorophyll-a and its gradient remained strongly correlated ($\rho = 0.79$), indicating that high-productivity regions were frequently associated with strong local chlorophyll contrasts.

\newpage

## Mean Fishing Exposure

\begin{figure}[htbp]
\centering
\includegraphics[height=0.33\textheight,keepaspectratio]{figures/fishing_activity_mean_2014-2023.png}
\caption{Mean fishing activity during 2014-2023 summarized as vessel-hours by H3 cell.}
\label{fig:appendix-fishing-activity-mean-2014-2023}
\end{figure}

## Seasonal Fishing Variability

\begin{figure}[htbp]
\centering
\includegraphics[width=0.76\textwidth]{figures/fishing_activity_monthly_totals_2014-2023.png}
\caption{Monthly fishing activity during 2014-2023 summarized as total fishing hours and unique vessel counts across the study area.}
\label{fig:appendix-fishing-activity-monthly-totals}
\end{figure}

## Additional Temporal Diagnostics

\begin{figure}[htbp]
\centering
\includegraphics[width=0.76\textwidth]{figures/fishing_activity_daily_totals_2022.png}
\caption{Daily fishing activity totals during 2022 summarized as total fishing hours and unique vessels across the study area.}
\label{fig:appendix-fishing-activity-daily-2022}
\end{figure}

\newpage

## Seascape Support Products

\begin{figure}[htbp]
\centering
\includegraphics[width=0.76\textwidth,keepaspectratio]{figures/monthly_dominant_mbon_seascapes_mbon_8day_area_weighted_2022.png}
\caption{Monthly dominant MBON seascape classes during 2022 after area-weighted assignment to the study H3 grid. This appendix figure supports the MBON coverage assessment discussed in the Results.}
\label{fig:appendix-mbon-dominant-seascapes-2022}
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

\begin{figure}[htbp]
\centering
\begin{tabular}{cc}
\includegraphics[width=0.48\textwidth]{figures/seascape_som_15x15_hierarchical_k30_joint_latent_risk_log_pred_non_zero_mean_BBAL_2022_monthly_matrix.png} &
\includegraphics[width=0.48\textwidth]{figures/seascape_som_15x15_hierarchical_k30_joint_latent_risk_log_pred_non_zero_mean_SAFS_2022_monthly_matrix.png} \\
\end{tabular}
\caption{Exploratory seascape-conditioned monthly latent-risk surrogate for BBAL (left) and SAFS (right) during 2022. Values were derived by summarizing final predicted species use by SOM-hierarchical k=30 class and projecting class-level values back to the H3/date grid before latent-risk calculation.}
\label{fig:appendix-seascape-risk-surrogate-2022}
\end{figure}

\newpage

## SOM-Hierarchical Seascape Class Profiles

\begin{figure}[htbp]
\centering
\includegraphics[width=0.76\textwidth,keepaspectratio]{figures/monthly_dominant_som_hierarchical_seascapes_som_15x15_hierarchical_k30_2022.png}
\caption{Monthly dominant SOM-hierarchical k=30 seascape classes during 2022. The figure shows the environmental-regime layer used to define grouped environmental validation folds and to support the exploratory seascape-risk surrogate.}
\label{fig:som-k30-dominant-seascapes-2022}
\end{figure}

\input{tables/som_k30_class_environment_profiles.tex}

\newpage
