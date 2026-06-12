# 🌾 Cluster Analysis: Seeds & Global Prosperity Indicators

**Unsupervised learning project applying hierarchical and partitional clustering to two distinct datasets**

> By Sandaru, Laura, Diego & Julio

---

## 📋 Table of Contents

- [Overview](#overview)
- [Repository Structure](#repository-structure)
- [Datasets](#datasets)
- [Methodology](#methodology)
- [Key Findings](#key-findings)
- [How to Run](#how-to-run)
- [R Package Requirements](#r-package-requirements)

---

## Overview

This project investigates clustering methods to uncover meaningful patterns and groupings within two distinct datasets:

**Part 1 — Global Prosperity & Development Indicators (GPI)**
Applies **hierarchical clustering** (single, complete, average, and Ward linkage) to 167 countries across 13 prosperity dimensions, classifying them as Developed, Developing, or Underdeveloped.

**Part 2 — Seeds Dataset**
Applies **partitional clustering** (K-means, K-means++, and K-medoids) to 210 wheat kernel measurements across 7 geometric features, recovering the three natural wheat varieties (Kama, Rosa, Canadian).

---

## Repository Structure

```
cluster-analysis-seeds-gpi/
│
├── README.md
│
├── data/
│   ├── global_prosperity_and_development_indicators.csv   ← 167 countries × 13 features
│   └── seeds_dataset.txt                                  ← 210 kernels × 7 features + label
│
├── analysis/
│   ├── Cluster_Analysis_-_GPI.Rmd       ← hierarchical clustering on GPI data
│   └── Cluster_Analysis_-_Seeds.Rmd     ← K-means / K-medoids on Seeds data
│
├── output/
│   ├── Cluster_Analysis_GPI.pdf         ← knitted PDF report (GPI)
│   └── Cluster_Analysis_Seeds.pdf       ← knitted PDF report (Seeds)
│
└── docs/
    └── Joint_Seeds_and_Prosperity_Presentation.pptx    ← combined project slides
```

---

## Datasets

### 1. Global Prosperity & Development Indicators (GPI)

**Source:** Legatum Prosperity Index (publicly available global development data)

| Feature | Description |
|---|---|
| `Country` | Country name (167 countries) |
| `AveragScore` | Overall prosperity score |
| `SafetySecurity` | Personal safety and national security |
| `PersonalFreedom` | Civil liberties and political rights |
| `Governance` | Institutional quality and rule of law |
| `SocialCapital` | Community cohesion and social networks |
| `InvestmentEnvironment` | Business climate and investment conditions |
| `EnterpriseConditions` | Entrepreneurship and market competition |
| `MarketAccessInfrastructure` | Trade access and physical infrastructure |
| `EconomicQuality` | Macroeconomic stability and growth |
| `LivingConditions` | Housing, utilities, material wellbeing |
| `Health` | Healthcare access and outcomes |
| `Education` | Education quality and attainment |
| `NaturalEnvironment` | Ecological health and sustainability |

**Dimensions:** 167 rows × 14 columns (13 numeric features + country name)  
**Missing values:** None

---

### 2. Seeds Dataset

**Source:** UCI Machine Learning Repository — Institute of Agrophysics, Polish Academy of Sciences

Measurements of geometrical properties of wheat kernels from three varieties (Kama=1, Rosa=2, Canadian=3), captured using non-destructive soft X-ray imaging.

| Feature | Description |
|---|---|
| `Area` | Kernel area (A) |
| `Perimeter` | Kernel perimeter (P) |
| `Compactness` | Compactness = 4πA/P² |
| `Length of kernel` | Length |
| `Width of kernel` | Width |
| `Asymmetry coefficient` | Degree of asymmetry |
| `Length of kernel groove` | Length of groove |
| `Variety` | Wheat type: 1=Kama, 2=Rosa, 3=Canadian |

**Dimensions:** 210 rows × 8 columns (7 numeric features + class label)  
**Balance:** Perfectly balanced — 70 observations per variety  
**Missing values:** None

---

## Methodology

### Part 1: Hierarchical Clustering (GPI)

1. **EDA** — Pairwise plots, boxplots, correlation heatmap; identified high correlations (>0.90) among prosperity sub-indices
2. **Cluster tendency** — Hopkins statistic confirmed clustering tendency
3. **Linkage comparison** — Evaluated single, complete, average, and Ward linkage via dendrograms
4. **Optimal clusters** — NbClust (30 indices), Elbow, Silhouette, and Gap statistic methods
5. **Validation** — Cophenetic correlation, Silhouette scores, Dunn Index, clValid stability (APN, AD, ADM, FOM)
6. **Result** — Average linkage with **k=3** selected (Cophenetic r = 0.713); clusters mapped to a world map

**Cluster Interpretation:**
| Cluster | Label | Characteristics |
|---|---|---|
| 1 | Developed | High scores across all indices; e.g., Denmark, Sweden, Norway |
| 2 | Developing | Mid-range scores; e.g., Brazil, China, Mexico |
| 3 | Underdeveloped | Low scores across indices; e.g., Chad, Niger, Yemen |

---

### Part 2: Partitional Clustering (Seeds)

1. **EDA** — Histograms, boxplots, correlation matrix; Area/Perimeter/Length/Width highly intercorrelated; Asymmetry coefficient weakly correlated with all others
2. **Scaling** — All 7 features standardized (z-score) prior to clustering
3. **Cluster tendency** — Hopkins statistic applied
4. **Methods compared:**
   - **K-means** — Standard random initialization, 25 restarts
   - **K-means++** — Improved initialization (spread-out seeds) for better convergence
   - **K-medoids (PAM)** — Actual data points as centers; more robust to outliers
5. **Optimal k** — k=3 assumed from known dataset structure; validated with Silhouette and Elbow
6. **Validation** — Average silhouette width; clValid stability analysis

**Results Summary:**
| Method | Avg. Silhouette Width | Between-SS / Total-SS |
|---|---|---|
| K-means | 0.40 | 70.7% |
| K-means++ | 0.40 | 70.7% |
| K-medoids | 0.39 | — |

**Best Method:** K-means++ — best balance of efficiency, stability, and convergence reliability.

---

## Key Findings

**GPI (Hierarchical Clustering):**
- Average linkage outperformed single, complete, and Ward linkage for this dataset
- Cophenetic correlation of 0.713 confirms a good tree-to-data fit
- 7 countries showed negative silhouette scores (potential outliers or borderline cases)
- Dunn Index of 0.204 indicates moderate cluster separation
- World map visualization clearly shows geographic clustering aligned with known global development patterns

**Seeds (Partitional Clustering):**
- K-means and K-means++ produced identical clustering (silhouette = 0.40), confirming a stable dataset structure
- K-medoids slightly lower (0.39) but more robust to the outliers in the Asymmetry coefficient
- All three methods successfully recovered the three wheat variety groups (~71% variance explained)
- Asymmetry coefficient was the most challenging feature due to its right-skewed distribution and high-end outliers

---

## How to Run

### Prerequisites

- R (version 4.0+) and RStudio

### Steps

1. Clone the repository:
   ```bash
   git clone https://github.com/your-username/cluster-analysis-seeds-gpi.git
   cd cluster-analysis-seeds-gpi
   ```

2. Open either `.Rmd` file in RStudio from the `analysis/` folder.

3. Ensure datasets are in the `data/` folder. Update file paths in the RMD if needed:
   ```r
   # GPI analysis
   country <- read.csv("../data/global_prosperity_and_development_indicators.csv")

   # Seeds analysis
   Seeds <- read.csv("../data/seeds_dataset.txt", sep = "", header = FALSE, col.names = column_names)
   ```

4. Click **Knit → PDF** to render the full report.

---

## R Package Requirements

Install all required packages with:

```r
install.packages(c(
  # Shared
  "ggplot2", "cluster", "factoextra", "GGally", "dplyr", "NbClust", "fpc", "clValid",

  # GPI analysis
  "clustertend", "hopkins", "dendextend", "tibble",
  "igraph", "reshape2", "rnaturalearth", "sf", "stringr", "visdat", "gridExtra",

  # Seeds analysis
  "corrplot"
))
```

> **Note:** `rnaturalearth` and `sf` are required for the world map visualization in the GPI analysis. On some systems, `sf` may require additional system-level dependencies (GDAL, GEOS). See the [sf installation guide](https://r-spatial.github.io/sf/) if installation fails.

---

*DANA Course — Clustering Analysis Project, November 2024*
