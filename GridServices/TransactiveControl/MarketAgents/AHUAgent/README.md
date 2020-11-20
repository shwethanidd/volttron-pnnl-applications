# AHU Agent

It is the transactive controls and coordination (TCC) agent that interacts with volttron market service
 as a consumer electricity and a supplier of cooling air to VAV TCC agent that represent VAV hardware devices
 that provide cooling to building zones 

## AHU Agent Configuration

* Agent config file:

```` json
{
    "campus": "CAMPUS", # if omitted defaults to ""
    "building": "BUILDING", # if omitted defaults to ""
    "input_data_timezone": "UTC", # if omitted defaults to "UTC"
    "supplier_market_name": "air1",
	"consumer_market_name": "electric",

    "agent_name": "ahu1",
    "inputs": [
        {
            "mapped": "sfs", # mapped value does not change (for rtu agent or vav agent)
            "point": "SupplyFanStatus",
            "topic": "devices/CAMPUS/BUILDING/AHU1/all",
            "inital_value": 0
        },
        {
            "mapped": "oat",
            "point": "OutdoorAirTemperature",
            "topic": "devices/CAMPUS/BUILDING/AHU1/all",
            "inital_value": 21.1
        },
        {
            "mapped": "mat",
            "point": "MixedAirTemperature",
            "topic": "devices/CAMPUS/BUILDING/AHU1/all",
            "inital_value": 21.1
        },
        {
            "mapped": "dat",
            "point": "DischargeAirTemperature",
            "topic": "devices/CAMPUS/BUILDING/AHU1/all",
            "inital_value": 13.8
        },
        {
            "mapped": "saf",
            "point": "SupplyAirFlow",
            "topic": "devices/CAMPUS/BUILDING/AHU1/all",
            "inital_value": 0.0
        }
    ],
    "outputs": [],
    "schedule":{},
    "model_parameters": {
        "equipment_configuration": {
            "has_economizer": true,
            "economizer_limit": 18.33,
            "supply-air sepoint": 13.0,
            "nominal zone-setpoint": 21.1,
            "building chiller": true
        },
        "model_configuration": {
            "c0": 0.0024916812889370643,
            "c1": 0.53244827213615642,
            "c2": -0.15144710994850016,
            "c3": 0.060900887939007789,
            "cpAir": 1.006, #kj/kgK for calculation of kW
            "COP" : 5.5
        }
    }
}
````
User can create a config file using the tcc-config-web-tool: https://tcc-configuration-tool.web.app/
and following instruction from the tcc-userguide https://tcc-userguide.readthedocs.io/en/latest/

## Install and activate VOLTTRON environment
For installing, starting, and activating the VOLTTRON environment, refer to the following VOLTTRON readthedocs: 
https://volttron.readthedocs.io/en/develop/introduction/platform-install.html

## Installing and Running AHU Agent
Install and start the AHU Agent using the script install-agent.py as describe below:

```
python scripts/install-agent.py -s <top most folder of the agent> 
                                -c <Agent config file>
                                -i agent.AHU
                                -t AHU
                                --start --force
```
-s : followed by path of top most folder of the AHU agent

-c : followed by path of the agent config file

-i : followed by agent identity

-t : followed by name tag
 
--start (optional): start after installation

--force (optional): overwrites existing ilc agent with identity "agent.AHU"  


