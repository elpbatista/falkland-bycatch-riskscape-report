# Discussion

> Provides clear, concise interpretation of the results of the project. Tiestogether concepts to create an interpretation that is greater than the individual results. Relates results back to the objectives of the project and to previous studies reported in the literature, if appropriate. Discusses uncertainties andassumptions that influenced the results.

---

The results show that the riskscape workflow can integrate environmental conditions, telemetry-derived species use, and fishing activity on a shared H3/day framework. This structure is important because it preserves the daily overlap between species-use predictions and fishing exposure rather than reducing risk to static habitat or effort summaries.

The feature-only seascape experiment provides a useful contrast with the primary hybrid species-use workflow. KMeans seascapes captured broad environmental regimes across shelf, shelf-break, and offshore waters, and these regimes helped summarize which environmental states were most represented in the species observations. However, the seascape-conditioned species-use maps did not retain the strongest localized hotspots present in the full Extra Trees/Bayesian-Gaussian mixture predictions. This limitation is expected: seascapes are categorical summaries of environmental structure, whereas the full species-use model uses the continuous predictor space and species identity to estimate fine-scale variation in residence index.

For this reason, feature-only seascapes are best interpreted as explanatory environmental classes, not as substitutes for the primary species-use or risk surfaces. They help distinguish broad environmental regimes from species-specific use patterns, but risk estimation should continue to rely on the full hybrid species-use predictions combined with observed or latent fishing exposure.

## Recommendations

Future work could use seascapes as a reporting or interpretation layer, for example by summarizing realized risk, fishing exposure, or predicted species use within each environmental regime. However, seascapes should not be used alone for hotspot detection unless the classification is explicitly redesigned and validated for that purpose.

\newpage
