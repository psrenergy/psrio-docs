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

## Renewable Generation per Technology

``` lua
renewable = Renewable();
gergnd = renewable:load("gergnd");

general = renewable.tech_type:eq(0);
wind = renewable.tech_type:eq(1);
solar = renewable.tech_type:eq(2);
biomass = renewable.tech_type:eq(3);
small_hydro = renewable.tech_type:eq(4);

concatenate(
    gergnd:select_agents(general):aggregate_agents(BY_SUM(), "General"),
    gergnd:select_agents(wind):aggregate_agents(BY_SUM(), "Wind"),
    gergnd:select_agents(solar):aggregate_agents(BY_SUM(), "Solar"),
    gergnd:select_agents(biomass):aggregate_agents(BY_SUM(), "Biomass"),
    gergnd:select_agents(small_hydro):aggregate_agents(BY_SUM(), "Small Hydro")
):save("gergnd_per_technology");
```