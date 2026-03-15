# Advancing Equitable Access to Burn Care
## HeatMap Hackathon 2026 · Team 13

**Primary Use Case:** Use Case #3 - Advancing Equitable Access to Burn Care  

------

## Team members

| Name |
|---|
| Emmanuel Fle Chea |
| Josh Spitzer-Resnick |
| Shreya Pramanik |
| Feifei Li |
| Lance Killian McDonald |

--- 

## Sponsors
**Data & Clinical Sponsors**
- BData Inc.
- American Burn Association (ABA)

**Convening Sponsors**
- Carlson School of Management - Business Advancement Center for Health (BACH)
- HealthcareMN
- Institute for Research in Statistics and its Applications (IRSA)
- MinneAnalytics

**Award Sponsors**
- Idea Fund
- Eide Bailly

---

## Table of Contents
- **Project Summary**
- **Repository Structure**
- **Data Sources**
- **Getting Started**
- **Methods Overview**
- **Key Findings**
- **Analysis Roadmap**
- **References**
- **License & Data Use**

---

<details>
<summary><h2>Project Summary</h2></summary>

Every year, ~600,000 Americans require emergency burn care. Only 120 adult burn centers serve all 50 states, and 7 states have zero adult burn centers (AK, DE, MS, MT, ND, NH, SD). Note: Wyoming is absent from the NIRD 2023 working dataset and is therefore not included in state-level analyses; available evidence from Lovick et al. 2024 indicates Wyoming has no burn centers. Using the NIRD 2023 database combined with CDC SVI (2022), USDA RUCC (2023), and US Census population-weighted county centroids, Team 13 built a multi-layer geospatial equity analysis to identify where burn care fails, who bears the highest burden, and which targeted interventions deliver the greatest impact.

**Key result:** Three combined interventions could reach **14,514 additional patients/year**, prevent **~1,015 infections**, and save **~$24M annually** in infection costs (from a modeled national burden of ~$112M/yr).

</details>

---

<details>
<summary><h2>Repository Structure</h2></summary>

```
├── Team13_HeatMap_Burn_2026_Hackathon.ipynb   # Main analysis notebook
├── outputs/
│   ├── fig1_burn_density_by_state.png
│   ├── fig2_burnbeds_per_100k.png
│   ├── fig3_referral_gap.png
│   ├── fig4_telemedicine_opportunity.png
│   ├── fig5_pediatric_gap.png
│   ├── fig6_aba_verification_rate.png
│   ├── fig7_5_distance_analysis.png
│   ├── fig7_equity_quadrant.png
│   ├── fig8_svi_access_poverty.png
│   ├── fig9_vulnerability_index.png
│   ├── fig10_tele_candidates_table.png
│   ├── fig11_choropleth_heatmap.png
│   ├── fig12_rural_urban_disparity.png
│   ├── fig13_patient_impact.png
│   ├── fig14_narrative_arc.png
│   ├── fig15_sensitivity_analysis.png
│   ├── fig16_demographic_disparity.png
│   ├── fig17_deployment_impact.png
│   └── NIRD_Analysis_Summary.xlsx
├── KEY_FINDINGS_SUMMARY.txt
└── README.md
```

</details>

---

<details>
<summary><h2>Data Sources</h2></summary>

| Dataset | Source | Year | Usage |
|---------|--------|------|-------|
| NIRD Database | BData / American Burn Association | 2023 | Hospital-level burn center, trauma, bed data |
| CDC/ATSDR Social Vulnerability Index | CDC | 2022 | County-level SES, mobility, demographic vulnerability |
| Rural-Urban Continuum Codes | USDA ERS | 2023 | County rural/urban classification |
| County Population Centroids | US Census Bureau | 2020 | Population-weighted distance computation |
| Census Geocoder API | *(live)* | 2020 | Batch geocoding of burn centers |

> **Note:** The NIRD database (`NIRD 20230130 Database_Hackathon.xlsx`) was provided by hackathon organizers and are not redistributed here. All other datasets are in the dataset folder.


</details>

---

<details>
<summary><h2>Getting Started</h2></summary>

### Prerequisites
```bash
pip install pandas numpy matplotlib seaborn openpyxl requests
```

### Libraries Used
```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import matplotlib.patches as mpatches
import seaborn as sns
import warnings, os
import openpyxl, statistics
from collections import defaultdict
import requests
import math
```

### Clone the Repository
```bash
git clone https://github.com/efchea1/HeatMap_Burn_2026_Hackathon_Burn_Care_Access_Analysis.git
cd HeatMap_Burn_2026_Hackathon_Burn_Care_Access_Analysis
```

### Data Access Notice
The primary dataset (NIRD 2023, provided by BData / American Burn Association) 
is governed by a Data Use Agreement and **cannot be redistributed publicly**.

To run this notebook, you must have been granted access to the following files
and place them in the project root directory:

| File | Description |
|---|---|
| `NIRD_2023.csv` (NIRD 2023 Database or equivalent) plus all the available public datasets listed in the **Data Sources**. | National Injury Resource Database |

If you are a hackathon judge or authorized reviewer and need data access,
please contact the team or BData directly.

All pre-computed **outputs** (figures, Excel workbook, summary files) are 
included in the `outputs/` folder and can be reviewed without re-running 
the notebook.

### Running the Analysis
Once data files are in place:
```bash
jupyter notebook Team13_HeatMap_Burn_2026_Hackathon.ipynb
```
Run all cells top to bottom. Outputs save automatically to `outputs/`.

### Variables Used:
Burn center location · ABA verification status · ABA verification rate (% by state) · Hospital trauma level · Hospital bed size · Burn bed count · Haversine distance to nearest burn center · Haversine distance to nearest ABA-verified center · Burn centers per million residents · Burn beds per 100k residents · State population · Burn incidence rate (per 100k) · EP_POV150 (poverty) · EP_NOVEH (no vehicle) · EP_MINRTY (minority status) · EP_DISABL (disability) · EP_LIMENG (limited English) · RUCC rural-urban classification · County population centroids

</details>

---

<details>
<summary><h2>Methods Overview</h2></summary>

### Geocoding Pipeline
- 136 burn center locations geocoded via **Census Geocoder Batch API** (118 auto-geocoded; 18 manually verified)
- **Haversine great-circle distance** computed from 3,221 county population-weighted centroids to every burn center
- Two metrics: distance to *any* burn center and distance to *ABA-verified* center only

### Composite Vulnerability Index (CVI)
Min-max normalized, inverted where applicable:

| Dimension | Weight | Variable |
|-----------|--------|----------|
| Access | 35% | Burn centers per million residents (inverted) |
| Quality | 25% | % ABA-verified centers (inverted) |
| Capacity | 25% | Burn beds per 100k residents (inverted) |
| Population burden | 15% | State population (direct) |

**Thresholds:** Critical > 0.65 · High > 0.50 · Moderate > 0.35 · Low ≤ 0.35  
**Validation:** Sensitivity analysis across 5 weight schemes; max rank shift = 15 (robust)

### Telemedicine Opportunity Score
Trauma hospitals without burn capability scored by:
- Base: +3.0 (qualifying criterion)
- Level I designation: +2.0
- Level II designation: +1.5
- Bed size ≥ 25th percentile: +0.5; ≥ 75th percentile: +0.5

### Impact Model (Floor Estimate)
```
Under-referred patients = 600,000 × 15% × 66%
Avoidable infections    = under-referred × 7% excess rate
Avoidable cost          = infections × $24,000/infection
```
**Sources:** Under-referral rate (66%, Huang et al. 2021) represents a conservative ceiling; four independent U.S. and global studies report rates of 48-67% (Carter 2010, Davis 2012, Zonies 2010, Bazzi 2022).

</details>

---

<details>
<summary><h2>Key Findings</h2></summary>

### National Snapshot
- **120** adult burn centers; **74** ABA-verified (62%)
- **7 states** with zero adult burn centers: AK, DE, MS, MT, ND, NH, SD
- **34 states** in Critical vulnerability tier (CVI > 0.65)
- **88.1%** of trauma centers (498/565) lack burn capability

### Distance Burden (Selected States)
| State | Distance to Any Center | Distance to ABA-Verified |
|-------|----------------------|--------------------------|
| Alaska | 1,427 mi | 1,427 mi |
| North Dakota | 336 mi | 341 mi |
| Montana | 260 mi | 397 mi |
| South Dakota | 220 mi | 220 mi |

### Equity Disparities
- High-poverty counties: **2.1×** farther to any burn center
- Minority-status counties: **0.4×** relative access to ABA-verified centers
- ABA-verified access is **worse** than any-center access in **every single state**

### Projected Impact of Interventions
| Scenario | Patients/yr | Infections Avoided | Annual Savings |
|----------|------------|-------------------|----------------|
| S1: Tele-Burn Hubs (IL+CA+TX) | 10,611 | 742 | $18M |
| S2: ABA Verification Expansion (SC+AL+AR+KY) | 1,432 | 100 | $2M |
| S3: New Centers in 7 zero-center states | 2,182 | 152 | $4M |
| **S4: All Three Combined** | **14,514** | **1,015** | **$24M** |



> **Note:** Detail findings (`KEY_FINDINGS_SUMMARY.txt`), results(`NIRD_Analysis_Summarycsv`), and all 19 figures are in the `Outputs` folder.

</details>

---

<details>
<summary><h2>Analysis Roadmap</h2></summary>

## Analysis Index

| # | Analysis | Output |
|---|---|---|
| 1 | Data Loading & Cleaning | Composite indicators |
| 2 | Text Export | Key finding summary |
| 3 | Excel Export | Summary workbook |

## Figures Index

| Figure | Description |
|--------|-------------|
| Fig 1 | Burn center density by state (per million residents) |
| Fig 2 | Burn bed capacity per 100k residents by state |
| Fig 3 | Referral gap: trauma centers without burn capability |
| Fig 4 | State-level telemedicine opportunity score |
| Fig 5 | Pediatric vs. adult burn center access gap |
| Fig 6 | ABA verification rate by state |
| Fig 7 | Equity quadrant: access vs. quality by state |
| Fig 7.5 | Population-weighted distance to nearest burn center |
| Fig 8 | Burn access vs. socioeconomic vulnerability (SVI) |
| Fig 9 | Composite Vulnerability Index by state |
| Fig 10 | Top 25 telemedicine hub candidates (hospital level) |
| Fig 11 | HeatMap choropleth: vulnerability index + access |
| Fig 12 | Rural-urban burn care access disparity |
| Fig 13 | Projected patient impact of under-referral |
| Fig 14 | Narrative arc: The Burn Care Access Crisis in America |
| Fig 15 | Vulnerability Index sensitivity analysis |
| Fig 16 | Demographic disparity: who bears the burden |
| Fig 17 | Projected impact of each deployment scenario |

---
Interactive Figs:https://mcdonaldlk.github.io/HeatmapHackathon/
---

## Team Contributions

### Feifei Li
- Interactive maps visualizing burn center access (see `code` folder)
- Alternative presentation slides focusing on visual storytelling (see `ppt` folder)
- Built on team's distance analysis to create judge-friendly visualizations

---

<details>
<summary><h2>References</h2></summary>

1. Ivanko et al. (2024). Burden of Burns: Comparative analysis of databases. J Burn Care Res. DOI: 10.1093/jbcr/irae053
2. Lovick et al. (2024). National Injury Resource Database. Burns. DOI: 10.1016/j.burns.2023.10.020
3. Huang et al. (2021). Under-triage of burn injuries in the U.S. Burns.
4. Huang et al. (2021). Burn center referral practice evaluation. Burns (statewide IL study; Table 2 infection rates).
5. Carmichael et al. (2019). Regional disparities in access to verified burn center care. J Trauma Acute Care Surg. 87:111-6.
6. Zonies et al. (2010). Verified vs. non-verified centers national analysis. J Am Coll Surg. 210:299-305.
7. CDC/ATSDR Social Vulnerability Index 2022. [https://www.atsdr.cdc.gov/placeandhealth/svi/](https://www.atsdr.cdc.gov/place-health/php/svi/svi-data-documentation-download.html)
8. USDA ERS Rural-Urban Continuum Codes 2023. https://www.ers.usda.gov/data-products/rural-urban-continuum-codes/
9. US Census Bureau County Population Centroids 2020. https://www.census.gov/geographies/reference-files/time-series/geo/centers-population.html
10. Carter JE, Neff LP, Holmes JH. Adherence to burn center referral criteria: are patients appropriately being referred? J Burn Care Res. 2010;31(1):26–30.
11. Davis JS, Dearwater S, Rosales O, et al. Tracking non-burn center care: what you don't know may surprise you. J Burn Care Res. 2012;33(6):e263–7.
12. Bazzi A, Ghazanfari MJ, Norouzi M, et al. Adherence to referral criteria for burn patients; a systematic review. Arch Acad Emerg Med. 2022;10(1):e43.
13. Saffle JR, Edelman L, Theurer L, Morris SE, Cochran A. Telemedicine evaluation of acute burns is accurate and cost-effective. J Trauma. 2009;67(2):358–365. doi: 10.1097/TA.0b013e3181ae9b02.
14. Garber RN, Garcia E, Goodwin CW, Deeter LA. Pictures do influence the decision to transfer: outcomes of a telemedicine program serving an eight-state rural population. J Burn Care Res. 2020;41(3):690–694. doi: 10.1093/jbcr/iraa017.
15. Park C, Cho Y, Harvey J, Arnoldo B, Levi B. Telehealth and burn care: from faxes to augmented reality. Bioengineering (Basel). 2022;9(5):211. doi: 10.3390/bioengineering9050211.

</details>

---

<details>
<summary><h2>License & Data Use</h2></summary>

The analysis code in this repository is released for educational and research purposes.  
The underlying NIRD dataset is proprietary to BData, Inc. and subject to a Data Use Agreement.  
Please contact BData for data access inquiries.

</details>

--- 

*Team 13 · HeatMap Hackathon 2026 · Primary Use Case #3: Advancing Equitable Access to Burn Care*

*Where you live should not determine whether you survive 
a burn injury. But today, it does*
