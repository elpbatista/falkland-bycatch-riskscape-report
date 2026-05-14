# Methods

This chapter describes the analytical workflow used to construct dynamic bycatch riskscapes for the Falkland Islands region. The workflow integrates environmental raster products, fishing effort observations, species telemetry records, and static spatial reference layers within a common H3-based spatial framework and daily temporal resolution. These harmonized datasets were used to train species-use models, estimate environmental plausibility, classify feature-only environmental seascapes, and combine predicted species use with fishing exposure to generate relative risk surfaces.

The methods are organized into six components: the overall riskscape framework, input datasets, spatial and temporal data processing, species-use modeling, feature-only seascape classification, and risk estimation. Validation procedures are then summarized, including implemented model diagnostics and additional validation approaches identified for future development.

## Framework

This project uses a spatially explicit riskscape framework to estimate potential bycatch risk as the overlap between species use, fishing activity, and environmental conditions. The framework integrates environmental and fisheries datasets covering the period 2014-2023 with species-use observations derived from telemetry records collected from tracked individuals during 2022-2023. Because telemetry observations represent limited sampling periods and only tracked individuals, the resulting riskscapes should be interpreted as relative indicators of species use and potential interaction risk rather than definitive representations of population-level species distributions or observed bycatch probability.

The framework does not attempt to predict observed bycatch events directly. Instead, it represents bycatch risk as a relative spatiotemporal index describing where and when species use and fishing activity co-occur under environmental conditions associated with observed species use.

All datasets were aligned to a common H3 grid and daily temporal resolution. Each record in the modeling framework represents one H3 cell on one date. Environmental variables describe the oceanographic state of each cell-day, species tracking data provide evidence of animal use, and fishing effort data represent operational exposure.

The conceptual framework separates three components. First, species-use modeling estimates where each species is likely to occur or concentrate as a function of environmental conditions. Second, fishing exposure represents the intensity of fishing activity in each H3 cell and date. Third, the risk surface combines predicted species use and fishing exposure to compute a relative index of potential interaction risk for each H3 cell and date.

The general risk relationship can be expressed as:

$$
\mathrm{Risk}(h,t,s)=\mathrm{SpeciesUse}(h,t,s)\times\mathrm{FishingExposure}(h,t)
$$

where $h$ is an H3 cell, $t$ is date, and $s$ is species. In the implemented workflow, species use and fishing exposure were represented on a transformed scale, so the risk index was computed as:

$$
\log\left(\mathrm{Risk}(h,t,s)\right)
=
\log\left(\mathrm{SpeciesUse}(h,t,s)\right)
+
\log\left(\mathrm{FishingExposure}(h,t)\right)
$$

This formulation treats risk as a relative index rather than an absolute probability of bycatch. High-risk cells therefore represent locations and dates where predicted species use and fishing activity are both high.

To represent minimum operational fishing exposure within the H3 framework, a baseline fishing effort unit of 0.5 vessel-hours per H3 cell was introduced. This value approximates the minimum time required for a fishing vessel operating at fishing speed to traverse an H3 resolution 6 cell. The baseline effort unit was used to estimate latent interaction risk through overlap with predicted species use surfaces, including locations and dates where observed fishing effort was absent or sparse.

The framework assumes that bycatch risk increases with spatiotemporal overlap between species use and fishing activity, and that environmental conditions help explain variation in species use.

## Data

The analytical framework integrates environmental, fisheries, biological, and spatial reference datasets describing oceanographic conditions, fishing activity, species presence, and management boundaries across the Falkland Islands region. Aggregated datasets were harmonized within a common spatiotemporal framework, enabling the integration of heterogeneous data sources into a unified modeling framework.

### Environmental Data

Environmental variables included sea surface temperature (SST), chlorophyll-a concentration (CHL), sea surface height (SSH), near-surface wind components, and bathymetry. Daily SST fields were obtained from the NASA Multi-scale Ultra-high Resolution (MUR) Level 4 product [@nasa/jplGHRSSTLevel42015], chlorophyll-a and SSH products were obtained from the Copernicus Marine Service [@europeanunion-copernicusmarineserviceGlobalOceanColour2022; @europeanunion-copernicusmarineserviceGLOBALOCEANGRIDDED2021], wind components were derived from ERA5 daily statistics distributed through the Copernicus Climate Data Store [@c3sERA5PostprocessedDaily2024], and bathymetric elevation data were obtained from GEBCO [@gebcobathymetriccompilationgroup2026GEBCO_2026GridContinuous2026].

Environmental raster datasets were spatially aligned to the H3 framework and temporally harmonized at daily resolution. Because these products provide continuous spatial coverage across the study area, environmental conditions were available for all H3 cells and dates within the analysis period.

### Fisheries Data

Fishing effort data were derived from Global Fishing Watch (GFW) AIS-based [@globalfishingwatchGlobalAISbasedApparent2025] fishing activity products covering the period 2014-2023. The dataset contains 2,297,069 records representing 3,094,974.5 fishing hours from 2,011 unique vessels.

The dominant fishing gear types were trawlers with 1,497,210 fishing hours from 567 vessels, squid jiggers with 1,256,653 fishing hours from 1,207 vessels, and set longlines thet account for 202,871 fishing hours with only 41 vessels. The dataset includes vessels operating under multiple flag states, with the largest fishing effort contributions associated with Argentina (ARG), China (CHN), Taiwan (TWN), South Korea (KOR), Spain (ESP), and the Falkland Islands (FLK).

Fishing effort observations were aggregated by H3 cell and date, producing daily spatial fishing effort features including total fishing hours and vessel counts for each `h3`/`date` combination.

### Biological Data

Species presence data were derived from field telemetry records provided by the South Atlantic Environmental Research Institute (SAERI). The dataset contains 59,182 records with valid geographic coordinates and timestamps collected during 2022-2023.

The dataset includes observations of Black-browed albatrosses (*Thalassarche melanophris*; BBAL) and South American fur seals (*Arctocephalus australis*; SAFS). BBAL accounts for 33,425 records from 27 individuals and 58 trips covering 16 days between 2022-12-02 and 2022-12-17. SAFS accounts for 25,757 records from 15 individuals and 18 trips collected between 2022-10-22 and 2023-03-16, covering 146 observation days (71 in 2022 and 75 in 2023).

Telemetry observations were aggregated by H3 cell, date, and species, resulting in 10,268 `h3`/`date`/`species` rows.

### Reference Data

The Falkland Islands fisheries grid system [@fisheries_grid_squares] covers the region between 47°–57° latitude and 64°–52° longitude. Grid cells measure 0.25° latitude by 0.50° longitude and are associated with fisheries licensing zones. The fisheries grid extent was used to define the study area, and an additional 50 km buffer was applied to reduce edge effects and ensure complete spatial coverage across datasets with different spatial resolutions.

Falkland Islands Conservation Zones [@ukho_ficz_focz_limits] defined for fisheries activities were also incorporated. Two zones were identified and classified as inner (FICZ) and outer (FOCZ) conservation zones.

Natural Earth coastline and land 1:10m physical vectors datasets [@ne_10m_coastline; @ne_10m_land] were used for cartographic reference, masking, and distance-based spatial analyses.

## H3 Spatial Framework

Spatial integration was performed using the H3 hierarchical hexagonal indexing system developed by Uber [@HomeH3]. The study area was discretized using H3 resolution 6 cells, providing an average cell area of approximately 36 km². The fisheries grid extent plus an additional 50 km buffer was converted to an H3 grid containing 37,209 cells. The H3 framework provides globally unique hierarchical spatial indexes and was used as a common spatial reference for integrating environmental variables, fishing effort, and species presence observations across daily temporal intervals.

Environmental raster variables were aggregated to the H3 grid using area-weighted means. Raster pixels were converted to polygon footprints and intersected with H3 cell polygons. The geodesic area of each pixel-H3 overlap was used to compute normalized weights within each H3 cell. Daily raster values were aggregated by multiplying intersecting pixel values by their overlap weights and summing across pixels while excluding missing or invalid raster values. This produced daily H3-level environmental features aligned to the common `h3`/`date` modeling framework.

## Data Processing

Data processing followed a staged pipeline that transformed raw environmental rasters, fishing observations, species telemetry records, and static spatial layers into H3-indexed feature tables. The pipeline used `h3` and `date` as common keys, with H3 indexes stored as unsigned 64-bit integers and dates converted to UTC daily timestamps. Intermediate and final tables were written as yearly Parquet partitions using ZSTD compression.

Environmental processing began with raster-to-H3 lookup tables computed separately for each raster product and reused during feature generation. Daily raster values were aggregated to H3 cells using the area-weighted procedure described above. Aggregated environmental tables were grouped by `h3` and `date`, duplicate contributions were averaged, and variables were stored as 32-bit floating point values.

Several derived environmental variables were generated after spatial aggregation. Near-surface wind speed was calculated from zonal and meridional wind components as:

$$
\mathrm{WindSpeed}
=
\sqrt{u_{10}^{2}+v_{10}^{2}}
$$

Chlorophyll-a concentration was log-transformed to reduce skew:

$$
\mathrm{CHL}_{\log}
=
\log\left(1+\mathrm{CHL}\right)
$$

Seasonality was represented with cyclic day-of-year predictors on a 365-day cycle. For non-leap years, the calendar day of year was used directly. For leap years, dates after February 28 were shifted back by one day so that February 29 was removed from the seasonal cycle. The adjusted day of year, $d^*$, was therefore defined as:

$$
d^* =
\begin{cases}
d - 1, & \text{if year is leap and } d > 59 \\
d, & \text{otherwise}
\end{cases}
$$

where $d$ is the calendar day of year and $d^*$ is the adjusted day of year. Seasonal predictors were then encoded as:

$$
\mathrm{doy\_sin}=\sin\left(\frac{2\pi d^*}{365}\right)
$$

$$
\mathrm{doy\_cos}=\cos\left(\frac{2\pi d^*}{365}\right)
$$

Spatial gradients were computed on the H3 grid to represent local environmental contrast. To improve processing efficiency, H3 ring-1 neighbor relationships were precomputed as lookup and index tables and reused during feature generation. For each date, each H3 cell was compared with its valid neighboring cells. Gradients were calculated as the root mean square difference between the focal cell and its neighbors:

$$
\mathrm{gradient}(i)=\sqrt{\mathrm{mean}\left((X_i-X_j)^2\right)}
$$

where $i$ is the focal H3 cell and $j$ are valid neighboring cells. Gradients were calculated for SST, log-transformed chlorophyll-a, and SSH.

Temporal anomalies were computed relative to local seasonal conditions. For each H3 cell and adjusted day-of-year, a climatological mean was calculated across the full environmental record. Daily anomalies were then calculated as:

$$
X_{\mathrm{anom}}(i,t)=X(i,t)-\mathrm{mean}\left(X(i,\mathrm{adjusted\_doy})\right)
$$

Anomalies were calculated for SST, log-transformed chlorophyll-a, SSH, and wind speed.

Fishing effort records were converted to geographic points, spatially joined to the H3 grid, and aggregated by H3 cell and date. Daily fishing features included total fishing hours and unique vessel counts. Fishing activity was calculated as:

$$
\mathrm{FishingActivity}(h,t)
=
\mathrm{FishingHours}(h,t)
\times
\mathrm{VesselCount}(h,t)
$$

where $h$ is an H3 cell and $t$ is date. `h3`/`date` combinations without fishing observations were retained and assigned zero fishing effort values.

Telemetry records were cleaned by parsing timestamps, removing invalid dates, and retaining records with valid coordinates. Observations were spatially joined to the H3 grid and aggregated by H3 cell, date, and species. Species-use support variables included telemetry record count, individual count, and trip count. For model training, observed `species`/`date` combinations were expanded across all H3 cells available in the environmental feature grid. Cells without telemetry observations for a given `species`/`date` combination were retained and assigned zero support values.

Static spatial features were generated once per H3 cell. Bathymetric depth and slope were derived from the GEBCO raster using the same area-weighted H3 aggregation procedure. Distance to coast was calculated geodesically from each H3 centroid to the nearest coastline geometry. H3 centroid latitude and longitude were encoded using sine and cosine transformations to avoid discontinuities at coordinate boundaries.

Final modeling tables were assembled by joining dynamic environmental variables, derived features, static spatial variables, species-use support variables, and fishing-effort features on common `h3`/`date` keys. Dynamic predictors included `sst`, `ssh`, `wind_speed`, log-transformed `chl`, environmental anomalies, H3-neighbor gradients, and seasonal sine/cosine terms. Static predictors included bathymetry, slope, distance to coast, and encoded H3 centroid coordinates. No temporal interpolation or rolling-window smoothing was applied; all features were derived directly from daily source observations and deterministic `h3`/`date` aggregation.

## Species-Use Modeling

Predictor variables represented environmental state, environmental variability, seasonality, and static spatial structure. Dynamic predictors described oceanographic conditions for each `h3`/`date` combination, while derived variables captured local gradients, seasonal anomalies, and cyclic temporal patterns. Static predictors represented persistent geographic structure, including bathymetry, slope, coastal proximity, and spatial position.

Species-use models were trained to predict relative species use from environmental and static spatial predictors. The training dataset was constructed from the `h3`/`date`/`species` species-presence table joined to the environmental feature grid. The response variable was a `ResidenceIndex` defined as the product of telemetry record count and individual count for each observed `h3`/`date`/`species` group:

$$
\mathrm{ResidenceIndex}(h,t,s)
=
\mathrm{PresenceCount}(h,t,s)
\times
\mathrm{IndividualCount}(h,t,s)
$$

where $h$ is an H3 cell, $t$ is date, and $s$ is species. This formulation was used to increase the relative influence of locations with both repeated observations and multiple tracked individuals. The modeling target was transformed as:

$$
y=\log\left(1+\mathrm{ResidenceIndex}\right)
$$

For each observed `species`/`date` combination, all H3 cells in the environmental feature grid were included. Cells without telemetry observations were retained and assigned zero target values, allowing the model to learn from both observed-use and unused cells within the same environmental domain.

Predictor variables included dynamic environmental conditions, derived environmental features, seasonal terms, and static spatial variables. Dynamic predictors included `sst`, `ssh`, `wind_speed`, and log-transformed `chl`. Derived predictors included environmental anomalies and H3-neighbor gradients. Seasonal predictors were represented using cyclic day-of-year sine and cosine terms. Static predictors included bathymetric depth, bathymetric slope, distance to coast, and encoded H3 centroid coordinates.

The implemented workflow used a joint-species modeling approach. Species identity was represented using one-hot encoded categorical variables appended to the numerical predictor matrix. This allowed a single model to learn shared environmental structure while preserving species-specific responses.

Because zero-use rows greatly outnumbered positive-use rows, the training dataset was balanced before model fitting. All positive rows were retained, and an equal number of zero-use rows was randomly sampled. Sample weights were applied during fitting to increase the influence of higher-use observations:

$$
w
=
1+\mathrm{ResidenceIndex}^{0.75}
$$

Four model classes were evaluated during model comparison: histogram gradient boosting, random forest, extra trees, and a Bayesian/Gaussian mixture approach. Tree-based models were trained using ensemble learning methods with regularization and constrained tree depth to reduce overfitting.

The Bayesian/Gaussian mixture implementation used a Gaussian mixture model fitted to positive-use observations in standardized feature space. The resulting environmental likelihood surface was normalized and combined with a histogram gradient boosting prior trained on the full dataset. Final predictions from this estimator were generated as an equal-weighted combination of the likelihood-based estimate and the prior model prediction.

Model outputs were expressed as `species_use_log_pred`, representing predicted species use on the log-transformed scale. Model comparison metrics were computed after back-transforming predictions to the original target scale and included $R^2$, root mean squared error, and mean absolute error.

## Feature-Only Seascape Classification

An additional feature-only seascape classification was implemented as an exploratory comparison with the telemetry-informed Bayesian/Gaussian mixture components. This step used the environmental and static predictor matrix only. Species identity, telemetry-derived response variables, environmental plausibility, fishing exposure, and species-use predictions were excluded from model fitting so that the resulting classes represented recurring environmental states rather than species-specific use or risk.

The feature-only classifier used KMeans clustering with 10 classes, matching the selected number of Bayesian/Gaussian mixture components. Predictors were standardized before clustering, and the fitted model was applied to the full 2014--2023 environmental feature grid. Each H3/date record therefore received a species-independent seascape label describing the dominant environmental regime for that cell-day.

Seascape classes were summarized by their environmental and static predictor distributions, including SST, SSH, wind speed, log-transformed chlorophyll-a, bathymetry, and distance to coast. The seascape labels were also joined back to observed positive species-use records to describe which environmental regimes were represented in the telemetry observations for each species.

Finally, the seascape classes were used in a post hoc comparison with the full hybrid species-use predictions. For each species, predicted log-transformed residence index from the hybrid model was summarized by seascape class and then projected back onto the H3/date grid as a seascape-conditioned species-use surface. This projection was used only to test how much of the predicted species-use structure could be represented by broad environmental regimes. It was not used as the primary risk input because seascape classes intentionally simplify the continuous predictor space and can smooth localized hotspots.

## Risk Estimation

Risk estimation was implemented as a relative spatiotemporal overlap index, not as a direct prediction of observed bycatch probability. The workflow combined predicted species use, environmental plausibility, and fishing exposure for each H3 cell, date, and species.

### Environmental plausibility

Environmental plausibility was estimated with the Bayesian/Gaussian mixture model. For each `h3`/`date`/`species` combination, the model calculated the log density of the environmental feature vector under the fitted Gaussian mixture model. Log densities were normalized to a bounded plausibility score using the fitted 1st and 99th percentile density limits:

$$
\mathrm{Plausibility}(h,t,s)
=
\mathrm{clip}
\left(
\frac{
\ell(h,t,s) - \ell_{\min}
}{
\ell_{\max} - \ell_{\min}
},
0,
1
\right)
$$

where $\ell(h,t,s)$ is the Gaussian mixture log density for H3 cell $h$, date $t$, and species $s$, and $\ell_{\min}$ and $\ell_{\max}$ are the lower and upper normalization limits estimated during model fitting. Plausibility values near 1 indicate environmental conditions similar to those associated with observed species use; values near 0 indicate weak environmental support relative to the fitted use-space distribution.

The plausibility score was used as an exploratory support filter for the Extra Trees species-use predictions. Let $d(h,t,s)$ be the Bayesian/Gaussian mixture log density for H3 cell $h$, date $t$, and species $s$. The normalized plausibility score was:

$$
p(h,t,s)
=
\mathrm{clip}
\left(
\frac{d(h,t,s)-d_{\min}}{d_{\max}-d_{\min}},
0,
1
\right)
$$

The plausibility gate was then defined as:

$$
g(h,t,s)
=
1-c_s\left(1-p(h,t,s)\right)
$$

Predicted Extra Trees species use was first converted from log space to the original target scale, multiplied by the gate, and then transformed back to log space:

$$
u^*(h,t,s)
=
u_{\mathrm{ExtraTrees}}(h,t,s)\times g(h,t,s)
$$

$$
\log\left(1+u^*(h,t,s)\right)
=
\mathrm{final\ species\mbox{-}use\ log\ prediction}
$$

where $c_s$ is the maximum proportional reduction allowed under the plausibility gate. In this implementation, $c_s = 0.10$ was applied to both species. Thus, the gate was bounded by:

$$
g(h,t,s)\in[0.9,1.0]
$$

Even when environmental plausibility was zero, predicted species use was only reduced by 10% rather than forced to zero.

The plausibility gate was used as an exploratory support filter rather than as a calibrated biological correction factor. Because the gate value was not estimated from independent validation data, plausibility-filtered outputs were interpreted alongside the ungated species-use and risk surfaces. This allowed areas of weak environmental support to be identified without treating low plausibility as confirmed species absence.

### Fishing exposure and realized risk

Observed fishing activity was used to estimate realized risk. For each `h3`/`date` combination, fishing activity was calculated as:

$$
\mathrm{FishingActivity}(h,t)
=
\mathrm{FishingHours}(h,t)
\times
\mathrm{VesselCount}(h,t)
$$

Fishing activity was transformed using:

$$
\mathrm{FishingActivityLog}(h,t)
=
\log\left(1 + \mathrm{FishingActivity}(h,t)\right)
$$

The realized risk index was then calculated additively in log space:

$$
\mathrm{RiskLogPred}(h,t,s)
=
\mathrm{SpeciesUseLogPred}(h,t,s)
+
\mathrm{FishingActivityLog}(h,t)
$$

This is equivalent to estimating risk as a multiplicative overlap between species use and fishing exposure on the original scale. Cells with no observed fishing activity received no realized fishing-exposure contribution, even when predicted species use was high.

### Latent risk

Latent risk was estimated using a standardized minimum fishing exposure instead of observed fishing activity. A baseline exposure of 0.5 vessel-hours per H3 cell-day was used, representing approximately one vessel operating within or traversing an H3 resolution 6 cell for about 30 minutes at fishing speed.

Latent risk identifies where predicted species use would imply potential interaction risk if fishing activity were present. In contrast, realized risk identifies where predicted species use overlapped with observed fishing activity.

For plausibility-filtered latent risk, low-plausibility cell-days were treated as environmentally weakly supported rather than confirmed absences. Where plausibility fell below the selected support threshold, latent plausible risk was not reported for that cell-day.

Final prediction outputs included H3 cell, date, species, hybrid species-use prediction, fishing exposure on the log scale, risk prediction on the log scale, plausibility, and gate value.

## Validation

Validation included data-quality checks, model-performance evaluation, and environmental-support assessment. During preprocessing, feature tables were checked for required columns, consistent `h3` and `date` keys, duplicate records, missing values, and expected data types. Environmental features were inspected after aggregation and transformation to confirm that yearly partitions retained the expected `h3`/`date` structure and that derived variables, including gradients and anomalies, were generated without row inflation.

Species-use models were evaluated using a random train-test split with 25% of rows withheld for testing. Predictions were evaluated after back-transforming from log space to the original residence-index scale. Model comparison metrics included coefficient of determination ($R^2$), root mean squared error (RMSE), and mean absolute error (MAE). Additional diagnostics included predicted-versus-observed plots, residual inspection, and feature-importance analysis. These diagnostics supported interpretation of model behavior but were not treated as independent ecological validation.

Environmental plausibility was evaluated separately from direct species-use prediction. The Bayesian/Gaussian mixture model was used to identify `h3`/`date`/`species` combinations whose environmental conditions were similar to those associated with observed telemetry locations. Plausibility values were therefore interpreted as environmental-support diagnostics rather than as direct validation of species presence or absence. Risk surfaces were interpreted alongside plausibility surfaces to distinguish well-supported predictions from environmental extrapolation.

Feature-only seascapes were evaluated as an interpretive diagnostic rather than as an independent predictive model. Their outputs were compared with Bayesian/Gaussian mixture components, observed positive species-use records, and hybrid species-use prediction surfaces. This comparison was used to assess whether broad environmental regimes could explain the spatial structure of predicted species use and whether they retained localized high-use areas.

Several additional validation approaches were not implemented in the current workflow but would strengthen future analyses. These include spatial or spatiotemporal block cross-validation, validation across individuals or trips, sensitivity analysis of the plausibility-gate parameter, comparison with independent bycatch or observer records, and uncertainty assessment across model classes and aggregation strategies.

\newpage
