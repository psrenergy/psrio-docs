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
useful_storage = require("sddp/useful_storage");
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
usecir = require("sddp/usecir");
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

We can load the `gerhid_per_bus` script with different suffixes. In PSR, we have models the produce the `gerhid__day`, `gerhid__week`, `gerhid__hour`, and `gerhid__trueup`:

```lua
gerhid_per_bus = require("sddp/gerhid_per_bus");

suffixes = {"__day", "__week", "__hour", "__trueup"};
for i, suffix in ipairs(suffixes) do 
    gerhid_per_bus(suffix):save("gerhid_per_bus" .. suffix); 
end
```

### Deficit Risk per Year

Deficit risk is measured by the probability that the system will not supply the energy demand by the load. The function [sddp/defcit_risk](https://github.com/psrenergy/psrio-base/blob/master/sddp/defcit_risk.lua) executes the calculation of the deficit risk per year in the study case. 

```lua
defcit_risk = require("sddp/defcit_risk");
defcit_risk():save("defcit_risk");
```

## SDDP Reports

### Load Marginal Cost Report

The load marginal cost determines the spot price of the electrical energy in the market. The [sddp-reports/sddpcmgd](https://github.com/psrenergy/psrio-base/blob/master/sddp-reports/sddpcmgd.lua) defines the script that averages the blocks and scenarios of the cmgdem output:

```lua
sddpcmgd = require("sddp-reports/sddpcmgd");
sddpcmgd():save("sddpcmgd");
```

### Averaged Load Marginal Cost Report

Similar to `sddpcmgd`, the [sddp-reports/sddpcmga](https://github.com/psrenergy/psrio-base/blob/master/sddp-reports/sddpcmga.lua) averages the load marginal cost per year.


```lua
sddpcmga = require("sddp-reports/sddpcmga");
sddpcmga():save("sddpcmga");
```