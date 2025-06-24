<details>
  <Summary><strong> Day 11 : CMOS Noise Margin Robustness Evaluation</strong></summary>

# Contents
- [Static Behavior Evaluation - CMOS Inverter Robustness - Noise Margin](#static-behavior-evaluation--cmos-inverter-robustness--noise-margin)
  - [Introduction to Noise Margin](#introduction-to-noise-margin)
  - [Noise Margin Volatge Parameters](#noise-margin-voltage-parameters)
  - [Noise Margin Equation and Summary](#noise-margin-equation-and-summary)
  - [Noise Marhin Variation with respect to PMOS width](#noise-margin-variation-with-respect-to-pmos-width)
  - [Sky130 Noise margin Labs](#sky130-noise-margin-labs)   
  

<a id="static-behavior-evaluation--cmos-inverter-robustness--noise-margin"></a>
# Static Behavior Evaluation - CMOS Inverter Robustness - Noise Margin

<a id="introduction-to-noise-margin"></a>
## Introduction to Noise Margin
**Noise margin** is the maximum noise voltage a CMOS circuit can tolerate without logic errors.
- i.e Noise margin is the amount of noise that a CMOS circuit could withstand without compromising the operation of circuit.
- Noise margin makes sure that:
  - any signal which is logic 1 with finite noise added to it, is still recognized as logic 1 and not logic 0.
  - similarly, any signal which is logic 0 with finite noise added to it, is still recognized as logic 0 and not logic 1.

The following images show an ideal and a piece-wise linear VTC of a CMOS inverter:
![Alt Text](images/nm_1.png)
![Alt Text](images/nm_2.png)

<a id="noise-margin-voltage-parameters"></a>
## Noise Margin Volatge Parameters

<a id="noise-margin-equation-and-summary"></a>
## Noise Margin Equation and Summary

<a id="noise-margin-variation-with-respect-to-pmos-width"></a>
## Noise Marhin Variation with respect to PMOS width

<a id="sky130-noise-margin-labs"></a>
## Sky130 Noise margin Labs
