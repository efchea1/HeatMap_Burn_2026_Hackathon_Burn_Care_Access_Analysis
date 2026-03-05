# HeatMap Hackathon 2026 - Burn Care Access Analysis

> **Advancing Equitable Access to Burn Care in the United States**  
> National Injury Resource Database (NIRD) · BData / American Burn Association  
> **Team 13** · HeatMap Hackathon 2026

---

## 📋 Table of Contents

- [Overview](#overview)
- [Key Findings](#key-findings)
- [Repository Structure](#repository-structure)
- [Data Sources](#data-sources)
- [Analysis Roadmap](#analysis-roadmap)
- [Getting Started](#getting-started)
- [Results & Outputs](#results--outputs)
- [Team](#team)
- [References](#references) 

---

## Overview

Burn care in the United States is critically uneven. Of the ~6,200 hospitals in the country, only 135 have designated burn centers - and fewer than 62% of those are verified by the American Burn Association (ABA). Meanwhile, an estimated **600,000 individuals** annually suffer a burn injury requiring emergent care (Ivanko et al., 2024), with the majority of that care occurring outside of specialized burn facilities.

This project uses the **National Injury Resource Database (NIRD)**, the most comprehensive, up-to-date database of US burn and trauma centers, to quantify structural gaps in burn care access across all 50 states. We analyze equity disparities, referral network failures, telemedicine deployment opportunities, and pediatric access gaps, and model the downstream burden of under-referral.

**Primary Use Case:** Advancing equitable access to burn care  
**Mechanism Focus:** Referral gaps and telemedicine are analyzed as drivers of *inequity*, not as ends in themselves

---

## Key Findings

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

### 📡 Telemedicine Opportunity
- **351** high-priority tele-burn deployment sites identified (score ≥ 5)
- Top tele-burn state: **Illinois** (52 candidate hospitals)

### Pediatric Access Gap
- **5 states** have adult burn centers but **no** pediatric burn capability: CT, KY, ME, VT, WV

### Burden & Impact *(Team 13 Modeled Estimate)*
- Estimated **~67,000 under-referred patients per year**
- Estimated **~$1.6 billion in avoidable annual costs**
- *Conservative modeled projection based on published literature — see [References](#references)*

---

> **Note:** The primary NIRD database file (`NIRD 20230130 Database_Hackathon.xlsx`) is governed by a Data Use Agreement and is **not included** in this repository. See `BData_HeatMapHackathon_DUA_Summary.docx` for access information.

---

## Data Sources

| Dataset | File | Description | Year |
|---|---|---|---|
| **NIRD** (National Injury Resource Database) | `NIRD 20230130 Database_Hackathon.xlsx` *(DUA-restricted)* | 635 hospitals · burn & trauma centers · capabilities · bed counts · verification status | 2023 |
| **CDC/ATSDR Social Vulnerability Index** | `SVI_2022_US_county.csv` | County-level social vulnerability: poverty, no-vehicle households, limited English proficiency, disability, minority status (3,143 US counties) | 2022 |
| **USDA Rural-Urban Continuum Codes** | `Ruralurbancontinuumcodes2023.xlsx` | County-level rural/urban classification used to contextualize geographic access | 2023 |
| **US Census Bureau County Population Centroids** | `CenPop2020_Mean_CO.txt` | Population-weighted lat/lon centroids for 3,221 US counties; used for Haversine distance analysis | 2020 |
| **US Census Geocoder API** | *(live API call)* | Batch geocoding of burn center addresses → coordinates; 118/136 centers auto-geocoded, 18 manually verified | 2020 |

**Geocoding endpoint:** `https://geocoding.geo.census.gov/geocoder/locations/addressbatch`  
**Distance method:** Haversine great-circle formula (Earth radius: 3,958.8 mi); minimum distance from each county centroid to all burn centers nationally.

---

## Analysis Roadmap

| # | Analysis | Output |
|---|---|---|
| 1 | Data Loading & Cleaning | Normalized NIRD flags, composite burn/trauma indicators |
| 2 | Burn Center Density by State | Fig 1: Centers per million residents |
| 3 | Burn Bed Capacity per 100k Residents | Fig 2: Bed density by state |
| 4 | Referral Gap — Trauma Centers w/o Burn Capability | Fig 3: L1/L2 gap chart + pie |
| 5 | Telemedicine Opportunity Score | Fig 4: State-level tele-burn scoring |
| 6 | Pediatric vs. Adult Access Gap | Fig 5: Side-by-side center counts |
| 7 | ABA Verification Rate by State | Fig 6: Verification % bar chart |
| 8 | Equity Quadrant: Access vs. Quality | Fig 7: Bubble scatter by state |
| 9 | Population-Weighted Distance to Nearest Burn Center | Fig 7.5: Haversine distance (any & ABA-verified) |
| 10 | Social Vulnerability (SVI) Integration | Fig 8: Poverty × burn access scatter |
| 11 | Composite Vulnerability Index | Fig 9: 0–1 index, 4 risk tiers |
| 12 | Executive Dashboard | Fig 10: Multi-panel summary |
| 13 | Sensitivity Analysis - Index Robustness | Fig 15: 5 weighting scenarios, rank stability |
| 14 | Export Summary Excel Workbook | `NIRD_Analysis_Summary.xlsx` |

The **Composite Vulnerability Index** integrates burn bed density, ABA verification rate, SVI social vulnerability, and population-adjusted access into a single 0-1 score, validated across 5 weighting scenarios. All top-10 most vulnerable states remain in the top 15 across every scenario, confirming the index is methodologically robust.

---

## Getting Started

### Prerequisites
```bash
pip install pandas numpy matplotlib seaborn openpyxl requests
```

### Running the Analysis

1. **Clone the repository**
```bash
   git clone https://github.com/your-org/heatmap-burn-2026.git
   cd heatmap-burn-2026
```

2. **Add required data files** to the repo root:

   | File | Source |
   |---|---|
   | `NIRD 20230130 Database_Hackathon.xlsx` | Requires DUA approval = see `BData_HeatMapHackathon_DUA_Summary.docx` |
   | `CenPop2020_Mean_CO.txt` | [US Census County Population Centroids](https://www2.census.gov/geo/docs/reference/cenpop2020/county/) |
   | `SVI_2022_US_county.csv` | Included in repo |
   | `Ruralurbancontinuumcodes2023.xlsx` | Included in repo |

3. **Run the notebook**
```bash
   jupyter notebook Team13_HeatMap_Burn_2026_Hackathon.ipynb
```
   Or as a script:
```bash
   python Team13_HeatMap_Burn_2026_Hackathon.py
```

4. **Outputs** are saved automatically to `outputs/`

> ⚠️ The notebook makes a live call to the **US Census Geocoder API** to batch-geocode burn center addresses. An active internet connection is required for this step. 18 hospitals that fail auto-geocoding are patched with manually verified coordinates already embedded in the script.

---

## Results & Outputs

The Excel workbook (`outputs/NIRD_Analysis_Summary.xlsx`) contains five sheets:

| Sheet | Contents |
|---|---|
| `State_Summary` | All state-level metrics merged with vulnerability scores |
| `Vulnerability_Ranking` | States ranked by composite vulnerability index with Risk Tier (Low / Moderate / High / Critical) |
| `Tele_Candidates_Top50` | Top 50 hospitals by tele-burn deployment score |
| `Referral_Gap` | State-level Level I/II trauma centers lacking burn capability |
| `Priority_Recommendations` | Actionable recommendations for High/Critical states |

### Modeled Economic Impact
