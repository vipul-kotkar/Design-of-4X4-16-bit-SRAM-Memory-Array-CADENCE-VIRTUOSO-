# Design of 4×4 16-bit SRAM Memory Array Using Cadence Virtuoso

## About

This is a personal project by **Vipul Kotkar**, Analog Layout Engineer, exploring the end-to-end design and simulation of a 4×4 SRAM Memory Array using the Cadence Virtuoso toolchain on **GPDK 180nm** technology. The project covers the full design stack — from individual 6T SRAM cell sizing through peripheral circuit design to a complete 16-bit memory array — with a focus on stability analysis, read/write performance, and layout-aware design methodology.

---

## Table of Contents

- [Introduction](#introduction)
- [SRAM Memory Architecture](#sram-memory-architecture)
  - [SRAM Cell](#sram-cell)
  - [Pre-charge Circuit](#pre-charge-circuit)
  - [Isolation Circuit](#isolation-circuit)
  - [Sense Amplifier Circuit](#sense-amplifier-circuit)
  - [Write Driver Circuit](#write-driver-circuit)
  - [Address Decoder Circuit](#address-decoder-circuit)
  - [SRAM Architecture](#sram-architecture)
- [Design Methodology](#design-methodology)
  - [SRAM Cell Design](#sram-cell-design)
    - [Write Operation](#write-operation)
    - [Read Operation](#read-operation)
  - [SRAM Cell Stability](#sram-cell-stability)
    - [Hold State Stability](#hold-state-stability)
    - [Read State Stability](#read-state-stability)
- [Experimental Setup and Simulation Results](#experimental-setup-and-simulation-results)
  - [Write Operation Analysis](#write-operation-analysis)
    - [Write Logic "0" Operation](#write-logic-0-operation)
    - [Write Logic "1" Operation](#write-logic-1-operation)
  - [Read Operation Analysis](#read-operation-analysis)
    - [Read Logic "1" Operation](#read-logic-1-operation)
    - [Read Logic "0" Operation](#read-logic-0-operation)
  - [Stability Analysis](#stability-analysis)
    - [Hold State Stability Analysis](#hold-state-stability-analysis)
    - [Read State Stability Analysis](#read-state-stability-analysis)
  - [Peripheral Device Design and Analysis](#peripheral-device-design-and-analysis)
    - [Pre-charge Circuit Analysis](#pre-charge-circuit-analysis)
    - [Write Driver Circuit Analysis](#write-driver-circuit-analysis)
    - [Sense Amplifier Analysis](#sense-amplifier-analysis)
  - [Single SRAM Architecture Analysis](#single-sram-architecture-analysis)
  - [4×4 (16-bit) Memory Array](#4x4-16-bit-memory-array)
- [Conclusion](#conclusion)

---

## Introduction

<p align="center">
  <img src="https://github.com/VardhanSuroshi/Memory-Design-And-Testing/assets/132068498/2bd9162e-16e1-4c0f-8dfa-0384a5e04e2f" alt="Image" width="600">
</p>

In modern computer systems, various types of memory are employed to store and manage data. These memories can be broadly categorised into the following categories.

---

### Volatile Memory

Volatile memory is temporary storage that loses its contents when the power is turned off. The primary volatile memory types include RAM, which allows quick read-and-write access, making it essential for active applications and the operating system. There are mainly two kinds of RAM:

1. **Dynamic Random Access Memory (DRAM):** Stores each bit of data in a separate capacitor within an integrated circuit. Requires periodic refresh cycles to maintain data integrity.

2. **Static Random Access Memory (SRAM):** Does not require refreshing. Often used for cache memory in CPUs due to its faster access times compared to DRAM.

---

### Non-Volatile Memory

Non-volatile memory retains its data even when power is turned off. Common types include:

1. **Read-Only Memory (ROM):** Used to store permanent data, typically in firmware and software.

2. **Flash Memory:** A type of non-volatile storage that can be electrically erased and reprogrammed. Commonly used in USB drives, SSDs, and memory cards.

---

### Static Random Access Memory (SRAM)

SRAM is a type of volatile memory that uses bistable latching circuitry to store each bit. Unlike DRAM, SRAM does not require refreshing, making it faster and more energy-efficient. SRAM cells are a fundamental building block of modern computing systems and a key technology for achieving high-speed data processing.

---

### Applications of SRAM

1. **Cache Memory:** Used in CPUs to provide fast access to frequently used data.
2. **Register Files:** Used in microprocessors for storing temporary data during computation.
3. **Networking Devices:** Employed for buffering and storing routing tables.
4. **Embedded Systems:** Provides quick access to critical data.

---

## Project Objectives

The primary objective of this project is to gain a comprehensive understanding of SRAM by designing a complete 6T SRAM cell, including its peripheral circuitry, using Cadence Virtuoso on the 180nm technology node (GPDK180). The project explores transistor sizing for read/write stability, peripheral circuit design for high-speed operation, and Static Noise Margin (SNM) analysis.

---

## SRAM Memory Architecture

The SRAM memory architecture employs a systematic arrangement of cells, word lines, and bit lines, all orchestrated through precise address decoding.

<p align="center">
  <img src="https://github.com/VardhanSuroshi/Memory-Design-And-Testing/assets/132068498/88071ae3-79a3-4988-8edc-d381d4e3c322" alt="Image" width="600">
</p>

---

### SRAM Cell

The fundamental storage unit is a 6T SRAM cell, which holds a single bit of data (logic 1 or 0). The architecture is designed for efficient data storage and retrieval.

<div style="display: flex; justify-content: space-between; align-items: center;">
    <img width="2475" height="1333" alt="image21" src="https://github.com/user-attachments/assets/ef2e9e12-5498-42b5-8486-d2d30ec13e7b" />
<img width="248" height="188" alt="image3" src="https://github.com/user-attachments/assets/6692e524-4968-4b29-bea5-1fd653fd7cce" />
</div>

---

### Pre-charge Circuit

The pre-charge circuit charges the bit lines (BL and BL') to VDD before a read operation. It consists of:

- **Equalizer transistor** — ensures both bit lines reach nearly equal voltages, reducing required pre-charge time.
- **Bias transistors** — pull each bit line to VDD.

<p align="center">
<img width="910" height="471" alt="image22" src="https://github.com/user-attachments/assets/16ab096d-d670-48f9-843c-e55624dd2a36" />

</p>

---

### Isolation Circuit

The isolation circuit isolates the sense amplifier from the bit lines once a small change on the bit lines is detected.

<p align="center">
<img width="1727" height="880" alt="image23" src="https://github.com/user-attachments/assets/3a818883-fc94-4bf0-93a5-95022e50fc45" />

</p>

---

### Sense Amplifier Circuit

The sense amplifier detects and amplifies the small voltage swing on bit lines to recognisable logic levels during a read operation, enabling reliable data interpretation.

<p align="center">
 <img width="948" height="680" alt="315063291-20d01ab4-30c0-4812-b3c7-1a342ab89898" src="https://github.com/user-attachments/assets/90f698c1-355e-4944-a8d9-a8d54b5a77b8" />

</p>

---

### Write Driver Circuit

The write driver forces one bit line to zero while maintaining the pre-charged value on the other, enabling accurate data to be written into the SRAM cell.

<p align="center">
<img width="2049" height="1330" alt="image25" src="https://github.com/user-attachments/assets/be2b6e07-320c-4c89-9101-e59b5107d18e" />
</p>

---

### Address Decoder Circuit

The row and column to be accessed within the storage array are selected through decoding of binary address information, enabling precise access to the desired cell.

<p align="center">
<img width="1145" height="926" alt="314407487-ec9339af-7f44-495b-a531-fc71a5b6b892" src="https://github.com/user-attachments/assets/db8f2390-52a5-4fff-9bdf-93d0c6e89896" />

</p>

---

### SRAM Architecture

Complete single-column SRAM block with all peripheral devices integrated:

<p align="center">
 <img width="3532" height="4120" alt="image26" src="https://github.com/user-attachments/assets/8024c151-0e51-43ba-bb98-8239661f622b" />

</p>

---

## Design Methodology

### SRAM Cell Design

<p align="center">
 <img width="2475" height="1333" alt="image21" src="https://github.com/user-attachments/assets/5033f18e-4e62-460a-af25-74fe21cbfd17" />

</p>

---

#### Write Operation

Consider writing logic "0" into a cell currently holding logic "1":

- The write driver sets BL = 0V and BL' = VDD.
- The decoder enables the corresponding word line (WL = 1), connecting bit lines to the cell via access transistors.
- Node Q is pulled below the threshold voltage of M1, turning it OFF and allowing the new value to be written.
- The pull-up ratio constraint ensures the access transistors are stronger than pull-up transistors, enabling a successful write.

> **Key constraint:** Access transistors must be stronger than pull-up transistors — governed by the **Pull-Up Ratio (PR)**.

---

#### Read Operation

Consider reading a cell storing logic "1" (Q = 1, Q' = 0):

- BL and BL' are pre-charged to VDD.
- WL goes HIGH, turning on access transistors M5 and M6.
- BL' discharges through M5–M1 while BL remains at VDD, creating a voltage difference ΔV.
- The sense amplifier detects ΔV and resolves the output to logic "1".

> **Key constraint:** The cell ratio (β) ensures ΔV does not exceed the threshold of M3, preventing read upset.

---

### Transistor Sizing

The two key parameters governing transistor sizing are:

**a) Cell Ratio (β):**
```
β = (W/L)_pulldown / (W/L)_access
```
β should be greater than 1 (typically ~1.25–1.5) to ensure read stability.

**b) Pull-Up Ratio (PR):**
```
PR = (W/L)_pullup / (W/L)_access
```
PR should be less than 1 (typically ~0.85) to ensure write-ability.

---

### SRAM Cell Stability

Cell stability is characterised by the **Butterfly Curve** — obtained by superimposing the voltage transfer curves (VTC) of both inverters. The **Static Noise Margin (SNM)** is measured as the side length of the largest square that fits within a butterfly wing.

<div style="display: flex; justify-content: space-between; align-items: center;">
   <img width="576" height="440" alt="image13" src="https://github.com/user-attachments/assets/25b60bb2-2911-43ce-bf69-035014c759f5" />
<img width="341" height="306" alt="image14" src="https://github.com/user-attachments/assets/5f390a06-3857-4cc8-8bda-689405917dee" />

</div>

*Figure: (a) Butterfly curve — Hold state. (b) Butterfly curve — Read state.*

---

#### Hold State Stability

With WL = 0 and access transistors OFF, the cell is isolated from BL/BL'. The inverters operate at their ideal VTC, yielding maximum SNM.

---

#### Read State Stability

With WL = 1 and bit lines pre-charged to VDD, the voltage divider effect at the internal node degrades the butterfly curve, reducing the box size and therefore the SNM. The larger the cell ratio β, the higher the read SNM.

---

## Experimental Setup and Simulation Results

**Design Specifications — 6T SRAM cell at 180nm (GPDK180), CR = 1.25, PR = 0.85:**

| Parameter | Value |
|---|---|
| Length of all MOS | 180 nm |
| Width of PMOS pull-up (Wp) | 400 nm (minimum) |
| Width of access transistors (Wa) | 470 nm (1.2 × Wp) |
| Width of NMOS pull-down (Wn) | 590 nm (1.5 × Wa) |
| External BL/BL' capacitance | 120 fF |

Circuit diagram of the 6T SRAM cell:

<p align="center">
 <img width="2412" height="1333" alt="image4" src="https://github.com/user-attachments/assets/70d2e627-337e-438f-95b1-9dbd671cef56" />

</p>

---

### Write Operation Analysis

#### Write Logic "0" Operation

Testbench schematic:

<p align="center">
  <img width="691" height="358" alt="image5" src="https://github.com/user-attachments/assets/d3191f2c-9a59-49a5-9d66-d6aff57d109d" />

</p>

Timing diagram:

<p align="center">
 <img width="800" height="600" alt="image6" src="https://github.com/user-attachments/assets/f94fbf7f-0bff-427d-9b4f-46b3aeb024c5" />

</p>

---

#### Write Logic "1" Operation

Testbench schematic:

<p align="center">
<img width="707" height="398" alt="image7" src="https://github.com/user-attachments/assets/a136ffda-3acb-4a0f-ab00-52831067d084" />

</p>

Timing diagram:

<p align="center">
  <img width="800" height="600" alt="image8" src="https://github.com/user-attachments/assets/0fe532ad-7b26-44bc-9127-dc8d2001de6d" />

</p>

---

### Read Operation Analysis

#### Read Logic "1" Operation

Testbench schematic:

<p align="center">
<img width="1348" height="730" alt="image9" src="https://github.com/user-attachments/assets/aadb7d7c-b0e1-4ce3-9bf2-d231f31cfa9b" />

</p>

Timing diagram:

<p align="center">
 <img width="1368" height="503" alt="image10" src="https://github.com/user-attachments/assets/0bd09a50-109c-47cd-ac52-a25c2a393618" />

</p>

---

#### Read Logic "0" Operation

Testbench schematic:

<p align="center">
 <img width="677" height="376" alt="image11" src="https://github.com/user-attachments/assets/b2c22fb9-8c98-44ec-bea5-72b3dd3509ac" />

</p>

Timing diagram:

<p align="center">
  <img width="1368" height="535" alt="image12" src="https://github.com/user-attachments/assets/89f893d9-acdf-48d2-8632-bd5f26d0d714" />

</p>

---

### Stability Analysis

#### Hold State Stability Analysis

<p align="center">
 <img width="1407" height="789" alt="image15" src="https://github.com/user-attachments/assets/4785b6e7-cf1e-46e5-9ca0-f42e29fe9098" />

</p>

Butterfly curve — Hold state:

<p align="center">
  <img src="vertopal_f4e039de98fe4b5e9e2edb8d44ba6bdc/media/image16.png" alt="Image" width="600">
</p>

```
SNM = min(SNM1, SNM2)
    = min(578.869 mV, 585.075 mV)
    = 578.869 mV
```

---

#### Read State Stability Analysis

<p align="center">
 <img width="710" height="410" alt="image17" src="https://github.com/user-attachments/assets/af6211f6-d28e-46be-afdb-fadc7fe8700c" />

</p>

Butterfly curve — Read state:

<p align="center">
<img width="800" height="600" alt="image18" src="https://github.com/user-attachments/assets/0ded70b6-4b5e-438a-a61c-c7742e0b30ab" />

</p>

```
SNM = min(SNM1, SNM2)
    = min(225.417 mV, 218.623 mV)
    = 218.623 mV
```

---

### Peripheral Device Design and Analysis

#### Pre-charge Circuit Analysis

The pre-charge circuit was designed to charge BL and BL' (120 fF load each) to VDD within **0.5 ns**.

Testbench schematic (transistor width = 12 µm):

<p align="center">
 <img width="1177" height="502" alt="image19" src="https://github.com/user-attachments/assets/4742bbcd-b211-46d1-82c9-521e1a292f4d" />

</p>

Timing diagram:

<p align="center">
 <img width="842" height="525" alt="image20" src="https://github.com/user-attachments/assets/5cf412e2-7eac-4196-a208-8494a32a82e5" />

</p>

---

#### Write Driver Circuit Analysis

The write driver was designed for minimal latency. Simulation measures the time taken to produce the required voltage difference on BL and BL':

<p align="center">
<img width="893" height="609" alt="image29" src="https://github.com/user-attachments/assets/8e815f76-2043-493a-9379-1d6028c34967" />

</p>

> **Write driver latency: 0.5249 ns**

---

#### Sense Amplifier Analysis

Simulation measures the time taken to detect a 200 mV sense margin between BL and BL' after WL goes HIGH:

<p align="center">
  <img width="1388" height="698" alt="image28" src="https://github.com/user-attachments/assets/da6e760a-2f73-4292-8d41-a4718aeef25b" />

</p>

> **Sense amplifier delay: 86.17 ps**

---

### Single SRAM Architecture Analysis

Full schematic with all peripheral devices connected:

<p align="center">
 <img width="3532" height="4120" alt="image26" src="https://github.com/user-attachments/assets/7f9d3df6-f985-4224-989f-1b2ec4891153" />

</p>

Timing diagram — Write "1" → Read "1" → Write "0" → Read "0":

<p align="center">
 <img width="1096" height="494" alt="image27" src="https://github.com/user-attachments/assets/fde18aa1-8add-47ef-8211-1f0c3693dd83" />

</p>

---

### 4×4 (16-bit) Memory Array

A complete 4×4 16-bit memory array was designed with row decoder, column decoder, and row enable circuits. Simulations were performed for both read and write operations across the full array.

Schematic:

<p align="center">
 <img width="3268" height="3016" alt="image30" src="https://github.com/user-attachments/assets/37ded8ac-e461-4155-bd45-207ab6a19479" />

</p>

---

## Key Results Summary

| Metric | Value |
|---|---|
| Hold State SNM | **578.87 mV** |
| Read State SNM | **218.62 mV** |
| Sense Amplifier Delay (200 mV margin) | **86.17 ps** |
| Write Driver Latency | **0.5249 ns** |
| Pre-charge Time (120 fF load) | **0.5 ns** |
| Technology Node | **GPDK 180nm** |
| Array Size | **4×4 (16-bit)** |

---

## Conclusion

Through this project, I designed, simulated, and analysed a complete SRAM architecture — comprising a 6T SRAM cell, pre-charge circuit, isolation circuit, sense amplifier, and write driver circuit — on GPDK 180nm using the Cadence Virtuoso toolchain. Key results include a Hold-state SNM of **578 mV**, a read sensing delay of **86 ps**, and a fully verified **4×4 16-bit memory array** with row and column decode logic.

The project represents a practical deep-dive into full-custom analog/mixed-signal memory design and serves as a portfolio reference for SRAM cell sizing, stability analysis, transistor ratio optimisation, and peripheral circuit design in a production-grade EDA environment.

