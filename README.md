# VSDOpen-PLL-OSU180nm
## VSD Open PLL/OSU180nm Documentation 
## 1. Introduction

* IP Intelectual Properties 
  * Reusable unit of Logic/cell/Layout
  * IPs reduce design and testing time
* On Chip CLock Multiplier
  * achieves high frequency clock signal from low frequency as quartz can only few MHz
  * need of clock signals for processor type synchronous
* PLL - Phase Locked Loop
  * A phase-locked loop or phase lock loop (PLL) is a control system that generates an output signal whose phase is related to the phase of an input signal. 
* Design Specification - Certain Design Requirements 
* Architecture Specifiction

## 2. Theory and Fundamental Concepts  
### 2.1 CMOS implementation Transistor Sizing

* CMOS is implemented with NMOS and PMOS.
* Transistor Sizing *the operation of enlarging or reducing the channel width of transistors*
  * Reduses Leakage current, Saves Power
  * *Example* - 3 input NAND gate PMOS >>> 2:1, NMOS >>> 2:1

### 2.2 Control System and Feedback Loop

* Working of PLL - A forward input is given, with is then again looped with the input. The Loop goes on until the desired value is obtained.
* This a second order control system.
* Damping Types - Overdamped, Underdamped, Undamped, **Critically damped for Ideal PLL**

### 2.3 IC Fabrication Process

* Resistance of Wire - Length and Area Dependancy
* Capacitance - Metal lines have some wide and they kind of act as capacitor. 

### 2.4 Eular Path

* An Euler path is a path that uses every edge of a graph exactly once
* Reduces parasitic resistance, and unnecessary wires 

   


