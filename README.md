# Smart DC Bus over Ethernet (SDBE)  
### A Next-Gen, Low-Voltage, Multi-Voltage Power Standard for Smart Buildings, Cabins, and DC Microgrids

---

## 🧠 TL;DR
**SDBE** (Smart DC Bus over Ethernet) is an open, plug-and-play **Class-2 DC power distribution standard** built on Ethernet cabling.  
It delivers **multiple safe DC voltages (12 V / 24 V / 48 V)** over standard Cat6A infrastructure using intelligent per-port negotiation, channel isolation, and current limiting — like **“USB-PD meets PoE for buildings.”**

A single SDBE cable can power **sensors, LED lights, small appliances, networking gear, and controls** while remaining under **60 V / 100 W per port**, preserving safety and compatibility with existing low-voltage regulations.

---

## 🚀 What It Is
SDBE defines how to safely deliver **configurable DC voltages and power levels** over twisted-pair cable while keeping the user experience simple:

- Smart **handshake** and **power negotiation** (voltage, current, priority)
- **Programmable per-port** voltage (12 / 24 / 48 V DC)
- **Safe default-off** design — no live voltage until negotiation completes
- **Dynamic current limiting, telemetry, and load prioritization**
- **Open Class-2 limits**: ≤ 57 V DC, ≤ 100 W per channel

Think of it as:  
> 🔌 *PoE evolved for entire DC microgrids.*

---

## ⚡️ Core Design Principles

| Principle | Description |
|------------|--------------|
| **Safety First** | Fully SELV/Class-2 compliant (≤ 57 V DC, ≤ 2 A). |
| **Plug & Play** | No configuration needed; handshake auto-negotiates voltage and current. |
| **Modular Channels** | Each cable carries two independent 2-pair channels (A & B), up to 100 W each. |
| **Negotiation** | Resistor-coded or digital handshake before power-up (voltage + current). |
| **Intelligence** | MCU-based Power Source Equipment (PSE) controls voltage, current, and fault recovery. |
| **Backward Safety** | Incompatible with RJ-45 — keyed connector prevents plugging into Ethernet gear. |
| **Transparency** | Open, royalty-free electrical spec for off-grid and building-scale DC systems. |

---

## 🧩 Electrical Overview

### Channel Profile S (Recommended)
- **2 channels per cable (A & B)**  
- Each channel uses **2 pairs per polarity** for current sharing.  
- **Per-channel power:** up to **100 W @ 48 V / 2 A**  
- **Default voltages:** 12 V / 24 V / 48 V DC  

| Pair | Pins | Channel | Function |
|------|------|----------|-----------|
| 1,2 | Pair 1 | A+ |
| 3,6 | Pair 2 | A− |
| 4,5 | Pair 3 | B+ |
| 7,8 | Pair 4 | B− |

### Safety Limits
- Max voltage: **57 V DC**
- Max per-conductor current: **0.5 A**
- Max per-channel current: **2.0 A continuous**
- Inrush: up to **4× Imax for 10 ms**

---

## 🔄 PSE/PD State Machine
```
[DISABLED]
↓ (cable present)
[PROBE]
-Apply 5 V / 10 kΩ probe
-Detect signature (24.9 kΩ)
-Decode requested V/I via resistor or UART
↓ (valid)
  [PRE-POWER]
-Soft-start to target voltage
-Enable eFuse @ 1.1× Imax
↓
  [STEADY-STATE]
-Regulate voltage
-Monitor current/temp
-Heartbeat check
↓ (fault)
[FAULT/LIMIT]
-Overcurrent, short, overtemp, or lost heartbeat
-Retry 3× exponential backoff
↓
[RECOVERY]
```

---

## 🔋 Example: One Cable, Many Loads
| Device | Voltage | Power | Channel |
|---------|----------|--------|---------|
| 6× Sensors | 12 V | 6 W total | A |
| 2× LED Fixtures | 24 V | 30 W total | B |
| 1× Network PSU | 48 V | 10 W | A |
**Total:** ~46 W, < 2 A total current, within Class-2 limits.

---

## 🧱 Hardware Building Blocks

### Power Source Equipment (PSE)
- Isolated DC/DC converters (12 / 24 / 48 V)
- Per-port eFuses & current sensors
- MCU with ADCs for voltage, current, temperature
- UART/CAN for telemetry & load policy
- Safe default-off firmware implementing handshake logic

### Powered Device (PD)
- Signature + ID resistors for voltage/current
- Ideal diode & soft-start FET
- Optional UART for telemetry
- Internal DC/DC as needed for final rail

---

## 🧰 Getting Started (Prototype Plan)
1. Build 2-channel PSE board (12/24/48 V @ 2 A each).  
2. Build PD dummies (10 W/30 W loads with resistor ID).  
3. Implement handshake in firmware (probe → negotiate → soft-start).  
4. Test protection events: short, overload, missing heartbeat.  
5. Add UART diagnostics and per-port metering.

---

## 🔐 Safety & Compliance
- **Always SELV / Class-2.** Never exceed 57 V or 100 W per channel.  
- **Isolated PSE outputs** with per-channel eFuse and short-circuit cutoff.  
- **Keyed connectors** to prevent connection to Ethernet networks.  
- **Color-coded ports & cables** by channel and power class.

---

## 🌎 Use Cases
- Off-grid cabins & solar sheds  
- Smart buildings & modular lighting  
- RV / van / marine DC systems  
- IoT and low-voltage automation  
- Educational & research microgrid testbeds  

---

## 🛠️ Project Goals
- Define open electrical + protocol spec  
- Publish reference schematics and firmware  
- Create test hardware for community validation  
- Develop “smart outlet” and “PD dongle” designs  
- Submit to open hardware community for review  

---

## 📜 License
Open hardware and documentation are released under **CERN-OHL-S v2**.  
Firmware under **MIT License** unless otherwise noted.

---

## 🧩 Roadmap
- [ ] Publish reference schematic & PCB (2-ch PSE)  
- [ ] Release PD dev board (adjustable load, UART telemetry)  
- [ ] Implement digital negotiation protocol (UART/CAN)  
- [ ] Build open source firmware library (C / PlatformIO)  
- [ ] Formalize safety testing (UL 1310 Class-2)  

---

## 🧑‍💻 Authors
Project concept & initial draft:  
**Gunnar Blomquist** (@gunnski) and **ChatGPT-5 collaborative prototype**

---

> ⚠️ Experimental technology — not UL listed. Use only in controlled lab environments until certified.


