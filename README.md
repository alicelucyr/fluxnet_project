# What controls terrestrial ecosystem responses to vapour pressure deficit?

### Investigating gross primary productivity predictability and sensitivity to VPD across Australian eddy-covariance flux sites

**MSci Biology research project — University of Bristol**
**Author:** Alice Rolfe
**Supervisor:** Martin De Kauwe

---

## Overview

Vapour pressure deficit (VPD) is an important driver of plant water stress and is expected to increase as the climate warms. High atmospheric dryness can reduce photosynthesis by increasing plant water loss and triggering stomatal closure, potentially reducing ecosystem productivity.

This project investigated **how terrestrial ecosystems respond to VPD**, using eddy-covariance observations from 20 Australian FLUXNET/OzFlux sites spanning a range of climates, vegetation types and environmental conditions.

I combined **machine learning, statistical modelling and VPD-response analysis** to investigate:

* how predictable gross primary productivity (GPP) is from environmental variables;
* how important VPD is for predicting GPP;
* how GPP sensitivity to VPD varies between ecosystems;
* which climatic factors are associated with differences in VPD sensitivity; and
* how disturbance, using the Calperum fire as a case study, alters ecosystem responses to VPD.

The analysis was conducted in **R** using eddy-covariance observations from 2001–2024.

---

## Why does VPD matter?

VPD describes the atmospheric demand for water. As VPD increases, the gradient driving water loss from leaves to the atmosphere becomes larger.

Plants can respond to high VPD by closing their stomata to reduce water loss. However, stomatal closure also restricts the uptake of CO₂ required for photosynthesis, potentially reducing **gross primary productivity (GPP)**.

As climate change increases atmospheric dryness, understanding how different ecosystems respond to VPD is therefore important for predicting future terrestrial carbon uptake.

<img width="527" height="308" alt="Screenshot 2026-09-04 at 12 16 01" src="https://github.com/user-attachments/assets/3d312bc5-7e7e-492c-bffe-4f316f710dae" />

*Conceptual overview of how increasing VPD can influence plant water loss, stomatal conductance, photosynthesis, and knock-on effects on wildfires and carbon sequestration.*

---

## Research questions

This project addressed four main questions:

### 1. How predictable is GPP across Australian ecosystems?

Can daily GPP be predicted from meteorological and water-availability variables, and does model performance vary between ecosystems?

### 2. How important is VPD for predicting GPP?

Does including VPD improve predictions of ecosystem productivity, and does its importance vary between sites?

### 3. How does ecosystem sensitivity to VPD vary across Australia?

Do ecosystems differ in the VPD threshold at which GPP begins to decline, and in the strength of that decline?

### 4. What drives differences in VPD sensitivity?

Are differences in ecosystem responses associated with climate, precipitation seasonality, vegetation type or disturbance history?

---

# Study design

## Eddy-covariance sites

The analysis used **20 Australian eddy-covariance sites** from the FLUXNET/OzFlux network.

The sites covered a broad environmental gradient, including:

* tropical and temperate forests;
* savannas;
* grasslands;
* woodlands; and
* arid and semi-arid ecosystems.

Data covered the period **2001–2024**, where available.

<img width="540" height="406" alt="Screenshot 2026-09-04 at 12 16 46" src="https://github.com/user-attachments/assets/77c66fc6-01ab-4f52-babc-78faca359c03" />

*Locations of the 20 Australian eddy-covariance sites used in this study, scaled by mean annual precipitation (MAP mm / year) across each sites measurement period.*

---

## Data

The primary data source was the **FLUXNET2015/OzFlux eddy-covariance dataset**, with derived data provided through FluxDataKit.

The analysis used:

* **GPP** — `GPP_NT_VUT_REF`
* VPD
* air temperature (`Tair`)
* shortwave radiation (`SW_rad`)
* net radiation (`Rn`)
* atmospheric pressure (`Pa`)
* precipitation
* latent heat (`LE`)
* evapotranspiration (`ET`)
* potential evapotranspiration (`PET`)
* cumulative water deficit (`CWD`)
* MODIS leaf area index (`LAI`)

Raw FLUXNET/OzFlux data are **not stored in this repository**.

---

# Analysis workflow

The project was organised into five main analytical stages:

```text
Raw FLUXNET/OzFlux data
          │
          ▼
01 — Data cleaning & preparation
          │
          ▼
02 — GPP–VPD sensitivity analysis
          │
          ├───────────────┐
          ▼               ▼
03 — Random Forest     04 — Calperum
    predictability          disturbance
          │               │
          └───────┬───────┘
                  ▼
05 — Cluster analysis /
    synthesis
```

---

# Results

## 01 — GPP predictability

Random Forest models were used to predict daily GPP from environmental conditions.

The models included:

* air temperature;
* shortwave radiation;
* VPD; and
* 14-day cumulative water deficit (CWD) aligned to the right.

A **leave-one-year-out (LOYO) cross-validation** approach was used so that each year was independently withheld for testing.

Each Random Forest contained **500 trees**, with `mtry = 3`.

Across sites, model performance varied substantially:

<img width="419" height="237" alt="Screenshot 2026-09-04 at 12 18 39" src="https://github.com/user-attachments/assets/064a037f-7ae0-496f-ad26-c8a02854f2d0" />

*Model coefficient of determination (R-squared) across sites ordered by mean annual precipitation. Dashed line represents median R-squared (0.43).*

* test-year R² ranged from approximately **0.05–0.78**;
* mean R² was approximately **0.41**;
* median R² was **0.43**.

Model performance was not significantly related to record length, suggesting that the differences between sites were not simply a consequence of having longer datasets.

<img width="464" height="293" alt="Screenshot 2026-09-04 at 12 18 58" src="https://github.com/user-attachments/assets/d19a17df-d326-45fb-a26c-d398444dbaf4" />

*Relationship between site record length (number of years) and random forest model R² across flux sites. No significant relationship was found (p = 0.812, R² = 0.003), indicating that record length alone does not predict GPP model performance.*

Including VPD generally improved model performance, with an average increase in R² of approximately **0.08**, although its contribution varied between sites.

<img width="415" height="239" alt="Screenshot 2026-09-04 at 12 19 22" src="https://github.com/user-attachments/assets/8156f08c-72fa-4b2f-a5ea-72dfada4509b" />

*Change in R² when VPD was added as a predictor to the model, dashed line represents mean change in R² (0.08).*

---

## 02 — Sensitivity of GPP to VPD

To quantify ecosystem sensitivity to atmospheric dryness, GPP was normalised by site maximum GPP and grouped into **0.2 kPa VPD bins**.

The **95th percentile GPP envelope** was calculated for each VPD bin to represent the upper potential productivity under increasing atmospheric dryness.

Two characteristics of the response were extracted:

### VPDpeak

The VPD at which the upper GPP envelope reached its maximum.

This represents the approximate VPD threshold beyond which GPP began to decline.

### VPD sensitivity slope

The slope of the descending limb of the GPP–VPD relationship following VPDpeak.

More negative values represent a stronger decline in GPP with increasing VPD.

<img width="317" height="235" alt="Screenshot 2026-09-04 at 12 20 11" src="https://github.com/user-attachments/assets/d3374a6e-667c-4ced-815c-159dd28c8ef3" />

*Sensitivity curve for Fogg Dam showing binned 95th percentile GPP (blue points), fitted slope on the descending limb (blue line), and the VPD threshold (dashed line).*

---

## 03 — What explains differences in VPD sensitivity?

VPD sensitivity varied considerably between ecosystems.

The descending GPP–VPD slopes ranged from approximately:

**−0.68 to −0.087**

with a mean of approximately **−0.26**.

The relationship between the VPD slope and **mean annual temperature (MAT)** was particularly strong:

**Spearman's ρ = −0.79, p < 0.001**

This indicates that warmer and summer-dominant precipitation regime ecosystems tended to show steeper declines in GPP at high VPD.

In contrast, relationships with mean annual precipitation (MAP) and mean annual light (MAL) were comparatively weak. Moreover, relationships between MAP, MAL and MAT with the VPDpeak were non-significant. 

<img width="507" height="244" alt="Screenshot 2026-09-04 at 12 22 06" src="https://github.com/user-attachments/assets/1c9b0c70-7e0a-49ee-ba9b-535f9700ec83" />

*Relationships between the GPP-VPD slope and MAT, MAP and MAL. Spearman's rank correlation coefficient is shown in the top right, with an Asterix indicating a significant relationship. Colours indicate precipitation regime and the black line indicates the significant MAT-slope relationship.*

---

## 04 — Ecosystem sensitivity clusters

Sites were grouped according to their combination of:

* VPD sensitivity slope; and
* VPDpeak.

The two variables were standardised and clustered using **k-means clustering**.

An elbow analysis was used to evaluate cluster number, with **three clusters** selected for the final analysis.

The resulting groups were classified as:

| Cluster                | Interpretation                             | Sites  |
| ---------------------- | ------------------------------------------ | ------ |
| **Low sensitivity**    | Relatively weak GPP decline under high VPD | n = 15 |
| **Medium sensitivity** | Intermediate response                      | n = 2  |
| **High sensitivity**   | Stronger GPP decline under high VPD        | n = 3  |

The high- and medium-sensitivity groups were composed entirely of non-forest ecosystems.

High sensitivity sites were concentrated in wetter, tropical non-forested ecosystems. Interestingly, the tropical Northern Territory sites included both highly and weakly VPD-sensitive ecosystems despite experiencing broadly similar climatic and precipitation conditions. This suggests that **ecosystem-level differences in plant hydraulic strategies and access to water may be important in determining VPD sensitivity**.

<img width="547" height="539" alt="Screenshot 2026-09-04 at 12 24 55" src="https://github.com/user-attachments/assets/83236b6a-2135-4166-bbb2-28f2131ba364" />

*Distribution of sensitivity clusters across Australian Köppen climate zones. (c) Tropical Northern Territory sites exhibiting contrasting high, medium and low sensitivity. (d) Low-sensitivity sites across southeastern Australia.*

---

# Calperum disturbance case study

The Calperum site provided an opportunity to investigate how ecosystem disturbance alters the relationship between environmental drivers and GPP. According to Sun & Marschner 2024, there was a large fire at this site in 2014. 

The record was divided into three periods:

1. **2010–2013 — pre-fire**
2. **2014–2019 — fire and immediate recovery**
3. **2020–2024 — post-recovery**

Random Forest models were independently fitted to each period.

Model performance was particularly poor across the full Calperum record, but improved substantially when the post-recovery period was considered separately.

<img width="398" height="253" alt="Screenshot 2026-09-04 at 12 29 44" src="https://github.com/user-attachments/assets/922d92e6-7365-47a1-9ff9-daf1fa151d49" />

*Observed (grey) and predicted (coloured) GPP at Calperum across the three disturbance periods with R² and normalised root mean square error (nRMSE) from models fitted independently to each period. Full record R² = 0.064 and nRMSE = 0.597.*

The importance of VPD also changed:

* **Pre-fire:** ΔR² ≈ 0.103
* **Fire/recovery:** ΔR² ≈ 0.013
* **Post-recovery:** ΔR² ≈ 0.007

The VPD response also changed following disturbance, with a lower VPDpeak and a flatter post-disturbance response.

<img width="401" height="232" alt="Screenshot 2026-09-04 at 12 30 07" src="https://github.com/user-attachments/assets/a2ad7e90-c702-4c3e-82be-55b28f74a021" />

*GPP-VPD slope and VPDpeak across Calperum disturbance periods; large points show the binned 95th percentile GPP used to fit the upper envelope, smaller points show raw daily values. Solid lines represent the slope and dashed vertical lines show the VPDpeak.*

These results highlight how **disturbance and ecosystem recovery can alter the relationships between environmental drivers and productivity**, potentially making long-term ecosystem responses more difficult to predict using stationary models.

---

# Key conclusions

### VPD sensitivity varies substantially between ecosystems

Australian ecosystems showed large differences in both the VPD threshold at which GPP began to decline and the strength of that decline.

### Climate and precipitation timing partly explain ecosystem responses to VPD

Warmer, summer wet season ecosystems tended to show stronger sensitivity to high VPD. This is could be attributed to the wet season coinciding with periods of the highest annual VPD, therefore, high sensitivity to VPD through isohydric strategies such as stomatal closure are appropriate during infrequent low rainfall/high VPD periods. 

In contrast, at winter wet season sites, plant species must tolerate a long, dry summer with high VPD, relying on other hydraulic mechanisms to conserve water.

### VPD and water availability are closely connected

In arid and semi-arid ecosystems, VPD can be strongly coupled with soil moisture and water limitation, making it difficult to separate their individual effects.

### Disturbance can change ecosystem–environment relationships

The Calperum case study demonstrates that fire and subsequent vegetation recovery can alter both GPP predictability and the apparent importance of VPD.

### Ecosystem vulnerability to increasing atmospheric dryness is uneven

The results suggest that rising VPD will not affect all ecosystems equally. Improving predictions of future carbon uptake will require models that account for **climate, water availability, plant hydraulic strategies and site disturbance history**.

---

# Reproducibility

The analysis was developed in **R 4.5.2** using RStudio.

The repository contains the R scripts used for data preparation, analysis and figure generation.

Because the underlying FLUXNET/OzFlux datasets are not redistributed within this repository, reproducing the complete analysis requires obtaining the relevant source data separately and placing them into the expected input structure.

The scripts are organised sequentially so that the workflow can be followed from data preparation through to the final analyses.

---

# Data sources

The project uses data derived from:

* **FLUXNET2015**
* **OzFlux**
* **FluxDataKit**
* **MODIS MCD15A3H LAI**

Raw datasets are not included in this repository.

Please refer to the original data providers and associated data-use conditions before redistributing or reusing the underlying datasets.

---

# Research outputs

This repository accompanies my **University of Bristol MSci Biology research project**:

> **What controls terrestrial ecosystem responses to vapour pressure deficit (VPD)? Investigating gross primary productivity (GPP) predictability and sensitivity to VPD across Australian eddy flux sites**

The project was awarded **First Class** marks and received the **Best Poster Award** at the University of Bristol Biology MSci research conference.

---

## Author

**Alice Rolfe**
Biology MSci — University of Bristol

[GitHub](https://github.com/alicelucyr)





