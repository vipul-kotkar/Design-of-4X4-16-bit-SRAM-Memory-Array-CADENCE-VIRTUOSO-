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
    <img src="vertopal_f4e039de98fe4b5e9e2edb8d44ba6bdc/media/image21.png" alt="Image 2" style="width: 40%;">
    <img src="vertopal_f4e039de98fe4b5e9e2edb8d44ba6bdc/media/image3.png" alt="Image 3" style="width: 40%;">
</div>

---

### Pre-charge Circuit

The pre-charge circuit charges the bit lines (BL and BL') to VDD before a read operation. It consists of:

- **Equalizer transistor** — ensures both bit lines reach nearly equal voltages, reducing required pre-charge time.
- **Bias transistors** — pull each bit line to VDD.

<p align="center">
  <img src="vertopal_f4e039de98fe4b5e9e2edb8d44ba6bdc/media/image22.png" alt="Image" width="600">
</p>

---

### Isolation Circuit

The isolation circuit isolates the sense amplifier from the bit lines once a small change on the bit lines is detected.

<p align="center">
  <img src="vertopal_f4e039de98fe4b5e9e2edb8d44ba6bdc/media/image23.png" alt="Image" width="600">
</p>

---

### Sense Amplifier Circuit

The sense amplifier detects and amplifies the small voltage swing on bit lines to recognisable logic levels during a read operation, enabling reliable data interpretation.

<p align="center">
  <img src="https://github.com/VardhanSuroshi/Memory-Design-And-Testing/assets/132068498/20d01ab4-30c0-4812-b3c7-1a342ab89898" alt="Image" width="600">
</p>

---

### Write Driver Circuit

The write driver forces one bit line to zero while maintaining the pre-charged value on the other, enabling accurate data to be written into the SRAM cell.

<p align="center">
  <img src="vertopal_f4e039de98fe4b5e9e2edb8d44ba6bdc/media/image25.png" alt="Image" width="600">
</p>

---

### Address Decoder Circuit

The row and column to be accessed within the storage array are selected through decoding of binary address information, enabling precise access to the desired cell.

<p align="center">
  <img src="https://github.com/VardhanSuroshi/Memory-Design-And-Testing/assets/132068498/ec9339af-7f44-495b-a531-fc71a5b6b892" alt="Image" width="600">
</p>

---

### SRAM Architecture

Complete single-column SRAM block with all peripheral devices integrated:

<p align="center">
  <img src="vertopal_f4e039de98fe4b5e9e2edb8d44ba6bdc/media/image26.png" alt="Image" width="600">
</p>

---

## Design Methodology

### SRAM Cell Design

<p align="center">
  <img src="vertopal_f4e039de98fe4b5e9e2edb8d44ba6bdc/media/image21.png" alt="Image" width="600">
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
    <img src="vertopal_f4e039de98fe4b5e9e2edb8d44ba6bdc/media/image13.png" alt="Image 2" style="width: 40%;">
    <img src="vertopal_f4e039de98fe4b5e9e2edb8d44ba6bdc/media/image14.png" alt="Image 3" style="width: 40%;">
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
  <img src="vertopal_f4e039de98fe4b5e9e2edb8d44ba6bdc/media/image4.png" alt="Image" width="600">
</p>

---

### Write Operation Analysis

#### Write Logic "0" Operation

Testbench schematic:

<p align="center">
  <img src="vertopal_f4e039de98fe4b5e9e2edb8d44ba6bdc/media/image5.png" alt="Image" width="600">
</p>

Timing diagram:

<p align="center">
  <img src="vertopal_f4e039de98fe4b5e9e2edb8d44ba6bdc/media/image6.png" alt="Image" width="600">
</p>

---

#### Write Logic "1" Operation

Testbench schematic:

<p align="center">
  <img src="vertopal_f4e039de98fe4b5e9e2edb8d44ba6bdc/media/image7.png" alt="Image" width="600">
</p>

Timing diagram:

<p align="center">
  <img src="vertopal_f4e039de98fe4b5e9e2edb8d44ba6bdc/media/image8.png" alt="Image" width="600">
</p>

---

### Read Operation Analysis

#### Read Logic "1" Operation

Testbench schematic:

<p align="center">
  <img src="vertopal_f4e039de98fe4b5e9e2edb8d44ba6bdc/media/image9.png" alt="Image" width="600">
</p>

Timing diagram:

<p align="center">
  <img src="vertopal_f4e039de98fe4b5e9e2edb8d44ba6bdc/media/image10.png" alt="Image" width="600">
</p>

---

#### Read Logic "0" Operation

Testbench schematic:

<p align="center">
  <img src="vertopal_f4e039de98fe4b5e9e2edb8d44ba6bdc/media/image11.png" alt="Image" width="600">
</p>

Timing diagram:

<p align="center">
  <img src="vertopal_f4e039de98fe4b5e9e2edb8d44ba6bdc/media/image12.png" alt="Image" width="600">
</p>

---

### Stability Analysis

#### Hold State Stability Analysis

<p align="center">
  <img src="vertopal_f4e039de98fe4b5e9e2edb8d44ba6bdc/media/image15.png" alt="Image" width="600">
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
  <img src="vertopal_f4e039de98fe4b5e9e2edb8d44ba6bdc/media/image17.png" alt="Image" width="600">
</p>

Butterfly curve — Read state:

<p align="center">
  <img src="vertopal_f4e039de98fe4b5e9e2edb8d44ba6bdc/media/image18.png" alt="Image" width="600">
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
  <img src="vertopal_f4e039de98fe4b5e9e2edb8d44ba6bdc/media/image19.png" alt="Image" width="600">
</p>

Timing diagram:

<p align="center">
  <img src="vertopal_f4e039de98fe4b5e9e2edb8d44ba6bdc/media/image20.png" alt="Image" width="600">
</p>

---

#### Write Driver Circuit Analysis

The write driver was designed for minimal latency. Simulation measures the time taken to produce the required voltage difference on BL and BL':

<p align="center">
  <img src="vertopal_f4e039de98fe4b5e9e2edb8d44ba6bdc/media/image29.png" alt="Image" width="600">
</p>

> **Write driver latency: 0.5249 ns**

---

#### Sense Amplifier Analysis

Simulation measures the time taken to detect a 200 mV sense margin between BL and BL' after WL goes HIGH:

<p align="center">
  <img src="vertopal_f4e039de98fe4b5e9e2edb8d44ba6bdc/media/image28.png" alt="Image" width="600">
</p>

> **Sense amplifier delay: 86.17 ps**

---

### Single SRAM Architecture Analysis

Full schematic with all peripheral devices connected:

<p align="center">
  <img src="vertopal_f4e039de98fe4b5e9e2edb8d44ba6bdc/media/image26.png" alt="Image" width="600">
</p>

Timing diagram — Write "1" → Read "1" → Write "0" → Read "0":

<p align="center">
  <img src="vertopal_f4e039de98fe4b5e9e2edb8d44ba6bdc/media/image27.png" alt="Image" width="600">
</p>

---

### 4×4 (16-bit) Memory Array

A complete 4×4 16-bit memory array was designed with row decoder, column decoder, and row enable circuits. Simulations were performed for both read and write operations across the full array.

Schematic:

<p align="center">
  <img src="vertopal_f4e039de98fe4b5e9e2edb8d44ba6bdc/media/image30.png" alt="Image" width="600">
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

---

## Acknowledgment

I built this project independently to deepen my hands-on proficiency with full-custom SRAM design using the Cadence toolchain. Thanks to the open-source community and public GPDK 180nm documentation for making this kind of self-directed learning possible. Contributions, issues, and discussions are welcome.
