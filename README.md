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
