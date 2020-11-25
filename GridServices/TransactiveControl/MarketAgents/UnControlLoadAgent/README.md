# UnControlLoad Agent

Transactive control and coordination (TCC) agent that interacts with the volttron market service
 as a consumer electricity. This agent bids in a fixed demand curve to the volttron market service that represents
 the non-transactive (non-controllable) building load.
  

## UnControlLoad Agent Configuration

You can specify the configuration in either json or yaml format. The json format is specified below:

* Agent config file:

```` json
{
	"market_name": "electric",
	"campus": "PNNL",
    "building": "BUILDING1",
	"agent_name": "uncontrol",
    "sim_flag": true, 	
	"devices": {
        "LIGHTING/Basement": {
            "points": ["Power"],
            "conversion": "Power/1000.0"
        },
        "LIGHTING/Core_bottom": {
            "points": ["Power"],
            "conversion": "Power/1000.0"
        },
        "LIGHTING/Core_mid": {
            "points": ["Power"],
            "conversion": "Power/1000.0"
        },
        "LIGHTING/Core_top": {
            "points": ["Power"],
            "conversion": "Power/1000.0"
        },
        "LIGHTING/Perimeter_bot_ZN_1": {
            "points": ["Power"],
            "conversion": "Power/1000.0"
        },
        "LIGHTING/Perimeter_bot_ZN_2": {
            "points": ["Power"],
            "conversion": "Power/1000.0"
        },
        "LIGHTING/Perimeter_bot_ZN_3": {
            "points": ["Power"],
            "conversion": "Power/1000.0"
        },
        "LIGHTING/Perimeter_bot_ZN_4": {
            "points": ["Power"],
            "conversion": "Power/1000.0"
        },
        "LIGHTING/Perimeter_mid_ZN_1": {
            "points": ["Power"],
            "conversion": "Power/1000.0"
        },
        "LIGHTING/Perimeter_mid_ZN_2": {
            "points": ["Power"],
            "conversion": "Power/1000.0"
        },
        "LIGHTING/Perimeter_mid_ZN_3": {
            "points": ["Power"],
            "conversion": "Power/1000.0"
        },
        "LIGHTING/Perimeter_mid_ZN_4": {
            "points": ["Power"],
            "conversion": "Power/1000.0"
        },
        "LIGHTING/Perimeter_top_ZN_1": {
            "points": ["Power"],
            "conversion": "Power/1000.0"
        },
        "LIGHTING/Perimeter_top_ZN_2": {
            "points": ["Power"],
            "conversion": "Power/1000.0"
        },
        "LIGHTING/Perimeter_top_ZN_3": {
            "points": ["Power"],
            "conversion": "Power/1000.0"
        },
        "LIGHTING/Perimeter_top_ZN_4": {
            "points": ["Power"],
            "conversion": "Power/1000.0"
        },
        "AHU1": {
            "points": ["SupplyFanPower"],
            "conversion": "SupplyFanPower/1000.0"
        },
        "AHU2": {
            "points": ["SupplyFanPower"],
            "conversion": "SupplyFanPower/1000.0"
        },
        "AHU3": {
            "points": ["SupplyFanPower"],
            "conversion": "SupplyFanPower/1000.0"
        },
        "AHU4": {
            "points": ["SupplyFanPower"],
            "conversion": "SupplyFanPower/1000.0"
        },
        "Chiller1": {
            "points": ["Power"],
            "conversion": "Power/(1000.0)"
        },
        "Chiller2": {
            "points": ["Power"],
            "conversion": "Power/(1000.0)"
        },
		"METERS": {
			"points": ["WholeBuildingPower"],
			"conversion": "WholeBuildingPower*(-0.001)"
		}

	},
    "power_20": 910.92720999999995, 
    "power_21": 620.0424549999999, 
    "power_22": 463.91088619999999, 
    "power_23": 354.8858176, 
    "power_5": 383.05938989999999, 
    "power_4": 341.89288340000002, 
    "power_7": 1046.510896, 
    "power_6": 793.5936805, 
    "power_1": 407.73100620000002, 
    "power_0": 285.74411989999999, 
    "power_3": 297.9663372, 
    "power_2": 285.74414289999999, 
    "power_9": 1002.4691770000001, 
    "power_8": 1040.259965, 
    "power_11": 973.61131440000008, 
    "power_10": 1023.4367090000001, 
    "power_13": 1054.2777890000002, 
    "power_12": 1045.760123, 
    "power_15": 1121.4733550000001, 
    "power_14": 1057.4063470000001, 
    "power_17": 1004.74153, 
    "power_16": 1112.1835679999999, 
    "power_19": 900.16603769999995, 
    "power_18": 967.11965269999996
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
and follow instructions from the tcc-userguide https://tcc-userguide.readthedocs.io/en/latest/

## Install and activate VOLTTRON environment
For installing, starting, and activating the VOLTTRON environment, refer to the following VOLTTRON readthedocs: 
https://volttron.readthedocs.io/en/develop/introduction/platform-install.html

## Installing and Running UnControlLoadAgent Agent
Install and start the UnControlLoadAgent Agent using the script install-agent.py as describe below:

```
python VOLTTRON_ROOT/scripts/install-agent.py -s <top most folder of the agent> 
                                -c <Agent config file>
                                -i agent.UnControlLoadAgent
                                -t UnControlLoadAgent
                                --start --force
```
, where VOLTTRON_ROOT is the root of the source directory of VOLTTRON.

-s : followed by path of top most folder of the UnControl Load agent

-c : followed by path of the agent config file

-i : followed by agent identity

-t : followed by name tag
 
--start (optional): start after installation

--force (optional): overwrites existing ilc agent with identity "agent.UnControlLoadAgent"  