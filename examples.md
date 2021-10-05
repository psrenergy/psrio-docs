---
title: Examples
nav_order: 10
---

# Examples
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Circuit Loading

``` lua
circuit = Circuit();
cirflw = circuit:load("cirflw");
    
(cirflw:abs() / circuit.capacity):convert("%"):save("usecir");
```

## Deficit Risk per Year

``` lua
system = System();
deficit = system:load("defcit");

deficit = deficit:aggregate_blocks(BY_SUM());
deficit = deficit:aggregate_stages(BY_SUM(), Profile.PER_YEAR);
    
ifelse(deficit:gt(0), 1, 0):aggregate_scenarios(BY_AVERAGE()):convert("%"):save("deficit_risk");
```

## Thermal Generation per Fuel

``` lua
thermal = Thermal();
gerter = thermal:load("gerter");
gerter:aggregate_agents(BY_SUM(), Collection.FUEL):save("gerter_per_fuel");
```
