# Introduction

> Introduces the reader to the report. Describes the problem that is to be address, what the current state of knowledge is in this area, and the motivation for resolving this issue. It also outlines the structure of the report.
> What the Introduction should do
> Target ~1200–1500 words. Three connected literatures, then the gap and the contribution,
> then research questions. Note there are more uptpdate references. Thes e are just some I have
> noted form my previous work:
> • Bycatch as a global conservation problem, specific to seabirds and pinnipeds. Lewison et
> al. (2014), Croxall et al. (2012), Phillips et al. (2016) for the global framing; Grémillet et > al.
> (2000), Catry et al. (2013), Favero et al. (2011), Tamini et al. (2015) for BBAL on the
> Patagonian shelf; Baylis et al. (2015) for SAFS in the Falklands.
> • Dynamic ocean management as the state-of-the-art response. Hobday et al. (2010),
> Žydelis et al. (2011, the closest direct antecedent — telemetry-driven dynamic habitat
> models for albatrosses overlapped with longline observer data), Maxwell et al. (2015),
> Hazen et al. (2018, EcoCast), Welch et al. (2019). The throughline: from static seasonal
> closures to environment-conditioned, telemetry-informed near-real-time risk surfaces.
> • Seascape ecology as the classification framework. Kavanaugh et al. (2014, 2016),
> Woodill, Kavanaugh, Harte & Watson (2021). The throughline: dynamic environmental
> classification gives a synoptic biogeographic vocabulary for marine ecosystems. The open
> question your work tests directly is whether seascape classifications, on their own, are
> sufficient for products as fine-scale as bycatch hotspot detection.
> • The gap and the contribution. No daily-resolution riskscape exists for the Falkland Islands
> EEZ; this project builds one, makes the plausibility-gated hybrid the central
> methodological move, separates realized from latent risk, and uses the dataset to test the
> seascape-substitution hypothesis directly.
> Then explicit research questions. I'd suggest something like:
> RQ1: Where and when do predicted BBAL and SAFS use and observed fishing effort co-occur in
> the Falkland Islands EEZ between 2014 and 2023?
> RQ2: How does observed (realized) bycatch-interaction risk differ from potential (latent) risk
> under a baseline minimum-exposure scenario?
> RQ3: Can unsupervised feature-only environmental seascapes substitute for telemetry-informed
> species-use predictions for hotspot detection, and if not, how does the substitution fail?
> On the seascape framing — important
> This is the piece I want you to be most thoughtful about. The seascape-substitution result is
> almost certainly going to be the most interesting part of the paper from Maria's perspective,
> because it sits directly inside her published framework and tests a proposition that the
> seascape literature has not previously tested quantitatively. Frame it accordingly:
> • Position your work as extending the seascape framework rather than critiquing it.
> Kavanaugh et al. (2014, 2016) established that dynamic seascapes capture coherent
> biogeochemical regimes. Woodill et al. (2021) showed seascapes predict where distant-
> water fleets fish. Your project asks the next question down: are seascape classifications,
> alone, sufficient for bycatch hotspot detection? Your answer is well-supported and
> substantive — they preserve broad structure (spatial correlations 0.54–0.83) but
> compress hotspot intensity at the upper tail (99th-percentile drops by roughly half for
> BBAL). This is a contribution to the seascape literature, not a negative result against it.
> • Be precise about what your KMeans seascapes are and aren't. You did a feature-only
> KMeans with 10 classes on the same predictor matrix used for the hybrid model,
> excluding species identity. That's a reasonable comparison object but it's not the same
> construction as the operational MBON/NOAA Seascape Pelagic Habitat Classification — be
> explicit about this in Methods so the comparison is read correctly.
> • The Woodill et al. paper is directly relevant. We used seascapes to predict EEZ incursions
> by distant-water vessels — i.e., we used seascapes as a predictive feature space for a
> fisheries-management question. Your paper is the natural follow-on: "if seascapes work
> for that question, do they also work for the more spatially fine-grained question of
> bycatch hotspot detection?" Cite it explicitly and frame your work as extending that line.
> • Suggest in the Discussion that the seascape-conditioned surfaces are useful as an
> interpretation layer (which they clearly are — Tables 4–9 show this nicely) even if they are
> not a substitute for telemetry-informed hotspot detection. This is a generous, accurate
> framing of your own result.

\newpage
