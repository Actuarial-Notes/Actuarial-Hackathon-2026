# Oasis Data Dictionary

## Hamilton Flood Risk Model - Field Specifications

---

## Blue Team: Hazard Files

### events.csv
Defines the catalogue of flood events to be modeled.

| Field | Type | Required | Description | Example |
|-------|------|----------|-------------|---------|
| `event_id` | INT | Yes | Unique identifier for the event | 1 |
| `event_name` | STRING | No | Descriptive name | "100yr_Spring_2024" |
| `return_period` | FLOAT | No | Return period in years | 100.0 |
| `rate` | FLOAT | No | Annual occurrence rate (1/return period) | 0.01 |

**Example:**
```csv
event_id,event_name,return_period,rate
1,FL_010_Base,10,0.1
2,FL_050_Base,50,0.02
3,FL_100_Base,100,0.01
4,FL_500_Base,500,0.002
5,FL_100_CC2050,100,0.01
```

---

### event_lookup.csv
Maps events to area perils and footprint partitions.

| Field | Type | Required | Description | Example |
|-------|------|----------|-------------|---------|
| `event_id` | INT | Yes | References events.csv | 1 |
| `areaperil_id` | INT | Yes | Area peril zone identifier | 101 |
| `footprint_id` | INT | No | Footprint partition (for large events) | 1 |

---

### footprint.csv
Contains hazard intensity values (flood depths) by area peril.

| Field | Type | Required | Description | Example |
|-------|------|----------|-------------|---------|
| `event_id` | INT | Yes | References events.csv | 1 |
| `areaperil_id` | INT | Yes | Area peril zone identifier | 101 |
| `intensity_bin_id` | INT | Yes | References intensity bin | 5 |
| `prob` | FLOAT | Yes | Probability of this intensity | 0.85 |

**Hamilton Context:**
- `areaperil_id` corresponds to grid cells (e.g., 100m x 100m) covering Hamilton
- `intensity_bin_id` maps to flood depth ranges (see footprint_metadata.csv)
- Multiple intensity bins per areaperil allow for uncertainty in depth predictions

---

### footprint_metadata.csv
Defines intensity bins and metadata.

| Field | Type | Required | Description | Example |
|-------|------|----------|-------------|---------|
| `intensity_bin_id` | INT | Yes | Unique bin identifier | 1 |
| `intensity_min` | FLOAT | Yes | Minimum depth (meters) | 0.0 |
| `intensity_max` | FLOAT | Yes | Maximum depth (meters) | 0.3 |
| `intensity_mid` | FLOAT | No | Midpoint value | 0.15 |

**Recommended Bins for Hamilton:**
```csv
intensity_bin_id,intensity_min,intensity_max,intensity_mid,description
1,0.00,0.30,0.15,Shallow flooding - cosmetic damage
2,0.30,0.60,0.45,Minor flooding - floor damage
3,0.60,1.00,0.80,Moderate flooding - significant damage
4,1.00,1.50,1.25,Major flooding - structural concerns
5,1.50,2.00,1.75,Severe flooding - major structural
6,2.00,3.00,2.50,Extreme flooding - potential total loss
7,3.00,99.00,5.00,Catastrophic - complete destruction
```

---

### lookup.csv (areaperil portion)
Links geographic locations to area peril zones.

| Field | Type | Required | Description | Example |
|-------|------|----------|-------------|---------|
| `loc_id` | INT | Yes | Location identifier | 10001 |
| `areaperil_id` | INT | Yes | Area peril zone | 101 |
| `peril_id` | STRING | Yes | Peril code | "FL" |

---

## Black Team: Exposure Files

### locations.csv (loc.csv)
Master file of all exposure locations.

| Field | Type | Required | Description | Example |
|-------|------|----------|-------------|---------|
| `loc_id` | INT | Yes | Unique location identifier | 10001 |
| `accntnum` | STRING | No | Account number reference | "ACC001" |
| `locnum` | STRING | No | Location number | "LOC001" |
| `locname` | STRING | No | Location name | "123 Main St" |
| `streetaddress` | STRING | No | Street address | "123 Main Street" |
| `city` | STRING | No | City | "Hamilton" |
| `postalcode` | STRING | No | Postal code | "L8P 1A1" |
| `latitude` | FLOAT | Yes | Latitude (WGS84) | 43.2557 |
| `longitude` | FLOAT | Yes | Longitude (WGS84) | -79.8711 |
| `countrycode` | STRING | No | Country code | "CA" |
| `occupancycode` | INT | No | Building occupancy type | 1000 |
| `constructioncode` | INT | No | Construction type | 1000 |
| `numberofstoreys` | INT | No | Building height | 2 |
| `yearbuilt` | INT | No | Construction year | 1965 |
| `buildingtiv` | FLOAT | Yes | Building total insured value | 350000.00 |
| `contentstiv` | FLOAT | No | Contents value | 75000.00 |
| `bitiv` | FLOAT | No | Business interruption value | 0.00 |
| `floorarea` | FLOAT | No | Floor area (sq meters) | 150.5 |
| `basement` | INT | No | Has basement (0/1) | 1 |
| `firstfloorht` | FLOAT | No | First floor height (m) | 0.3 |

**Hamilton Occupancy Codes:**
| Code | Description |
|------|-------------|
| 1000 | Residential - Single Family |
| 1001 | Residential - Multi-Family |
| 1002 | Residential - Apartment/Condo |
| 2000 | Commercial - Retail |
| 2001 | Commercial - Office |
| 3000 | Industrial - Light |
| 3001 | Industrial - Heavy |
| 4000 | Institutional - School |
| 4001 | Institutional - Hospital |
| 4002 | Institutional - Government |

---

### items.csv
Defines modeled exposure items linked to vulnerability.

| Field | Type | Required | Description | Example |
|-------|------|----------|-------------|---------|
| `item_id` | INT | Yes | Unique item identifier | 1 |
| `coverage_id` | INT | Yes | References coverages.csv | 1 |
| `areaperil_id` | INT | Yes | Area peril zone | 101 |
| `vulnerability_id` | INT | Yes | References vulnerability.csv | 1 |
| `group_id` | INT | No | Correlation group | 1 |

---

### coverages.csv
Defines coverage types and values.

| Field | Type | Required | Description | Example |
|-------|------|----------|-------------|---------|
| `coverage_id` | INT | Yes | Unique coverage identifier | 1 |
| `tiv` | FLOAT | Yes | Total insured value | 350000.00 |
| `coverage_type_id` | INT | No | Type of coverage | 1 |

**Coverage Type Codes:**
| Code | Description |
|------|-------------|
| 1 | Building |
| 2 | Contents |
| 3 | Business Interruption |
| 4 | Other Structures |

---

### accounts.csv
Account-level information.

| Field | Type | Required | Description | Example |
|-------|------|----------|-------------|---------|
| `accntnum` | STRING | Yes | Account number | "ACC001" |
| `polnumber` | STRING | No | Policy number | "POL001" |
| `poression` | STRING | No | Portfolio | "RES_HAM" |
| `accname` | STRING | No | Account name | "Smith Residence" |

---

### vulnerability.csv
Damage ratio curves mapping intensity to damage.

| Field | Type | Required | Description | Example |
|-------|------|----------|-------------|---------|
| `vulnerability_id` | INT | Yes | Unique curve identifier | 1 |
| `intensity_bin_id` | INT | Yes | References footprint_metadata | 3 |
| `damage_bin_id` | INT | Yes | Damage ratio bin | 5 |
| `prob` | FLOAT | Yes | Probability of this damage | 0.4 |

**Example Vulnerability Curve (1-story residential with basement):**
```csv
vulnerability_id,intensity_bin_id,damage_bin_id,prob
1,1,1,0.70
1,1,2,0.25
1,1,3,0.05
1,2,2,0.30
1,2,3,0.50
1,2,4,0.20
1,3,4,0.40
1,3,5,0.45
1,3,6,0.15
```

---

### vulnerability_metadata.csv
Describes vulnerability curve families.

| Field | Type | Required | Description | Example |
|-------|------|----------|-------------|---------|
| `vulnerability_id` | INT | Yes | References vulnerability.csv | 1 |
| `vulnerability_name` | STRING | No | Descriptive name | "1Story_Basement_Pre1980" |
| `peril_id` | STRING | Yes | Peril code | "FL" |
| `damage_bin_min` | FLOAT | No | Minimum damage bin boundary | 0.0 |
| `damage_bin_max` | FLOAT | No | Maximum damage bin boundary | 1.0 |
| `num_damage_bins` | INT | No | Number of damage bins | 10 |

**Damage Bin Definitions:**
```csv
damage_bin_id,damage_min,damage_max,damage_mid,description
1,0.00,0.05,0.025,Negligible
2,0.05,0.15,0.10,Minor
3,0.15,0.30,0.225,Moderate
4,0.30,0.50,0.40,Significant
5,0.50,0.70,0.60,Major
6,0.70,0.85,0.775,Severe
7,0.85,0.95,0.90,Very Severe
8,0.95,1.00,0.975,Total Loss
```

---

## Red Team: Financial Files

### fm_input_items.csv
Merged exposure/hazard representation for financial calculations.

| Field | Type | Required | Description | Example |
|-------|------|----------|-------------|---------|
| `item_id` | INT | Yes | References items.csv | 1 |
| `output_id` | INT | Yes | Output reference | 1 |
| `agg_id` | INT | Yes | Aggregation identifier | 1 |
| `gul_item_id` | INT | No | Ground-up loss item ID | 1 |
| `coverage_id` | INT | Yes | References coverages.csv | 1 |

---

### fm_policytc.csv
Policy terms and conditions.

| Field | Type | Required | Description | Example |
|-------|------|----------|-------------|---------|
| `layer_id` | INT | Yes | Financial model layer | 1 |
| `level_id` | INT | Yes | Hierarchy level | 1 |
| `agg_id` | INT | Yes | Aggregation ID | 1 |
| `policytc_id` | INT | Yes | Policy terms reference | 1 |
| `deductible1` | FLOAT | No | Primary deductible | 1000.00 |
| `limit1` | FLOAT | No | Primary limit | 350000.00 |
| `deductible2` | FLOAT | No | Secondary deductible | 0.00 |
| `limit2` | FLOAT | No | Secondary limit | 0.00 |
| `attachment` | FLOAT | No | Attachment point | 0.00 |
| `share` | FLOAT | No | Share proportion | 1.00 |

---

### fm_profile.csv
Financial calculation profiles.

| Field | Type | Required | Description | Example |
|-------|------|----------|-------------|---------|
| `policytc_id` | INT | Yes | Policy terms ID | 1 |
| `calcrule_id` | INT | Yes | Calculation rule | 1 |
| `deductible1` | FLOAT | No | Deductible amount | 1000.00 |
| `limit1` | FLOAT | No | Limit amount | 350000.00 |
| `deductible_type` | INT | No | Deductible type (flat/pct) | 1 |

**Calculation Rules:**
| ID | Description |
|----|-------------|
| 1 | Deductible and limit |
| 2 | Deductible only |
| 3 | Limit only |
| 4 | Pass-through (no terms) |
| 12 | Deductible % of TIV |

---

### fm_programme.csv
Links items/accounts to financial model structure.

| Field | Type | Required | Description | Example |
|-------|------|----------|-------------|---------|
| `from_agg_id` | INT | Yes | Source aggregation | 1 |
| `level_id` | INT | Yes | Hierarchy level | 1 |
| `to_agg_id` | INT | Yes | Target aggregation | 1 |

---

### fm_xref.csv
Cross-reference file connecting components.

| Field | Type | Required | Description | Example |
|-------|------|----------|-------------|---------|
| `output_id` | INT | Yes | Output identifier | 1 |
| `agg_id` | INT | Yes | Aggregation ID | 1 |
| `layer_id` | INT | Yes | Financial layer | 1 |

---

## Extended Community Impact Files

### population_exposure.csv
Population counts by flood zone and scenario.

| Field | Type | Required | Description | Example |
|-------|------|----------|-------------|---------|
| `areaperil_id` | INT | Yes | Area peril zone | 101 |
| `event_id` | INT | Yes | Event reference | 1 |
| `population` | INT | Yes | Total population | 1250 |
| `households` | INT | No | Number of households | 485 |
| `pop_children` | INT | No | Children under 18 | 275 |
| `pop_seniors` | INT | No | Seniors 65+ | 180 |
| `pop_density` | FLOAT | No | People per sq km | 3200.5 |

---

### vulnerable_groups.csv
At-risk population identification.

| Field | Type | Required | Description | Example |
|-------|------|----------|-------------|---------|
| `areaperil_id` | INT | Yes | Area peril zone | 101 |
| `elderly_alone` | INT | No | Seniors living alone | 45 |
| `disabled` | INT | No | People with disabilities | 85 |
| `low_income` | INT | No | Below poverty line | 210 |
| `no_vehicle` | INT | No | Households without vehicle | 120 |
| `renters` | INT | No | Renting households | 195 |
| `svi_score` | FLOAT | No | Social vulnerability index | 0.72 |

---

### critical_facilities.csv
Essential services and infrastructure.

| Field | Type | Required | Description | Example |
|-------|------|----------|-------------|---------|
| `facility_id` | INT | Yes | Unique facility ID | 1 |
| `facility_name` | STRING | Yes | Name | "Hamilton General Hospital" |
| `facility_type` | STRING | Yes | Type code | "HOSPITAL" |
| `latitude` | FLOAT | Yes | Latitude | 43.2570 |
| `longitude` | FLOAT | Yes | Longitude | -79.8650 |
| `capacity` | INT | No | Service capacity | 500 |
| `backup_power` | INT | No | Has backup power (0/1) | 1 |
| `flood_protection` | INT | No | Has flood protection (0/1) | 0 |
| `areaperil_id` | INT | Yes | Associated flood zone | 101 |
| `vulnerability_id` | INT | No | Damage curve reference | 50 |

**Facility Type Codes:**
| Code | Description |
|------|-------------|
| HOSPITAL | Medical facility |
| FIRE | Fire station |
| POLICE | Police station |
| SCHOOL | Educational facility |
| POWER | Electrical infrastructure |
| WATER | Water treatment/pumping |
| TRANSIT | Public transportation hub |
| SHELTER | Emergency shelter |
| TELECOM | Communications infrastructure |

---

### recovery_timeline.csv
Estimated restoration periods.

| Field | Type | Required | Description | Example |
|-------|------|----------|-------------|---------|
| `event_id` | INT | Yes | Event reference | 1 |
| `areaperil_id` | INT | Yes | Area peril zone | 101 |
| `damage_state` | INT | Yes | Damage severity (1-5) | 3 |
| `recovery_days_housing` | INT | No | Days to restore housing | 45 |
| `recovery_days_business` | INT | No | Days to restore business | 30 |
| `recovery_days_infra` | INT | No | Days to restore infrastructure | 14 |
| `displacement_pop` | INT | No | Population displaced | 485 |
| `shelter_needed` | INT | No | Emergency shelter capacity | 150 |

---

*Last updated: January 2026*
*Actuarial Hacks 2026 - Hamilton Flood Risk Project*
