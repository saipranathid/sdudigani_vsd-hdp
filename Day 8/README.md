<details>
  <Summary><strong> Day 8 : Circuit Design and Spice Simulation</strong></summary>

## SPICE Simulation
SPICE (Simulation Program with Integrated Circuit Emphasis) is a powerful simulation tool developed at UC Berkeley in the early 1970’s, used in electronics design to model and analyze the behavior of electronic circuits before they are physically built.

The input file is often called a ***SPICE deck*** and each line is called a ***card*** because it was once provided to a mainframe as a deck of punch cards.

A circuit simulator is provided with an input file that contains:

- A netlist consisting of components and nodes detailing the circuit connectivity.
- The netlist can be entered by hand or extracted from a circuit schematic or layout in a CAD program.
- Component behaviour by means of device models and model parameters.
- The Initial state of the circuit -- initial conditions
- Inputs to the circuit, called stimulus
- Simulation options & analysis commands that explain the type of simulation to be run.

