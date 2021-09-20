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

Some of the attributes, and its values, of `gerter` in this example are:

`exp:stages()` = 12

`gerter:initial_stage()` = 10

`gerter:initial_year()` = 2016

`gerter:stage_type()` = 2 

`gerter:is_hourly()` = true

`gerter:scenarios()` = 1200

`gerter:unit()` = "GWh"

`gerter:agents()` = {"Thermal 1", "Thermal 2", "Thermal 3"}
   
`gerter:agents_size()` = 3

All attributes related to stages, block, scenarios, agents and units of an expression are presented in the tables below.

## Stage

| Operator        | Type            |            Syntax                 |
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

## Blocks

| Operator        | Type            |            Syntax                 |
|:----------------|:---------------:|:----------------------------------|
| Blocks          | number          | `attribute = exp:blocks(stage)`   |
| Has blocks      | boolean         | `attribute = exp:has_blocks()`    |
| Is hourly       | boolean         | `attribute = exp:is_hourly()`     |

## Scenarios

| Operator        | Type            |            Syntax                 |
|:----------------|:---------------:|:----------------------------------|
| Scenarios       | number          | `attribute = exp:scenarios()`     |

## Agents

| Operator        | Type              |            Syntax                 |
|:----------------|:-----------------:|:----------------------------------|
| Agents          | vector of strings | `attribute = exp:agents()`        |
| Agents Size     | number            | `attribute = exp:agents_size()`   |
| Agent           | string            | `attribute = exp:agent(index)`    |

## Unit

| Operator        | Type            |            Syntax                 |
|:----------------|:---------------:|:----------------------------------|
| Unit            | string          | `attribute = exp:unit()`          |