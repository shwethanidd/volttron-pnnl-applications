# Intelligent Load Control (ILC) Application

ILC supports traditional demand response as well as transactive energy
services. ILC manages controllable loads while also mitigating
service-level excursions (e.g., occupant comfort, minimizing equipment
ON/OFF cycling) by dynamically prioritizing available loads for curtailment
using both quantitative (deviation of zone conditions from set point) and
qualitative rules (type of zone).

## ILC Agent Configuration

For this agent you will require four config files; 1. ILC (main) config f 2. Control config file,
and 3. criteria config file The json format of the config files are specified below.
In control and criteria files contains both curtail setting and augment settings.
 
These config files can be created using the config-web-tool: https://ilc-configuration-tool.web.app/.
You can follow instruction from the ilc-readthedocs https://userguide-ilc.readthedocs.io/en/latest/ for while using this
config-web-tool

*  ILC config file:

```` json
{
    "campus": "CAMPUS",
    "building": "BUILDING",
    "power_meter": {
        "device_topic": "CAMPUS/BUILDING/METERS",
        "point": "WholeBuildingPower",
         "demand_forumla": {
            "operation": "Abs(WholeBuildingPower)",
            "operation_args": ["WholeBuildingPower"]
         }
    },
    "agent_id": "ILC",
    "demand_limit": 30.0,
    "control_time": 20.0,
    "curtailment_confirm": 5.0,
    "curtailment_break": 20.0,
    "average_building_power_window": 15.0,
    "stagger_release": true,
    "stagger_off_time": true,
    "clusters": [ 
        {
            "device_control_file": "control_config",
            "device_criteria_file": "criteria_config",
            "pairwise_criteria_file": "pairwise_criteria.json",
            "cluster_priority": 1.0
        }
    ]
 }

````

* Control config file:  

````json
{
    "HP1": {
        "FirstStageCooling": {
            "device_topic": "CAMPUS/BUILDING/HP1",
            "device_status": {
                "curtail": {
                    "condition": "FirstStageCooling", 
                    "device_status_args": ["FirstStageCooling"]
                },
                "augment": {
                    "condition": "FirstStageCooling < 1",
                    "device_status_args": ["FirstStageCooling"]
                }
            },
            "curtail_settings": {
                "point": "ZoneTemperatureSetPoint",
                "curtailment_method": "offset",
                "offset": 2.0,
                "load": 6.0
            },
            "augment_settings": {
                "point": "ZoneTemperatureSetPoint",
                "curtailment_method": "offset",
                "offset": -2.0,
                "load": 6.0
            }
        }
    },
    "HP2": {
        "FirstStageCooling": {
            "device_topic": "CAMPUS/BUILDING/HP2",
            "device_status": {
                "curtail": {
                    "condition": "FirstStageCooling",
                    "device_status_args": ["FirstStageCooling"]
                },
                "augment": {
                    "condition": "FirstStageCooling < 1",
                    "device_status_args": ["FirstStageCooling"]
                }
            },
            "curtail_settings": {
                "point": "ZoneTemperatureSetPoint",
                "curtailment_method": "equation",
                "equation": {
                    "operation": "ZoneTemperature+0.5",
                    "equation_args": ["ZoneTemperature"],
                    "minimum": 69.0,
                    "maximum": 77.0
                },
                "load": 6.5
            },
            "augment_settings": {
                "point": "ZoneTemperatureSetPoint",
                "curtailment_method": "value",
                "value": 69.0,
                "load": 6.5
            }
        }
    }
}

````
* Criteria Config file:

In this config file, any number of relevant criteria can be define to prioritize loads for curtail or augment
the electricity consumption. In the following example, five criteria are used; 1. Zonetemperature-setpoint 2. rated-power, 3. room-type, 4. stage, 5. history-zonetemperature. This criterial differentiate by their 
 operation types  
````json
{
    "HP1": {
        "FirstStageCooling": {
            "curtail": {
                "device_topic": "CAMPUS/BUILDING/HP1",
                "zonetemperature-setpoint":{
                    "operation": "1/(AverageZoneTemperature-CoolingTemperatureSetPoint)",
                    "operation_type": "formula",
                    "operation_args": {
                                        "always": ["CoolingTemperatureSetPoint", "AverageZoneTemperature"]
                                      },
                    "minimum": 0,
                    "maximum": 10
                },
                "rated-power": {
                    "on_value": 6.0,
                    "off_value": 0.0,
                    "operation_type": "status",
                    "point_name": "FirstStageCooling"
                },
                "room-type": {
                    "map_key": "Office",
                    "operation_type": "mapper",
                    "dict_name": "zone_type"
                },
                "stage": {
                    "value": 1.0,
                    "operation_type": "constant"
                },
                "history-zonetemperature": {
                    "comparison_type": "direct",
                    "operation_type": "history",
                    "point_name": "AverageZoneTemperature",
                    "previous_time": 15,
                    "minimum": 0,
                    "maximum": 10
                }
            },
            "augment": {
                "device_topic": "CAMPUS/BUILDING/HP1",
                "zonetemperature-setpoint":{
                    "operation": "1/(CoolingTemperatureSetPoint-AverageZoneTemperature)",
                    "operation_type": "formula",
                    "operation_args": {
                                        "always": ["CoolingTemperatureSetPoint", "AverageZoneTemperature"]
                                      },
                    "minimum": 0,
                    "maximum": 10
                },
                "rated-power": {
                    "on_value": 0.0,
                    "off_value": 6.0,
                    "operation_type": "status",
                    "point_name": "FirstStageCooling"
                },
                "room-type": {
                    "map_key": "Office",
                    "operation_type": "mapper",
                    "dict_name": "zone_type"
                },
                "stage": {
                    "value": 1.0,
                    "operation_type": "constant"
                },
                "history-zonetemperature": {
                    "comparison_type": "direct",
                    "operation_type": "history",
                    "point_name": "AverageZoneTemperature",
                    "previous_time": 15,
                    "minimum": 0,
                    "maximum": 10
                }
            }
        }
    },
    "HP2": {
        "FirstStageCooling": {
            "curtail": {
                "device_topic": "CAMPUS/BUILDING/HP2",
                "zonetemperature-setpoint":{
                    "operation": "1/(AverageZoneTemperature-CoolingTemperatureSetPoint)",
                    "operation_type": "formula",
                    "operation_args": ["CoolingTemperatureSetPoint", "AverageZoneTemperature"],
                    "minimum": 0,
                    "maximum": 10
                },
                "rated-power": {
                    "on_value": 4.4,
                    "off_value": 0.0,
                    "operation_type": "status",
                    "point_name": "FirstStageCooling"
                },
                "room-type": {
                    "map_key": "Office",
                    "operation_type": "mapper",
                    "dict_name": "zone_type"
                },
                "stage": {
                    "value": 1.0,
                    "operation_type": "constant"
                },
                "history-zonetemperature": {
                    "comparison_type": "direct",
                    "operation_type": "history",
                    "point_name": "AverageZoneTemperature",
                    "previous_time": 15,
                    "minimum": 0,
                    "maximum": 10
                }
            },
            "augment": {
                "device_topic": "CAMPUS/BUILDING/HP2",
                "zonetemperature-setpoint":{
                    "operation": "1/(CoolingTemperatureSetPoint-AverageZoneTemperature)",
                    "operation_type": "formula",
                    "operation_args": ["CoolingTemperatureSetPoint", "AverageZoneTemperature"],
                    "minimum": 0,
                    "maximum": 10
                },
                "rated-power": {
                    "on_value": 0.0,
                    "off_value": 4.4,
                    "operation_type": "status",
                    "point_name": "FirstStageCooling"
                },
                "room-type": {
                    "map_key": "Office",
                    "operation_type": "mapper",
                    "dict_name": "zone_type"
                },
                "stage": {
                    "value": 1.0,
                    "operation_type": "constant"
                },
                "history-zonetemperature": {
                    "comparison_type": "direct",
                    "operation_type": "history",
                    "point_name": "AverageZoneTemperature",
                    "previous_time": 15,
                    "minimum": 0,
                    "maximum": 10
                }
            }
        }
    }
}
````
* Pair-Wise Comparison of Selected Criteria:

The relative importance of the two criteria is measured and evaluated
according to a numerical scale from 1 to 9. The higher the value, the more important the corresponding criterion is. Pair-wise
comparison is conducted to determine qualitatively which criteria are more important and assign to each
criterion a qualitative weight.

for more detail about pair-wise criteria, please refer following documentation
https://www.pnnl.gov/main/publications/external/technical_reports/PNNL-26034.pdf

```json
{
    "curtail": {
        "history-zonetemperature": {
            "room-type": 5,
            "rated-power": 3
        },
        "room-type": {},
        "rated-power": {
            "room-type": 3
        },
        "zonetemperature-setpoint": {
            "history-zonetemperature": 5,
            "room-type": 8,
            "rated-power": 6,
            "stage": 2
        },
        "stage": {
            "history-zonetemperature": 3,
            "room-type": 6,
            "rated-power": 4
        }
    },
    "augment": {
        "history-zonetemperature": {
            "room-type": 5,
            "rated-power": 3
        },
        "room-type": {},
        "rated-power": {
            "room-type": 3
        },
        "zonetemperature-setpoint": {
            "history-zonetemperature": 5,
            "room-type": 8,
            "rated-power": 6,
            "stage": 2
        },
        "stage": {
            "history-zonetemperature": 3,
            "room-type": 6,
            "rated-power": 4
        }
    }
}
```

## Install and activate volttron environment
Refer following volttron readthedocs for Installing, starting and activating volttron environment: 
https://volttron.readthedocs.io/en/develop/introduction/platform-install.html

## Installing and Running ILC Agent
Install and start the ILC Agent using the script install-agent.py as describe below:

```
python scripts/install-agent.py -s <top most folder of the agent> 
                                -c <Agent config file>
                                -i agent.ILC
                                -t ILC
                                --start --force
```
-s : followed by path of top most folder of the ILC agent

-c : followed by path of the main config file

-i : followed by agent identity

-t : followed by name tag
 
--start (optional): start after installation

--force (optional): overwrites existing ilc agent with identity "agent.ILC" 
