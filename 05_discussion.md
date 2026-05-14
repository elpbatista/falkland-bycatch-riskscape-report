# Discussion

> Provides clear, concise interpretation of the results of the project. Tiestogether concepts to create an interpretation that is greater than the individual results. Relates results back to the objectives of the project and to previous studies reported in the literature, if appropriate. Discusses uncertainties andassumptions that influenced the results.
>
> What the Discussion should do
> Target ~1500–2000 words. Three main blocks:
> Applied findings
> • Where the BBAL hotspots are (western/northwestern shelf and shelf-break) and how this
> matches the literature on BBAL warp-strike risk on the Patagonian shelf.
> • Where the SAFS hotspots are (compact, inner-shelf, near the islands) and what that
> implies for mitigation tractability.
> • How the realized vs. latent distinction reframes displacement concerns about static
> closures, with a concrete example from your maps.
> • How the FICZ/FOCZ structure appears in the realized-risk surfaces because effort honors
> those boundaries.
> • Species-specific implications: broad BBAL risk → fleet-wide gear measures (bird-scaring
> lines, warp deflectors, weighted lines) more efficient than closures; compact SAFS risk →
> near-colony spatial measures tractable.
> Methodological findings
> • The plausibility-gated hybrid behaves as designed and is portable to other regions.
> • The quantitative seascape-substitution result — frame as positive contribution to the
> seascape literature (see above).
> • Static spatial predictors (bathymetry, distance-to-coast) dominate feature importance.
> Honest discussion: real ecology of two shelf-associated species, or sign that limited
> telemetry temporal coverage constrains the dynamic signal? Probably both, and worth
> saying so.
> • The R² caveat — straightforward once the block CV is in.
> • Pipeline portability — would run for South Georgia, Crozet, Kerguelen with telemetry +
> GFW + Copernicus + GEBCO coverage. Worth flagging.
> Limitations, prioritized
> • Telemetry coverage is a snapshot — BBAL 16 days in December 2022, SAFS 146 days.
> Acknowledge first.
> • Fishing exposure is gear-agnostic. GFW data has gear-type metadata; one paragraph on
> gear-conditional risk (trawler-dominated BBAL warp-strike vs. longline-dominated
> hooking) would strengthen the management interpretation significantly.
> • Plausibility-gate c_s = 0.10 is a demonstration value. A single sensitivity figure (c_s ∈ {0,
> 0.1, 0.5, 1.0}) on BBAL latent risk closes this nicely.
> • No independent bycatch observer validation. Frame as top-priority next step, conditional
> on SAERI data access.

---

The results show that the riskscape workflow can integrate environmental conditions, telemetry-derived species use, and fishing activity on a shared H3/day framework. This structure is important because it preserves the daily overlap between species-use predictions and fishing exposure rather than reducing risk to static habitat or effort summaries.

The feature-only seascape experiment provides a useful contrast with the primary hybrid species-use workflow. KMeans seascapes captured broad environmental regimes across shelf, shelf-break, and offshore waters, and these regimes helped summarize which environmental states were most represented in the species observations. However, the seascape-conditioned species-use maps did not retain the strongest localized hotspots present in the full Extra Trees/Bayesian-Gaussian mixture predictions. This limitation is expected: seascapes are categorical summaries of environmental structure, whereas the full species-use model uses the continuous predictor space and species identity to estimate fine-scale variation in residence index.

For this reason, feature-only seascapes are best interpreted as explanatory environmental classes, not as substitutes for the primary species-use or risk surfaces. They help distinguish broad environmental regimes from species-specific use patterns, but risk estimation should continue to rely on the full hybrid species-use predictions combined with observed or latent fishing exposure.

## Recommendations

Future work could use seascapes as a reporting or interpretation layer, for example by summarizing realized risk, fishing exposure, or predicted species use within each environmental regime. However, seascapes should not be used alone for hotspot detection unless the classification is explicitly redesigned and validated for that purpose.

\newpage
