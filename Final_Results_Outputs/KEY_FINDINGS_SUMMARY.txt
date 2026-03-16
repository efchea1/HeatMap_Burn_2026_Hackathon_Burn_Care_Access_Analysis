=================================================================
  KEY FINDINGS SUMMARY - HeatMap Hackathon 2026 · Team 13
  Primary Use Case: Advancing Equitable Access to Burn Care
=================================================================

NATIONAL SNAPSHOT
  - 635 hospitals analyzed across 50 states
  - 120 adult burn centers  |  89 pediatric burn centers
  - 74 ABA-verified burn centers (62% of adult BCs)
  - 7 states with ZERO adult burn centers: AK, DE, MS, MT, ND, NH, SD
  - Wyoming: absent from NIRD 2023 working dataset

EQUITY GAPS
  - Top 5 most vulnerable states: MS, NH, MT, DE, SD
  - 34 states classified as Critical vulnerability tier (CVI > 0.65)
  - Burn bed density ranges from 0 -> 6.11 per 100k residents

DISTANCE BURDEN (Team 13 Original Geocoding Pipeline)
  - 136 burn centers geocoded | 3,221 county population-weighted centroids
  - Method: Haversine great-circle distance, every county -> nearest burn center
  - AK: 1426.8 mi population-weighted avg | 100.0% of counties exceed 100 miles
  - PR: N/A | U.S. territory with zero burn centers
  - HI: 47.3 mi to any center | 2384.7 mi to ABA-verified only
  - ND: 335.6 mi | 100.0% of counties exceed 100 miles
  - MT: 260.2 mi (any center) / 397.4 mi (ABA-verified)
  - SD: 220.3 mi | 100.0% of counties exceed 100 miles
  - MS: 93.3 mi | zero centers; all patients cross state lines
  - KEY FINDING: ABA-verified access is worse than any-center access
    in every single state. Presence does not equal quality.

REFERRAL NETWORK GAPS
  - 143/229 (62.4%) Level I trauma centers lack burn capability
  - 319/336 (94.9%) Level II trauma centers lack burn capability
  - 498/565 (88.1%) of ALL trauma centers lack burn capability
  - 498 total trauma centers = potential referral bottlenecks

TELEMEDICINE OPPORTUNITY
  - 351 high-priority tele-burn sites identified (score >= 5)
  - Top 3 states by total candidates:    IL (52 total candidates) | CA (49 total candidates) | TX (38 total candidates)
  - Top 3 states by high-priority sites: CA (30 score≥5) | IL (28 score≥5) | MI (24 score≥5)
  - NOTE: 'Total candidates' = all trauma centers lacking burn capability per state
  - NOTE: 'High-priority' (score >= 5) = subset qualifying for immediate deployment

PEDIATRIC ACCESS
  - 5 states have adult burn centers but NO pediatric capability
  - States: CT, KY, ME, VT, WV

BURDEN & IMPACT (Team 13 Modeled Estimate)
  - Formula: 600,000 x 15% x 66% x 7% excess infection rate x $24,000/infection
  - ~67,213 under-referred patients/yr (modeled)
  - ~4,681 avoidable infections/yr (modeled)
  - ~$0.112B in avoidable annual infection costs (modeled)
  - Conservative projection, not a direct NIRD observational finding. This is a floor.
  - Sources:
      Ivanko et al. 2024  -> 600,000/yr national burn incidence
      Huang et al. 2021   -> 66% under-referral rate (Illinois statewide)
      Huang et al. 2021   -> 7% excess infection rate (Table 2: 12.5% vs 5.5%)
      Benchmark           -> $24,000/infection conservative cost assumption

All figures saved to -> outputs/
=================================================================