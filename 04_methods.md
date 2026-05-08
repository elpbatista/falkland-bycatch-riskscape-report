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

### Environmental Data

### Fisheries Data

### Biological Data

### Reference Data

would only contain layers used for:

* mapping,
* masking,
* clipping,
* visualization,
* boundaries.

* Falkland Islands fisheries zones
* EEZ boundaries
* land polygons

## H3 Spatial Framework

This is where you describe:

* resolution,
* indexing,
* aggregation logic,
* temporal structure,
* spatial consistency.

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
