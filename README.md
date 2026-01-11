# Actuarial Hacks 2026
## Flood Risk Assessment in Hamilton, Ontario

### Overview
This hackathon challenges participants to analyze and model flood risk in Hamilton, Ontario, using a multidisciplinary approach that combines physical modeling, damage assessment, and financial analysis. The project is divided into three interconnected parts that build a comprehensive understanding of flood risk from the ground up.

Hamilton's location at the western end of Lake Ontario, combined with its unique topography featuring the Niagara Escarpment, creates specific flood vulnerabilities. Understanding these risks is critical for urban planning, infrastructure resilience, and insurance pricing.

---

## Part 1: Topology and Physics of Flooding

### Problem Statement
Develop a physical model to predict flood extent, depth, and flow characteristics in Hamilton, Ontario, given various precipitation and water level scenarios. The challenge is to understand how Hamilton's unique topography—including the Niagara Escarpment, creek systems (Red Hill Creek, Spencer Creek), and proximity to Lake Ontario—influences flood behavior during extreme weather events.

### Starting Points

**Idea:** Create a digital elevation model (DEM) based flood simulation that accounts for:
- Topographic drainage patterns and natural watersheds
- Urban infrastructure (storm sewers, culverts, retention ponds)
- Soil permeability and land use (impervious surfaces in urban areas)
- Historical precipitation patterns and climate change projections

**Data Sources:**
- Natural Resources Canada: Canadian Digital Elevation Model (CDEM)
- Environment and Climate Change Canada: Historical weather data and precipitation records
- City of Hamilton Open Data: Stormwater infrastructure, land use, zoning maps
- Hamilton Conservation Authority: Watershed studies and flood plain mapping
- USGS Earth Explorer: Satellite imagery and terrain data

**Models:**
- HEC-RAS (Hydrologic Engineering Centers River Analysis System): 2D hydraulic modeling
- SWMM (Storm Water Management Model): Urban drainage simulation
- Simple empirical models: Manning's equation for flow velocity, rational method for runoff
- Machine learning approaches: Flood extent prediction using historical flood data

**Software:**
- QGIS: Open-source GIS for spatial analysis and visualization
- HEC-RAS: Free hydraulic modeling software from US Army Corps of Engineers
- Python libraries: `pysheds` (watershed delineation), `rasterio` (DEM processing), `geopandas` (spatial data)
- MATLAB/Octave: Numerical modeling of flood dynamics

---

## Part 2: Damage to Buildings and Infrastructure

### Problem Statement
Estimate the physical damage to residential, commercial, and critical infrastructure resulting from flood events of varying severity. The challenge is to create vulnerability functions (depth-damage curves) specific to Hamilton's building stock and infrastructure, and to identify high-risk assets that require mitigation or protection.

### Starting Points

**Idea:** Develop a damage assessment framework that:
- Classifies buildings by construction type, age, and occupancy
- Maps critical infrastructure (hospitals, power stations, water treatment, roads, bridges)
- Applies depth-damage relationships to estimate repair/replacement costs
- Identifies cascading failures (e.g., power loss affecting hospitals)

**Data Sources:**
- MPAC (Municipal Property Assessment Corporation): Building characteristics and valuations
- City of Hamilton: Infrastructure asset inventory, critical facility locations
- HAZUS damage functions: FEMA's standardized depth-damage curves
- Insurance Bureau of Canada: Historical flood claims data
- Statistics Canada: Building age, construction type by census tract

**Models:**
- Depth-damage curves: Relationship between water depth and % damage
- Fragility curves: Probability of damage states (minor, moderate, severe, complete)
- Network analysis: Critical infrastructure interdependencies
- Monte Carlo simulation: Uncertainty in damage estimates

**Software:**
- HAZUS-MH: FEMA's multi-hazard loss estimation tool
- ArcGIS/QGIS: Spatial overlay of flood zones and building footprints
- Python libraries: `pandas` (data analysis), `matplotlib/seaborn` (visualization), `networkx` (infrastructure networks)
- R: Statistical modeling and damage function calibration (`dplyr`, `ggplot2`)

---

## Part 3: Financial Impact and Insurance Coverage

### Problem Statement
Quantify the economic consequences of flood events and design appropriate insurance products to transfer and manage flood risk. The challenge is to estimate expected annual losses (EAL), price flood insurance premiums, assess affordability and uptake, and evaluate the role of government in catastrophic scenarios.

### Starting Points

**Idea:** Build a catastrophe model that:
- Generates stochastic event sets (thousands of synthetic flood scenarios)
- Calculates ground-up losses and insured losses (after deductibles, limits)
- Prices premiums using risk-based and community-rated approaches
- Analyzes insurer solvency and reinsurance needs
- Evaluates policy options (mandatory coverage, subsidies, mitigation incentives)

**Data Sources:**
- IBC (Insurance Bureau of Canada): Industry loss data and exposure information
- OSFI (Office of the Superintendent of Financial Institutions): Insurance solvency requirements
- CMHC: Property values and mortgage insurance data
- Statistics Canada: Household income and insurance affordability metrics
- Climate change scenarios: IPCC projections for precipitation intensity

**Models:**
- Exceedance probability curves: Return period vs. loss magnitude
- Loss distribution: Modeling frequency and severity of flood losses
- Premium calculation: Pure premium, expense loading, profit margin, risk margin
- Capital modeling: Tail Value-at-Risk (TVaR), Probable Maximum Loss (PML)
- Affordability analysis: Premium-to-income ratios across neighborhoods

**Software:**
- Excel/Google Sheets: Quick calculations and premium sensitivity analysis
- R/Python: Catastrophe modeling and loss aggregation
  - `actuar` package (R): Actuarial functions and loss distributions
  - `numpy/scipy` (Python): Statistical distributions and Monte Carlo
- Oasis Loss Modelling Framework: Open-source catastrophe modeling platform
- Tableau/Power BI: Dashboard visualization of risk metrics

---

## Getting Started

1. **Form Teams:** Interdisciplinary teams (actuaries, engineers, data scientists, GIS specialists) work best
2. **Choose Your Focus:** Teams can tackle one part in depth or attempt an integrated solution
3. **Collaboration:** Parts 1, 2, and 3 build on each other—coordinate with other teams to share outputs
4. **Deliverables:** Prepare a presentation covering methodology, findings, and recommendations
5. **Impact:** Consider real-world applicability—what would you recommend to the City of Hamilton or insurers?

## Resources

- Hamilton Conservation Authority: https://conservationhamilton.ca
- City of Hamilton Open Data: https://open.hamilton.ca
- Government of Canada Climate Data: https://climate.weather.gc.ca
- Insurance Bureau of Canada: http://www.ibc.ca

## Questions?

Contact the hackathon organizers or open an issue in this repository.

Good luck, and may your models converge quickly!
