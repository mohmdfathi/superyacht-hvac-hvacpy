# superyacht-hvac-hvacpy
Concept-level HVAC sizing for a synthetic superyacht accommodation zone, implemented in Python with [`hvacpy`](https://pypi.org/project/hvacpy/), an ASHRAE-based open-source HVAC calculation package.

![Superyacht accommodation zone deck plans and HVAC zones](superyacht.png)

Covers CLTD/CLF peak zone cooling loads, psychrometric fresh-air AHU sizing, FCU selection per room, chilled-water plant sizing with N+1 redundancy, concept duct sizing, a winter heating check, and an independent supplier-proposal verification.

## Result summary

| Quantity | Value |
|---|---|
| Peak zone cooling load | 23.2 kW (16:00, SHR 0.84) |
| Fresh-air AHU duty | 24.7 kW (0.48 m³/s, 48 occupants) |
| Combined plant load | 47.3 kW (SHR 0.56) |
| Chiller plant | 3 × 26.0 kW, N+1, COP 5.5 |
| Condenser heat rejection | 55.9 kW |
| Main duct | 400 mm dia., 3.8 m/s, 0.42 Pa/m |
| Winter heating load | 7.6 kW |
| Supplier-proposal check | 2 of 3 requirements met (fresh-air flow undersized) |

## Contents

- [`superyacht_hvac_concept_minimal.ipynb`](superyacht_hvac_concept_minimal.ipynb) — the calculation
- [`theory.md`](theory.md) — plain-language walkthrough of the engineering logic

## Scope

All geometry, loads, and equipment figures are synthetic inputs for a portfolio exercise, not a real yacht design. 
 
