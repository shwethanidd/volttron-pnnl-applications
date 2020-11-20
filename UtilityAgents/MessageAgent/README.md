# MessageAgent

On start, publishes a message on a configured topic with a configured message payload. 
 Intended to enable/disable actuation of the TCC control agents during a control test

## MessageAgent Configuration

The json format of the config files are specified below. 

1.  Agent config file:

```` json
"topic": "topic_name"
"value" : 15
````
## Install and activate volttron environment
Refer following volttron readthedocs for Installing, starting and activating volttron environment: 
https://volttron.readthedocs.io/en/develop/introduction/platform-install.html

## Installing and Running Message Agent
Install and start the Message Agent using the script install-agent.py as describe below:

```
python scripts/install-agent.py -s <top most folder of the agent> 
                                -c <Agent config file>
                                -i agent.Message
                                -t Message
                                --start --force
```
-s : followed by path of top most folder of the Message agent

-c : followed by path of the agent config file

-i : followed by agent identity

-t : followed by name tag
 
--start (optional): start after installation

--force (optional): overwrites existing ilc agent with identity "agent.Message"  
