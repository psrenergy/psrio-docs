---
title: Attributes
nav_order: 4
---

# Attributes
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

Attributes are properties that characterize an object or, in the context of PSRIO, an expression. Expressions related to results of a PSR model, whose data can change over stages, scenarios, and blocks, for example, have a set of attributes in PSRIO that can be useful in some tasks. In the example below, we load the thermal generation:

``` lua
thermal = Thermal();
gerter = thermal:load("gerter");
```

And the log shows some expression's attributes information:

```sh
[info] Loading gerter (stages: 12 [1:12] [monthly] [10/2016], blocks: hour, scenarios: 1200, unit: GWh, agents: 3 [thermal])
```
From the log, it is possible to see that `gerter` has:
* 12 stages in a month-level resolution
* initial stage in 10/2016
* blocks 
* 1200 scenarios
* generation data given in GWh
* 3 agents

All attributes related to stages, block, scenarios, agents and units of an expression and their respective methods are presented in the tables below.

## Stage

| Method          | Type            |            Syntax                 |
|:----------------|:---------------:|:----------------------------------|
| Stages          | number          | `attribute = exp:stages()`        |
| Stage           | number          | `attribute = exp:stage(index)`    |
| Stage Type      | number          | `attribute = exp:stage_type()`    |
| Initial Stage   | number          | `attribute = exp:initial_stage()` |
| Initial Year    | number          | `attribute = exp:initial_year()`  |
| Final Year      | number          | `attribute = exp:final_year()`    |
| Week            | number          | `attribute = exp:week(stage)`     |
| Month           | number          | `attribute = exp:month(stage)`    |
| Year            | number          | `attribute = exp:year(stage)`     |

#### Example 
{: .no_toc}

```lua 
stages = gerter:stages() -- 12

stage1 = gerter:stage(1) -- 10
stage2 = gerter:stage(2) -- 11
stage3 = gerter:stage(3) -- 12
stage4 = gerter:stage(4) -- 13

stage_type = gerter:stage_type() -- 2 (monthly)

initial_stage = gerter:initial_stage() -- 10
initial_year = gerter:initial_year() -- 2016
final_year = gerter:final_year() -- 2017

month1 = gerter:month(1) -- 10
month2 = gerter:month(2) -- 11
month3 = gerter:month(3) -- 12
month4 = gerter:month(4) --  1

year1 = gerter:year(1) -- 2016
year4 = gerter:year(4) -- 2017
```

## Blocks

| Method          | Type            |            Syntax                 |
|:----------------|:---------------:|:----------------------------------|
| Blocks          | number          | `attribute = exp:blocks(stage)`   |
| Has blocks      | boolean         | `attribute = exp:has_blocks()`    |
| Is hourly       | boolean         | `attribute = exp:is_hourly()`     |

#### Example 
{: .no_toc}

```lua 
blocks1 = gerter:blocks(1) -- 744
blocks2 = gerter:blocks(2) -- 720

has_blocks = gerter:has_blocks() -- true

is_hourly = gerter:is_hourly() -- true
```

## Scenarios

| Method          | Type            |            Syntax                 |
|:----------------|:---------------:|:----------------------------------|
| Scenarios       | number          | `attribute = exp:scenarios()`     |

#### Example 
{: .no_toc}

```lua 
scenarios = gerter:scenarios() -- 1200
```

## Agents

| Method          | Type              |            Syntax                 |
|:----------------|:-----------------:|:----------------------------------|
| Agents          | vector of strings | `attribute = exp:agents()`        |
| Agents Size     | number            | `attribute = exp:agents_size()`   |
| Agent           | string            | `attribute = exp:agent(index)`    |

#### Example 
{: .no_toc}

```
[info] Thermal 1 at index 1
[info] Thermal 2 at index 2
[info] Thermal 3 at index 3
```

```lua 
agents = gerter:agents() -- {"Thermal 1", "Thermal 2", "Thermal 3"}
for i, agent in ipairs(agents) do
    info(agent .. " at index " .. i);
end
```

```lua 
size = gerter:agents_size() -- 3
for i = 1,size do
    agent = gerter:agent(i);
    info(agent .. " at index " .. i);
end
```

## Unit

| Method          | Type            |            Syntax                 |
|:----------------|:---------------:|:----------------------------------|
| Unit            | string          | `attribute = exp:unit()`          |

#### Example 
{: .no_toc}

```lua 
unit = gerter:unit() -- "GWh"
```