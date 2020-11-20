# METER Agent
It is the transactive controls and coordination (TCC) agent that interacts with volttron market service as a electric seller.
 The meter agent provides either a fixed price or fixed demand supply curve to the volttron market service. 
 
## METER Agent Configuration

You can specify the configuration in either json or yaml format. The json format is specified below:

* Agent config file:
```` json
{
    "campus": "PNNL", # if omitted defaults to ""
    "building": "BRSW", # if omitted defaults to ""
    "input_data_timezone": "UTC", # if omitted defaults to "UTC"
    "supplier_market_name": "electric",
    "tns": false,

    "agent_name": "meter",
    "inputs": [],
    "outputs": [],
    "schedule":{},

    "model_parameters": {
        "model_type": "simple"
	}
}
````
User can create this config file using the tcc-config-web-tool: https://tcc-configuration-tool.web.app/
and following instruction from the tcc-userguide https://tcc-userguide.readthedocs.io/en/latest/

## Install and activate volttron environment
Refer following volttron readthedocs for Installing, starting and activating volttron environment: 
https://volttron.readthedocs.io/en/develop/introduction/platform-install.html

## Installing and Running Meter Agent
Install and start the Meter Agent using the script install-agent.py as describe below:

```
python scripts/install-agent.py -s <top most folder of the agent> 
                                -c <Agent config file>
                                -i agent.Meter
                                -t Meter
                                --start --force
```
-s : followed by path of top most folder of the Meter agent

-c : followed by path of the agent config file

-i : followed by agent identity

-t : followed by name tag
 
--start (optional): start after installation

--force (optional): overwrites existing ilc agent with identity "agent.Meter"  


