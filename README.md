# Isolated Bidirectional Symmetrical Resonant Converter for 400V–800V Wireless EV Charging

An analytical modeling and simulation project investigating a 2 kW isolated, non-contact bidirectional symmetrical resonant converter tailored for next-generation 800V Electric Vehicle (EV) wireless charging architectures.

The converter integrates Dual Active Bridge (DAB) full bridges with a symmetrical resonant network to enable high-efficiency Grid-to-Vehicle (G2V) charging and Vehicle-to-Grid (V2G) power transfer.

---

## Architecture Overview

* **Power Stage:** Dual Active Bridge (Primary & Secondary Full Bridges) with loosely coupled inductive coils.
* **Resonant Tank:** Symmetrical resonant network operating at a nominal 100 kHz resonance point.
* **Grid Port (Primary):** Nominal 400 V DC bus.
* **Battery Port (Secondary):** 670 V to 800 V high-voltage EV battery system.
* **Modulation Technique:** Pulse Frequency Modulation (PFM) spanning 75 kHz to 130 kHz for wide-range output voltage regulation.
* **Digital Control Core:** TMS320F28335 DSP-based closed-loop CC-CV charging architecture.

---

## Operating Specifications

| Parameter | Value |
| :--- | :--- |
| Primary Voltage ($V_{Grid}$) | 400 V |
| Secondary Battery Voltage ($V_{Bat}$) | 670 V – 800 V |
| Rated Power Capacity ($P$) | 2 kW |
| Nominal Resonant Frequency ($f_{r}$) | 100 kHz |
| Switching Frequency Range ($f_{s}$) | 75 kHz – 130 kHz |
| Coil Turns Ratio ($N = N_p / N_s$) | 0.65 |
| Primary Resonant Tank ($L_{r1}, C_{r1}$) | 35.2 µH, 71 nF (designed) |
| Forward Mode Efficiency (G2V, 400V -> 800V) | 90.2% |
| Reverse Mode Efficiency (V2G, 800V -> 400V) | 91.4% |

---

## Analytical Modeling via Fundamental Harmonic Approximation (FHA)

Because high-frequency resonant tanks attenuate higher harmonics, the complex non-contact network is represented via an AC equivalent circuit model.

The equivalent battery load reflected to the primary side is:

$$R_{eq} = \frac{8 N^2}{\pi^2} \frac{V_{Bat}}{I_{Bat}} = \frac{8 N^2}{\pi^2} R_{Bat}$$

The forward voltage gain $M_{vr\_forward}(s)$ is defined across the resonant tank impedance branches:

$$M_{vr\_forward}(s) = \frac{v_{eq}(s)}{v_{in\_FHA}(s)} = \frac{Z_2(s)}{Z_1(s)} \cdot \frac{Z_4(s)}{Z_3(s)}$$

Where:
* $Z_1(s) = sL_{r1} + \frac{1}{sC_{r1}} + Z_2(s)$
* $Z_2(s) = sL_m \parallel \left( sL_{r2}' + \frac{1}{sC_{r2}'} + R_{eq} \right)$
* $Z_3(s) = sL_{r2}' + \frac{1}{sC_{r2}'} + R_{eq}$
* $Z_4(s) = R_{eq}$

---

## Soft-Switching & Control Strategy

* **Zero Voltage Switching (ZVS):** Operating in the inductive resonance region allows the primary switches to discharge their parasitic drain-source capacitances ($C_{oss}$) to zero during the dead-time interval prior to gate turn-on.
* **Zero Current Switching (ZCS):** By operating with switching periods larger than the resonant period, the resonant tank enters a decoupling interval where current drops to zero prior to commutation, cutting secondary-side conduction and switching losses.
* **Dual-Loop Regulation:** Proportional-Integral (PI) control drives the frequency generator (FM block) to toggle between Constant Current (CC) and Constant Voltage (CV) profiles based on sampled feedback from the battery port.
