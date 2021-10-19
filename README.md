# VSDOpen-PLL-OSU180nm
## VSD Open PLL/OSU180nm Documentation 
## 1. Introduction

* IP Intelectual Properties 
  * Reusable unit of Logic/cell/Layout
  * IPs reduce design and testing time
  ![IP](https://user-images.githubusercontent.com/92792321/137954581-fbe2d987-e91c-4143-9075-d9bb57084b88.PNG)
* On Chip CLock Multiplier
  * achieves high frequency clock signal from low frequency as quartz can only few MHz
  * need of clock signals for processor type synchronous
* PLL - Phase Locked Loop
   * A phase-locked loop or phase lock loop (PLL) is a control system that generates an output signal whose phase is related to the phase of an input signal.
   ![PLL](https://user-images.githubusercontent.com/92792321/137954654-b1b1e3e3-b40b-4b92-80d6-c927aaaa8496.PNG) 
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

![IC fab](https://user-images.githubusercontent.com/92792321/137954785-4750bc2c-5bcf-4644-95cb-96fb8f4dca5d.PNG)


### 2.4 Eular Path

* An Euler path is a path that uses every edge of a graph exactly once
* Reduces parasitic resistance, and unnecessary wires 

## 3. Pre-Layout Implementation and Simulation
### 3.1 Desgin Flow
1. Tool Installation
2. Circuit Design
3. Netlist Extraction
4. Netlist Modification
5. Run

### 3.2 Phase Frequency Detector
* A phase detector or phase comparator is a frequency mixer, analog multiplier or logic circuit that generates a signal which represents the difference in phase between two signal inputs.
* Possible Outcomes 
  * Leading UP=1, DOWN=0
  * Lagging UP=0, DOWN=1
  * In Phase UP=DOWN

![PFD](https://user-images.githubusercontent.com/92792321/137955467-260847b0-f550-40df-a404-7d2cc817e6b3.PNG)

* Verification

![pfd](https://user-images.githubusercontent.com/92792321/137955523-703c3730-6568-4e68-a4cf-155c0020a08d.PNG)

### 3.3 Charge Pump
* A charge pump is a kind of DC-to-DC converter that uses capacitors for energetic charge storage to raise or lower voltage.
![charge pump](https://user-images.githubusercontent.com/92792321/137955688-015f67dc-d145-4905-8229-33c84406125a.PNG)
* Verification
  * Without Low-Pass Filter
![pfd_cp_without lowpass](https://user-images.githubusercontent.com/92792321/137955803-a6a28774-ba00-46ca-bf13-99986dc36bed.PNG)
  * With Low-Pass Filter
![pfd_cp_with lowpass](https://user-images.githubusercontent.com/92792321/137955877-49b9da39-dca9-45a6-b923-ca26a9f2a9a2.PNG)

### 3.4 VCO
* A voltage-controlled oscillator (VCO) is an electronic oscillator whose oscillation frequency is controlled by a voltage input
* Higher the voltage, Higher Frequency at output

![VCO](https://user-images.githubusercontent.com/92792321/137956224-a3629c89-77ab-4180-8940-1236e66e8e2b.PNG)

* Verification

![VCO](https://user-images.githubusercontent.com/92792321/137956281-46fcee77-bb2d-468c-af5b-687f7be88b7b.PNG)

### 3.5 Frequency Divider
* Simply a D flip-flop.
* Divides frequency by factor of 2
* Verification

![FrepD](https://user-images.githubusercontent.com/92792321/137956406-5b9d84fe-63e6-4ff8-b71f-9eb4acdc3e5e.PNG)

### 3.6 PLL Pre-Playout

![PLL](https://user-images.githubusercontent.com/92792321/137956601-d3bb9afb-96b7-4db9-9f44-4de76f31d4ab.PNG)
