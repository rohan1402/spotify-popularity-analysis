# What Makes a Song Popular on Spotify?
### A Statistical Analysis of Audio Features

**MSDS 16:954:567 – Statistical Models and Computing**  
Rutgers University · Group 16  
Rohan Pant · Pranav Immaneni

---

## Overview

This project investigates whether a track's measurable audio characteristics can predict its popularity on Spotify. Using a dataset of 97,625 tracks across 114 genres, we apply **Principal Component Analysis (PCA)** to address multicollinearity among audio features, followed by **Multiple Linear Regression** to model popularity.

**Central finding:** Audio features alone explain just 1.6% of popularity variation (R² = 0.016). Adding genre raises this to 38.2% (R² = 0.382) — genre is the dominant driver of popularity on Spotify, not how a song sounds.

---

## Dataset

**Source:** [Kaggle — Spotify Tracks Dataset](https://www.kaggle.com/datasets/maharshipandya/-spotify-tracks-dataset)

Download `spotify_tracks.csv` from Kaggle and place it at `data/spotify_tracks.csv` before running the scripts.

**Raw:** 114,000 tracks × 21 columns  
**After cleaning:** 97,625 tracks × 17 columns  
**Target variable:** `popularity` (Spotify score, 0–100)

**Key audio features:** danceability, energy, loudness, tempo, acousticness, speechiness, instrumentalness, liveness, valence  
**Control variables:** duration_ms, explicit, mode, track_genre (114 levels)

---

## Project Structure

```
spotifyproject/
├── data/                    # Place spotify_tracks.csv here (see above)
├── scripts/
│   ├── 01_data_import.Rmd   # Data cleaning and preprocessing
│   ├── 02_eda.Rmd           # Exploratory data analysis
│   ├── 03_feature_engineering.Rmd  # Outlier removal, standardization
│   ├── 04_pca.Rmd           # Principal Component Analysis
│   └── 05_regression.Rmd    # Multiple Linear Regression
├── outputs/
│   ├── eda/                 # EDA plots and correlation table
│   ├── pca/                 # Scree plot, loadings heatmap, biplot
│   └── regression/          # Diagnostic plots, genre effects, coefficients
└── README.md
```

---

## How to Run

Run scripts in order from the project root in RStudio (set working directory to project root):

```r
# Option 1: Knit each Rmd in RStudio
# Option 2: Render programmatically
rmarkdown::render("scripts/01_data_import.Rmd")
rmarkdown::render("scripts/02_eda.Rmd")
rmarkdown::render("scripts/03_feature_engineering.Rmd")
rmarkdown::render("scripts/04_pca.Rmd")
rmarkdown::render("scripts/05_regression.Rmd")
```

Each script loads its input from the `data/` folder and saves outputs to `outputs/`.

### Required R Packages

```r
install.packages(c("tidyverse", "skimr", "janitor", "ggcorrplot", "factoextra", "broom"))
```

---

## Key Results

| Model | R² | RMSE |
|-------|----|------|
| Audio features only (PC1 + PC2 + PC3 + controls) | 0.016 | 19.02 |
| Full model (audio + genre) | **0.382** | **15.09** |

**Principal Components retained (Kaiser criterion):**
| Component | Variance Explained | Interpretation |
|-----------|-------------------|----------------|
| PC1 | 31.1% | Acoustic vs. Electronic Intensity |
| PC2 | 16.1% | Danceability & Positive Valence |
| PC3 | 13.8% | Liveness & Speechiness |

**Top genres by popularity:** pop-film, k-pop, pop, electro, house  
**Bottom genres by popularity:** iranian, romance, detroit-techno, chicago-house, grindcore  
**Genre spread:** 52.7 popularity points between highest and lowest genre coefficients

---

## Tools

- **Language:** R 4.x
- **Key packages:** tidyverse, ggplot2, skimr, janitor, ggcorrplot, factoextra, broom
- **IDE:** RStudio
