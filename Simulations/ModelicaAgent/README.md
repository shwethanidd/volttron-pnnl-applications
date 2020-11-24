# ModelicaAgent

On start, publishes to Modelica on a configured topic with a configured Modelica payload.  
Intended to enable/disable actuation of the TCC control agents during a control test.

Note:
Explain what inputs and outputs define?
I assume inputs represent topics+ data sent to modelica and outputs
 define topics and data received from modelica

## Modelica Installation
For installing setup in Linux based systems, follow the steps described in
https://sparxsystems.com/enterprise_architect_user_guide/14.0/model_simulation/installing_openmodelica_on_linux_.html

## ModelicaAgent Configuration

User can specify the configuration in either json or yaml format. The json format is specified below: 

* Agent config file:

```` json
{
    "model": "IBPSA.Utilities.IO.RESTClient.Examples.PIDTest",
    "model_runtime": 361,
    "result_file": "PIDTest",
    "mos_file_path": "/home/volttron/dymola/run_PID.mos",
    "advance_simulation_topic": "modelica/advance",
    "inputs" : {
        "control_setpoint" : {
            "name" : "control_setpoint",
            "topic" : "building/device",
            "field" : "control_setpoint"

        },
        "control_output" : {
            "name" : "control_output",
            "topic" : "building/device",
            "field" : "control_output"

        }
    },
    "outputs" : {
        "measurement" : {
            "name" : "measurement",
            "topic" : "building/device",
            "field" : "measurement",
            "meta" : {"type": "Double", "unit": "none"}
        },
        "setpoint" : {
            "name" : "setpoint",
            "topic" : "building/device",
            "field" : "control_setpoint",
            "meta" : {"type": "Double", "unit": "none"}
        }
}          
````
## Install and activate VOLTTRON environment
For installing, starting, and activating the VOLTTRON environment,
refer to the following VOLTTRON readthedocs:
 
https://volttron.readthedocs.io/en/develop/introduction/platform-install.html

## Installing and Running Modelica Agent
Install and start the Modelica Agent using the script install-agent.py as describe below:

```
python VOLTTRON_ROOT/scripts/install-agent.py -s <top most folder of the agent> 
                                -c <Agent config file>
                                -i agent.Modelica
                                -t Modelica
                                --start --force
```

, where VOLTTRON_ROOT is the root of the source directory of VOLTTRON.

-s : followed by path of top most folder of the Modelica agent

-c : followed by path of the agent config file

-i : followed by agent identity

-t : followed by name tag
 
--start (optional): start after installation

--force (optional): overwrites existing ilc agent with identity "agent.Modelica"  
