MS-A2 Zero Export Control (High-Speed & SoC Protection)

A Home Assistant Blueprint for precise zero export control of the Hoymiles MS-A2 battery system.

The automation dynamically adjusts the battery charge/discharge power to keep the grid power around 0 W while protecting the battery from deep discharge using a configurable SoC limit.

Designed for fast response and stable regulation in combination with Tibber Pulse, smart meters, or other real-time grid sensors.

Features

✔ High-speed grid regulation
✔ Keeps grid import/export close to 0 W
✔ Adjustable control loop interval
✔ SoC protection to prevent deep discharge
✔ Configurable charge and discharge limits
✔ Built-in watchdog mechanism to keep the inverter responsive
✔ Compatible with MQTT-controlled MS-A2 setups

How it Works

The automation continuously monitors the grid power sensor and adjusts the battery power limit accordingly.

Core control logic:

Read current grid power

Calculate a correction value using a configurable gain

Adjust the battery power limit

Enforce:

charge limits

discharge limits

minimum battery SoC

Send the updated limit to the MS-A2 controller

This keeps the house consumption balanced without exporting energy to the grid.

Requirements

You need the following entities in Home Assistant:

Entity	Description
Grid Power Sensor	Measures current grid import/export in watts
Battery SoC Sensor	Battery state of charge (%)
MS-A2 Mode Select	Select entity controlling MS-A2 mode
MS-A2 Power Limit	Number entity controlling power

Typical setups use:

Tibber Pulse

Smart meter integrations

MQTT MS-A2 integration

Installation
1. Import Blueprint

Copy the YAML file into:

config/blueprints/automation/

Example:

config/blueprints/automation/ms_a2_zero_export.yaml

Restart Home Assistant.

2. Create Automation

Navigate to:

Settings → Automations & Scenes → Blueprints

Create a new automation from:

MS-A2 Zero Export Control
Configuration
Required Inputs
Grid Sensor

Power sensor representing grid flow.

Example:

sensor.tibber_power

Positive values = import
Negative values = export

Battery SoC Sensor

Battery charge level in percent.

Example:

sensor.ms_a2_state_of_charge
MS-A2 Mode Select

Select entity controlling the battery operation mode.

The automation forces the mode to:

mqtt_ctrl
Battery Power Limit

Number entity that sets the power limit for charging/discharging.

Optional Settings
Minimum SoC

Prevents the battery from discharging below a defined level.

Example:

10 %

When the SoC drops below this value, discharging stops.

Max Charge Power

Maximum charging power.

Example:

800 W
Max Discharge Power

Maximum discharging power.

Example:

800 W
Control Gain

Defines how aggressively the system reacts to grid changes.

Recommended values:

Value	Behavior
0.1	smooth and stable
0.2	fast response
>0.3	aggressive control

Recommended:

0.15 – 0.2
Update Interval

How often the controller updates the power limit.

Recommended:

Interval	Use Case
1–2 s	Tibber Pulse / fast meters
3–5 s	normal smart meters
>5 s	slow sensors
Internal Logic

The regulation follows this formula:

correction = grid_power * gain
new_limit = current_limit + correction

Then constraints are applied:

SoC protection

min/max charge limits

stability correction

Stability & Watchdog

The blueprint includes a watchdog trick.

Some inverters ignore unchanged power values.
If the calculated value does not change, the automation slightly adjusts the limit to force an update.

This keeps the MS-A2 responsive even during stable load conditions.

Example Setup

Example configuration for a typical setup:

Setting	Value
Grid Sensor	Tibber Pulse
Battery SoC	MS-A2 integration
Max Charge	800 W
Max Discharge	800 W
Control Gain	0.2
Update Interval	2 s
Min SoC	10 %
Tips for Best Performance

✔ Use a fast grid sensor (Tibber Pulse recommended)
✔ Keep the gain between 0.15 and 0.2
✔ Avoid very small update intervals if your meter is slow
✔ Monitor the first hours using the Home Assistant history graph

Disclaimer

This blueprint is provided as-is.
Incorrect configuration may lead to unexpected battery behavior.
Always verify the limits configured for your MS-A2 system.

If you want, I can also make you a GitHub-ready version with:

badges

screenshots

a control diagram

a nice regulation graphic explaining the algorithm

That usually helps a lot if you plan to publish the blueprint.
