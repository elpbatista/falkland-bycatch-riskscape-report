---
title: "Dynamic Bycatch Riskscape Framework for the Falkland Islands"
author: "Jorge L. Batista Echevarría"
date: "2026"
toc-depth: 3
numbersections: true
geometry: margin=1in
fontsize: 11pt
bibliography: references.bib
csl: apa.csl
header-includes:
  - \usepackage{graphicx}
  - \usepackage[font=small,labelfont=bf]{caption}
---

\newpage

\tableofcontents

\newpage

`pandoc report.md --citeproc -o report.pdf`

Text citation [@kavanaughHierarchicalDynamicSeascapes2014].

## Compilation Bash

```bash
pandoc \
  00_title.md \
  01_abstract.md \
  02_introduction.md \
  03_literature_review.md \
  04_study_area.md \
  05_data.md \
  06_methods.md \
  07_results.md \
  08_discussion.md \
  09_conclusion.md \
  10_references.md \
  11_appendix.md \
  --citeproc \
  -o batistaj_capstone.pdfc
```

## Compilation Bash (the one I actually used)

```bash
pandoc \
  00_title.md \
  01_abstract.md \
  02_introduction.md \
  03_methods.md \
  04_results.md \
  05_discussion.md \
  06_conclusion.md \
  07_references.md \
  08_appendices.md \
  --citeproc \
  -o batistaj_capstone.pdf
```

The risk is defined as $R = P \times E$.

---

$$
R = P \times E
$$

\newpage

\begin{figure}[htbp]
\centering
\includegraphics[width=0.75\textwidth]{figures/study_area.png}
\caption{Study area showing the Falkland Islands region, bathymetry, and H3 spatial grid.}
\label{fig:study-area}
\end{figure}

\newpage

```bash
pandoc \
  00_title.md \
  01_abstract.md \
  02_introduction.md \
  03_methods.md \
  04_results.md \
  05_discussion.md \
  06_conclusion.md \
  07_references.md \
  08_appendices.md \
  --citeproc \
  -o _draft_batistaj_capstone.pdf
```

\newpage

## References
