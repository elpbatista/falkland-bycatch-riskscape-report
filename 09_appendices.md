# Apendices

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
