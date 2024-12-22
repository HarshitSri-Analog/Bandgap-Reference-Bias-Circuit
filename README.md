# Bandgap Reference (BGR) Bias Circuit

## Table of Contents
- [Overview](#overview)
- [What is a Bandgap Reference (BGR) and Why Is It Needed?](#what-is-a-bandgap-reference-bgr-and-why-is-it-needed)
- [Understanding PTAT and CTAT Voltages](#understanding-ptat-and-ctat-voltages)
- [Balanced Amplifier in BGR Circuit](#balanced-amplifier-in-bgr-circuit)
  - [Specifications of the Balanced Amplifier](#specifications-of-the-balanced-amplifier)
- [Types of Bandgap Reference Circuits](#types-of-bandgap-reference-circuits)
  - [1. BGR with Current Mirror](#1-bgr-with-current-mirror)
  - [2. BGR with Op-Amp](#2-bgr-with-op-amp)
- [Final Circuit: BGR with Balanced Amplifier and Current Mirror](#final-circuit-bgr-with-balanced-amplifier-and-current-mirror)
- [Importance of the Startup Circuit](#importance-of-the-startup-circuit)
- [Results Achieved](#results-achieved)
- [Repository Contents](#repository-contents)

## Overview
A Bandgap Reference (BGR) circuit is a fundamental building block in integrated circuits, designed to provide a stable reference voltage that remains constant across variations in temperature, power supply, and fabrication process. This makes it an essential component in analog, mixed-signal, and digital systems. 

This repository documents the design, analysis, and simulation of a BGR bias circuit, providing detailed explanations of its principles, different implementations, and final results achieved through simulations.

## What is a Bandgap Reference (BGR) and Why Is It Needed?
A Bandgap Reference (BGR) circuit generates a voltage reference that is independent of temperature, power supply, and process variations. It typically provides a reference voltage of approximately 1.2V for silicon-based processes. This stability is crucial for ensuring consistent performance in circuits like analog-to-digital converters (ADCs), digital-to-analog converters (DACs), voltage regulators, and other systems relying on precise voltage references.

Temperature variations can cause significant drift in voltage levels. A BGR circuit addresses this by balancing temperature-dependent voltages to produce a consistent output across the operating range.

***Fun Fact**: This bias circuit is called the Bandgap Reference Bias circuit because the constant reference voltage (Vref) obtained, approximately 1.2V, is nearly equal to the energy bandgap of silicon.*

## Understanding PTAT and CTAT Voltages
To achieve temperature independence, BGR circuits rely on two temperature-dependent voltage components:

- **PTAT (Proportional to Absolute Temperature):** This component increases linearly with temperature and is typically derived from the thermal voltage.

- **CTAT (Complementary to Absolute Temperature):** This component decreases linearly with temperature and is generated using the base-emitter voltage (V_BE) of a bipolar junction transistor (BJT).

By combining the PTAT and CTAT components in appropriate proportions, the temperature dependencies cancel each other out, resulting in a stable reference voltage.

***Note**: We cannot just add any PTAT with any CTAT to get a constant reference voltage, because they both need to cancel out each other completely in order to give a constant voltage.*

| ![PTAT-CTAT](https://github.com/HarshitSri-Analog/Bandgap-Reference-Bias-Circuit/blob/main/Images/PTAT%20and%20CTAT.png) | 
| :---: | 
| Fig 1: PTAT and CTAT behaviour |

## Balanced Amplifier in BGR Circuit
In this design, a **balanced amplifier** is used instead of a conventional operational amplifier. The balanced amplifier offers several advantages, including:

- **Higher Gain:** Ensures better precision in the feedback loop.
- **Improved Stability:** Enhances the circuit's overall robustness against process and temperature variations.
- **Superior Power Supply Rejection (PSR):** Reduces the influence of power supply fluctuations on the reference voltage.

The detailed design and analysis of the balanced amplifier are provided in the repository under the documentation section [here](https://github.com/HarshitSri-Analog/Bandgap-Reference-Bias-Circuit/tree/main/Balanced%20Ampr). 

### Specifications of the Balanced Amplifier
| **Parameter**       | **Value**       |
|---------------------|-----------------|
| Voltage Gain        | 55 dB          |
| Unity Gain Bandwidth (UGB) | 13.42 MHz    |
| Phase Margin        | 69.42 degrees  |
| Power Consumption   | 0.28 mW        |

This amplifier's performance significantly contributes to the accuracy and stability of the BGR circuit.

## Types of Bandgap Reference Circuits

### 1. BGR with Current Mirror
This implementation uses a current mirror to combine PTAT and CTAT currents. The currents are mirrored and converted into voltages, which are then summed to generate the stable reference voltage. This approach is relatively simple and compact but may require precise current scaling to ensure accurate cancellation of temperature variations.


| ![BGR current mirror](https://github.com/HarshitSri-Analog/Bandgap-Reference-Bias-Circuit/blob/main/Images/BGR%20Current%20Mirror.png) | 
| :---: | 
| Fig 2: BGR using Current Mirror |

### 2. BGR with Op-Amp
This design incorporates an operational amplifier (op-amp) to achieve higher precision. The op-amp enforces proper feedback in the circuit, ensuring accurate voltage levels and improved stability by minimizing mismatches. The op-amp ensures equal voltages across the PTAT and CTAT branches, allowing for precise addition of the temperature-dependent components.

| ![BGR OpAmp](https://github.com/HarshitSri-Analog/Bandgap-Reference-Bias-Circuit/blob/87618c8f63555c633de72ab075af02026f515996/Images/BGR%20opamp.png) | 
| :---: | 
| Fig 3: BGR using Op-Amp |

## Final Circuit: BGR with Balanced Amplifier and Current Mirror
The final design combines the benefits of both the current mirror and the balanced amplifier. Key features include:

- **Current Mirror:** Ensures accurate current scaling and improves matching, leading to better cancellation of temperature-dependent components.
- **Balanced Amplifier:** Enhances stability, precision, and power supply rejection, ensuring the circuit operates consistently across all conditions.

This hybrid design is optimized for compactness, low power consumption, and robustness, making it suitable for integration in modern systems.

| ![BGR final](https://github.com/HarshitSri-Analog/Bandgap-Reference-Bias-Circuit/blob/main/Images/BGR%20Final.png) | 
| :---: | 
| Fig 4: BGR using Current Mirror & Op-Amp (Final optimized deisgn) |

## Importance of the Startup Circuit
A BGR circuit requires a **startup circuit** to ensure proper initialization. Without this, the circuit could remain in an undesirable zero-current state during power-up, failing to generate the reference voltage. There are 2 main components of any startup circuit:
1. The component that disturbs the zero current condition either by pulling a node to +Vdd or gnd.
2. Another component disconnects the first one from the main circuit after the main circuit begins operating in its normal current state.

The startup circuit:
1. Drives the circuit out of the zero-current state during initialization.
2. Automatically deactivates once the circuit achieves its normal operating state, minimizing power consumption and interference.

***Note: Any self-bias circuitry requires a startup circuit to ensure it always operates in its normal current state, and this circuit is no exception.***

## Results Achieved
The designed Bandgap Reference circuit demonstrates exceptional performance, achieving the following metrics:

| **Metric**                   | **Value**              |
|------------------------------|-----------------------|
| Reference Voltage (Vref)    | 1.2V                 |
| Temperature Variation of Vref | 1.68 mV (-40 to 125 degree celsius) |
| Power Supply Variation of Vref | 12 mV (1.8 to 3.6 Volts) |
| Temperature Coefficient ([TempCo](https://www.allaboutcircuits.com/technical-articles/understanding-the-temperature-coefficient-of-a-voltage-reference/)) | 8.905 ppm/°C       |

These results highlight the circuit’s stability and reliability in maintaining a constant reference voltage under varying conditions.

## Repository Contents

This repository includes:

- **Schematics:** Detailed circuit diagrams for each implementation.
- **Simulation Results:** Graphs and data showcasing the stability of the reference voltage across temperature and power supply variations.
- **Documentation:** Step-by-step explanations of the design process and methodologies.
- **Balanced Amplifier Report:** A detailed analysis of the balanced amplifier design and its performance specifications.

Feel free to explore the repository for insights into the design and implementation of Bandgap Reference circuits. Contributions and feedback are welcome!
***If you find this repository helpful, please consider giving it a ⭐!***

