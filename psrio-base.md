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

SDDP is one of the most used model of PSR. PSRIO Base has wide collection of functions to help users create their scripts for post processing tasks.

PSRIO Base library for SDDP: [psrio-base/sddp](https://github.com/psrenergy/psrio-base/tree/master/sddp)

### Useful Storage

[psrio-base/sddp/useful_storage.lua](https://github.com/psrenergy/psrio-base/blob/master/sddp/useful_storage.lua)

```lua
local function useful_storage()
    local hydro = Hydro();
    return hydro.vmax - hydro.vmin;
end
return useful_storage;
```

```lua
useful_storage = require("sddp/useful_storage");
useful_storage():save("useful_storage");
```

### Circuit Loading

[psrio-base/sddp/usecir.lua](https://github.com/psrenergy/psrio-base/blob/master/sddp/usecir.lua)

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

[psrio-base/sddp/gerhid_per_bus.lua](https://github.com/psrenergy/psrio-base/blob/master/sddp/gerhid_per_bus.lua)

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

[psrio-base/sddp/defcit_risk.lua](https://github.com/psrenergy/psrio-base/blob/master/sddp/defcit_risk.lua)

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

[psrio-base/sddp-reports](https://github.com/psrenergy/psrio-base/tree/master/sddp-reports)

### Load Marginal Cost Report

[psrio-base/sddp-reports/sddpcmgd.lua](https://github.com/psrenergy/psrio-base/blob/master/sddp-reports/sddpcmgd.lua)

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

[psrio-base/sddp-reports/sddpcmga.lua](https://github.com/psrenergy/psrio-base/blob/master/sddp-reports/sddpcmga.lua)

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

