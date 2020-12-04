# Transactive Control and Coordination
The TCC agent behavior is defined by configuration.  As shown in Figure 1, the TCC agents have a
hierarchical inheritance structure: functions are encapsulated into classes at different levels.
This hierarchical structure avoids duplication of function implementation and thus significantly
simplify maintenance and upgrades.

All TCC agents inherit from the control class (transactive base class) and use a standardized
configuration format.  Agents that participate in multiple markets (e.g., the TCC AHU agent which is a
supplier of air and a consumer of electricity) and/or require the aggregate demand curve of one commodity
to determine their respective demand of the same or a different commodity also inherit from the aggregator class.

![img.png](img.png)

Figure 1.  TCC agent structure

All TCC agent configuration files have top level configurations, an inputs section, 
outputs section, schedule section, and model_parameters section.  The configuration section and the required parameters will be described in the 
following section of this report.  The following is a configuration file for a TCC VAV agent.

The TCC agent top level configuration parameters contains agent identifiers (e.g., agent_name and market_name).  The TCC agent top level configurations parameters are as follows:

•	campus – string value for the campus name.  This value is used to build record topic for storage of TCC results in a local or remote database.

•	building –string value for the building name.  This value is used to build record topic for storage of TCC results in a local or remote database.

•	agent_name – string value for the agent name.  This value is used to build record topic for storage of TCC results in a local or remote database.

•	market_type – string value.  If tns is set to false the TCC will assume a single timestep market (i.e., the market clears periodically at a fixed interval).  If tns is set to true then the TCC agent will assume that the market is hourly and will project device demand for the next 24 hours at each hourly market interval.

•	market_name – the market name that the agent will participate in.  In the above example the VAV agent is a consumer of air provided by an air handling unit.  The VAV agent will submit a price-capacity curve (demand curve) to the “air2” market.

•	actuation_method – string value used to determine how the TCC agent will control.
o	market_clear – The TCC agent will send a control command to its respective device each time the market clears.
o	periodic – The TCC agent will send a control command to the device at a fix rate (e.g., once every 10 minutes.

•	actuaion_enabled_onstart – boolean value.  true indicates that actuation is enabled when the agent starts.  False indicates that the agent is not enabled to actuate until it receives a message on the message bus on the topic “campus/building/actuate” with a message of true (or 1).

For aggregators (such as the AHU agent) two additional fields are required:

•	consumer_market – list of strings or string.  Name or list of names for the market(s) where the aggregator is a consumer of commodity. For the AHU agent this is the “electric”.

•	supplier_market – list of strings or string.  Name or list of names for the market(s) where the aggregator is a supplier of commodity.  For AHU agent this will is an “air” market (e.g., “air1” for AHU1).


The following describes the input parameters for one input entry:

•	topic – string value.  The topic that the driver will publish the device data.

•	point - string value.  The point name for the data.  The point name is used to identify the data point in the VOLTTRON driver data payload.

•	mapped – this is an internal parameter that is set in the TCC agents model.

•	initial_value – prior to receiving any data from the drivers the TCC agent will initialize the input to this value

The schedule section describes the building occupancy or when the device is considered ON.  
Currently, in the VAV agent during unoccupied periods the agent will assume it has zero demand 
for cooling.  Each day, Monday – Sunday, can be given a start and end time as a string 
(format should be “hh:mm” or “hh:mm:ss”), the string “always_on” for 24 hour operation, 
or the string “always_off” for days where the equipment is intended to remain off (e.g., weekends).

The following describes the outputs parameters:

•	topic – string value.  Topic for device that the TCC agent will control.  Note this topic does not contain the prefix “devices” as the topic is used to make an RPC call to the Actuator agent or building simulation engines set_point method.

•	point – string value.  The point on the device that the TCC agent will control.

•	flexibility_range – list or array.  This value bounds the demand prediction of the TCC agent.  For the VAV agent this list consists of the maximum and minimum airflow rates.  If control_flexibility is omitted from the outputs configuration then the flexibility_range is also the control_flexibility.

•	control_flexibility – list or array.  We impose a linear relationship between the price and the control action. Because the demands of individual TCC are directly determined by their control action, the relationship actually leads to price-responsive demand. Note that such a linear relationship is only selected for easy demonstration of the proposed idea and may not reflect the actual preferences of building occupants.  This behavior can be overwritten in the transactive base class (volttron-GS/pnnl/transactive/transactive.py) in the determine_control method.

•	release – string value.  If release is set to “default” then the TCC agent will store the original value of the control point and restore set point upon agent shutdown or when transitioning to unoccupied periods.  Otherwise the TCC agent will write None to the point.  Writing None will revert the point to its default value for BACnet devices and EnergyPlus simulations.

•	actuator – string value.  The vip identity of the VOLLTRON actuator agent or building simulation engine.  Defaults to “platform.actuator”.

•	mapped – this is an internal parameter that is set in the 
