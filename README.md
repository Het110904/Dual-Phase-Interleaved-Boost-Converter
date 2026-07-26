# Closed-Loop Dual-Phase Interleaved Boost Converter using SG3525

<img width="1280" height="720" alt="MASTER PIC 3" src="https://github.com/user-attachments/assets/fd572cf9-7125-4254-b6bf-b0b5ea699a4a" />

> **Project Summary**
> This repository documents the complete breadboard implementation of a closed-loop, dual-phase interleaved boost converter driven by the SG3525 PWM controller. Built entirely from the ground up, this project details the step-by-step engineering bring-up sequence—from initial controller logic verification to final active load testing with a 24 V BLDC fan. The chosen topology minimizes input/output ripple and distributes thermal stress across two independent power stages.

---

## Architecture & Action Proof

https://github.com/user-attachments/assets/d43f7dc7-85cf-4df3-8651-d1d11d1a3d49


---

## Specifications & Hardware Components

**Project Specifications**
* **Topology:** Closed-Loop Dual-Phase Interleaved Boost Converter
* **PWM Controller:** SG3525A
* **Input Voltage:** 12 V DC
* **Output Voltage (No Load):** 11.8 V – 28.3 V
* **Output Voltage (24 V BLDC Fan Load):** Up to 18.85 V
* **Oscillator Frequency:** 95.3 kHz
* **Switching Frequency per Phase:** 47.6 kHz
* **Interleaving:** 180° out of phase
* **Number of Phases:** 2
* **Power MOSFETs:** 2 × IRFZ44N
* **Inductors:** 2 × 220 µH Toroidal
* **Output Capacitor:** 4700 µF
* **Load Tested:** 24 V Circle BLDC Fan (120 × 120 mm)

**Complete Bill of Materials (BOM)**
* 1 × SG3525A PWM Controller IC
* 2 × IRFZ44N N-Channel Power MOSFETs
* 2 × 220 µH, 2.4 Amps Toroidal Inductors
* 2 × SF24 Fast Recovery Boost Diodes
* 2 x 2.2 µF Electroylytic Soft Start Capacitors
* 1 × 4700 µF Electrolytic Output Capacitor
* 1 x WH148 Potentiometer
* 1 × 4.7 kΩ Bleeder Resistor (2 W)
* 2 x 22 Ω Gate Resistors
* 1 × SF24 Flyback Diode
* 1 x MAS830L Multimeter
* 1 x 12 V, 2.5 Amps Adapter 
* 1 x Female DC Jack
* Assorted Ceramic Capacitors (1 nF, 100 nF, 2.2 nF, 4.7 nF)
* Assorted Electrolytic Capacitors (2.2 µF)
* Assorted Resistors (1 kΩ, 2.2 kΩ, 10 kΩ, 15 kΩ)
* Breadboards and Solid Core Jumper Wires

---

## Complete Building Timeline

This converter was systematically brought up subsystem by subsystem to isolate variables, verify control logic, and ensure total stability before scaling to high-power, dual-phase switching.

**Stage 1: SG3525 Controller Bring-up**
Verified VCC, VREF, and basic oscillator operation.

**Stage 2: Oscillator Configuration**
Configured RT (15 kΩ) and CT (1 nF) to establish the 95.3 kHz primary switching frequency.

**Stage 3: Dead-Time Configuration**
Minimized dead-time by shorting Pin 7 (Discharge) directly to Pin 5 (CT).

**Stage 4: Soft-Start Circuit**
Implemented a 4.4 µF (2 × 2.2 µF) capacitance on Pin 8 to ground for smooth start-up.

**Stage 5: Reference Network**
Created a reference voltage divider from Pin 16 (5 V) to properly bias Pin 2.

**Stage 6: Compensation Network**
Added the series resistor and capacitor network between Pin 1 and Pin 9 for error amplifier stability.


<img width="720" height="1280" alt="SG3525 and surroundings" src="https://github.com/user-attachments/assets/717fc17a-55e4-44aa-924a-af1324768b3f" />


**Stage 7: Temporary Feedback Verification**
Designed the feedback divider network and verified its tracking capability safely using an independent, temporary 12 V source.

**Stage 8: Single-Phase Boost Converter**
Built and powered the first power stage (Phase 1) using a single IRFZ44N and 220 µH inductor.

**Stage 9: Closed-Loop Verification**
Confirmed successful closed-loop voltage regulation on the single-phase setup.

**Stage 10: Breadboard Rearrangement**
Dismantled temporary components and rearranged the board to accommodate the interleaving hardware.

**Stage 11: Second Boost Phase**
Built and tested Phase 2. Both phases now operate 180° out of phase, sharing the single output node.

**Stage 12: Feedback Network Rebuilt**
Reconstructed the fixed feedback divider network from the newly established dual-phase output node.

**Stage 13: Final Feedback & Compensation Network**
Locked in the final resistor/capacitor values for the control loop based on hardware constraints.

**Stage 14: Completed Converter**
Successfully achieved a fully functional Closed-Loop Dual-Phase Interleaved Boost Converter.

**Stage 15: Final BLDC Fan Validation**
Connected the 24 V Circle BLDC Fan and proved the converter's stability under an active, continuous load.

<img width="1280" height="720" alt="18 68 V, 24 V BLDC FAN LOAD" src="https://github.com/user-attachments/assets/185a076e-d61a-45db-b258-6bb7fdac8e3b" />

---

## Final SG3525 Configuration

**Core Logic Setup:**
* **Oscillator:** RT = 15 kΩ, CT = 1 nF (102) → 95.3 kHz Total
* **Dead Time:** Pin 7 shorted to Pin 5
* **Soft Start:** 4.4 µF capacitor (Pin 8 → Ground)
* **Shutdown:** Pin 10 → Ground
* **Reference:** Pin 16 (5 V) divided down to bias Pin 2 to ≈ 2.5 V

**Final Feedback Network:**

       VOUT
        │
      10 kΩ
        │
        ├────────────► Pin 1 (Inv. Input)
        │
       1 kΩ
        │
       GND


**Final Compensation Network:**

      Pin 9 (COMP)
        │
        ├── 2.2 kΩ
        │
        ├── 2.2 nF (222)
        │
        └────────────► Pin 1 (Inv. Input)

      Pin 1
        │
        └── 100 nF (104)
        │
       GND


**Output & Protection Network:**
* **Output Filter:** 4700 µF Electrolytic + 100 nF Ceramic in parallel.
* **Bleeder:** 4.7 kΩ across output for fast, safe voltage settling.
* **RC Filter:** 10 kΩ + 100 nF (104) across VOUT for cleaner feedback sensing.
* **Supply Decoupling:** 2 × 100 nF (104) between Pin 15 and Pin 12; 1 × 100 nF (104) between Pin 16 and Pin 12.
* **Motor Protection:** SF24 Flyback Diode connected directly across the BLDC Fan load.

---

## Final Results & Electrical Measurements

**Final Hardware Validation Results:**
* Successfully designed and implemented a Closed-Loop Dual-Phase Interleaved Boost Converter using the SG3525 PWM Controller.
* Achieved a stable output adjustable from 11.8 V to 28.3 V under no-load conditions.
* Successfully powered a 24 V Circle BLDC Fan with a maximum regulated output of 18.85 V.
* Completed continuous operation testing for approximately 45 minutes with zero shutdowns or instability observed.

### Performance Data
| Condition | Input Voltage | Output Voltage | Notes |
| :--- | :--- | :--- | :--- |
| **No Load (Minimum)** | 12 V DC | 11.8 V | Stable regulation |
| **No Load (Maximum)** | 12 V DC | 28.3 V | Hardware ceiling |
| **Active Load** | 12 V DC | 18.85 V | 24 V Circle BLDC Fan attached |

### Final SG3525 Pin Measurements
*Measured directly at a continuous 18.85 V output driving the BLDC Fan load.*

| Pin | Function | Measured Voltage | Pin | Function | Measured Voltage |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **1** | Inverting Input | 1.64 V | **9** | Compensation Output | 5.58 V |
| **2** | Non-Inverting Input | 2.44 V | **10** | Shutdown | -0.10 V |
| **3** | Sync | -0.09 V | **11** | Output A | 5.91 V |
| **4** | Oscillator Output | 0.00 V | **12** | Ground | -0.10 V |
| **5** | CT | 1.53 V | **13** | VC | 11.73 V |
| **6** | RT | 3.61 V | **14** | Output B | 4.92 V |
| **7** | Discharge | 1.53 V | **15** | VCC | 11.72 V |
| **8** | Soft-Start | 4.67 V | **16** | VREF (5V) | 4.96 V |

---

## Additional Hardware Angles

<details>
  <summary>Click here to expand additional routing and layout photographs</summary>

  * **Full Breadboard Layout:** 

  <img width="1280" height="720" alt="MASTER PIC 2" src="https://github.com/user-attachments/assets/b511659b-5887-4db2-91fb-bc100afba613" />

  <img width="1280" height="720" alt="Diagonal 1" src="https://github.com/user-attachments/assets/259382cd-bbe9-4ead-834c-bc43269da7ab" />

  <img width="1280" height="720" alt="Top view" src="https://github.com/user-attachments/assets/fbc33e0b-cf35-4579-8771-a9d6295978ae" />



  * **Secondary Macro Angle:** 

  <img width="1280" height="720" alt="Power Stage Close Up" src="https://github.com/user-attachments/assets/771c5950-00fe-465d-987c-dfc79c386eda" />

  <img width="1280" height="720" alt="Close UP MASTER PIC" src="https://github.com/user-attachments/assets/ee0b03ce-5489-4b4a-8ab8-ff158ccf0c33" />



</details>
