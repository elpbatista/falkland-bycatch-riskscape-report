# Appendices

The appendices are organized as a technical companion to the main report. They provide reproducibility details, supporting diagnostics, extended map products, and complete seascape class summaries that are referenced from the Methods, Results, and Discussion. The main text gives the interpretation; the appendices preserve the evidence and implementation context needed to inspect or reproduce the workflow.

## Appendix A. Reproducibility and Workflow Reference

The analytical workflow is released as a reusable code repository and the derived data products are archived separately. The separation is intentional: source code, configuration, notebooks, and documentation belong in Git, while large derived tables, model outputs, and complete plot archives belong in Zenodo.

**Software repository.** The workflow code is archived as:

Batista Echevarría, J. L. (2026). *Falkland Bycatch Riskscape Workflow* [Software]. Zenodo. https://doi.org/10.5281/zenodo.20348906

**Data bundle.** The derived data and plot bundle used by the public workflow is archived as:

Batista Echevarría, J. L. (2026). *Falkland Bycatch Riskscape Data Bundle* [Data set]. Zenodo. https://doi.org/10.5281/zenodo.20337229

The workflow uses a common H3 spatial framework and daily temporal resolution. The key table fields are `h3`, `date`, and, for species-expanded products, `species`. H3 refers to the spatial indexing system; `h3` refers to the stored table column. The public repository contains notebooks that document the study area, input datasets, feature engineering, model design, prediction products, operational outputs, and quality checks. These notebooks are presentation and inspection material rather than the canonical pipeline orchestrator.

The main derived product families are:

- `data/grids/`: H3 grid files and spatial index products.
- `data/processed/`: lookup tables, preprocessed features, and validation summaries.
- `data/features/`: yearly environmental, fishing, and species-use feature partitions.
- `data/modeling/`: model-ready tables, environmental regimes, prediction products, retained model artifacts, and validation metrics.
- `data/plot_exports/`: tabular products used by plotting and diagnostic scripts.
- `plots/`: static figures, weekly products, diagnostics, and animation-ready outputs.

\FloatBarrier
\clearpage

## Appendix B. Environmental Feature Diagnostics

This appendix section provides extended environmental feature products used to inspect the dynamic predictor space. The monthly matrices summarize how the base variables, anomaly variables, and neighbor-gradient fields vary through 2022. The correlation matrix summarizes broader relationships among dynamic predictors across the 2014-2023 feature grid.

### B.1 Monthly Environmental Fields

\begin{figure}[htbp]
\centering
\includegraphics[height=0.72\textheight,keepaspectratio]{figures/sst_mean_monthly_matrix_2022.png}
\caption{Monthly mean sea surface temperature during 2022.}
\label{fig:app-sst-mean-monthly-matrix-2022}
\end{figure}

\begin{figure}[htbp]
\centering
\includegraphics[height=0.72\textheight,keepaspectratio]{figures/chl_log_mean_monthly_matrix_2022.png}
\caption{Monthly mean log-transformed chlorophyll-a concentration during 2022.}
\label{fig:app-chl-log-mean-monthly-matrix-2022}
\end{figure}

\begin{figure}[htbp]
\centering
\includegraphics[height=0.72\textheight,keepaspectratio]{figures/ssh_mean_monthly_matrix_2022.png}
\caption{Monthly mean sea surface height during 2022.}
\label{fig:app-ssh-mean-monthly-matrix-2022}
\end{figure}

\begin{figure}[htbp]
\centering
\includegraphics[height=0.72\textheight,keepaspectratio]{figures/wind_speed_mean_monthly_matrix_2022.png}
\caption{Monthly mean near-surface wind speed during 2022.}
\label{fig:app-wind-speed-mean-monthly-matrix-2022}
\end{figure}

\begin{figure}[htbp]
\centering
\includegraphics[height=0.72\textheight,keepaspectratio]{figures/sst_anom_mean_monthly_matrix_2022.png}
\caption{Monthly mean sea surface temperature anomaly during 2022.}
\label{fig:app-sst-anom-mean-monthly-matrix-2022}
\end{figure}

\begin{figure}[htbp]
\centering
\includegraphics[height=0.72\textheight,keepaspectratio]{figures/chl_log_anom_mean_monthly_matrix_2022.png}
\caption{Monthly mean log-transformed chlorophyll-a anomaly during 2022.}
\label{fig:app-chl-log-anom-mean-monthly-matrix-2022}
\end{figure}

\begin{figure}[htbp]
\centering
\includegraphics[height=0.72\textheight,keepaspectratio]{figures/ssh_anom_mean_monthly_matrix_2022.png}
\caption{Monthly mean sea surface height anomaly during 2022.}
\label{fig:app-ssh-anom-mean-monthly-matrix-2022}
\end{figure}

\begin{figure}[htbp]
\centering
\includegraphics[height=0.72\textheight,keepaspectratio]{figures/wind_speed_anom_mean_monthly_matrix_2022.png}
\caption{Monthly mean near-surface wind-speed anomaly during 2022.}
\label{fig:app-wind-speed-anom-mean-monthly-matrix-2022}
\end{figure}

\begin{figure}[htbp]
\centering
\includegraphics[height=0.72\textheight,keepaspectratio]{figures/sst_grad_mean_monthly_matrix_2022.png}
\caption{Monthly mean sea surface temperature gradient during 2022.}
\label{fig:app-sst-grad-mean-monthly-matrix-2022}
\end{figure}

\begin{figure}[htbp]
\centering
\includegraphics[height=0.72\textheight,keepaspectratio]{figures/chl_log_grad_mean_monthly_matrix_2022.png}
\caption{Monthly mean log-transformed chlorophyll-a gradient during 2022.}
\label{fig:app-chl-log-grad-mean-monthly-matrix-2022}
\end{figure}

\begin{figure}[htbp]
\centering
\includegraphics[height=0.72\textheight,keepaspectratio]{figures/ssh_grad_mean_monthly_matrix_2022.png}
\caption{Monthly mean sea surface height gradient during 2022.}
\label{fig:app-ssh-grad-mean-monthly-matrix-2022}
\end{figure}

\newpage

### B.2 Environmental Predictor Correlations

Spearman rank correlations among dynamic environmental predictors across the full 2014-2023 environmental feature grid are shown in Figure \ref{fig:environmental-correlation}. This diagnostic checks whether the derived fields add distinct information or simply reproduce the base environmental variables.

\begin{figure}[htbp]
\centering
\includegraphics[width=0.65\textwidth]{figures/environmental_predictors_spearman_correlation_all.png}
\caption{Spearman correlation matrix for environmental predictors across the 2014-2023 analysis period.}
\label{fig:environmental-correlation}
\end{figure}

The environmental predictor space showed moderate positive correlations among SST, SSH, and log-transformed chlorophyll-a ($\rho \approx 0.49$-$0.57$), indicating that warmer conditions were generally associated with elevated SSH and higher chlorophyll concentrations across the study region. Wind speed showed weak negative correlations with the other base environmental variables ($\rho \approx -0.10$ to $-0.15$).

Anomaly variables were generally weakly correlated with the corresponding base environmental fields, indicating that the anomaly representation captured departures from expected seasonal conditions rather than simply reproducing the original environmental gradients. The strongest anomaly relationship occurred between SST and SSH anomalies ($\rho = 0.47$), suggesting partial coupling between thermal and sea-surface-height variability.

Spatial-gradient predictors also showed largely distinct behavior relative to the base environmental variables. SST and SSH gradients had weak correlations with most base predictors, while SSH gradient exhibited moderate negative correlations with SSH ($\rho = -0.57$) and log-transformed chlorophyll-a ($\rho = -0.38$). Log-transformed chlorophyll-a and its gradient remained strongly correlated ($\rho = 0.79$), indicating that high-productivity regions were frequently associated with strong local chlorophyll contrasts.

\FloatBarrier
\clearpage

## Appendix C. Fishing Exposure Diagnostics

The following figures support the description of fishing activity in the Results. They summarize mean spatial exposure, monthly seasonality, and daily variability. Fishing activity is treated as an apparent fishing-effort layer derived from AIS-based products and then aligned to the same H3/day framework used by the environmental and species-use products.

### C.1 Mean Fishing Exposure

\begin{figure}[htbp]
\centering
\includegraphics[height=0.63\textheight,keepaspectratio]{figures/fishing_activity_mean_2014-2023.png}
\caption{Mean fishing activity during 2014-2023 summarized as vessel-hours by H3 cell.}
\label{fig:appendix-fishing-activity-mean-2014-2023}
\end{figure}

\newpage

### C.2 Seasonal Fishing Variability

\begin{figure}[htbp]
\centering
\includegraphics[width=0.73\textwidth]{figures/fishing_activity_monthly_totals_2014-2023.png}
\caption{Monthly fishing activity during 2014-2023 summarized as total fishing hours and unique vessel counts across the study area.}
\label{fig:appendix-fishing-activity-monthly-totals}
\end{figure}

### C.3 Additional Temporal Diagnostics

\begin{figure}[htbp]
\centering
\includegraphics[width=0.73\textwidth]{figures/fishing_activity_daily_totals_2022.png}
\caption{Daily fishing activity totals during 2022 summarized as total fishing hours and unique vessels across the study area.}
\label{fig:appendix-fishing-activity-daily-2022}
\end{figure}

\FloatBarrier
\clearpage

## Appendix D. Seascape and Plausibility Support Products

This section preserves the supporting seascape and plausibility products that are useful for interpretation but too detailed for the main Results. The MBON panel documents why the external 8-day product was not retained as the final environmental-regime framework. The plausibility panels show seasonal environmental support for each species before risk products are interpreted.

### D.1 MBON Seascape Coverage Diagnostic

\begin{figure}[htbp]
\centering
\includegraphics[height=0.72\textheight,keepaspectratio]{figures/monthly_dominant_mbon_seascapes_mbon_8day_area_weighted_2022.png}
\caption{Monthly dominant MBON seascape classes during 2022 after area-weighted assignment to the study H3 grid. This appendix figure supports the MBON coverage assessment discussed in the Results.}
\label{fig:appendix-mbon-dominant-seascapes-2022}
\end{figure}

\newpage

### D.2 Monthly Environmental Plausibility

\begin{figure}[htbp]
\centering
\begin{tabular}{cc}
\includegraphics[width=0.45\textwidth]{figures/monthly_non_zero_mean_plausibility_BBAL_2014-2023.png} &
\includegraphics[width=0.45\textwidth]{figures/monthly_non_zero_mean_plausibility_SAFS_2014-2023.png} \\
\end{tabular}
\caption{Monthly environmental plausibility distributions for BBAL (left) and SAFS (right) across 2014-2023, summarized as non-zero mean plausibility by H3 cell and calendar month. The panels show seasonal differences in the environmental-support layer rather than species presence probability.}
\label{fig:appendix-monthly-plausibility-2014-2023}
\end{figure}

\FloatBarrier
\clearpage

## Appendix E. Extended Risk-Product Matrices

The matrices below support the risk-product interpretation in the Results and Discussion. The first pair shows the primary monthly latent-risk products, which combine predicted species use with a standardized minimum fishing exposure. The second pair shows the exploratory seascape-conditioned surrogate, which summarizes predictions through broad environmental regimes and therefore smooths local H3-scale hotspot structure.

### E.1 Primary Monthly Latent Risk

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

### E.2 Seascape-Conditioned Latent-Risk Surrogate

\begin{figure}[htbp]
\centering
\begin{tabular}{cc}
\includegraphics[width=0.48\textwidth]{figures/seascape_som_15x15_hierarchical_k30_joint_latent_risk_log_pred_non_zero_mean_BBAL_2022_monthly_matrix.png} &
\includegraphics[width=0.48\textwidth]{figures/seascape_som_15x15_hierarchical_k30_joint_latent_risk_log_pred_non_zero_mean_SAFS_2022_monthly_matrix.png} \\
\end{tabular}
\caption{Exploratory seascape-conditioned monthly latent-risk surrogate for BBAL (left) and SAFS (right) during 2022. Values were derived by summarizing final predicted species use by SOM-hierarchical k=30 class and projecting class-level values back to the H3/day grid before latent-risk calculation.}
\label{fig:appendix-seascape-risk-surrogate-2022}
\end{figure}

\FloatBarrier
\clearpage

## Appendix F. Complete SOM-Hierarchical Seascape Class Profiles

Table \ref{tab:appendix-som-k30-class-profile} gives the complete environmental profile for the selected SOM-hierarchical k=30 seascape classes. The table is retained in full because the main text summarizes only the most important telemetry-supported patterns. Environmental values are means with standard deviations across 2014-2023 H3 cell-days.

\input{tables/som_k30_class_environment_profiles.tex}

\newpage
