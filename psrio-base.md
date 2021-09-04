---
title: PSRIO Base
nav_order: 9
---

# PSRIO Base
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

PSRIO Base is a library whose purpose is to provide a set of functions which execute frequently requested output processing tasks. So, instead of rewriting PSRIO recipes, or searching for written similar scripts, one can make use of the functions offered by PSRIO Base.

PSRIO Base repository: [psrio-base](https://github.com/psrenergy/psrio-base)

## SDDP

SDDP is one of the most used model of PSR. PSRIO Base has a wide collection of functions to help users create their scripts for post processing tasks.

PSRIO Base library for SDDP: [psrio-base/sddp](https://github.com/psrenergy/psrio-base/tree/master/sddp)

### Useful Storage

Useful storage is the part of the reservoir from the hidroelectric plant that can be turbined for energy conversion in the electric generators. Below, it is possible to see how the function that calculates this parameter is defined in PSRIO Base:

link to function definition: [psrio-base/sddp/useful_storage.lua](https://github.com/psrenergy/psrio-base/blob/master/sddp/useful_storage.lua)

```lua
local function useful_storage()
    local hydro = Hydro();
    return hydro.vmax - hydro.vmin;
end
return useful_storage;
```

To use the funcion above, the `require` command must be used in order to access the method. After that, the function can be used in the script as shown below:
```lua
useful_storage = require("sddp/useful_storage");
useful_storage():save("useful_storage");
```

### Circuit Loading

The circuit loading data is useful to monitor the electric system represented in SDDP, indicating eventual overload in the system's circuits. The percentage of power flow with respect to the circuit capacity is calculated throught use of the function `usecir` provided by PSRIO Base.

link to function definition: [psrio-base/sddp/usecir.lua](https://github.com/psrenergy/psrio-base/blob/master/sddp/usecir.lua)

``` lua
local function usecir(suffix)
    local circuit = Circuit();
    local cirflw = circuit:load("cirflw" .. (suffix or ""));
    return (cirflw:abs() / circuit.capacity):convert("%");
end
return usecir;
```

``` lua
usecir = require("sddp/usecir");
usecir():save("usecir");
```

### Hydro Generation per Bus

Hydro generation per bus allows the knowledge of the contribution of hydro power plants to the power injected in the buses declared in SDDP. This variable can be calculated by the function `gerhid_per_bus`.

link to function definition: [psrio-base/sddp/gerhid_per_bus.lua](https://github.com/psrenergy/psrio-base/blob/master/sddp/gerhid_per_bus.lua)

```lua
local function gerhid_per_bus(suffix)
    local hydro = Hydro();
    local gerhid = hydro:load("gerhid" .. (suffix or ""));
    return gerhid:aggregate_agents(BY_SUM(), Collection.BUSES);
end
return gerhid_per_bus;
```

```lua
gerhid_per_bus = require("sddp/gerhid_per_bus");

suffixes = {"__day", "__week", "__hour", "__trueup"};
for i, suffix in ipairs(suffixes) do 
    gerhid_per_bus(suffix):save("gerhid_per_bus" .. suffix); 
end
```

### Deficit Risk per Year

Deficit risk is measured by the probability that the system will not be able to supply the energy demand by the load. The function `deficit_risk` executes the calculation of the deficit risk per year in study case. 

link to function definition: [psrio-base/sddp/defcit_risk.lua](https://github.com/psrenergy/psrio-base/blob/master/sddp/defcit_risk.lua)

```lua
local function defcit_risk(suffix)
    local system = System();
    local defcit = system:load("defcit" .. (suffix or "")); 
    return ifelse(defcit:aggregate_blocks(BY_SUM())
        :aggregate_stages(BY_SUM(), Profile.PER_YEAR):gt(0), 1, 0)
        :aggregate_scenarios(BY_AVERAGE()):convert("%");
end
return defcit_risk;
```

```lua
defcit_risk = require("sddp/defcit_risk");
defcit_risk():save("defcit_risk");
```

## SDDP Reports

SDDP reports are remarkable outputs produced by the model. PSRIO Base also provides a set of functions for these types of outputs.

PSRIO Base library for SDDP reports: [psrio-base/sddp-reports](https://github.com/psrenergy/psrio-base/tree/master/sddp-reports)

### Load Marginal Cost Report

The load marginal cost is a valuable information, since, in some systems, it determines the spot price of the electrical energy in the market. Throught the use of `sddpcmgd` it is possible to calculate this variable.

link to function definition: [psrio-base/sddp-reports/sddpcmgd.lua](https://github.com/psrenergy/psrio-base/blob/master/sddp-reports/sddpcmgd.lua)

```lua
local function sddpcmgd(suffix)
    local system = System();
    local cmgdem = system:load("cmgdem" .. (suffix or ""));
    return cmgdem:aggregate_blocks(BY_AVERAGE()):aggregate_scenarios(BY_AVERAGE());
end
return sddpcmgd;
```

```lua
sddpcmgd = require("sddp/sddpcmgd");
sddpcmgd():save("sddpcmgd");
```

### Averaged Load Marginal Cost Report

Similar to `sddpcmgd`, the function `sddpcmga` returns the average load marginal cost of a system considering the stages of a case study.

link to function definition: [psrio-base/sddp-reports/sddpcmga.lua](https://github.com/psrenergy/psrio-base/blob/master/sddp-reports/sddpcmga.lua)

```lua
local function sddpcmga(suffix)
    local sddpcmgd = require("sddp-reports/sddpcmgd");
    return sddpcmgd(suffix):aggregate_stages(BY_AVERAGE(), Profile.PER_YEAR);
end
return sddpcmga;
```

```lua
sddpcmga = require("sddp/sddpcmga");
sddpcmga():save("sddpcmga");
```

