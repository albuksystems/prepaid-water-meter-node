# ESP32 Cellular Smart Liquid Dispenser & Flow Controller

An open-source, edge-computing firmware blueprint for automated, cell-connected liquid dispensing systems (Water ATMs, Milk ATMs, Cooking Oil Dispensers, and Smart Meters). 

Built on the **ESP32**, **SIMCom A7670 4G LTE modem**, and **YF-S201 Hall-Effect Flow Sensor**, this controller uses hardware interrupts for real-time volumetric measurement and automated relay/valve cutoff over MQTT.

---

## Key Features

* **High-Precision Flow Tracking:** Utilizes non-blocking hardware interrupts (`IRAM_ATTR` ISR) on the ESP32 for millisecond-level pulse counting.
* **Automated Valve Cutoff:** Opens motorized valves/relays on command and auto-closes once target volumes (Liters/mL) are reached.
* **4G LTE Cellular Telemetry:** Communicates via standard AT commands with SIMCom A7670 Cat-1 modems using MQTT/HTTP protocols.
* **Modular Calibration:** Built-in configurable $K$-factor scaling to easily adapt between water, milk, and higher-viscosity liquids like cooking oil.
* **Fail-Safe Operation:** Optocoupler-isolated relay logic prevents back-EMF spikes from affecting processing stability.

---

## System Architecture

```text
 [ User Payment / MQTT Command ] 
              │
              ▼
   [ SIMCom A7670 4G Modem ]
              │ (UART AT Commands)
              ▼
    [ ESP32 Microcontroller ] ◄─── (Interrupt Pulses) ─── [ YF-S201 Flow Sensor ]
              │
              ▼ (GPIO Signal)
 [ 5V Optocoupled Relay Module ]
              │
              ▼
  [ 12V Motorized Ball Valve ]
