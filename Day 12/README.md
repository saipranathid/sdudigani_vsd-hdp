<details>
  <Summary><strong> Day 12 : CMOS Power Supply and device variation robustness evaluation</strong></summary>

# Contents
- [Static Behavior Evaluation-CMOS Inverter-Power Supply Variation](#static-behavior-evaluation--cmos-inverter--power-supply-variation)
  - [Smart SPICE simulation for power supply variations](#smart-spice-simulation-for-power-supply-variations)
  - [Advantages and Disadvantages using low supply voltage](#advantages-and-disadvantages-using-low-supply-voltage)
  - [Sky130 supply variation Labs](#sky130-supply-variation-labs)   
- [Static Behavior Evaluation-CMOS Inverter-Device Variation](#static-behavior-evaluation--cmos-inverter--device-variation)
  - [Sources of Variation-Etching Process](#sources-of-variation--etching-process)
  - [Sources of Variation-Oxide Thickness](#sources-of-variation--oxide-thickness)
  - [Smart SPICE simulation for device variations](#smart-spice-simulation-for-device-variations)
  - [Conclusion](#conclusion)
  - [Sky130 device variation Labs](#sky130-device-variation-labs)   
    

<a id="static-behavior-evaluation--cmos-inverter--power-supply-variation"></a>
# Static Behavior Evaluation-CMOS Inverter-Power Supply Variation
Power supply scaling directly affects the static behavior of a CMOS inverter — changing its switching threshold (Vm), noise margins, and overall robustness.
<a id="smart-spice-simulation-for-power-supply-variations"></a>
## Smart SPICE simulation for power supply variations
**SPICE Simulation**
![Alt Text](images/ps_1.png)
- The CMOS inverter is simulated at two different supply voltages: Vdd = 2.5V → scaled down to Vdd = 1V
- PMOS and NMOS sizes remain constant: Wp = 0.9375 μm, Wn = 0.375 μm

<a id="advantages-and-disadvantages-using-low-supply-voltage"></a>
## Advantages and Disadvantages using low supply voltage


<a id="sky130-supply-variation-labs"></a>
## Sky130 supply variation Labs



<a id="static-behavior-evaluation--cmos-inverter--device-variation"></a>
# Static Behavior Evaluation-CMOS Inverter-Device Variation

<a id="sources-of-variation--etching-process"></a>
## Sources of Variation-Etching Process

<a id="sources-of-variation--oxide-thickness"></a>
## Sources of Variation-Oxide Thickness

<a id="smart-spice-simulation-for-device-variations"></a>
## Smart SPICE simulation for device variations

<a id="conclusion"></a>  
## Conclusion

<a id="sky130-device-variation-labs"></a>  
## Sky130 device variation Labs
