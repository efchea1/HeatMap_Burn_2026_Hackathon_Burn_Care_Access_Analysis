# 🔥 HeatMap Hackathon 2026 - Burn Care Access Analysis

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

📦 heatmap-burn-2026/
├── 📓 Team13_HeatMap_Burn_2026_Hackathon.ipynb        # Main analysis notebook
├── 🐍 Team13_HeatMap_Burn_2026_Hackathon.py           # Python script version
├── 📊 Burn_NIRD_2023_Burns.pdf                        # NIRD source publication (Lovick et al.)
├── 📄 Burden_of_Burn_2024.pdf                         # Burn incidence reference (Ivanko et al.)
├── 📋 Data_Mapping_Document.docx                      # Data dictionary & field definitions
├── 📋 BData_HeatMapHackathon_DUA_Summary.docx         # Data use agreement summary
├── 📋 Use_Case_Publications.docx                      # Supporting literature
├── 🗂️ SVI_2022_US_county.csv                          # CDC/ATSDR Social Vulnerability Index
├── 🗂️ Ruralurbancontinuumcodes2023.xlsx               # USDA Rural-Urban Continuum Codes
└── 📁 outputs/
    ├── NIRD_Analysis_Summary.xlsx                     # Full results workbook (5 sheets)
    ├── KEY_FINDINGS_SUMMARY.md                        # Auto-generated summary
    ├── fig1_burn_density_by_state.png
    ├── fig2_burnbeds_per_100k.png
    ├── fig3_referral_gap.png
    ├── fig4_telemedicine_opportunity.png
    ├── fig5_pediatric_gap.png
    ├── fig6_aba_verification_rate.png
    ├── fig7_equity_quadrant.png
    ├── fig7_5_distance_analysis.png
    ├── fig8_svi_integration.png
    ├── fig9_vulnerability_index.png
    ├── fig10_executive_dashboard.png
    └── fig15_sensitivity_analysis.png

---

## Overview

Burn care in the United States is critically uneven. Of the ~6,200 hospitals in the country, only 135 have designated burn centers — and fewer than 62% of those are verified by the American Burn Association (ABA). Meanwhile, an estimated **600,000 individuals** annually suffer a burn injury requiring emergent care (Ivanko et al., 2024), with the majority of that care occurring outside of specialized burn facilities.

This project uses the **National Injury Resource Database (NIRD)** — the most comprehensive, up-to-date database of US burn and trauma centers — to quantify structural gaps in burn care access across all 50 states. We analyze equity disparities, referral network failures, telemedicine deployment opportunities, and pediatric access gaps, and model the downstream burden of under-referral.

**Primary Use Case:** Advancing equitable access to burn care  
**Mechanism Focus:** Referral gaps and telemedicine are analyzed as drivers of *inequity*, not as ends in themselves

---

## Key Findings

### 🏥 National Snapshot
| Metric | Value |
|---|---|
| Hospitals in dataset | 635 across 50 states |
| Adult burn centers | 120 |
| Pediatric burn centers | 89 |
| ABA-verified burn centers | 74 (62% of adult BCs) |
| States with **zero** adult burn centers | 7 — AK, DE, MS, MT, ND, NH, SD |

### ⚖️ Equity Gaps
- **Top 5 most vulnerable states:** MS, NH, MT, DE, SD
- **34 states** classified as Critical vulnerability tier
- Burn bed density ranges from **0 → 6.110 per 100k residents**

### 🚑 Referral Network Gaps
- **143 / 229 (62.4%)** Level I trauma centers lack burn capability
- **319 / 336 (94.9%)** Level II trauma centers lack burn capability
- **498 total** trauma centers represent potential referral bottlenecks

### 📡 Telemedicine Opportunity
- **351** high-priority tele-burn deployment sites identified (score ≥ 5)
- Top tele-burn state: **Illinois** (52 candidate hospitals)

### 👶 Pediatric Access Gap
- **5 states** have adult burn centers but **no** pediatric burn capability: CT, KY, ME, VT, WV

### 💰 Burden & Impact *(Team 13 Modeled Estimate)*
- Estimated **~67,000 under-referred patients per year**
- Estimated **~$1.6 billion in avoidable annual costs**
- *Conservative modeled projection based on published literature — see [References](#references)*

---

## Repository Structure
