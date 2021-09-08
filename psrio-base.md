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

[PSRIO Base](https://github.com/psrenergy/psrio-base) is a library embedded with PSRIO that provides implementations of PSR model outputs. Instead of rewriting recipes or searching for written similar scripts, we can use the functions provided by PSRIO Base.

We use the function `require` to load the scripts defined in the PSRIO Base. 

## SDDP

### Useful Storage

The [sddp/useful_storage](https://github.com/psrenergy/psrio-base/blob/master/sddp/useful_storage.lua) defines the useful storage output:

```lua
local function useful_storage()
    local hydro = Hydro();
    return hydro.vmax - hydro.vmin;
end
return useful_storage;
```

First, we load the script with the `require` function and then save the output.

```lua
local useful_storage = require("sddp/useful_storage");
useful_storage():save("useful_storage");
```

### Circuit Loading

The second example presents the [sddp/usecir](https://github.com/psrenergy/psrio-base/blob/master/sddp/usecir.lua) script:

``` lua
local function usecir(suffix)
    local circuit = Circuit();
    local cirflw = circuit:load("cirflw" .. (suffix or ""));
    return (cirflw:abs() / circuit.capacity):convert("%");
end
return usecir;
```

The circuit loading data is helpful to monitor the electric system represented in SDDP, indicating eventual overload in the system's circuits. The following code shows how we load and save the percentage of power flow concerning the circuit capacity:

``` lua
local usecir = require("sddp/usecir");
usecir():save("usecir");
```

### Hydro Generation per Bus

The third example loads the [sddp/gerhid_per_bus](https://github.com/psrenergy/psrio-base/blob/master/sddp/gerhid_per_bus.lua) script. Hydro generation per bus allows understanding the contribution of hydropower plants to the power injected in the buses declared in SDDP. 

```lua
local function gerhid_per_bus(suffix)
    local hydro = Hydro();
    local gerhid = hydro:load("gerhid" .. (suffix or ""));
    return gerhid:aggregate_agents(BY_SUM(), Collection.BUSES);
end
return gerhid_per_bus;
```

```lua
local gerhid_per_bus = require("sddp/gerhid_per_bus");

local suffixes = {"__day", "__week", "__hour", "__trueup"};
for i, suffix in ipairs(suffixes) do 
    gerhid_per_bus(suffix):save("gerhid_per_bus" .. suffix); 
end
```

### Deficit Risk per Year

Deficit risk is measured by the probability that the system will not supply the energy demand by the load. The function [sddp/defcit_risk](https://github.com/psrenergy/psrio-base/blob/master/sddp/defcit_risk.lua) executes the calculation of the deficit risk per year in the study case. 

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
local defcit_risk = require("sddp/defcit_risk");
defcit_risk():save("defcit_risk");
```

## SDDP Reports

### Load Marginal Cost Report

The load marginal cost determines the spot price of the electrical energy in the market. The [sddp-reports/sddpcmgd](https://github.com/psrenergy/psrio-base/blob/master/sddp-reports/sddpcmgd.lua) defines the script that averages the blocks and scenarios of the cmgdem output:

```lua
local function sddpcmgd(suffix)
    local system = System();
    local cmgdem = system:load("cmgdem" .. (suffix or ""));
    return cmgdem:aggregate_blocks(BY_AVERAGE()):aggregate_scenarios(BY_AVERAGE());
end
return sddpcmgd;
```

```lua
local sddpcmgd = require("sddp/sddpcmgd");
sddpcmgd():save("sddpcmgd");
```

### Averaged Load Marginal Cost Report

Similar to `sddpcmgd`, the [sddp-reports/sddpcmga](https://github.com/psrenergy/psrio-base/blob/master/sddp-reports/sddpcmga.lua) averages the load marginal cost per year.

```lua
local function sddpcmga(suffix)
    local sddpcmgd = require("sddp-reports/sddpcmgd");
    return sddpcmgd(suffix):aggregate_stages(BY_AVERAGE(), Profile.PER_YEAR);
end
return sddpcmga;
```

```lua
local sddpcmga = require("sddp/sddpcmga");
sddpcmga():save("sddpcmga");
```