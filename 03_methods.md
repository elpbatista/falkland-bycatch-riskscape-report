# Methods

This chapter describes the analytical workflow used to construct dynamic bycatch riskscapes for the Falkland Islands region. The workflow integrates environmental raster products, fishing effort observations, species telemetry records, and static spatial reference layers within a common H3-based spatial framework and daily temporal resolution. The implementation is modular and largely script-driven, with automated stages for data acquisition, feature construction, model training, prediction generation, and batch production of map products. These harmonized datasets were used to train species-use models, estimate environmental plausibility, evaluate environmental validation designs, and translate daily prediction outputs into relative risk surfaces and operator-facing summaries.

The methods are organized around the study area and spatial framework, input datasets, data processing, environmental seascape classification, species-use modeling, structured validation, risk estimation, and operator-facing risk-product generation.

## Study Area and Spatial Framework

The study focused on the Falkland Islands fisheries region, where fishing activity is managed through licence areas and conservation zones described by the Falkland Islands Government Fisheries Department [@FIG-FD_statistics_2024]. The spatial domain was defined from the Falkland Islands fisheries grid and configured to span approximately 47°S-57°S and 64°W-51°W, with an additional 50 km buffer to reduce edge effects and support spatial alignment across environmental, fisheries, and biological datasets (Figure \ref{fig:study-area}).

\begin{figure}[htbp]
\centering
\includegraphics[width=0.86\textwidth]{figures/study_area.png}
\caption{Study area for the Falkland Islands riskscape workflow, showing bathymetry, the fisheries-grid extent, the Falkland Islands Inner and Outer Conservation Zones (FICZ and FOCZ), and the H3 spatial grid used for daily data integration.}
\label{fig:study-area}
\end{figure}

All datasets were aligned to a common H3 grid and daily temporal resolution. Spatial integration was performed using the H3 hierarchical hexagonal indexing system developed by Uber [@HomeH3], a discrete global grid framework [@sahrGeodesicDiscreteGlobal2003]. The study area was discretized using H3 resolution 6 cells, providing an average cell area of approximately 36 km² and producing 37,209 cells across the fisheries-grid extent and buffer. Each record in the modeling framework represents one H3 cell on one date. Environmental variables describe the oceanographic state of each cell-day, species tracking data provide evidence of animal use, and fishing effort data represent operational exposure.

\begin{tcolorbox}[title={Box 2. Why use H3 hexagons?},colback=gray!5,colframe=gray!45,arc=1mm,boxrule=0.4pt,left=1.5mm,right=1.5mm,top=1mm,bottom=1mm,width=0.85\textwidth,center]
The study region was divided into 37,209 H3 cells, each roughly 36 km². H3 is a formal global grid system used in modern spatial computing: each cell has a stable identifier, and the grid can be summarized at finer or broader resolutions. This makes it useful for reproducible workflows that combine many data types. The hexagonal layout also helps compare neighboring cells more evenly than a latitude-longitude grid. In this project, H3 provided the common spatial framework for environmental data, animal tracking records, fishing activity, and risk scores.
\end{tcolorbox}

The environmental and fisheries datasets cover 2014-2023. Species-use observations were derived from telemetry records collected from tracked individuals during 2022-2023. Because telemetry observations represent limited sampling periods and only tracked individuals, the resulting riskscapes should be interpreted as relative indicators of species use and potential interaction risk rather than definitive representations of population-level species distributions or observed bycatch probability.

## Input Data

The analytical framework integrates environmental, fisheries, biological, and spatial reference datasets describing oceanographic conditions, fishing activity, species presence, and management boundaries across the Falkland Islands region. Aggregated datasets were harmonized within a common spatiotemporal framework, enabling the integration of heterogeneous data sources into a unified modeling framework.

### Environmental Data

Environmental variables included sea surface temperature (SST), chlorophyll-a concentration (CHL), sea surface height (SSH), near-surface wind components, and bathymetry. Daily SST fields were obtained from the NASA Multi-scale Ultra-high Resolution (MUR) Level 4 product [@nasa/jplGHRSSTLevel42015], chlorophyll-a and SSH products were obtained from the Copernicus Marine Service [@europeanunion-copernicusmarineserviceGlobalOceanColour2022; @europeanunion-copernicusmarineserviceGLOBALOCEANGRIDDED2021], wind components were derived from ERA5 daily statistics distributed through the Copernicus Climate Data Store [@c3sERA5PostprocessedDaily2024], and bathymetric elevation data were obtained from GEBCO [@gebcobathymetriccompilationgroup2026GEBCO_2026GridContinuous2026].

Environmental raster datasets were spatially aligned to the H3 framework and temporally harmonized at daily resolution. Daily H3 environmental feature tables were generated where valid source data were available.

### Fisheries Data

Fishing effort data were derived from Global Fishing Watch (GFW) AIS-based [@globalfishingwatchGlobalAISbasedApparent2025] fishing activity products covering the period 2014-2023. The raw GFW extract contains 2,297,069 records representing 3,094,974.5 fishing hours from 2,011 unique vessels before spatial aggregation to the study grid.

The dominant fishing gear types were trawlers with 1,497,210 fishing hours from 567 vessels, squid jiggers with 1,256,653 fishing hours from 1,207 vessels, and set longlines that accounted for 202,871 fishing hours from 41 vessels. The dataset includes vessels operating under multiple flag states, with the largest fishing effort contributions associated with Argentina (ARG), China (CHN), Taiwan (TWN), South Korea (KOR), Spain (ESP), and the Falkland Islands (FLK).

Fishing effort observations were aggregated by H3 cell and date, producing daily spatial fishing effort features including total fishing hours and vessel counts for each `h3`/`date` combination. The processed H3 fishing-exposure table retains the subset of raw fishing hours that intersected the study H3 grid. Fishing activity was used as an exposure layer for realized-risk products and operational summaries; it was not used as a predictor in the species-use model or in the grouped environmental cross-validation design.

### Biological Data

Species presence data were derived from field telemetry records provided by the South Atlantic Environmental Research Institute (SAERI). The raw observations are GPS tracking points, and therefore represent observed species presence at specific locations and times. The dataset contains 59,182 records with valid geographic coordinates and timestamps collected during 2022-2023.

The dataset includes observations of Black-browed albatrosses (*Thalassarche melanophris*; BBAL) and South American fur seals (*Arctocephalus australis*; SAFS). BBAL accounts for 33,425 records from 27 individuals and 58 trips covering 16 days between 2022-12-02 and 2022-12-17. SAFS accounts for 25,757 records from 15 individuals and 18 trips collected between 2022-10-22 and 2023-03-16, covering 146 observation days (71 in 2022 and 75 in 2023).

Telemetry observations were aggregated by H3 cell, date, and species, resulting in 10,268 `h3`/`date`/`species` rows. After aggregation, these rows were treated as a species-use index for each occupied cell-day rather than as individual point-presence records.

### Reference Data

The Falkland Islands fisheries grid system [@fisheries_grid_squares] covers the waters around the Falkland Islands with grid cells measuring 0.25° latitude by 0.50° longitude and associated with fisheries licensing zones. The configured study extent spans approximately 47°S-57°S and 64°W-51°W, and an additional 50 km buffer was applied to reduce edge effects and ensure complete spatial coverage across datasets with different spatial resolutions.

Falkland Islands Conservation Zones [@ukho_ficz_focz_limits] defined for fisheries activities were also incorporated. Two zones were identified and classified as inner (FICZ) and outer (FOCZ) conservation zones.

Natural Earth coastline and land 1:10m physical vectors datasets [@ne_10m_coastline; @ne_10m_land] were used for cartographic reference, masking, and distance-based spatial analyses.

### External Seascape Product Evaluated

NOAA/MBON 8-day global seascape assignments [@NOAA/MBON] were evaluated as an external environmental-regime product but were not retained in the final modeling workflow. In the Falkland Islands study region, class 0 represented unassigned conditions and non-zero class coverage was insufficient during key seasonal windows. For the 2022 diagnostic, non-zero MBON classes covered 54.0% of H3 cell-days overall, but coverage dropped sharply from April through August: 13.3% in April, 2.3% in May, 0.0% in June, 1.2% in July, and 44.3% in August. The final environmental seascape and validation-block framework was therefore derived from the H3 environmental feature matrix rather than from the external MBON product. The 2022 monthly MBON assignment diagnostic is retained in the appendices as visual support for this coverage assessment (Figure \ref{fig:appendix-mbon-dominant-seascapes-2022}).

## Data Processing

Data processing followed a staged pipeline that transformed raw environmental rasters, fishing observations, species telemetry records, and static spatial layers into H3-indexed feature tables. The pipeline used `h3` and `date` as common keys, with H3 indexes stored as unsigned 64-bit integers and dates converted to UTC daily timestamps. Intermediate and final tables were written as yearly Parquet partitions using ZSTD compression. The main processed outputs were the daily environmental feature grid, species-use support table, fishing-exposure table, static spatial covariates, and environmental seascape assignments used for validation and interpretation.

Environmental processing began with raster-to-H3 lookup tables computed separately for each raster product and reused during feature generation. Environmental raster variables were aggregated to the H3 grid using area-weighted means. Raster pixels were converted to polygon footprints and intersected with H3 cell polygons. The geodesic area of each pixel-H3 overlap was used to compute normalized weights within each H3 cell. Daily raster values were aggregated by multiplying intersecting pixel values by their overlap weights and summing across pixels while excluding missing or invalid raster values. Aggregated environmental tables were grouped by `h3` and `date`, duplicate contributions were averaged, and variables were stored as 32-bit floating point values.

Several derived environmental variables were generated after spatial aggregation. Near-surface wind speed was calculated from zonal and meridional wind components as:

$$
W(h,t)
=
\sqrt{u_{10}(h,t)^{2}+v_{10}(h,t)^{2}}
$$

Chlorophyll-a concentration was log-transformed to reduce skew:

$$
C_{\log}(h,t)
=
\log\left(1+C(h,t)\right)
$$

Seasonality was represented with cyclic day-of-year predictors on a 365-day cycle. For non-leap years, the calendar day of year was used directly. For leap years, dates after February 28 were shifted back by one day so that February 29 was removed from the seasonal cycle. The adjusted day of year, $d^*$, was therefore defined as:

$$
d^* =
\begin{cases}
d - 1, & \text{if year is leap and } d > 59 \\
d, & \text{otherwise}
\end{cases}
$$

where $d$ is the calendar day of year and $d^*$ is the adjusted day of year. The seasonal predictors stored as `doy_sin` and `doy_cos` were then encoded as:

$$
S_d=\sin\left(\frac{2\pi d^*}{365}\right)
$$

$$
C_d=\cos\left(\frac{2\pi d^*}{365}\right)
$$

Spatial gradients were computed on the H3 grid to represent local environmental contrast. To improve processing efficiency, H3 ring-1 neighbor relationships were precomputed as lookup and index tables and reused during feature generation. For each date, each H3 cell was compared with its valid neighboring cells. Gradients were calculated as the root mean square difference between the focal cell and its neighbors:

$$
G_X(i,t)=\sqrt{\mathrm{mean}_{j \in N(i)}\left((X(i,t)-X(j,t))^2\right)}
$$

where $G_X(i,t)$ is the local gradient for variable $X$, $i$ is the focal H3 cell, $t$ is date, and $N(i)$ is the set of valid neighboring cells. Gradients were calculated for SST, log-transformed chlorophyll-a, and SSH.

Temporal anomalies were computed relative to local seasonal conditions. For each H3 cell and adjusted day-of-year, a climatological mean was calculated across the full environmental record. Daily anomalies were then calculated as:

$$
A_X(i,t)=X(i,t)-\bar{X}_i(d^*)
$$

Anomalies were calculated for SST, log-transformed chlorophyll-a, SSH, and wind speed.

Fishing effort records were converted to geographic points, spatially joined to the H3 grid, and aggregated by H3 cell and date. Daily fishing-exposure features included total fishing hours and unique vessel counts. Fishing activity was calculated as:

$$
F(h,t)
=
H(h,t)
\times
V(h,t)
$$

where $F(h,t)$ is fishing activity, $H(h,t)$ is total fishing hours, $V(h,t)$ is unique vessel count, $h$ is an H3 cell, and $t$ is date. `h3`/`date` combinations without fishing observations were retained and assigned zero fishing effort values.

Telemetry records were cleaned by parsing timestamps, removing invalid dates, and retaining records with valid coordinates. Observations were spatially joined to the H3 grid and aggregated by H3 cell, date, and species. Species-use support variables included telemetry record count, individual count, and trip count. For model training, observed `species`/`date` combinations were expanded across all H3 cells available in the environmental feature grid. Cells without telemetry observations for a given `species`/`date` combination were retained and assigned zero support values. These zero-use rows represent unobserved H3 cells within the modeled species-date domain, not confirmed biological absences.

Static spatial features were generated once per H3 cell. Bathymetric depth and slope were derived from the GEBCO raster using the same area-weighted H3 aggregation procedure. Distance to coast was calculated geodesically from each H3 centroid to the nearest coastline geometry. H3 centroid latitude and longitude were encoded using sine and cosine transformations to avoid discontinuities at coordinate boundaries.

The standardized environmental and static feature matrix was also used to construct feature-only seascape assignments. A 15 x 15 self-organizing map was fitted to the H3 environmental feature space, producing 225 environmental prototypes. These prototypes were then grouped with hierarchical agglomerative clustering, and the selected 30-class cut was exported to yearly `h3`/`date` seascape assignment tables. Species identity, telemetry-derived species-use values, fishing exposure, environmental plausibility, and risk predictions were excluded from seascape fitting, so the seascape labels represent recurring environmental states rather than species-specific use or risk.

Final modeling tables were assembled by joining dynamic environmental variables, derived features, static spatial variables, species-use support variables, and fishing-exposure fields on common `h3`/`date` keys. Dynamic predictors included `sst`, `ssh`, `wind_speed`, log-transformed `chl`, environmental anomalies, H3-neighbor gradients, and seasonal sine/cosine terms. Static predictors included bathymetry, slope, distance to coast, and encoded H3 centroid coordinates. Fishing-exposure fields were retained for risk-product generation but were excluded from the species-use predictor matrix and from the grouped environmental cross-validation design. No temporal interpolation or rolling-window smoothing was applied; all features were derived directly from daily source observations and deterministic `h3`/`date` aggregation.

## Environmental Seascape Classification

A feature-only seascape classification was implemented to define recurring environmental regimes across the Falkland Islands study region. The approach follows the general logic of hierarchical dynamic seascape frameworks, in which multivariate oceanographic conditions are represented in environmental space and then grouped into interpretable classes [@kavanaughHierarchicalDynamicSeascapes2014; @kavanaughSeascapesNewVernacular2016; @montesDynamicSatelliteSeascapes2020a]. This classification used the environmental and static predictor matrix only. Species identity, telemetry-derived response variables, environmental plausibility, fishing exposure, and species-use predictions were excluded from seascape fitting so that the resulting classes represented recurring environmental states rather than species-specific use or risk.

\begin{tcolorbox}[title={Box 3. How seascapes are used here},colback=gray!5,colframe=gray!45,arc=1mm,boxrule=0.4pt,left=1.5mm,right=1.5mm,top=1mm,bottom=1mm,width=0.85\textwidth,center]
Seascapes group similar ocean conditions into recurring environmental regimes. In this report, they provide a shared environmental vocabulary for testing and interpreting the riskscape workflow. First, they define grouped validation folds, so the model is tested across different ocean regimes rather than only across randomly mixed records. Second, they provide an exploratory way to summarize how predicted species use and risk vary among broad environmental conditions. The results show that seascapes are useful for interpretation and communication, while the full H3 prediction surfaces are needed to retain finer local hotspots.
\end{tcolorbox}

Predictors were standardized before classification. A 15 x 15 self-organizing map [@kohonenSelforganizingMap1990a] was fitted to the standardized H3 environmental feature space, producing 225 environmental prototypes. The prototype weight vectors were then grouped using Ward hierarchical agglomerative clustering. The selected 30-class cut was exported as a species-independent seascape label for each assigned `h3`/`date` record in the 2014-2023 feature grid. The rationale for selecting the 30-class cut and using it in grouped environmental cross-validation is described in the validation section.

Seascape classes were summarized by their environmental and static predictor distributions, including SST, SSH, wind speed, log-transformed chlorophyll-a, bathymetry, and distance to coast. The seascape labels were also joined back to observed positive species-use records to describe which environmental regimes were represented in the telemetry observations for each species.

In addition to their role in grouped environmental cross-validation, the SOM-hierarchical seascape classes were used in an exploratory proof-of-concept analysis of how environmental regimes could support future risk-estimation products. This analysis was not intended to assess or replace existing dynamic seascape products. Rather, it explored whether relationships between feature-only environmental regimes and telemetry-informed species use could support seascape-conditioned risk summaries. For each species, predicted log-transformed residence index from the hybrid model was summarized by seascape class and projected back onto the `h3`/`date` grid as a seascape-conditioned species-use surface. These surfaces were interpreted as diagnostic products for examining how species use varies across environmental regimes, not as the primary risk input.

## Species-Use Modeling

Predictor variables represented environmental state, environmental variability, seasonality, and static spatial structure. Dynamic predictors described oceanographic conditions for each `h3`/`date` combination, while derived variables captured local gradients, seasonal anomalies, and cyclic temporal patterns. Static predictors represented persistent geographic structure, including bathymetry, slope, coastal proximity, and spatial position.

Species-use models were trained to predict relative species use from environmental and static spatial predictors. The training dataset was constructed from the `h3`/`date`/`species` species-use support table joined to the environmental feature grid. The response variable was `residence_index`, defined as the product of telemetry record count and individual count for each observed `h3`/`date`/`species` group:

$$
R(h,t,s)
=
C(h,t,s)
\times
I(h,t,s)
$$

where $R(h,t,s)$ is the response value, $C(h,t,s)$ is the telemetry record count, $I(h,t,s)$ is the number of tracked individuals, $h$ is an H3 cell, $t$ is date, and $s$ is species. This formulation was used to increase the relative influence of locations with both repeated observations and multiple tracked individuals. The modeling target was transformed as:

$$
Y(h,t,s)=\log\left(1+R(h,t,s)\right)
$$

For each observed `species`/`date` combination, all H3 cells in the environmental feature grid were included. Cells without telemetry observations were retained and assigned zero target values, allowing the model to learn from both observed-use and unobserved-use cells within the same environmental domain. These zero values were treated as modeling support for relative species use, not as confirmed biological absences.

Predictor variables included dynamic environmental conditions, derived environmental features, seasonal terms, and static spatial variables. Dynamic predictors included `sst`, `ssh`, `wind_speed`, and log-transformed `chl`. Derived predictors included environmental anomalies and H3-neighbor gradients. Seasonal predictors were represented using cyclic day-of-year sine and cosine terms. Static predictors included bathymetric depth, bathymetric slope, distance to coast, and encoded H3 centroid coordinates.

The implemented workflow used a joint-species modeling approach. Species identity was represented using one-hot encoded categorical variables appended to the numerical predictor matrix. This allowed a single model to learn shared environmental structure while preserving species-specific responses.

Because zero-use rows greatly outnumbered positive-use rows, the training dataset was balanced before model fitting. All positive rows were retained, and an equal number of zero-use rows was randomly sampled. Sample weights were applied during fitting to increase the influence of higher-use observations:

$$
w(h,t,s)
=
1+R(h,t,s)^{0.75}
$$

Four model classes were evaluated during learner screening using scikit-learn implementations [@pedregosaScikitlearnMachineLearning]: histogram gradient boosting [@friedmanGreedyFunctionApproximation2001], random forest [@breimanRandomForests2001], extra trees [@geurtsExtremelyRandomizedTrees2006], and a custom Bayesian/GMM-style candidate. The random forest and extra trees models used 300 trees, a maximum depth of 20, and a minimum leaf size of 5. The histogram gradient boosting model used 300 boosting iterations, a learning rate of 0.05, 31 maximum leaf nodes, and L2 regularization. The custom Bayesian/GMM-style candidate fitted a 30-component Gaussian mixture model to positive-use observations in standardized feature space, using expectation-maximization logic [@dempsterMaximumLikelihoodIncomplete1977], and combined the resulting likelihood-based prediction with a histogram gradient boosting prior. This Bayesian/GMM-style model was not selected as the primary species-use learner, but its environmental-density component was retained as the plausibility layer described below.

The final production species-use learner was selected through the validation workflow described below and then refit on all balanced training rows. Model outputs were expressed as `species_use_log_pred`, representing predicted species use on the log-transformed scale.

## Validation

Validation included data-quality checks, learner screening, structured transferability tests, and environmental-support assessment. During preprocessing, feature tables were checked for required columns, consistent `h3` and `date` keys, duplicate records, missing values, and expected data types. Environmental features were inspected after aggregation and transformation to confirm that yearly partitions retained the expected `h3`/`date` structure and that derived variables, including gradients and anomalies, were generated without row inflation.

The first validation stage screened candidate species-use learners using a row-level random split, with 25% of balanced training rows withheld for testing. This random split was used only as an initial learner-comparison benchmark because randomly mixed training and test rows can overstate transferability when observations are spatially or environmentally structured [@valaviBlockCVPackage2019]. Predictions were evaluated after back-transforming from log space to the original `residence_index` scale, and model comparison metrics included coefficient of determination ($R^2$), root mean squared error (RMSE), and mean absolute error (MAE).

After learner screening, Extra Trees was evaluated under more structured validation designs. These included a row-random 12% holdout benchmark, spatial H3 parent-block holdouts, buffered spatial holdouts, custom Bayesian/GMM-style environmental-component holdouts, and SOM-hierarchical seascape grouped folds. Following the logic of spatially and environmentally separated cross-validation folds [@valaviBlockCVPackage2019], these structured validation designs were intended to test transfer across geographic or environmental partitions rather than interpolation among randomly mixed cell-days.

The selected validation design used SOM-hierarchical k=30 seascape classes as environmental groups in five-fold grouped cross-validation. Complete seascape groups were assigned to folds; groups were allocated to balance total rows and positive species-use support across species as much as possible. Each fold withheld one set of environmental seascape groups for testing and trained the model on the remaining groups. This produced an environmental transferability diagnostic for the joint Extra Trees species-use model and supported the final choice of the SOM-hierarchical k=30 grouped environmental cross-validation design, with the quantitative comparison among validation variants reported in the results.

The final production species-use model was refit after validation using the selected Extra Trees learner and all balanced training rows. Production-fit diagnostics were retained for reproducibility and model inspection but were not treated as independent validation because the production model was fit to the full balanced training dataset.

Environmental plausibility was evaluated separately from direct species-use prediction. The Bayesian/Gaussian mixture model was used to identify `h3`/`date`/`species` combinations whose environmental conditions were similar to those associated with observed telemetry locations. Plausibility values were therefore interpreted as environmental-support diagnostics rather than as direct validation of species presence or absence. Risk surfaces were interpreted alongside plausibility surfaces to distinguish well-supported predictions from environmental extrapolation.

Additional validation would be required to assess realized bycatch prediction directly. In particular, independent observer bycatch records, individual- or trip-level holdouts, and sensitivity analysis of the plausibility-gate parameter would strengthen future versions of the workflow.

## Risk Estimation

Risk estimation was implemented as a relative spatiotemporal overlap index, not as a direct prediction of observed bycatch probability. This follows the broader use of telemetry-informed habitat or distribution models combined with fisheries activity to assess potential bycatch risk through spatial overlap [@zydelisDynamicHabitatModels2011; @clayComprehensiveLargescaleAssessment2019]. The workflow combined predicted species use, environmental plausibility, and fishing exposure for each `h3` cell, `date`, and `species`.

The conceptual framework separates three components. First, species-use modeling estimates where each species is likely to occur or concentrate as a function of environmental conditions. Second, fishing exposure represents the intensity of fishing activity in each H3 cell and date. Third, the risk surface combines predicted species use and fishing exposure to compute a relative index of potential interaction risk for each H3 cell and date. This framing is consistent with dynamic-management approaches that translate changing biological and fisheries information into spatial decision-support products [@maxwellDynamicOceanManagement2015; @hazenDynamicOceanManagement2018].

\begin{tcolorbox}[title={Box 4. Realized risk and latent risk},colback=gray!5,colframe=gray!45,arc=1mm,boxrule=0.4pt,left=1.5mm,right=1.5mm,top=1mm,bottom=1mm,width=0.85\textwidth,center]
Two main forms of risk are reported. Realized risk is where predicted animal use overlaps vessel-derived apparent fishing activity: the modeled hotspots under recorded fishing patterns. Latent risk is where predicted use is high under a standardized minimum fishing exposure, even if no boats happened to be fishing there during the summarized period: places where risk could emerge if effort is redistributed. The distinction matters because reducing a current hotspot is most useful when effort does not simply move into a latent hotspot. Showing both maps lets managers see that redistribution possibility before, rather than after, a decision.
\end{tcolorbox}

Conceptually, relative risk increases when species use and fishing exposure overlap:

$$
Q(h,t,s)=U(h,t,s)\times E(h,t)
$$

where $h$ is an H3 cell, $t$ is date, $s$ is species, $Q(h,t,s)$ is relative risk, $U(h,t,s)$ is predicted species use, and $E(h,t)$ is fishing exposure. In the implemented workflow, both terms were represented on transformed scales, so stored risk values should be interpreted as relative risk scores rather than raw products or absolute bycatch probabilities. High-risk cells therefore represent locations and dates where predicted species use and fishing activity are both high. The framework assumes that bycatch risk increases with spatiotemporal overlap between species use and fishing activity, and that environmental conditions help explain variation in species use.

### Environmental plausibility

Environmental plausibility was estimated with the Bayesian/Gaussian mixture model. For each `h3`/`date`/`species` combination, the model calculated the log density of the environmental feature vector under the fitted Gaussian mixture model. The model was fitted on positive-use observations, and its 1st and 99th percentile training log-density values were stored as normalization limits. For a predicted cell-day, the normalized plausibility score was:

$$
p_s(h,t)
=
\mathrm{clip}
\left(
\frac{d_s(h,t)-d_{s,\min}}{d_{s,\max}-d_{s,\min}},
0,
1
\right)
$$

where $d_s(h,t)$ is the Gaussian mixture log density, and $d_{s,\min}$ and $d_{s,\max}$ are the lower and upper normalization limits estimated during model fitting. Plausibility values near 1 indicate environmental conditions similar to those associated with observed species use; values near 0 indicate weak environmental support relative to the fitted use-space distribution.

\begin{tcolorbox}[title={Box 5. What environmental plausibility means},colback=gray!5,colframe=gray!45,arc=1mm,boxrule=0.4pt,left=1.5mm,right=1.5mm,top=1mm,bottom=1mm,width=0.85\textwidth,center]
The species-use model can predict across the full study region, including areas that are environmentally unlike those where tracked animals were observed. Those predictions are less certain. To flag this, a second model, a Gaussian mixture fitted to observed-use environmental conditions, scores each cell-day by how similar it is to the environmental space represented in the tracking data. Predictions in low-plausibility cells are slightly damped but not set to zero because low plausibility means "we are extrapolating," not "no animals are here."
\end{tcolorbox}

The plausibility gate was then defined as:

$$
g_s(h,t)
=
1-c_s\left(1-p_s(h,t)\right)
$$

Predicted Extra Trees species use was first converted from log space to the original target scale, multiplied by the gate, and then transformed back to log space:

$$
u^*_s(h,t)
=
\left(\exp(m_s(h,t))-1\right)\times g_s(h,t)
$$

$$
m^*_s(h,t)
=
\log\left(1+u^*_s(h,t)\right)
$$

where $m_s(h,t)$ is the ungated Extra Trees prediction on the log-transformed species-use scale, $u^*_s(h,t)$ is gated species use on the original target scale, and $m^*_s(h,t)$ is the final gated species-use prediction stored as `species_use_log_pred`. The ungated Extra Trees prediction was retained as `species_use_ml_log_pred`. The parameter $c_s$ is the maximum proportional reduction allowed under the plausibility gate. In this implementation, $c_s = 0.10$ was applied to both species. Thus, the gate was bounded by:

$$
g_s(h,t)\in[0.9,1.0]
$$

Even when environmental plausibility was zero, predicted species use was only reduced by 10% rather than forced to zero. The plausibility gate was used as an exploratory support filter rather than as a calibrated biological correction factor. Because the gate value was not estimated from independent validation data, plausibility-filtered outputs were interpreted alongside the ungated species-use and risk surfaces. This allowed areas of weak environmental support to be identified without treating low plausibility as confirmed species absence.

### Fishing exposure and realized risk

Observed fishing activity was used to estimate realized risk. Global Fishing Watch defines apparent fishing effort as AIS-derived apparent fishing activity summarized as fishing hours for a vessel or area over time [@GFW_FAQs]. These apparent fishing hours were first aggregated to the `h3`/`date` grid. For realized-risk mapping, fishing exposure was then represented as a derived fleet-concentration-weighted activity index, calculated as apparent fishing hours multiplied by the number of unique vessels observed in the same cell-day:

$$
F(h,t)
=
H(h,t)
\times
V(h,t)
$$

where $H(h,t)$ is total apparent fishing hours and $V(h,t)$ is the number of unique vessels observed in the cell-day. This derived index is not the native Global Fishing Watch effort metric; it was used as a relative proxy to emphasize cell-days with both high apparent fishing duration and multiple active vessels. The index was transformed using:

$$
f(h,t)
=
\log\left(1 + F(h,t)\right)
$$

The realized risk score was then calculated additively on the transformed scale:

$$
r_s(h,t)
=
m^*_s(h,t)
+
f(h,t)
$$

This score is monotonic in both gated species use and fishing exposure, but it is not a calibrated bycatch probability and is not stored as a raw product of species use and fishing activity. Cells with no observed fishing activity received no fishing-exposure contribution, even when predicted species use was high.

### Latent risk

Latent risk was estimated using a standardized minimum fishing exposure instead of observed fishing activity. A baseline exposure of 0.5 vessel-hours per H3 cell-day was used, represented on the transformed scale as $\log(1+0.5)$. This baseline corresponds to approximately one vessel operating within or traversing an H3 resolution 6 cell for about 30 minutes at fishing speed.

Latent risk identifies where predicted species use would imply potential interaction risk if fishing activity were present. In contrast, realized-risk surfaces add the observed fishing-exposure term where activity was present.

For plausibility-aware latent-risk maps, low-plausibility cell-days were treated as environmentally weakly supported rather than confirmed absences. These products were interpreted together with the plausibility layer, so weakly supported predictions could be flagged without treating low plausibility as proof of absence.

### Operator-facing risk products

Operator-facing products were generated as aggregations and visual translations of the daily H3 prediction cube, not as separate models. Monthly prediction maps and monthly latent-risk matrices summarized the daily prediction outputs by species and H3 cell using the same spatial extent, basemap layers, and binned risk-color conventions as the main prediction maps.

Weekly planning products were derived from latent risk. Daily latent-risk predictions were grouped by ISO week for each `h3` cell and `species`. A 2014-2023 weekly climatology was produced by averaging weekly latent risk across years, providing an expected seasonal-risk surface for each ISO week. A separate 2022 weekly sequence was exported as an animation-oriented product to show one realized annual progression through the same weekly plotting framework.

As an applied management-unit example, the weekly H3 climatology was also aggregated to the Falklands Fisheries grid. Fisheries-grid summaries were plotted with protection-zone overlays and grid boundaries to illustrate how the H3 prediction cube can be translated into management units without changing the underlying model. These products were interpreted as planning and communication summaries of the modeled risk surfaces rather than as new validation evidence.

Final prediction outputs included `h3`, `date`, `species`, the hybrid species-use prediction, fishing exposure on the log scale, risk prediction on the log scale, `plausibility`, and `plausibility_gate`.

\newpage
