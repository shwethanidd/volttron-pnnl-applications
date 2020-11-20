# RTU Agent

It is the transactive controls and coordination (TCC) agent that interacts with volttron market service
 as a consumer electricity and a supplier of cooling air to VAV TCC agent that represent VAV hardware devices
 that provide cooling to building zones 

## RTU Agent Configuration

The json format of the config files are specified below. 

Agent config file:

```` json
{
    "campus": "PNNL", 
    "building": "350_BUILDING",
    "device": "HP1A",
    "input_data_timezone": "US/Pacific",
    "control_interval": 300, 
    "agent_name": "hp1a",
    "actuation_method": "periodic",
    "market_type": "rtp",
    "actuation_enable_topic": "PNNL/350_BUILDING/Actuate",
    "actuation_enabled_onstart": true,
    "market_name": "electric",
    "static_price_flag": true,
    "static_minimum_price": 6.0,
    "static_maximum_price": 155.0,
    "inputs": [
        {
            "mapped": "sfs", 
            "point": "SupplyFanStatus", 
            "topic": "devices/PNNL/350_BUILDING/HP1A/all",
            "inital_value": 0
        }, 
        {
            "mapped": "oat", 
            "point": "OutdoorAirTemperature", 
            "topic": "devices/PNNL/350_BUILDING/HP1A/all",
            "inital_value": 75.0
        }, 
        {
            "mapped": "zt", 
            "point": "ZoneTemperature", 
            "topic": "devices/PNNL/350_BUILDING/HP1A/all",
            "inital_value": 73.0
        }, 
        {
            "mapped": "mclg", 
            "point": "FirstStageCooling", 
            "topic": "devices/PNNL/350_BUILDING/HP1A/all",
            "inital_value": 0
        }
    ], 
    "outputs": [
        {
            "mapped": "csp", 
            "point": "CoolingTemperatureSetPoint",
            "topic": "PNNL/350_BUILDING/HP1A/CoolingTemperatureSetPoint",
            "flexibility_range": [
                71.0,
                73.0
            ], 
            "off_setpoint": 80,
            "actuator": "platform.actuator", 
            "release": "None"
        }
    ], 
    "schedule": {
        "Monday": {
            "start": "6:00",
            "end": "20:00"
        }, 
        "Tuesday": {
            "start": "6:00",
            "end": "20:00"
        }, 
        "Wednesday": {
            "start": "6:00",
            "end": "20:00"
        }, 
        "Thursday": {
            "start": "6:00",
            "end": "20:00"
        }, 
        "Friday": {
            "start": "6:00",
            "end": "20:00"
        }, 
        "Saturday": "always_off", 
        "Sunday": "always_off"
    }, 
    "model_parameters": {
    }
}
````
User can create a config file using the tcc-config-web-tool: https://tcc-configuration-tool.web.app/
and following instruction from the tcc-userguide https://tcc-userguide.readthedocs.io/en/latest/b.app/

## Install and activate VOLTTRON environment
For installing, starting, and activating the VOLTTRON environment, refer to the following VOLTTRON readthedocs: 
https://volttron.readthedocs.io/en/develop/introduction/platform-install.html

## Installing and Running RTU Agent
Install and start the RTU Agent using the script install-agent.py as describe below:

```
python scripts/install-agent.py -s <top most folder of the agent> 
                                -c <Agent config file>
                                -i agent.RTU
                                -t RTU
                                --start --force
```
-s : followed by path of top most folder of the RTU agent

-c : followed by path of the agent config file

-i : followed by agent identity

-t : followed by name tag
 
--start (optional): start after installation

--force (optional): overwrites existing ilc agent with identity "agent.RTU"  


