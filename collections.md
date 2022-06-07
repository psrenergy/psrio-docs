---
title: Collections
nav_order: 3
---

# Collections
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Loading a Collection

|           Collection          | Syntax                                                           |
|:------------------------------|:-----------------------------------------------------------------|
| Area                          | `area = Area()`                                                  |
| Balancing Area                | `balancing_area = BalancingArea()`                               |
| Balancing Area Hydro          | `balancing_area_hydro = BalancingAreaHydro()`                    |
| Balancing Area Thermal        | `balancing_area_thermal = BalancingAreaThermal()`                |
| Battery                       | `battery = Battery()`                                            |
| Bus                           | `bus = Bus()`                                                    |
| Circuit                       | `circuit = Circuit()`                                            |
| Circuits Sum                  | `circuits_sum = CircuitsSum()`                                   |
| DC Link                       | `dclink = DCLink()`                                              |
| Demand                        | `demand = Demand()`                                              |
| Demand Segment                | `demand_segment = DemandSegment()`                               |
| Expansion Capacity            | `expansion_capacity = ExpansionCapacity()`                       |
| Expansion Project             | `expansion_project = ExpansionProject()`                         |
| Fuel                          | `fuel = Fuel()`                                                  |
| Fuel Consumption              | `fuel_consumption = FuelConsumption()`                           |
| Fuel Contract                 | `fuel_contract = FuelContract()`                                 |
| Fuel Reservoir                | `fuel_reservoir = FuelReservoir()`                               |
| Generator                     | `generator = Generator()`                                        |
| Generation Constraint         | `generation_constraint = GenerationConstraint()`                 |
| Generic                       | `generic = Generic()`                                            |
| Hydro                         | `hydro = Hydro()`                                                |
| Interconnection               | `interconnection = Interconnection()`                            |
| Interconnection Sum           | `interconnection_sum = InterconnectionSum()`                     |
| Power Injection               | `power_injection = PowerInjection()`                             |
| Renewable                     | `renewable = Renewable()`                                        |
| Renewable Gauging Station     | `renewable_gauging_station = RenewableGaugingStation()`          |
| Reserve Generation Constraint | `reserve_generation_constraint = ReserveGenerationConstraint()`  |
| Study                         | `study = Study()`                                                |
| System                        | `system = System()`                                              |
| Thermal                       | `thermal = Thermal()`                                            |


## Loading an Output

| Operator    | Syntax                                 |
|:------------|:---------------------------------------|
| Load method | `output = collection:load("filename")` |

The following example loads two outputs, gerhid and fprodt, considering the agents as hydro plants collection:

#### Example 1
{: .no_toc }

``` lua
hydro = Hydro();
gerhid = hydro:load("gerhid");
fprodt = hydro:load("fprodt");
```

#### Example 2
{: .no_toc }

The following example loads two outputs, cmgdem and demand, considering the agents as system collection:

``` lua
system = System();
cmgdem = system:load("cmgdem");
demand = system:load("demand");
```

#### Example 3
{: .no_toc }

The following example loads two outputs, gerter and coster, considering the agents as thermal plants collection:

``` lua
thermal = Thermal();
gerter = thermal:load("gerter");
coster = thermal:load("coster");
```

#### Example 4
{: .no_toc }

The following example loads two outputs, objcop and outdfact, considering the agents as generic:

``` lua
generic = Generic();
objcop = generic:load("objcop");
outdfact = generic:load("outdfact");
```

**The generic collection can load any output with any agent type.**

## Loading an Input


<!-- ### Area -->

<!-- ### Balancing Area -->

<!-- ### Balancing Area Hydro -->

<!-- ### Balancing Area Thermal -->

<!-- ### Battery -->

<!-- ### Bus -->

<!-- ### Circuit

| Data                                                                | Unit | Syntax                                  |
|:--------------------------------------------------------------------|:----:|:----------------------------------------|
| Resistance                                                          | %    | `exp = circuit.resistance`              |
| Reactance                                                           | %    | `exp = circuit.reactance`               |
| Nominal Flow Limit                                                  | MW   | `exp = circuit.capacity`                |
| Emergency Flow Limit                                                | MW   | `exp = circuit.emergency_capacity`      |
| Circuit Operative Condition (connected = 0 or disconnected = 1)     | ---  | `exp = circuit.status`                  |
| Selected for Monitoring                                             | ---  | `exp = circuit.monitored`               |
| Selected for Monitoring (contingencies)                             | ---  | `exp = circuit.monitored_contingencies` |

### Circuits Sum

| Data             | Unit | Syntax                                  |
|:-----------------|:----:|:----------------------------------------|
| Lower bound      | MW   | `exp = circuits_sum.lb`                 |
| Upper bound      | MW   | `exp = circuits_sum.ub`                 |

### DC Link

| Data                                            | Unit | Syntax                                  |
|:------------------------------------------------|:----:|:----------------------------------------|
| DC Link Type (existing = 0 or future = 1)       | ---  | `exp = dclink.existing`                 |
| Nominal Flow Limit in the FROM -> TO direction  | MW   | `exp = dclink.capacity_right`           |
| Nominal Flow Limit in the FROM <- TO direction  | MW   | `exp = dclink.capacity_left`            |

### Demand

| Data                                                   | Unit | Syntax                                  |
|:-------------------------------------------------------|:----:|:----------------------------------------|
| Type of The First Level (inelastic = 0 or elastic = 1) | ---  | `exp = demand.is_elastic`               |
| Energy to be consumed by inelastic demand (hourly)     | MW   | `exp = demand.inelastic_hour`           |
| Energy to be consumed by inelastic demand (block)      | GWh  | `exp = demand.inelastic_block`          |

### Demand Segment

| Data                                             | Unit | Syntax                                  |
|:-------------------------------------------------|:----:|:----------------------------------------|
| Energy to be consumed by demand segment (hourly) | MW   | `exp = demand_segment.hour`             |
| Energy to be consumed by demand segment (block)  | GWh  | `exp = demand_segment.block`            |

### Expansion Project

| Data             | Unit     | Syntax                                 |
|:-----------------|:--------:|:---------------------------------------|
| O&M Cost         | $/kWyear | `exp = expansion_project.omcost`       |

### Fuel

| Data             | Unit  | Syntax                                 |
|:-----------------|:-----:|:---------------------------------------|
| Fuel Cost        | $/gal | `exp = fuel.cost`                      |

<!-- ### Fuel Consumption

### Fuel Contract

| Data                        | Unit  | Syntax                                  |
|:----------------------------|:-----:|:----------------------------------------|
| Fuel Cost                   | $/gal | `exp = fuel_contract.cost`              |
| Contracted Amount           | ---   | `exp = fuel_contract.amount`            |
| Take-or-Pay Fuel Cost       | $/gal | `exp = fuel_contract.take_or_pay`       |
| Extra Take-or-Pay Fuel Cost | $/gal | `exp = fuel_contract.extra_take_or_pay` |
| Maximum Offtake Rate        | ---   | `exp = fuel_contract.max_offtake`       |

### Fuel Reservoir

| Data                                    | Unit  | Syntax                                            |
|:----------------------------------------|:-----:|:--------------------------------------------------|
| Maximum Injection Limit                 | ---   | `exp = fuel_reservoir.maxinjection`               |
| Maximum Injection Limit (chronological) | ---   | `exp = fuel_reservoir.maxinjection_chronological` |

<!-- ### Generator

### Generation Constraint

| Data             | Unit  | Syntax                                           |
|:-----------------|:-----:|:-------------------------------------------------|
|                  | MW    | `exp = generation_constraint.capacity`           |

<!-- ### Generic

### Hydro

| Data                                                        | Unit  | Syntax                                                       |
|:------------------------------------------------------------|:-----:|:-------------------------------------------------------------|
| Construction Status (existing = 0 or future = 1)            | ---   | `exp = hydro.status`                                         |
| Number of Generating Units                                  | ---   | `exp = hydro.units`                                          |
| Total Installed Capacity                                    | MW    | `exp = hydro.capacity`                                       |
| Total Installed Capacity (maintenance)                      | MW    | `exp = hydro.capacity_maintenance`                           |
| Forced Outage Rate                                          | %     | `exp = hydro.FOR`                                            |
| Historical Outage Factor                                    | %     | `exp = hydro.COR`                                            |
| Minimum Storage                                             | hm3   | `exp = hydro.vmin`                                           |
| Maximum Storage                                             | hm3   | `exp = hydro.vmax`                                           |
| Minimum Turbining Outflow                                   | m3/s  | `exp = hydro.qmin`                                           |
| Maximum Turbining Outflow                                   | m3/s  | `exp = hydro.qmax`                                           |
| O&M Cost                                                    | $/MWh | `exp = hydro.omcost`                                         |
| Irrigation                                                  | m3/s  | `exp = hydro.irrigation`                                     |
| Target Storage Tolerance                                    | %     | `exp = hydro.target_storage_tolerance`                       |

| Data                                                        | Unit  | Syntax                                                       |
|:------------------------------------------------------------|:-----:|:-------------------------------------------------------------|
| Minimum Total Outflow                                       | m3/s  | `exp = hydro.min_total_outflow`                              |
| Minimum Total Outflow (modifications)                       | m3/s  | `exp = hydro.min_total_outflow_modification`                 |
| Minimum Total Outflow Historical Scenarios                  | m3/s  | `exp = hydro.min_total_outflow_historical_scenarios`         |
| Minimum Total Outflow Historical Scenarios (no data)        | ---   | `exp = hydro.min_total_outflow_historical_scenarios_nodata`  |
| Maximum Total Outflow                                       | m3/s  | `exp = hydro.max_total_outflow`                              |
| Maximum Total Outflow Historical Scenarios                  | m3/s  | `exp = hydro.max_total_outflow_historical_scenarios`         |
| Maximum Total Outflow Historical Scenarios (no data)        | ---   | `exp = hydro.max_total_outflow_historical_scenarios_nodata`  |
| Minimum Volume                                              | hm3   | `exp = hydro.vmin_chronological`                             |
| Minimum Volume Historical Scenarios                         | hm3   | `exp = hydro.vmin_chronological_historical_scenarios`        |
| Minimum Volume Historical Scenarios (no data)               | ---   | `exp = hydro.vmin_chronological_historical_scenarios_nodata` |
| Maximum Volume                                              | hm3   | `exp = hydro.vmax_chronological`                             |
| Maximum Volume Historical Scenarios                         | hm3   | `exp = hydro.vmax_chronological_historical_scenarios`        |
| Maximum Volume Historical Scenarios (no data)               | ---   | `exp = hydro.vmax_chronological_historical_scenarios_nodata` |
| Flood Control Storage                                       | hm3   | `exp = hydro.flood_control`                                   |
| Flood Control Storage Historical Scenarios                  | hm3   | `exp = hydro.flood_control_historical_scenarios`              |
| Flood Control Storage Historical Scenarios (no data)        | ---   | `exp = hydro.flood_control_historical_scenarios_nodata`       |
| Alert Storage                                               | hm3   | `exp = hydro.alert_storage`                                  |
| Alert Storage Historical Scenarios                          | hm3   | `exp = hydro.alert_storage_historical_scenarios`             |
| Alert Storage Historical Scenarios (no data)                | ---   | `exp = hydro.alert_storage_historical_scenarios_nodata`      |
| Min Spillage                                                | m3/s  | `exp = hydro.min_spillage`                                   |
| Min Spillage Historical Scenarios                           | m3/s  | `exp = hydro.min_spillage_historical_scenarios`              |
| Min Spillage Historical Scenarios (no data)                 | ---   | `exp = hydro.min_spillage_historical_scenarios_nodata`       |
| Max Spillage                                                | m3/s  | `exp = hydro.max_spillage`                                   |
| Max Spillage Historical Scenarios                           | m3/s  | `exp = hydro.max_spillage_historical_scenarios`              |
| Max Spillage Historical Scenarios (no data)                 | ---   | `exp = hydro.max_spillage_historical_scenarios_nodata`       |
| Min Bio Spillage                                            | %     | `exp = hydro.min_bio_spillage`                               |
| Min Bio Spillage Historical Scenarios                       | %     | `exp = hydro.min_bio_spillage_historical_scenarios`          |
| Min Bio Spillage Historical Scenarios (no data)             | ---   | `exp = hydro.min_bio_spillage_historical_scenarios_nodata`   |
| Target Storage                                              | hm3   | `exp = hydro.target_storage`                                 |
| Target Storage Historical Scenarios                         | hm3   | `exp = hydro.target_storage_historical_scenarios`            |
| Target Storage Historical Scenarios (no data)               | ---   | `exp = hydro.target_storage_historical_scenarios_nodata`     |

### Interconnection

| Data                                 | Unit  | Syntax                                                       |
|:-------------------------------------|:-----:|:-------------------------------------------------------------|
| Power Exchange Capacity (FROM -> TO) | MW    | `exp = interconnection.capacity_right`                       |
| Power Exchange Capacity (TO -> FROM) | MW    | `exp = interconnection.capacity_left`                        |
| Transmission Cost (TO -> FROM)       | $/MWh | `exp = interconnection.cost_right`                           |
| Transmission Cost (FROM -> TO)       | $/MWh | `exp = interconnection.cost_left`                            |

### Interconnection Sum

| Data                | Unit  | Syntax                                                       |
|:--------------------|:-----:|:-------------------------------------------------------------|
| Lower Bound         | MW   | `exp = interconnection_sum.lb`                                |
| Upper Bound         | MW   | `exp = interconnection_sum.ub`                                |


### Power Injection

| Data                | Unit  | Syntax                                                        |
|:--------------------|:-----:|:--------------------------------------------------------------|
| Hourly Capacity     | MW    | `exp = power_injection.hour_capacity`                         |
| Hourly Price        | $/MWh | `exp = power_injection.hour_price`                            |

### Renewable

| Data                                             | Unit  | Syntax                                                       |
|:-------------------------------------------------|:-----:|:-------------------------------------------------------------|
| Construction Status (existing = 0 or future = 1) |       | `exp = renewable.existing`                                   |
| Technology Type                                  |       | `exp = renewable.tech_type`                                  |
| Installed Capacity                               | MW    | `exp = renewable.capacity`                                   |
| O&M Cost                                         | $/MWh | `exp = renewable.omcost`                                     | -->

<!-- ### Renewable Gauging Station

| Data                | Unit  | Syntax                                                       |
|:--------------------|:-----:|:-------------------------------------------------------------|
|                     | MW   | `exp = renewable_gauging_station.hourgeneration`                |

<!-- ### Reserve Generation Constraint

### Study

| Data                | Unit  | Syntax                                                       |
|:--------------------|:-----:|:-------------------------------------------------------------|
|                     | %     | `exp = study.discount_rate`                                  |
|                     | ---   | `exp = study.stage`                                          |
|                     | ---   | `exp = study.stage_in_year`                                  |
|                     | ---   | `exp = study.scenario`                                       |
|                     | ---   | `exp = study.block`                                          |
|                     | ---   | `exp = study.blocks`                                         |
|                     | ---   | `exp = study.hour`                                           |
|                     | ---   | `exp = study.hours`                                          |

| Method              | Type    | Syntax                                                       |
|:--------------------|:-------:|:-------------------------------------------------------------|
|                     | boolean | `attribute = study:is_hourly()`                              |
|                     | boolean | `attribute = study:is_genesys()`                             |
|                     | boolean | `attribute = study:is_hourly_load()`                         |
|                     | string  | `attribute = study:path()`                                   |
|                     | string  | `attribute = study:parent_path()`                            |
|                     | number  | `attribute = study:get_parameter(key)`                       |
|                     | number  | `attribute = study:stages()`                                 |
|                     | number  | `attribute = study:stages_per_year()`                        |
|                     | number  | `attribute = study:scenarios()`                              | 
|                     | number  | `attribute = study:scenarios()`                              | 
-->

### System

| Data                                         | Unit    |
|:---------------------------------------------|:-------:|
| `system.load_level_length`                   | ---     |
| `system.hour_block_map`                      | ---     |
| `system.sensitivity`                         | ---     |

### Thermal

| Data                                         | Unit    |
|:---------------------------------------------|:-------:|
| `thermal.state`                              | ---     |
| `thermal.system`                             | ---     |
| `thermal.minimum_generation`                 | MW      |
| `thermal.minimum_generation_available`       | MW      |
| `thermal.minimum_generation_constraint`      | MW      |
| `thermal.maximum_generation`                 | MW      |
| `thermal.maximum_generation_available`       | MW      |
| `thermal.forced_outage_rate`                 | %       |
| `thermal.historical_outage_factor`           | %       |
| `thermal.startup_cost`                       | k$      |
| `thermal.om_cost`                            | $/MWh   |
| `thermal.specific_consumption_segment_1`     | gal/MWh |
| `thermal.specific_consumption_segment_2`     | gal/MWh |
| `thermal.specific_consumption_segment_3`     | gal/MWh |
| `thermal.fuel_transportation_cost`           | $/gal   |
| `thermal.operation_mode`                     | ---     |
| `thermal.forced_generation`                  | MW      |

## Loading a Generic CSV

| Operator          | Syntax                                     |
|:------------------|:-------------------------------------------|
| Load table method | `table = study:load_table("filename.csv")` |

The following example loads a generic csv table and iterates through the rows:

#### Example 1
{: .no_toc }

`file.csv`:
```
Name         , Node, Interval, Startup, Participation
H_1_1        , 1   , 1       , 1      , 0.79
R_1_3        , 1   , 1       , 1      , 0.20
CONTRATO_FLAT, 1   , 1       , 1      , 1
```

script:
```lua
study = Study();
table = study:load_table("file.csv");

info("Name,Node,Interval,Startup,Participation");
for i=1,#table do
    info(
        table[i]["Name"] .. "," .. 
        table[i]["Node"] .. "," .. 
        table[i]["Interval"] .. "," .. 
        table[i]["Startup"] .. "," .. 
        table[i]["Participation"]
    );
end
```