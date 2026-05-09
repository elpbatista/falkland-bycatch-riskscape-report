# Methods

The opening text without subtitle should probably be a short introductory paragraph that:

* summarizes the workflow,
* states the general analytical approach,
* and briefly introduces the components.

Not detailed implementation.

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

Fishing effort data were derived from Global Fishing Watch (GFW) AIS-based [@globalfishingwatchGlobalAISbasedApparent2025] fishing activity products covering the period 2014-2023. The dataset contains 2,297,069 records representing approximately 3.1 million fishing hours from 2,011 unique vessels.

The dominant fishing gear types were trawlers with 1,497,210 fishing hours from 567 vessels, squid jiggers with 1,256,653 fishing hours from 1,207 vessels, and set longlines thet account for 202,871 fishing hours with only 41 vessels. The dataset includes vessels operating under multiple flag states, with the largest fishing effort contributions associated with Argentina (ARG), China (CHN), Taiwan (TWN), South Korea (KOR), Spain (ESP), and the Falkland Islands (FLK).

Fishing effort observations were aggregated by H3 cell and date, producing daily spatial fishing effort features including total fishing hours and vessel counts for each H3/date combination.

### Biological Data

Species presence data were derived from field telemetry records provided by the South Atlantic Environmental Research Institute (SAERI). The dataset contains 59,182 records with valid geographic coordinates and timestamps collected during 2022-2023.

The dataset includes observations of Black-browed albatrosses (*Thalassarche melanophris*; BBAL) and South American fur seals (*Arctocephalus australis*; SAFS). BBAL accounts for 33,425 records from 27 individuals and 58 trips covering 16 days between 2022-12-02 and 2022-12-17. SAFS accounts for 25,757 records from 15 individuals and 18 trips collected between 2022-10-22 and 2023-03-16, covering 146 observation days (71 in 2022 and 75 in 2023).

Telemetry observations were aggregated by H3 cell, date, and species, resulting in 10,268 H3/date/species rows.

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

Telemetry records were cleaned by parsing timestamps, removing invalid dates, and retaining records with valid coordinates. Observations were spatially joined to the H3 grid and aggregated by H3 cell, date, and species. Species-use support variables included telemetry record count, individual count, and trip count. For model training, observed species/date combinations were expanded across all H3 cells available in the environmental feature grid. Cells without telemetry observations for a given species/date combination were retained and assigned zero support values.

Static spatial features were generated once per H3 cell. Bathymetric depth and slope were derived from the GEBCO raster using the same area-weighted H3 aggregation procedure. Distance to coast was calculated geodesically from each H3 centroid to the nearest coastline geometry. H3 centroid latitude and longitude were encoded using sine and cosine transformations to avoid discontinuities at coordinate boundaries.

Final modeling tables were assembled by joining dynamic environmental variables, derived features, static spatial variables, species-use support variables, and fishing-effort features on common `h3`/`date` keys. Dynamic predictors included `sst`, `ssh`, `wind_speed`, log-transformed `chl`, environmental anomalies, H3-neighbor gradients, and seasonal sine/cosine terms. Static predictors included bathymetry, slope, distance to coast, and encoded H3 centroid coordinates. No temporal interpolation or rolling-window smoothing was applied; all features were derived directly from daily source observations and deterministic `h3`/`date` aggregation.

## Species-Use Modeling

Keep focused on:

* predictors,
* response variables,
* training strategy,
* model selection,
* outputs.

should focus on:

* which features were used,
* model inputs/outputs,
* training strategy,
* model configuration.

Not the detailed construction of the features themselves.

Avoid discussing results here.

## Risk Estimation

This is where:

* interaction surfaces,
* exposure combination,
* probability integration,
* or risk scoring
    should be described.

## Validation

Include:

* train/test split,
* metrics,
* uncertainty,
* plausibility checks,
* feature importance if used.
