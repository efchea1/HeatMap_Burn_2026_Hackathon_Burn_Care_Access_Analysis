# HeatMap Hackathon 2026 - Burn Care Access Analysis

> **Advancing Equitable Access to Burn Care in the United States**  
> National Injury Resource Database (NIRD) · BData / American Burn Association  

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

Burn care in the United States is critically uneven. Of the ~6,200 hospitals in the country, only 135 have designated burn centers, and fewer than 62% of those are verified by the American Burn Association (ABA). Meanwhile, an estimated **600,000 individuals** annually suffer a burn injury requiring emergent care (Ivanko et al., 2024), with the majority of that care occurring outside of specialized burn facilities.

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

### Telemedicine Opportunity
- **351** high-priority tele-burn deployment sites identified (score ≥ 5)
- Top tele-burn state: **Illinois** (52 candidate hospitals)

### Pediatric Access Gap
- **5 states** have adult burn centers but **no** pediatric burn capability: CT, KY, ME, VT, WV

### Burden & Impact *(Team 13 Modeled Estimate)*
- Incidence (Ivanko et al. 2024)            :   **600,000 burn patients/year**
- Under-referral rate (Huang et al. 2021)   :        **× 66%  ->  ~396,000 under-referred**
- Proportion meeting referral criteria      :        **× 17%  ->  ~67,000 patients affected**
- Excess hospital days (Murray et al. 2019) :        **× 6.9 days/patient**
- Cost per day (ABA benchmark)              :        **× $3,500/day**
- Estimated **~67,000 under-referred patients per year**
- Estimated **~$1.6 billion in avoidable annual costs**

> ⚠️ *Conservative modeled projection for illustrative purposes. Not a direct observational finding from the NIRD data.* - see *[References](#references)*

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
| **US Census Geocoder API** | *(live API call)* | Batch geocoding of burn center addresses -> coordinates; 118/136 centers auto-geocoded, 18 manually verified | 2020 |

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

---

## Team

**Team 13 - HeatMap Hackathon 2026**

| Name |
|---|
| Emmanuel Fle Chea |
| Josh Spitzer-Resnick |
| Shreya Pramanik |
| Feifei Li |
| Lance Killian McDonald |

---

## References

1. **Ivanko A, et al.** The Burden of Burns: An Analysis of Public Health Measures. *J Burn Care Res.* 2024. https://doi.org/10.1093/jbcr/irae053
2. **Lovick EA, et al.** Development of the National Injury Resource Database (NIRD). *Burns.* 2024;50(2):315–320. https://doi.org/10.1016/j.burns.2023.10.020
3. **Huang JF, et al.** Under-triage of burn injuries in the United States. *Burns.* 2021.
4. **Murray CK, et al.** Excess hospital days associated with burn transfer delays. *J Burn Care Res.* 2019.
5. **Carmichael H, et al.** Regional disparities in access to verified burn center care. *J Trauma Acute Care Surg.* 2019;87:111–6.
6. **Zonies D, et al.** Verified centers, nonverified centers, or other facilities: a national analysis. *J Am Coll Surg.* 2010;210:299–305.
7. **CDC/ATSDR Social Vulnerability Index 2022.** https://www.atsdr.cdc.gov/placeandhealth/svi/
8. **USDA Economic Research Service.** Rural-Urban Continuum Codes, 2023. https://www.ers.usda.gov/data-products/rural-urban-continuum-codes/
9. **US Census Bureau.** 2020 County Population Centroids. https://www.census.gov/geographies/reference-files/time-series/geo/centers-population.html
10. **US Census Geocoder.** Batch Address Geocoding API. https://geocoding.geo.census.gov/geocoder/

---

## License & Data Use

The analysis code in this repository is released for educational and research purposes. The underlying NIRD dataset is proprietary to BData, Inc. and subject to a Data Use Agreement. Please contact BData for data access inquiries.

---

*HeatMap Hackathon 2026 · American Burn Association · BData, Inc.*
