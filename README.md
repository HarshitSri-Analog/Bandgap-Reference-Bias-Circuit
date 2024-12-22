# Bandgap Reference (BGR) Bias Circuit

## Overview
A Bandgap Reference (BGR) circuit is a fundamental building block in integrated circuits, designed to provide a stable reference voltage that remains constant across variations in temperature, power supply, and fabrication process. This makes it an essential component in analog, mixed-signal, and digital systems. 

This repository documents the design, analysis, and simulation of a BGR bias circuit, providing detailed explanations of its principles, different implementations, and final results achieved through simulations.

## What is a Bandgap Reference (BGR) and Why Is It Needed?
A Bandgap Reference (BGR) circuit generates a voltage reference that is independent of temperature, power supply, and process variations. It typically provides a reference voltage of approximately 1.2V for silicon-based processes. This stability is crucial for ensuring consistent performance in circuits like analog-to-digital converters (ADCs), digital-to-analog converters (DACs), voltage regulators, and other systems relying on precise voltage references.

Temperature variations can cause significant drift in voltage levels. A BGR circuit addresses this by balancing temperature-dependent voltages to produce a consistent output across the operating range.

## Understanding PTAT and CTAT Voltages
To achieve temperature independence, BGR circuits rely on two temperature-dependent voltage components:

- **PTAT (Proportional to Absolute Temperature):** This component increases linearly with temperature and is typically derived from the thermal voltage.

- **CTAT (Complementary to Absolute Temperature):** This component decreases linearly with temperature and is generated using the base-emitter voltage (V_BE) of a bipolar junction transistor (BJT).

| ![PTAT-CTAT]() | 
| :---: | 
| Fig 1: PTAT and CTAT behaviour |

By combining the PTAT and CTAT components in appropriate proportions, the temperature dependencies cancel each other out, resulting in a stable reference voltage.

## Types of Bandgap Reference Circuits

### 1. BGR with Current Mirror
This implementation uses a current mirror to combine PTAT and CTAT currents. The currents are mirrored and converted into voltages, which are then summed to generate the stable reference voltage. This approach is relatively simple and compact but may require precise current scaling to ensure accurate cancellation of temperature variations.

| ![BGR current mirror]() | 
| :---: | 
| Fig 2: BGR using Current Mirror |

### 2. BGR with Op-Amp
This design incorporates an operational amplifier (op-amp) to achieve higher precision. The op-amp enforces proper feedback in the circuit, ensuring accurate voltage levels and improved stability by minimizing mismatches. The op-amp ensures equal voltages across the PTAT and CTAT branches, allowing for precise addition of the temperature-dependent components.

| ![BGR OPAMP]() | 
| :---: | 
| Fig 3: BGR using Op-Amp |

## Final Circuit: BGR with Op-Amp and Current Mirror
The final design combines the benefits of both the current mirror and op-amp approaches. Key features include:

- **Current Mirror:** Ensures accurate current scaling and improves matching, leading to better cancellation of temperature-dependent components.
- **Op-Amp:** Enhances stability and precision through feedback, ensuring the circuit operates consistently across all conditions.

This hybrid design is optimized for compactness, low power consumption, and robustness, making it suitable for integration in modern systems.

| ![BGR final]() | 
| :---: | 
| Fig 2: BGR using Current Mirror & Op-Amp (Final optimised deisgn) |

## Importance of the Startup Circuit
A BGR circuit requires a **startup circuit** to ensure proper initialization. Without this, the circuit could remain in an undesirable zero-current state during power-up, failing to generate the reference voltage. 

The startup circuit:
1. Drives the circuit out of the zero-current state during initialization.
2. Automatically deactivates once the circuit achieves its normal operating state, minimizing power consumption and interference.

## Results Achieved
The designed Bandgap Reference circuit demonstrates exceptional performance, achieving the following metrics:

- **Reference Voltage (V_ref):** 1.2V
- **Temperature Variation of V_ref:** 1.68mV across the specified temperature range (-40 to 125 degree celsius)
- **Power Supply Variation of V_ref:** 12mV across supply variations (1.8 V to 3.6 V)
- **Temperature Coefficient (TempCo):** 8.905 ppm/°C

These results highlight the circuit’s stability and reliability in maintaining a constant reference voltage under varying conditions.

## Repository Contents

This repository includes:

- **Schematics:** Detailed circuit diagrams for each implementation.
- **Simulation Results:** Graphs and data showcasing the stability of the reference voltage across temperature and power supply variations.
- **Documentation:** Step-by-step explanations of the design process and methodologies.

Feel free to explore the repository for insights into the design and implementation of Bandgap Reference circuits. Contributions and feedback are welcome!
