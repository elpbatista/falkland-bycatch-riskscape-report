# Methods

The opening text without subtitle should probably be a short introductory paragraph that:

* summarizes the workflow,
* states the general analytical approach,
* and briefly introduces the components.

Not detailed implementation.

## Framework

Describe the overall framework and how the different components fit together. This is where you can introduce the risk equation and how it will be applied in this context.

This is where you define:

* conceptual structure,
* component relationships,
* analytical flow,
* assumptions,
* and possibly the overall risk equation.

This is probably where your workflow diagram belongs.

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

Environmental raster variables were aggregated to the H3 grid using area-weighted means. Raster pixels were converted to polygon footprints and intersected with H3 cell polygons. The geodesic area of each pixel-H3 overlap was used to compute normalized weights within each H3 cell. Daily raster values were aggregated by multiplying intersecting pixel values by their overlap weights and summing across pixels while excluding missing or invalid raster values. This produced daily H3-level environmental features aligned to the common H3/date modeling framework.

## Data Processing

What goes here? This is where you can describe the data processing steps, including any cleaning, transformation, or integration of the different data sources. You can also describe how the H3 spatial framework was applied...

because the section is really:

* harmonization,
* transformation,
* aggregation,
* alignment,
* cube generation.
  
would naturally include:

* temporal harmonization,
* spatial aggregation,
* interpolation,
* derived variables,
* gradients,
* rolling statistics,
* front metrics,
* encoding,
* normalization,
* feature engineering.

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
