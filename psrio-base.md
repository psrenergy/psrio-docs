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

https://github.com/psrenergy/psrio-base

## SDDP

https://github.com/psrenergy/psrio-base/tree/master/sddp

### Useful Storage

https://github.com/psrenergy/psrio-base/blob/master/sddp/useful_storage.lua

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

### Circuit Loading (usecir)

https://github.com/psrenergy/psrio-base/blob/master/sddp/usecir.lua

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

https://github.com/psrenergy/psrio-base/blob/master/sddp/gerhid_per_bus.lua

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

https://github.com/psrenergy/psrio-base/blob/master/sddp/defcit_risk.lua

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

https://github.com/psrenergy/psrio-base/tree/master/sddp-reports

### Generation Report (sddpgrxxd)

```lua
local function sddpgrxxd(suffix)
    local hydro = Hydro();
    local gerhid = hydro:load("gerhid" .. (suffix or ""));

    local thermal = Thermal();
    local gerter = thermal:load("gerter" .. (suffix or ""));
    
    local renewable = Renewable();
    local gergnd = renewable:load("gergnd" .. (suffix or ""));
    
    local battery = Battery()
    local gerbat = battery:load("gerbat" .. (suffix or ""));

    local system = System()
    local defcit = system:load("defcit" .. (suffix or ""));

    return concatenate(
        gerhid:aggregate_agents(BY_SUM(), "Total Hydro"):aggregate_blocks(BY_SUM()):aggregate_scenarios(BY_AVERAGE()),
        gerter:aggregate_agents(BY_SUM(), "Total Thermal"):aggregate_blocks(BY_SUM()):aggregate_scenarios(BY_AVERAGE()),
        gergnd:aggregate_agents(BY_SUM(), "Total Renewable"):aggregate_blocks(BY_SUM()):aggregate_scenarios(BY_AVERAGE()),
        gerbat:aggregate_agents(BY_SUM(), "Total Battery"):aggregate_blocks(BY_SUM()):aggregate_scenarios(BY_AVERAGE()):convert("GWh"),
        defcit:aggregate_agents(BY_SUM(), "Deficit"):aggregate_blocks(BY_SUM()):aggregate_scenarios(BY_AVERAGE())
    );
end
return sddpgrxxd;
```

```lua
sddpgrxxd = require("sddp/sddpgrxxd");
sddpgrxxd():save("sddpgrxxd");
```