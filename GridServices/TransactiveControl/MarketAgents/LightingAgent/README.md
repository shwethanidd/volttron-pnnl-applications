# Lighting Agent

It is the transactive controls and coordination (TCC) agent that interacts with volttron market service
 as a consumer electricity and represent light devices that provide lighting to building zones 

## Lighting Agent Configuration

You can specify the configuration in either json or yaml format. The json format is specified below:
```` json
{
    "campus": "CAMPUS", 
    "building": "BUILDING1",
    "device": "LIGHTING/ZONE102",
    "actuation_enable_topic": "default", 
    "input_data_timezone": "UTC", 
    "actuation_enabled_onstart": true, 
    "control_interval": 20, 
    "agent_name": "light_102", 
    "market_name": "electric",
    "decrease_load_only": true,
    "inputs": [
        {
            "mapped": "occ", 
            "point": "SupplyFanStatus", 
            "topic": "devices/CAMPUS/BUILDING1/AHU1/all", 
            "inital_value": 0
        }
    ], 
    "outputs": [
        {
            "mapped": "light", 
            "point": "DimmingLevelOutput", 
            "topic": "CAMPUS/BUILDING1/LIGHTING/ZONE102/DimmingLevelOutput", 
            "flexibility_range": [
                0.9, 
                0.7
            ], 
            "off_setpoint": 0.15, 
            "actuator": "platform.actuator", 
            "release": "None"
        }
    ], 
    "schedule": {
        "Monday": {
            "start": "5:00", 
            "end": "17:00"
        }, 
        "Tuesday": {
            "start": "5:00", 
            "end": "17:00"
        }, 
        "Wednesday": {
            "start": "5:00", 
            "end": "17:00"
        }, 
        "Thursday": {
            "start": "5:00", 
            "end": "17:00"
        }, 
        "Friday": {
            "start": "5:00", 
            "end": "17:00"
        }, 
        "Saturday": "always_off", 
        "Sunday": "always_off"
    }, 
    "model_parameters": {
        "default_lighting_schedule": {
            0: 0.5, 
            1: 0.5, 
            2: 0.5, 
            3: 0.5, 
            4: 0.7, 
            5: 0.9, 
            6: 0.9, 
            7: 1.0, 
            8: 1.0, 
            9: 1.0, 
            10: 1.0, 
            11: 1.0, 
            12: 1.0, 
            13: 1.0, 
            14: 1.0, 
            15: 1.0, 
            16: 0.9, 
            17: 0.8, 
            18: 0.8, 
            19: 0.7, 
            20: 0.7, 
            21: 0.5, 
            22: 0.5, 
            23: 0.5
        },
        "model_type": "light.simple_profile", 
        "rated_power": 0.31729199999999996
    }
}
````
User can create a config file using the tcc-config-web-tool: https://tcc-configuration-tool.web.app/
and following instruction from the tcc-userguide https://tcc-userguide.readthedocs.io/en/latest/

## Install and activate VOLTTRON environment
For installing, starting, and activating the VOLTTRON environment, refer to the following VOLTTRON readthedocs: 
https://volttron.readthedocs.io/en/develop/introduction/platform-install.html

## Installing and Running Lighting Agent
Install and start the Lighting Agent using the script install-agent.py as describe below:

```
python scripts/install-agent.py -s <top most folder of the agent> 
                                -c <Agent config file>
                                -i agent.Lighting
                                -t Lighting
                                --start --force
```
-s : followed by path of top most folder of the Lighting agent

-c : followed by path of the agent config file

-i : followed by agent identity

-t : followed by name tag
 
--start (optional): start after installation

--force (optional): overwrites existing ilc agent with identity "agent.Lighting"  


