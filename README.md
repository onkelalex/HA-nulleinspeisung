# MS-A2 Zero Export Control (High-Speed & SoC Protection)

A **Home Assistant Blueprint** for precise **zero export control** of the **Hoymiles MS-A2 battery system**.

The automation dynamically adjusts the battery charge/discharge power to keep the grid power around **0 W** while protecting the battery from **deep discharge using a configurable SoC limit**.

Designed for **fast response and stable regulation** in combination with **Tibber Pulse, smart meters, or other real-time grid sensors**.

---

# Features

✔ High-speed grid regulation  
✔ Keeps grid import/export close to **0 W**  
✔ Adjustable **control loop interval**  
✔ **SoC protection** to prevent deep discharge  
✔ Configurable **charge and discharge limits**  
✔ Built-in **watchdog mechanism** to keep the inverter responsive  
✔ Compatible with **MQTT-controlled MS-A2 setups**

---

# How it Works

The automation continuously monitors the **grid power sensor** and adjusts the battery power limit accordingly.

Core control logic:

1. Read current **grid power**
2. Calculate a **correction value** using a configurable gain
3. Adjust the **battery power limit**
4. Enforce:
   - charge limits
   - discharge limits
   - minimum battery SoC
5. Send the updated limit to the **MS-A2 controller**

This keeps the house consumption balanced without exporting energy to the grid.

---

# Requirements

You need the following entities in Home Assistant:

| Entity | Description |
|------|------|
| Grid Power Sensor | Measures current grid import/export in watts |
| Battery SoC Sensor | Battery state of charge (%) |
| MS-A2 Mode Select | Select entity controlling MS-A2 mode |
| MS-A2 Power Limit | Number entity controlling power |

Typical setups use:

- Tibber Pulse
- Smart meter integrations
- MQTT MS-A2 integration

---

# Installation

## 1. Import Blueprint

Copy the YAML file into:
