# MS-A2 Zero Export Control (Ultra-Stable Edition)

A **Home Assistant Blueprint** for highly stable **zero export control** of the **Hoymiles MS-A2 battery system**.

This version focuses on **maximum stability** by introducing:

- a configurable **deadband (hysteresis)**
- a **damped control loop**
- protection against **battery deep discharge**

The deadband prevents the controller from reacting to very small grid fluctuations, which significantly reduces oscillation and unnecessary inverter updates.

---

# Features

✔ Ultra-stable grid regulation  
✔ Configurable **deadband / hysteresis**  
✔ Adjustable **control gain**  
✔ Adjustable **update interval**  
✔ **SoC protection** to prevent deep discharge  
✔ Configurable **charge and discharge limits**  
✔ Built-in **watchdog update mechanism**  
✔ Designed for **MQTT-controlled MS-A2 setups**

---

# How it Works

The automation continuously monitors the **grid power sensor** and adjusts the battery charge or discharge power accordingly.

Core control flow:

1. Read current **grid power**
2. Ignore small fluctuations within the **deadband**
3. Apply a **damped correction factor**
4. Adjust the **battery power limit**
5. Enforce limits:
   - charge power
   - discharge power
   - minimum SoC
6. Send the updated limit to the **MS-A2 controller**

This keeps the grid flow close to **0 W** while maintaining smooth battery behaviour.

---

# Deadband (Hysteresis)

A **deadband** prevents the controller from reacting to tiny grid fluctuations.

Example:

Deadband = `20 W`

| Grid Power | Controller Reaction |
|-------------|--------------------|
| 10 W | ignored |
| -15 W | ignored |
| 25 W | regulation starts |
| -40 W | regulation starts |

This dramatically reduces **oscillation and inverter chatter**.

---

# Requirements

You need the following entities in Home Assistant.

| Entity | Description |
|------|------|
| Grid Power Sensor | Measures grid import/export power |
| Battery SoC Sensor | Battery charge level (%) |
| MS-A2 Mode Select | Select entity controlling MS-A2 mode |
| MS-A2 Power Limit | Number entity controlling power |

Typical integrations used with this blueprint:

- Tibber Pulse
- Smart meter integrations
- MQTT MS-A2 integration

---

# Installation

## 1. Import Blueprint

Copy the YAML file into:

```
config/blueprints/automation/
```

Example:

```
config/blueprints/automation/ms_a2_zero_export_ultra_stable.yaml
```

Restart Home Assistant.

---

## 2. Create Automation

Navigate to:

```
Settings → Automations & Scenes → Blueprints
```

Create a new automation from:

```
MS-A2 Zero Export Control (Ultra-Stable)
```

---

# Configuration

## Grid Sensor

Sensor that measures grid import/export power.

Example:

```
sensor.tibber_power
```

Positive values = grid import  
Negative values = grid export

---

## Battery SoC Sensor

Battery charge level in percent.

Example:

```
sensor.ms_a2_state_of_charge
```

---

## MS-A2 Mode Select

Select entity controlling the battery operation mode.

The automation forces the mode to:

```
mqtt_ctrl
```

---

## Battery Power Limit

Number entity controlling the battery charge/discharge power.

---

# Configuration Options

## Minimum SoC

Stops battery discharge below the configured value.

Example:

```
10 %
```

---

## Deadband (Hysteresis)

Defines a power range where the controller does **nothing**.

Default:

```
20 W
```

Recommended range:

| Value | Behaviour |
|------|------|
| 10 W | very responsive |
| 20 W | balanced |
| 30-50 W | ultra stable |

---

## Max Charge Power

Maximum charging power.

Example:

```
800 W
```

---

## Max Discharge Power

Maximum discharging power.

Example:

```
800 W
```

---

## Control Gain

Defines how strongly the controller reacts to grid changes.

Recommended values:

| Value | Behaviour |
|------|------|
| 0.1 | very smooth |
| 0.2 | balanced (recommended) |
| 0.3 | faster response |

---

## Update Interval

Defines how often the controller sends updates.

Default:

```
3 seconds
```

Recommended values:

| Interval | Use Case |
|------|------|
| 1-2 s | very fast meters |
| 3 s | recommended |
| 5-10 s | slower sensors |

---

# Internal Control Logic

The regulation follows this simplified model.

```
if abs(grid_power) < deadband:
    correction = 0
else:
    effective_grid = grid_power - deadband
    correction = effective_grid * gain

new_limit = current_limit + correction
```

Constraints applied:

- charge limit
- discharge limit
- minimum SoC

---

# Watchdog Mechanism

Some inverters ignore identical power values.

To prevent this, the blueprint slightly changes the value if the calculated limit does not change.  
This ensures the MS-A2 always receives updates.

---

# Example Setup

Example configuration for a typical system.

| Setting | Value |
|------|------|
| Grid Sensor | Tibber Pulse |
| Battery SoC | MS-A2 integration |
| Deadband | 20 W |
| Max Charge | 800 W |
| Max Discharge | 800 W |
| Control Gain | 0.2 |
| Update Interval | 3 s |
| Min SoC | 10 % |

---

# Tips for Best Performance

✔ Use a **fast grid sensor** (Tibber Pulse recommended)  
✔ Start with **deadband = 20 W**  
✔ Use **gain = 0.2** for balanced behaviour  
✔ Monitor behaviour using the **Home Assistant history graph**

---

# Disclaimer

This blueprint is provided as-is.

Incorrect configuration may lead to unexpected battery behaviour.  
Always verify the limits configured for your **MS-A2 system**.
