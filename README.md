# healthcare-supply-chain-case-study
This repository documents a case study for estimating weekly IV-Serum demand across selected Vancouver Island Health Authority (VIHA) hospitals during COVID-19. The case study supports research on **AI-driven decision-making for sustainable and resilient healthcare supply chain management (SR-HCSCM)**.

## Project Purpose

Healthcare supply chains face uncertainty during public health crises, especially when direct procurement, inventory, or ward-level utilization data are unavailable. This repository provides a structured approach for estimating hospital-level PPE demand using publicly available regional hospitalization data, hospital bed capacity, community population, and stochastic simulation.

The methodology is designed to support AI-based healthcare supply chain analysis, including scenario generation, demand uncertainty modeling, and resilient resource-allocation experiments.

## Case Study Scope

The hospital set includes eight healthcare facilities:

| Code | Hospital / Health Facility |
|---|---|
| CR | Campbell River |
| CV | Comox Valley |
| CD | Cowichan District |
| NA | Nanaimo |
| VG | Victoria General |
| RJ | Royal Jubilee |
| WCG | West Coast General |
| SP | Saanich Peninsula |

## Demand Estimation Method

Weekly gown consumption attributable to hospitalized COVID-19 patients is estimated using average daily inpatient counts.

Let:

- $k$ denote a hospital.
- $n$ denote an epidemiological week.
- $d_k^n$ denote the average daily number of hospitalized COVID-19 patients during week $n$ at hospital $k$.
- $\sigma = 33$ denote the average number of gowns used per hospitalized COVID-19 patient per day.
- $GB = 20$ denote the number of gowns per box.

Weekly gown-box demand is estimated as:

$$
G_k^n = \left(\frac{7\sigma}{GB}\right)d_k^n
$$

Therefore, under the routine-use assumption, each additional hospitalized COVID-19 patient in the weekly average daily census corresponds to approximately:

$$
\frac{7 \times 33}{20} = 11.55
$$

gown boxes per week.

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

## Suggested Repository Structure

```text
sr-hcscm-case-study/
├── README.md
├── docs/
│   └── case-study.md
├── data/
│   └── README.md
├── Figures/
│   └── demand_distribution.png
└── src/
    └── README.md
```

## Notes

- The methodology is intended for research and simulation purposes.
- Hospital-level demand estimates are approximations based on public regional data, community population, geographic assignment, and hospital bed capacity.
- Direct hospital procurement or ward-level utilization data should be used when available.

