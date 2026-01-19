# Oasis Loss Modelling Framework Evaluation

## Hamilton, Ontario Flood Risk Assessment Project

---

## Executive Summary

This document evaluates the Oasis Loss Modelling Framework (Oasis LMF) structure for the Hamilton flood risk hackathon. While Oasis provides an excellent foundation for catastrophe modeling, **we recommend extending beyond traditional financial metrics** to capture the full impact on lives, livelihoods, and community resilience.

The framework divides naturally into three research streams that align with the hackathon's three parts, making team collaboration intuitive. However, to serve City Planners and community groups effectively, we must augment the standard Oasis outputs with **social vulnerability indices**, **community impact metrics**, and **actionable planning recommendations**.

---

## 1. Oasis Framework Structure Analysis

### 1.1 Research Streams Mapping

| Research Stream | Oasis Layer | Hackathon Part | Primary Focus |
|-----------------|-------------|----------------|---------------|
| **1. Natural Hazards** | Hazard events and intensities | Part 1: Topology & Physics | Physical flood characteristics |
| **2. Buildings & Infrastructure** | Physical assets, taxonomy, vulnerability assignment | Part 2: Damage Assessment | Exposure and vulnerability |
| **3. Value and Loss** | Damage-to-loss translation, policy terms | Part 3: Financial Impact | Economic consequences |

### 1.2 Team Responsibility Matrix

The framework assigns 20 deliverable files across three teams:

#### Blue Team: Hazard Modeling (Natural Hazards)
| File | Purpose | Hamilton Application |
|------|---------|---------------------|
| `events.csv` | Catalogue of event IDs | Flood scenarios (10yr, 50yr, 100yr, 500yr return periods) |
| `event_lookup.csv` | Maps events to area perils and footprint partitions | Link scenarios to Hamilton watersheds |
| `footprint.csv` | Hazard intensity values (flood depth by area peril) | Depth grids from HEC-RAS/SWMM output |
| `footprint_metadata.csv` | Metadata on hazard units and footprint structure | Units (meters), grid resolution, CRS |
| `lookup.csv` (area_peril_id) | Links geocoded locations to area peril regions | Hamilton address-to-flood-zone mapping |

#### Black Team: Exposure Modeling (Buildings & Infrastructure)
| File | Purpose | Hamilton Application |
|------|---------|---------------------|
| `loc.csv / locations.csv` | Coordinates, addresses, geospatial attributes | Building locations from MPAC data |
| `items.csv` | Modelled exposure items linked to coverage/vulnerability | Individual structures with attributes |
| `coverages.csv` | Coverage identifiers and metadata | Building, contents, business interruption |
| `accounts.csv` | Account-level grouping and policyholder characteristics | Residential vs commercial portfolios |
| `portfolios.csv` (optional) | Portfolio/group-level metadata | Sector aggregations |
| `lookup.csv` (vulnerability_id) | Assigns exposures to vulnerability curves | Building type to damage curve mapping |
| `vulnerability.csv` | Damage ratio curves (intensity → damage distribution) | Depth-damage curves for Hamilton building stock |
| `vulnerability_metadata.csv` | Metadata on vulnerability families, peril type, units | Curve sources, calibration notes |

#### Red Team: Financial Modeling (Value & Loss)
| File | Purpose | Hamilton Application |
|------|---------|---------------------|
| `fm_input_items.csv` | Merged exposure/hazard for financial calculations | Combined damage estimates |
| `fm_policytc.csv` | Policy terms (deductibles, limits, attachment) | Insurance product structures |
| `fm_profile.csv` | Financial calculation profiles | Premium pricing rules |
| `fm_programme.csv` | Links items/accounts to financial model structure | Aggregation hierarchy |
| `fm_xref.csv` | Cross-reference connecting items, coverages, FM layers | Linkage tables |
| `ri_info.csv` (optional) | Reinsurance contract descriptions | Excess-of-loss, quota share treaties |
| `ri_scope.csv` (optional) | Defines exposures under reinsurance | Protected portfolios |

---

## 2. Extending Beyond Financial Metrics

### 2.1 The Gap: Financial vs. Human Impact

Traditional Oasis outputs focus on:
- Ground-up losses (repair/replacement costs)
- Insured losses (after policy terms)
- Portfolio risk metrics (AAL, TVaR, PML)

**What's missing for City Planners and Community Groups:**
- Population displacement and shelter needs
- Vulnerable populations at risk (elderly, disabled, low-income)
- Critical service disruptions (hospitals, schools, transit)
- Recovery timeline and community resilience
- Cascading social impacts (mental health, economic disruption)

### 2.2 Proposed Extensions

We recommend adding a **fourth research stream** or extending each team's deliverables:

#### Stream 4: Community Impact (Suggested Addition)

| Deliverable | Purpose | Data Sources |
|-------------|---------|--------------|
| `population_exposure.csv` | Population counts by flood zone and depth | Census data, building occupancy |
| `vulnerable_groups.csv` | At-risk populations (seniors, children, disabled, low-income) | Census demographics |
| `critical_facilities.csv` | Essential services with flood vulnerability | City infrastructure inventory |
| `evacuation_zones.csv` | Evacuation requirements by scenario | Emergency management plans |
| `recovery_timeline.csv` | Estimated restoration periods by service/sector | Historical recovery data |
| `community_resilience.csv` | Social vulnerability indices by neighborhood | SoVI, CDC/ATSDR SVI adapted for Canada |

### 2.3 Human Impact Metrics

| Metric | Definition | Calculation |
|--------|------------|-------------|
| **Population at Risk (PAR)** | People in flood zones by depth threshold | Pop × P(depth > threshold) |
| **Displacement Days** | Person-days of housing unavailability | Pop × Avg. repair time by damage state |
| **Vulnerable Population Index** | Weighted count of high-risk individuals | Σ(Pop_i × Vulnerability_weight_i) |
| **Critical Service Hours Lost** | Hours of essential service disruption | Σ(Facility_downtime × Service_capacity) |
| **Economic Disruption Index** | Employment and business activity impact | Jobs_affected × Avg_duration |
| **Recovery Inequality Ratio** | Disparity in recovery times by income | High_income_recovery / Low_income_recovery |

---

## 3. Hamilton-Specific Recommendations

### 3.1 Geographic Priorities

Hamilton's flood risk concentrates in specific areas:

1. **Red Hill Creek Watershed** - Major urbanized watershed with stormwater challenges
2. **Spencer Creek Corridor** - Western Hamilton flood plain
3. **Lake Ontario Shoreline** - Storm surge and wave action risk
4. **Escarpment Base** - Runoff concentration from elevation change
5. **Downtown Core** - Combined sewer overflow concerns

### 3.2 Building Stock Considerations

| Building Type | Estimated Count | Primary Vulnerability | Recommended Curve |
|---------------|-----------------|----------------------|-------------------|
| Residential - Pre-1950 | ~40,000 | Basement flooding, no backwater valves | HAZUS 1-story no basement (modified) |
| Residential - 1950-1990 | ~60,000 | Slab-on-grade or basement, variable | HAZUS 1-2 story with basement |
| Residential - Post-1990 | ~25,000 | Better drainage, some flood-resistant | HAZUS with reduced damage factors |
| Commercial/Industrial | ~8,000 | Equipment, inventory, business interruption | HAZUS commercial by occupancy |
| Critical Infrastructure | ~150 key facilities | Service disruption, cascading failures | Custom curves required |

### 3.3 Scenario Design

| Event ID | Return Period | Primary Driver | Expected Depth (Peak) | Duration |
|----------|---------------|----------------|----------------------|----------|
| FL_010 | 10-year | Spring melt + rain | 0.3-0.5m | 12-24 hrs |
| FL_050 | 50-year | Summer convective storm | 0.5-1.0m | 6-12 hrs |
| FL_100 | 100-year | Multi-day rainfall | 1.0-2.0m | 24-48 hrs |
| FL_500 | 500-year | Lake surge + heavy rain | 2.0-4.0m | 48-72 hrs |
| FL_CC_100 | 100-year (2050 climate) | Intensified precipitation | 1.5-2.5m | 24-48 hrs |

---

## 4. Project Structure Recommendations

### 4.1 Directory Organization

```
hamilton-flood-model/
├── README.md
├── docs/
│   ├── oasis-framework-evaluation.md    # This document
│   ├── data-dictionary.md               # Field definitions
│   └── methodology.md                   # Modeling approach
├── data/
│   ├── raw/                             # Original source data
│   │   ├── hazard/                      # DEM, precipitation, etc.
│   │   ├── exposure/                    # MPAC, infrastructure
│   │   └── financial/                   # Insurance terms, values
│   ├── processed/                       # Cleaned, transformed data
│   └── oasis/                           # Oasis-formatted CSVs
│       ├── hazard/                      # Blue team outputs
│       │   ├── events.csv
│       │   ├── event_lookup.csv
│       │   ├── footprint.csv
│       │   ├── footprint_metadata.csv
│       │   └── lookup_areaperil.csv
│       ├── exposure/                    # Black team outputs
│       │   ├── locations.csv
│       │   ├── items.csv
│       │   ├── coverages.csv
│       │   ├── accounts.csv
│       │   ├── vulnerability.csv
│       │   ├── vulnerability_metadata.csv
│       │   └── lookup_vulnerability.csv
│       ├── financial/                   # Red team outputs
│       │   ├── fm_input_items.csv
│       │   ├── fm_policytc.csv
│       │   ├── fm_profile.csv
│       │   ├── fm_programme.csv
│       │   └── fm_xref.csv
│       └── community/                   # Extended impact files
│           ├── population_exposure.csv
│           ├── vulnerable_groups.csv
│           ├── critical_facilities.csv
│           └── recovery_timeline.csv
├── models/
│   ├── hazard/                          # Flood simulation code
│   ├── damage/                          # Vulnerability functions
│   ├── loss/                            # Financial calculations
│   └── impact/                          # Community impact analysis
├── analysis/
│   ├── notebooks/                       # Jupyter notebooks
│   └── scripts/                         # Analysis scripts
├── outputs/
│   ├── maps/                            # Flood maps, risk maps
│   ├── reports/                         # Summary reports
│   └── dashboards/                      # Interactive visualizations
└── tests/                               # Validation tests
```

### 4.2 Team Workflow

```
┌─────────────────────────────────────────────────────────────────────────┐
│                        OASIS DATA FLOW                                  │
└─────────────────────────────────────────────────────────────────────────┘

   BLUE TEAM                BLACK TEAM                RED TEAM
   (Hazard)                 (Exposure)               (Financial)
      │                         │                        │
      ▼                         ▼                        ▼
┌──────────────┐        ┌──────────────┐        ┌──────────────┐
│ events.csv   │        │ locations.csv│        │ fm_policytc  │
│ footprint.csv│───────►│ items.csv    │───────►│ fm_profile   │
│ lookup.csv   │        │ vulnerability│        │ fm_programme │
└──────────────┘        └──────────────┘        └──────────────┘
      │                         │                        │
      │                         │                        │
      └─────────────────────────┼────────────────────────┘
                                │
                                ▼
                    ┌───────────────────────┐
                    │   OASIS LOSS CALC     │
                    │   (ktools engine)     │
                    └───────────────────────┘
                                │
                                ▼
                    ┌───────────────────────┐
                    │  FINANCIAL OUTPUTS    │
                    │  AAL, OEP, AEP, etc.  │
                    └───────────────────────┘
                                │
                                ▼
                    ┌───────────────────────┐
                    │  COMMUNITY IMPACT     │    ◄── EXTENSION
                    │  (Population, social) │
                    └───────────────────────┘
```

---

## 5. Outputs for City Planners and Community Groups

### 5.1 Planning-Focused Deliverables

| Deliverable | Audience | Content |
|-------------|----------|---------|
| **Flood Risk Maps** | All stakeholders | Interactive maps showing depth, extent, probability |
| **Risk Priority Index** | City Planners | Combined hazard-vulnerability-consequence ranking |
| **Infrastructure Vulnerability Report** | Public Works | Critical facilities at risk, mitigation priorities |
| **Neighborhood Risk Profiles** | Community Groups | Per-ward/census tract risk summaries |
| **Evacuation Planning Data** | Emergency Management | Population counts, routes, shelter capacity |
| **Insurance Affordability Analysis** | Social Services | Premium burden by income quintile |
| **Climate Adaptation Scenarios** | Long-term Planning | Future risk under climate change projections |

### 5.2 Key Questions to Answer

For **City Planners**:
1. Which neighborhoods face the highest flood risk?
2. What infrastructure investments would most reduce risk?
3. Where should future development be restricted or require elevation?
4. What is the cost-benefit ratio of various mitigation options?

For **Emergency Management**:
1. How many people require evacuation in each scenario?
2. Where are vulnerable populations concentrated?
3. What is the cascade of critical infrastructure failures?
4. How long will recovery take by area?

For **Community Groups**:
1. Which residents face unaffordable insurance premiums?
2. What assistance programs are needed post-flood?
3. How can communities build resilience?
4. What early warning systems would help?

---

## 6. Implementation Timeline (4-Week Hackathon)

### Week 1: Foundation
- **Blue Team**: Acquire DEM, run initial flood simulations for 2-3 scenarios
- **Black Team**: Obtain building data, classify building types, map critical infrastructure
- **Red Team**: Define policy structures, source vulnerability curves

### Week 2: Core Development
- **Blue Team**: Complete footprint.csv for all scenarios, validate against historical floods
- **Black Team**: Complete locations.csv, items.csv, vulnerability assignments
- **Red Team**: Build financial model structure, link to exposure

### Week 3: Integration
- **All Teams**: Integrate datasets, run Oasis loss calculations
- **Extension**: Add community impact layer (population exposure, vulnerable groups)
- **Validation**: Check outputs against available benchmarks

### Week 4: Analysis & Presentation
- Generate maps, reports, and dashboards
- Document methodology and limitations
- Prepare recommendations for City Planners
- Final presentation

---

## 7. Success Criteria

### Technical Criteria
- [ ] Complete Oasis-compliant dataset for Hamilton
- [ ] Minimum 4 flood scenarios (10yr, 50yr, 100yr, 500yr)
- [ ] Validated vulnerability curves for major building types
- [ ] Functioning loss calculation pipeline

### Impact Criteria
- [ ] Population exposure estimates by scenario
- [ ] Vulnerable population identification
- [ ] Critical infrastructure risk assessment
- [ ] Actionable recommendations for City Planners
- [ ] Insurance affordability analysis

### Presentation Criteria
- [ ] Clear visualization of risk (maps, charts)
- [ ] Quantified human impact (not just dollars)
- [ ] Specific, implementable recommendations
- [ ] Transparent methodology and limitations

---

## 8. Conclusion

The Oasis Loss Modelling Framework provides an excellent foundation for catastrophe modeling, but **for this hackathon to truly serve City Planners and community groups, we must go beyond financial metrics**.

By extending the framework to include:
- Population exposure and displacement
- Vulnerable population identification
- Critical infrastructure dependencies
- Recovery timelines and resilience metrics

...teams can produce outputs that inform not just insurance pricing, but **equitable urban planning, emergency preparedness, and community resilience**.

The goal is not just to model the cost of a flood, but to understand **who is affected, how severely, and what can be done about it**.

---

*Document prepared for Actuarial Hacks 2026*
*Hamilton, Ontario Flood Risk Assessment Project*
