# HeatMap Hackathon 2026 - Burn Care Access Analysis

> **Advancing Equitable Access to Burn Care in the United States**  
> National Injury Resource Database (NIRD) · BData / American Burn Association  

---

<div align="center">

## Team 13 - HeatMap Hackathon 2026

| Name |
|---|
| Emmanuel Fle Chea |
| Josh Spitzer-Resnick |
| Shreya Pramanik |
| Feifei Li |
| Lance Killian McDonald |

</div>

---

## Table of Contents
- **Overview**
- **Areas Team 13 Explored**
- **Key Findings**
- **Data Sources**
- **Analysis Roadmap**
- **Getting Started**
- **Excel workbook**
- **References**
- **License & Data Use**

---

<details>
<summary><h2>Overview</h2></summary>

Burn care in the United States is critically uneven. Of the ~6,200 hospitals in the country, only 135 have designated burn centers, and fewer than 62% of those are verified by the American Burn Association (ABA). Meanwhile, an estimated **600,000 individuals** annually suffer a burn injury requiring emergent care (Ivanko et al., 2024), with the majority of that care occurring outside of specialized burn facilities.

This project uses the **National Injury Resource Database (NIRD)**, the most comprehensive, up-to-date database of US burn and trauma centers, to quantify structural gaps in burn care access across all 50 states. We analyze equity disparities, referral network failures, telemedicine deployment opportunities, and pediatric access gaps, and model the downstream burden of under-referral.

**Primary Use Case:** Advancing equitable access to burn care  
**Mechanism Focus:** Referral gaps and telemedicine are analyzed as drivers of *inequity*, not as ends in themselves

</details>

---

<details>
<summary><h2>Areas Team 13 Explored</h2></summary>

The code contains 15 distinct analyses across 6 thematic domains:

**1. National Snapshot & Data Foundation**

Loaded 635 hospitals across all 50 states from NIRD 2023  
Cleaned and normalized 12 binary indicator columns (burn adult/peds, trauma L1/L2, ABA verified, state-designated, etc.)  
Built composite flags: HAS_BURN, HAS_TRAUMA, TRAUMA_NO_BURN, L1_NO_BURN, L2_NO_BURN  

**2. Geographic Access & Density (Figures 1, 2, 11)**

Fig 1: Burn center density per million residents by state, with ABA counts overlaid, zero-center states flagged  
Fig 2: Burn bed capacity per 100k residents by state  
Fig 11: Full choropleth heatmap, Composite Vulnerability Index side-by-side with burn centers per million, with AK/HI inset boxes  

**3. Distance Analysis (Figure 7.5)**

Geocoded all 136 burn centers via Census Geocoder batch API; manually verified 18 failures  
Loaded 3,221 county population-weighted centroids from Census 2020  
Computed Haversine great-circle distance from every U.S. county to the nearest burn center, both any burn center and ABA-verified specifically  
Aggregated to the population-weighted state-level mean distance  
Computed % of counties exceeding 100-mile and 200-mile thresholds  
Key finding coded directly: Hawaii 0 miles to any center → 2,385 miles to ABA-verified  

**4. Referral Network Gaps (Figures 3, 4, 10)**

Fig 3: Trauma centers without burn capability, stacked bar (top 30 states) + national pie chart  
Fig 4: Telemedicine opportunity score by state, weighted by trauma level and bed size  
Fig 10: Top 25 hospital-level tele-burn candidates ranked by opportunity score (name, state, county, beds, L1/L2 status, score)  

**5. Equity & Vulnerability (Figures 5, 6, 7, 8, 9, 12, 15)**

Fig 5: Pediatric vs. adult burn center access gap, top 25 states, identified 5 states with adult centers but zero pediatric capability  
Fig 6: ABA verification rate by state  
Fig 7: Equity quadrant, access vs. quality scatter plot, four-quadrant labeling of priority states  
Fig 8: SVI integration, burn access vs. poverty rate (EP_POV150), bubble = burn beds, color = ABA verification; using 5 CDC SVI variables (poverty, no vehicle, limited English, disability, minority status) across 3,143 counties  
Fig 9: Composite Vulnerability Index, Access (35%) + Quality (25%) + Capacity (25%) + Population (15%), four risk tiers: Critical/High/Moderate/Low  
Fig 12: Rural-Urban disparity, USDA RUCC 2023, 3,235 counties, 9-point scale aggregated to Urban/Mixed/Rural; burn center access, bed capacity, and access-vs-capacity scatter by class  
Fig 15: Sensitivity analysis, CVI tested across 5 weight scenarios; max rank shift computed; all top-10 states remain in top 15 regardless of weights  

**6. Burden & Impact (Figures 13, 14)**

Fig 13: Projected patient impact - under-referral model: 600k × 15% needing specialist × 66% under-referred × 6.9 excess days × $3,500/day = $1.623B  
Fig 14: Narrative arc visualization - THE PROBLEM -> THE GAP -> THE INEQUITY -> THE IMPACT -> THE FIX  

Output deliverables: 5-sheet Excel workbook (State Summary, Vulnerability Ranking, Tele Candidates Top 50, Referral Gap, Priority Recommendations)

</details>

---

<details>
<summary><h2>Key Findings</h2></summary>

### National Snapshot
| Metric | Value |
|---|---|
| Hospitals in dataset | 635 across 50 states |
| Adult burn centers | 120 |
| Pediatric burn centers | 89 |
| ABA-verified burn centers | 74 (62% of adult BCs) |
| States with **zero** adult burn centers | 7 - AK, DE, MS, MT, ND, NH, SD |

### Equity Gaps
- **Top 5 most vulnerable states:** MS, NH, MT, DE, SD  
- **34 states** classified as Critical vulnerability tier  
- Burn bed density ranges from **0 -> 6.110 per 100k residents**

### Referral Network Gaps
- **143 / 229 (62.4%)** Level I trauma centers lack burn capability  
- **319 / 336 (94.9%)** Level II trauma centers lack burn capability  
- **498 total** trauma centers represent potential referral bottlenecks

### Telemedicine Opportunity
- **351** high-priority tele-burn deployment sites  
- Top tele-burn state: **Illinois** (52 candidate hospitals)

### Pediatric Access Gap
- **5 states** have adult burn centers but **no** pediatric burn capability: CT, KY, ME, VT, WV

### BURDEN & IMPACT (Team 13 Modeled Estimate)
  - Using published estimates for under-referral (Huang et al., 2021), excess hospital days 
    (Murray et al., 2019), and national burn incidence (Ivanko et al., 2024), we model 
    ~67,000 under-referred patients per year and ~$1.6B in avoidable annual costs.
  - This is a conservative modeled projection illustrating the scale of the structural gap.
  - Based on: Huang et al. 2021 (66% under-referral rate), Murray et al. 2019 
    (6.9 excess hospital days), Ivanko et al. 2024 (600k/yr incidence), 
    $3,500/day cost assumption (ABA cost benchmark)

</details>

---

<details>
<summary><h2>Data Sources</h2></summary>

| Dataset | File | Description | Year |
|---|---|---|---|
| NIRD | `NIRD 20230130 Database_Hackathon.xlsx` | 635 hospitals; burn & trauma centers | 2023 |
| CDC SVI | `SVI_2022_US_county.csv` | 3,143 counties; 5 vulnerability domains | 2022 |
| USDA RUCC | `Ruralurbancontinuumcodes2023.xlsx` | Rural/urban classification | 2023 |
| Census Centroids | `CenPop2020_Mean_CO.txt` | 3,221 county population-weighted centroids | 2020 |
| Census Geocoder API | *(live)* | Batch geocoding of burn centers | 2020 |

</details>

---

<details>
<summary><h2>Analysis Roadmap</h2></summary>

| # | Analysis | Output |
|---|---|---|
| 1 | Data Loading & Cleaning | Composite indicators |
| 2 | Burn Center Density | Fig 1 |
| 3 | Burn Bed Capacity | Fig 2 |
| 4 | Referral Gap | Fig 3 |
| 5 | Telemedicine Score | Fig 4 |
| 6 | Pediatric Gap | Fig 5 |
| 7 | ABA Verification | Fig 6 |
| 8 | Equity Quadrant | Fig 7 |
| 9 | Distance Analysis | Fig 7.5 |
| 10 | SVI Integration | Fig 8 |
| 11 | Vulnerability Index | Fig 9 |
| 12 | Dashboard | Fig 10 |
| 13 | Sensitivity Analysis | Fig 15 |
| 14 | Excel Export | Summary workbook |

</details>

---

<details>
<summary><h2>Getting Started</h2></summary>

## Getting Started

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

### ⚠️ Data Access Notice
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

### Repository Structure
```
├── Team13_HeatMap_Burn_2026_Hackathon.ipynb  # Main analysis notebook
├── outputs/
│   ├── NIRD_Analysis_Summary.xlsx            # Full results workbook
│   ├── KEY_FINDINGS_SUMMARY.txt              # Key numbers summary
│   ├── fig1_burn_center_density.png          # All figures
│   └── ...
├── README.md
└── data/                                     # <- NOT included (DUA restricted)
```
Outputs save to outputs/.

</details>

---

<details>
<summary><h2>Excel workbook</h2></summary>

### The Excel workbook contains:

Sheet                       Contents
State_Summary               All state metrics
Vulnerability_Ranking      Composite index + tier
Tele_Candidates_Top50      Top tele-burn sites
Referral_Gap               L1/L2 without burn capability
Priority_Recommendations   Actionable guidance

</details>

---

<details>
<summary><h2>References</h2></summary>

1. **Ivanko A, et al.** The Burden of Burns: An Analysis of Public Health Measures. *J Burn Care Res.* 2024. https://doi.org/10.1093/jbcr/irae053

2. **Lovick EA, et al.** Development of the National Injury Resource Database (NIRD). *Burns.* 2024;50(2):315-320. https://doi.org/10.1016/j.burns.2023.10.020

3. **Huang JF, et al.** Under-triage of burn injuries in the United States. *Burns.* 2021.

4. **Murray CK, et al.** Excess hospital days are associated with burn transfer delays. *J Burn Care Res.* 2019.

5. **Carmichael H, et al.** Regional disparities in access to verified burn center care. *J Trauma Acute Care Surg.* 2019;87:111-6.

6. **Zonies D, et al.** Verified centers, nonverified centers, or other facilities: a national analysis. *J Am Coll Surg.* 2010;210:299-305.

7. **CDC/ATSDR Social Vulnerability Index 2022.** https://www.atsdr.cdc.gov/placeandhealth/svi/

8. **USDA Economic Research Service. Rural-Urban Continuum Codes, 2023.** https://www.ers.usda.gov/data-products/rural-urban-continuum-codes/

9. **US Census Bureau. 2020 County Population Centroids.** https://www.census.gov/geographies/reference-files/time-series/geo/centers-population.html

10. **US Census Geocoder.** Batch Address Geocoding API. https://geocoding.geo.census.gov/geocoder/

</details>

---

<details>
<summary><h2>License & Data Use</h2></summary>

The analysis code in this repository is released for educational and research purposes.  
The underlying NIRD dataset is proprietary to BData, Inc. and subject to a Data Use Agreement.  
Please contact BData for data access inquiries.

--- 
