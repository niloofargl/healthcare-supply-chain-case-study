# IV-Fluid Demand Estimation for Vancouver Island Hospital Network
This repository documents a case study for estimating weekly IV-Serum demand across selected Vancouver Island Health Authority (VIHA) hospitals during COVID-19. The case study supports research on **AI-driven decision-making for sustainable and resilient healthcare supply chain management (SR-HCSCM)**.

## Project Purpose

Healthcare supply chains face uncertainty during public health crises, especially when direct procurement, inventory, or ward-level utilization data are unavailable. This repository provides a structured approach for estimating hospital-level IV-fluid serum demand using publicly available regional hospitalization data, hospital bed capacity, community population, and stochastic simulation.

The methodology is designed to support AI-based healthcare supply chain analysis, including scenario generation, demand uncertainty modeling, and resilient resource-allocation experiments.

## Case Study Scope

The hospital set includes eight healthcare facilities:

| Paper node | Hospital abbreviation | Hospital name |
|---|---|---|
| Node $A'$ | CR | Campbell River Hospital |
| Node $B'$ | CV | Comox Valley Hospital |
| Node $C'$ | CD | Cowichan District Hospital |
| Node $D'$ | NA | Nanaimo Regional General Hospital |
| Node $E'$ | VG | Victoria General Hospital |
| Node $F'$ | RJ | Royal Jubilee Hospital |
| Node $G'$ | WCG | West Coast General Hospital |
| Node $H'$ | SP | Saanich Peninsula Hospital |


## Demand Estimation Method

The planning horizon consists of **10 weekly periods**, representing a 70-day COVID-19 wave. The reference period corresponds to **BCCDC epidemiological weeks 1--10 of 2022**, approximately **January 2--March 12, 2022**, which covers the Omicron hospitalization wave in British Columbia. The demand estimates represent **total hospital IV-fluid usage**, not only IV-fluid usage for COVID-19 patients.

For each hospital $h$ and week $t$, the expected demand is estimated as the sum of a baseline inpatient component and a COVID-wave surge component:

```math
\mu_{h,t}
=
\left\lceil
7 \rho B_h q
+
H_t^{VI} s_h L q
\right\rceil
```

where $B_h$ is the staffed bed capacity of hospital $h$, $\rho=0.85$ is the assumed acute-care occupancy level, $q=0.373 \times 1.177 \approx 0.439$ is the estimated number of 1-L IV-fluid bags per occupied bed-day, $H_t^{VI}$ is the weekly number of COVID-19 hospitalizations reported for Vancouver Island Health, $s_h$ is the hospital-level allocation share, and $L=10.1$ days is the average COVID-19 acute length of stay. The IV-fluid coefficient $q$ is based on an adult inpatient IV-fluid audit in which 37.3% of patients received IV fluids and the mean delivered volume among those patients was 1177 mL/day [@eastwood_2012]. The COVID-19 length-of-stay value is based on CIHI’s reported average acute length of stay for COVID-19 hospitalizations in Canada in 2021--2022 [@cihi_2023]. The occupancy assumption follows the use of 85% acute-care occupancy as a high-utilization planning threshold [@oecd_2023].

The hospital allocation share $s_h$ is obtained by mapping communities to hospitals using catchment-area assignments and staffed bed capacities. Communities assigned to a single hospital are allocated directly to that hospital. Greater Victoria demand is divided among Royal Jubilee, Victoria General, and Saanich Peninsula hospitals in proportion to their bed capacities. This creates hospital-level demand trajectories that vary across both time and location, reflecting the regional hospitalization wave, hospital size, and assigned service population.


## Data Allocation Logic

Because hospital-level COVID-19 inpatient demand was not publicly available, regional demand was allocated to hospitals using community population and hospital bed capacity.

For communities assigned to one hospital, all demand was allocated to that hospital. For communities assigned to multiple hospitals, demand was divided proportionally based on bed capacity.

For example, Greater Victoria demand was allocated across RJ, SP, and VG according to bed-capacity shares:

| Hospital | Demand Share |
|---|---:|
| RJ | 50.6% |
| SP | 14.5% |
| VG | 34.9% |

## Community Assignment Data

| Community | Total Population | Assigned Hospital(s) |
|---|---:|---|
| Greater Victoria | 344,615 | RJ, SP, VG |
| Nanaimo | 98,021 | NA |
| Parksville--Qualicum | 27,822 | NA |
| Ladysmith | 7,921 | NA |
| Comox Valley | 55,213 | CV |
| Cowichan | 43,252 | CD |
| Campbell River | 36,096 | CR |
| Port Alberni | 25,465 | WCG |

## Geographic and Capacity Data

| HF | Latitude | Longitude | Beds |
|---|---:|---:|---:|
| CR | 50.0264 | -125.2463 | 96 |
| CV | 49.7016 | -124.9488 | 153 |
| CD | 48.7824 | -123.7025 | 134 |
| NA | 49.1867 | -123.9855 | 409 |
| VG | 48.4828 | -123.4599 | 344 |
| RJ | 48.4350 | -123.3340 | 500 |
| WCG | 49.2488 | -124.8105 | 52 |
| SP | 48.6499 | -123.4181 | 143 |

The total acute-care capacity across the eight sites is **1,831 beds**.

## Stochastic Scenario Generation

After weekly hospital-level demand was estimated, stochastic variability was incorporated to better represent real-world uncertainty. The estimated number of hospitalized COVID-19 patients at each hospital was treated as the mean of a Poisson random variable.

Using this approach, **1,000 demand scenarios** were generated for each hospital and product type.

## Demand Simulation Figure

Place the demand distribution figure in:

```text
Figures/demand_distribution.png
```

Then display it in this README using:

```markdown
![Weekly demand patterns for PPE services across eight hospitals](Figures/demand_distribution.png)
```

![Weekly demand patterns for PPE services across eight hospitals](Figures/demand_distribution.png)


## Notes

- The methodology is intended for research and simulation purposes.
- Hospital-level demand estimates are approximations based on public regional data, community population, geographic assignment, and hospital bed capacity.
- Direct hospital procurement or ward-level utilization data should be used when available.


---

## `references.bib`

```bibtex
@misc{bccdc_archive_2022,
  author = {{BC Centre for Disease Control}},
  title = {Archived B.C. COVID-19 Data: Situation Reports, 2022},
  year = {2022},
  note = {Epidemiological weeks 1--10 of 2022}
}

@misc{bccdc_week10_2022,
  author = {{BC Centre for Disease Control}},
  title = {British Columbia COVID-19 Situation Report, Week 10: March 06--March 12, 2022},
  year = {2022}
}

@article{eastwood_2012,
  author = {Eastwood, Glenn M. and Peck, Leah and Young, Helen and Prowle, John and Vasudevan, Vandana and Jones, Daryl and Bellomo, Rinaldo},
  title = {Intravenous fluid administration and monitoring for adult ward patients in a teaching hospital},
  journal = {Nursing \& Health Sciences},
  volume = {14},
  number = {2},
  pages = {265--271},
  year = {2012},
  doi = {10.1111/j.1442-2018.2012.00689.x}
}

@misc{cihi_2023,
  author = {{Canadian Institute for Health Information}},
  title = {Hospital stays in Canada, 2021--2022},
  year = {2023}
}

@book{oecd_2023,
  author = {{OECD}},
  title = {Health at a Glance 2023: OECD Indicators},
  publisher = {OECD Publishing},
  year = {2023},
  doi = {10.1787/7a7afb35-en}
}

@misc{bowers_baxter_saline,
  author = {{Bowers Medical Supply}},
  title = {Baxter 0.9\% Sodium Chloride Injection, USP in VIAFLEX Plastic Container},
  year = {2026},
  note = {Catalogue listing for 1000 mL product: Case/12 Each}
}
