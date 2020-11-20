# ProactiveDiagnosis Agent

Proactive Diagnosis agent allows you to write different recipes for diagnosis and it evaluates
different diagnostics rules. Based on whether the diagnosis rules are true or false, it publishes a fault code
or non-fault code mentioned in the config file.

## ProactiveDiagnosis Agent Configuration

In activated VOLTTRON environment, install all the ProactiveDiagnosis dependent python packages

```
cd EnergyEfficiency/ProactiveDiagnosticAgent
pip install -r requirements.txt
```
For this agent you will require two config files; 1. Agent config file, and 2. Diagnosis config file

The json format of the config files are specified below. 

*  Agent config file:

```` json
{
    "campus": "campus",
    "building": "building",
    "device": ["AHU1"],
    # all diagnostics in diagnostics array are run consecutively.
    # this is initiated based on a cron scheduling string - https://crontab.guru
    # example - "0 18 * * *" is every day at 6pm
    "run_schedule": "*/3 * * * *",
    "prerequisites": {
        "conditions": ["Abs(OutdoorAirTemperature - ReturnAirTemperature)>5.0", "OutdoorAirTemperature>35.0"],
        "condition_args": ["OutdoorAirTemperature", "ReturnAirTemperature"]
    },
    "diagnostics": [
        "config://diagnostic1.config"
    ]
}
````

*  Diagnosis Config File:


```json
{
    "name": "MAT_DAT_CONSISTENCY",
    "fault_code": 1,
    "non_fault_code": 0,
    "fault_condition": "all", 
    # There are two types of fault conditions
    # 1. all: if all conditions are "true" in the rules list, then only proactive agent sends fault code.
    # Otherwise, it sends non fault code.
    # 2. any: if atleast one of the conditions is true in the list, then it sends fault code.
    # Otherwisem it sends non fault code. 
    "control": [
        {
            "points": {
                "OutdoorDamperSignal": 0,
                "ChilledWaterValvePosition": 0,
                "FirstStageHeatingOutput": 0,
                "SupplyFanSpeedCommand": 100,
                "OccupancySchedule": 1,
                "SupplyFanStatusCommand": 1
            },
            # Time in seconds after control action to wait for steady
            # state prior to performing analysis
            "steady_state_interval": 20,
            # data collection interval in seconds after proactive diagnostic
            # application will perform get_point 10 times evenly space over collection interval
            "data_collection_interval": 20,
            "analysis": {
            
                "rule_list": ["Abs(MixedAirTemperature - DischargeAirTemperature) > 6"],
                "inconclusive_conditions_list": ["Abs(ReturnAirTemperature - OutdoorAirTemperature) > 6"],
                "points": ["MixedAirTemperature", "DischargeAirTemperature", "ReturnAirTemperature", "OutdoorAirTemperature"]
            }
        },
        {
            "points": {
                "OutdoorDamperSignal": 100,
                "ChilledWaterValvePosition": 0,
                "FirstStageHeatingOutput": 0,
                "SupplyFanSpeedCommand": 100,
                "OccupancySchedule": 1,
                "SupplyFanStatusCommand": 1
            },
            "steady_state_interval": 20,
            "data_collection_interval": 20,
            "analysis": {
                "rule_list": ["Abs(MixedAirTemperature - DischargeAirTemperature) > 6"],
                "inconclusive_conditions_list": ["Abs(ReturnAirTemperature - OutdoorAirTemperature) > 6"],
                "points": ["MixedAirTemperature", "DischargeAirTemperature", "ReturnAirTemperature", "OutdoorAirTemperature"]
            }
        }
    ]
}
````
## Install and activate volttron environment
Refer following volttron readthedocs for Installing, starting and activating volttron environment: 
https://volttron.readthedocs.io/en/develop/introduction/platform-install.html

## Installing and Running ProactiveDiagnosis Agent
Install and start the ProactiveDiagnosis Agent using the script install-agent.py as describe below:

```
python scripts/install-agent.py -s <top most folder of the agent> 
                                -c <Agent config file>
                                -i agent.proactivediagnosis
                                -t proactivediagnosis
                                --start --force
```
-s : followed by path of top most folder of the Proactive Diagnosis agent

-c : followed by path of the agent config file

-i : followed by agent identity

-t : followed by name tag
 
--start (optional): start after installation

--force (optional): overwrites existing ilc agent with identity "agent.proactivediagnosis" 


