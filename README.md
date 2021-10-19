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
* Code
```
****************************
*PFD
***************************

.include osu018.lib


*______________________________________________
XX1 N001 N005 N002 VDD 0 nand101
XX2 N002 N008 N006 VDD 0 nand101
XX3 N006 N007 N008 VDD 0 nand101
XX4 N007 N010 N011 VDD 0 nand101
XX5 N011 N009 N010 VDD 0 nand101
XX6 N013 N012 N009 VDD 0 nand101
XX7 f_clk_in N005 VDD 0 inv101
XX8 f_VCO N013 VDD 0 inv101
XX9 N002 N003 VDD 0 inv101
XX10 N003 N004 VDD 0 inv101
XX11 N009 N014 VDD 0 inv101
XX12 N014 N015 VDD 0 inv101
XX13 N004 N006 N007 N001 VDD 0 nand301
XX14 N007 N010 N015 N012 VDD 0 nand301
XX15 N012 down VDD 0 inv101
XX16 N006 N002 N009 N010 VDD 0 N007 nand401
XX17 N001 up VDD 0 inv101



*______________________________________________
* _______SUBCIRCUITS_________________
*______________________________________________

.subckt nand101 in1 in2 out VDD GND
M1 out in1 N001 N001 nfet l=180n w=180n
M2 VDD in2 out VDD pfet l=180n w=360n
M3 VDD in1 out VDD pfet l=180n w=360n
M4 N001 in2 GND GND nfet l=180n w=360n
.ends nand101

.subckt inv101 in out VDD GND
M1 out in GND GND nfet l=180n w=180n
M2 VDD in out VDD pfet l=180n w=360n
.ends inv101

.subckt nand301 in1 in2 in3 out VDD GND
M1 out in1 N001 N001 nfet l=180n w=540n
M2 VDD in2 out VDD pfet l=180n w=360n
M3 VDD in1 out VDD pfet l=180n w=360n
M4 N001 in2 N002 N002 nfet l=180n w=540n
M5 N002 in3 GND GND nfet l=180n w=540n
M6 VDD in3 out VDD pfet l=180n w=360n
.ends nand301

.subckt nand401 in1 in2 in3 in4 VDD GND out
M1 out in1 N001 N001 nfet l=180n w=720n
M2 VDD in2 out VDD pfet l=180n w=360n
M3 VDD in1 out VDD pfet l=180n w=360n
M4 N001 in2 N002 N002 nfet l=180n w=720n
M5 N002 in3 N003 N003 nfet l=180n w=720n
M6 VDD in3 out VDD pfet l=180n w=360n
M7 N003 in4 GND GND nfet l=180n w=720n
M8 VDD in4 out VDD pfet l=180n w=360n
.ends nand401
*______________________________________________
* _______SUBCIRCUITS_ENDS________________
*______________________________________________


V1 f_clk_in 0 pulse 0 1.8 0 100p 100p 5n 10n
V2 f_VCO 0 pulse 0 1.8 2n 100p 100p 5n 9n
V3 VDD 0 1.8


.control
tran .1ns 50n
plot V(f_clk_in)+6 V(f_VCO)+4 V(up)+2 V(down)
.endc


.end
```
![PFD](https://user-images.githubusercontent.com/92792321/137955467-260847b0-f550-40df-a404-7d2cc817e6b3.PNG)

* Verification

![pfd](https://user-images.githubusercontent.com/92792321/137955523-703c3730-6568-4e68-a4cf-155c0020a08d.PNG)

### 3.3 Charge Pump
* A charge pump is a kind of DC-to-DC converter that uses capacitors for energetic charge storage to raise or lower voltage.
![charge pump](https://user-images.githubusercontent.com/92792321/137955688-015f67dc-d145-4905-8229-33c84406125a.PNG)
* Code
```
****************************
*PFD
***************************

.include osu018.lib


*______________________________________________
XX1 N001 N005 N002 VDD 0 nand101
XX2 N002 N008 N006 VDD 0 nand101
XX3 N006 N007 N008 VDD 0 nand101
XX4 N007 N010 N011 VDD 0 nand101
XX5 N011 N009 N010 VDD 0 nand101
XX6 N013 N012 N009 VDD 0 nand101
XX7 f_clk_in N005 VDD 0 inv101
XX8 f_VCO N013 VDD 0 inv101
XX9 N002 N003 VDD 0 inv101
XX10 N003 N004 VDD 0 inv101
XX11 N009 N014 VDD 0 inv101
XX12 N014 N015 VDD 0 inv101
XX13 N004 N006 N007 N001 VDD 0 nand301
XX14 N007 N010 N015 N012 VDD 0 nand301
XX15 N012 down VDD 0 inv101
XX16 N006 N002 N009 N010 VDD 0 N007 nand401
XX17 N001 up VDD 0 inv101



*______________________________________________
* _______SUBCIRCUITS_________________
*______________________________________________

.subckt nand101 in1 in2 out VDD GND
M1 out in1 N001 N001 nfet l=180n w=180n
M2 VDD in2 out VDD pfet l=180n w=360n
M3 VDD in1 out VDD pfet l=180n w=360n
M4 N001 in2 GND GND nfet l=180n w=360n
.ends nand101

.subckt inv101 in out VDD GND
M1 out in GND GND nfet l=180n w=180n
M2 VDD in out VDD pfet l=180n w=360n
.ends inv101

.subckt nand301 in1 in2 in3 out VDD GND
M1 out in1 N001 N001 nfet l=180n w=540n
M2 VDD in2 out VDD pfet l=180n w=360n
M3 VDD in1 out VDD pfet l=180n w=360n
M4 N001 in2 N002 N002 nfet l=180n w=540n
M5 N002 in3 GND GND nfet l=180n w=540n
M6 VDD in3 out VDD pfet l=180n w=360n
.ends nand301

.subckt nand401 in1 in2 in3 in4 VDD GND out
M1 out in1 N001 N001 nfet l=180n w=720n
M2 VDD in2 out VDD pfet l=180n w=360n
M3 VDD in1 out VDD pfet l=180n w=360n
M4 N001 in2 N002 N002 nfet l=180n w=720n
M5 N002 in3 N003 N003 nfet l=180n w=720n
M6 VDD in3 out VDD pfet l=180n w=360n
M7 N003 in4 GND GND nfet l=180n w=720n
M8 VDD in4 out VDD pfet l=180n w=360n
.ends nand401
*______________________________________________
* _______SUBCIRCUITS_ENDS________________
*______________________________________________


V1 f_clk_in 0 pulse 0 1.8 0 100p 100p 5n 10n
V2 f_VCO 0 pulse 0 1.8 2n 100p 100p 5n 9n
V3 VDD 0 1.8


.control
tran .1ns 50n
plot V(f_clk_in)+6 V(f_VCO)+4 V(up)+2 V(down)
.endc


.end*PFD_Charge

.include osu018.lib

XX1 f_in f_out_8 up down pfd_501
XX2 up down vin_vco charge_pump


*___________LPF________________*
C1 Vin_vco 0 200p
C2 N001 0 500p
R1 Vin_vco N001 500
*______________________________*


* block symbol definitions
.subckt pfd_501 f_clk_in f_VCO up down
XX1 N001 N005 N002 vddd 0 nand101
XX2 N002 N008 N006 vddd 0 nand101
XX3 N006 N007 N008 vddd 0 nand101
XX4 N007 N010 N011 vddd 0 nand101
XX5 N011 N009 N010 vddd 0 nand101
XX6 N013 N012 N009 vddd 0 nand101
XX7 f_clk_in N005 vddd 0 inv101
XX8 f_VCO N013 vddd 0 inv101
XX9 N002 N003 vddd 0 inv101
XX10 N003 N004 vddd 0 inv101
XX11 N009 N014 vddd 0 inv101
XX12 N014 N015 vddd 0 inv101
XX13 N004 N006 N007 N001 vddd 0 nand301
XX14 N007 N010 N015 N012 vddd 0 nand301
XX15 N012 down vddd 0 inv101
XX16 N006 N002 N009 N010 vddd 0 N007 nand401
XX17 N001 up vddd 0 inv101
V1 vddd 0 1.8
.ends pfd_501

.subckt charge_pump UP Down CP
M7 UP_bar UP 0 0 nfet l=180n w=180n
M8 DOWN_bar Down 0 0 nfet l=180n w=180n
M12 VDD Down DOWN_bar VDD pfet l=180n w=540n
M14 VDD UP UP_bar VDD pfet l=180n w=540n
M15 UP_bar 0 N001 N001 nfet l=180n w=180n
M16 DOWN_bar 0 N004 N004 nfet l=180n w=180n
M17 N003 N004 N003 N003 nfet l=180n w=180n
M18 0 VDD VDD 0 nfet l=180n w=180n
M19 CP VDD N003 N003 nfet l=180n w=180n
M20 N003 Down 0 0 nfet l=180n w=15u
M21 N001 VDD UP_bar N001 pfet l=180n w=540n
M22 N004 VDD DOWN_bar N004 pfet l=180n w=540n
M23 N002 UP N002 N002 pfet l=180n w=540n
M24 0 0 VDD VDD pfet l=180n w=540n
M25 N002 0 CP N002 pfet l=180n w=540n
M26 VDD N001 N002 VDD pfet l=180n w=45u
V4 VDD 0 1.8
.ends charge_pump

.subckt nand101 in1 in2 out VDD GND
M1 out in1 N001 N001 nfet l=180n w=180n
M2 VDD in2 out VDD pfet l=180n w=360n
M3 VDD in1 out VDD pfet l=180n w=360n
M4 N001 in2 GND GND nfet l=180n w=360n
*V1 VDD 0 1.8
.ends nand101

.subckt inv101 in out VDD GND
M1 out in GND GND nfet l=180n w=180n
M2 VDD in out VDD pfet l=180n w=360n
*V1 VDD 0 1.8
.ends inv101

.subckt nand301 in1 in2 in3 out VDD GND
M1 out in1 N001 N001 nfet l=180n w=540n
M2 VDD in2 out VDD pfet l=180n w=360n
M3 VDD in1 out VDD pfet l=180n w=360n
M4 N001 in2 N002 N002 nfet l=180n w=540n
M5 N002 in3 GND GND nfet l=180n w=540n
M6 VDD in3 out VDD pfet l=180n w=360n
*V1 VDD 0 1.8
.ends nand301

.subckt nand401 in1 in2 in3 in4 VDD GND out
M1 out in1 N001 N001 nfet l=180n w=720n
M2 VDD in2 out VDD pfet l=180n w=360n
M3 VDD in1 out VDD pfet l=180n w=360n
M4 N001 in2 N002 N002 nfet l=180n w=720n
M5 N002 in3 N003 N003 nfet l=180n w=720n
M6 VDD in3 out VDD pfet l=180n w=360n
M7 N003 in4 GND GND nfet l=180n w=720n
M8 VDD in4 out VDD pfet l=180n w=360n
*V1 VDD 0 1.8
.ends nand401

V1 f_in 0 PULSE 0 1.8 10p 60p 60p 100n 200n
V2 f_out_8 0 PULSE 0 1.8 40n 60p 60p 100n 180n
V3 VDD 0 1.8


.control
tran .1n 4u
plot V(f_in)+8 V(f_out_8)+6 V(up)+4 V(down)+2 V(vin_vco)
.endc


.end
```
* Verification
  * Without Low-Pass Filter
![pfd_cp_without lowpass](https://user-images.githubusercontent.com/92792321/137955803-a6a28774-ba00-46ca-bf13-99986dc36bed.PNG)
  * With Low-Pass Filter
![pfd_cp_with lowpass](https://user-images.githubusercontent.com/92792321/137955877-49b9da39-dca9-45a6-b923-ca26a9f2a9a2.PNG)

### 3.4 VCO
* A voltage-controlled oscillator (VCO) is an electronic oscillator whose oscillation frequency is controlled by a voltage input
* Higher the voltage, Higher Frequency at output

![VCO](https://user-images.githubusercontent.com/92792321/137956224-a3629c89-77ab-4180-8940-1236e66e8e2b.PNG)
* Code

```
****************************
*VCO
***************************

.include osu018.lib

*______________________________________________
M7 Vp Vn 0 0 nfet l=180n w=360n
M8 Vp Vp VDD VDD pfet l=180n w=1800n
R1 Vn Vinvco 1
XU22 osc_fb Osc inv_20_10
XX3 n1 osc_fb Vp Vn cs_inv
XX16 N005 n1 Vp Vn cs_inv
XX17 N004 N005 Vp Vn cs_inv
XX18 N003 N004 Vp Vn cs_inv
XX19 N002 N003 Vp Vn cs_inv
XX20 N001 N002 Vp Vn cs_inv
XX21 osc_fb N001 Vp Vn cs_inv
*______________________________________________

*______________________________________________
* _______SUBCIRCUITS_________________
*______________________________________________

.subckt inv_20_10 In Out
M1 Out In 0 0 nfet l=180n w=360n
M2 Out In N001 N001 pfet l=180n w=720n
V1 N001 0 1.8
.ends inv_20_10

.subckt cs_inv In Out Vp Vn
M1 N003 Vn 0 0 nfet l=180n w=360n
M4 N002 Vp N001 VDD pfet l=180n w=720n
M3 Out In N002 VDD pfet l=180n w=720n
M2 Out In N003 0 nfet l=180n w=360n
*C1 Out 0 1pf
V1 N001 0 1.8
.ends cs_inv

*______________________________________________
* _______SUBCIRCUITS_ENDS________________
*______________________________________________


V2 Vinvco 0 .5
V3 VDD 0 1.8

.control
tran .1ns 1u
plot V(Vinvco)+2 V(osc_fb)
.endc


.end

```
* Verification

![VCO](https://user-images.githubusercontent.com/92792321/137956281-46fcee77-bb2d-468c-af5b-687f7be88b7b.PNG)

### 3.5 Frequency Divider
* Simply a D flip-flop.
* Divides frequency by factor of 2
* Code

```
****************************
*freq_div_2
***************************


.include osu018.lib

M1 N004 N002 0 0 nfet l=180n w=180n
M2 VDD N002 N004 VDD pfet l=180n w=540n
M5 N003 N004 0 0 nfet l=180n w=180n
M6 VDD N004 N003 VDD pfet l=180n w=540n
M7 D clock_b N002 0 nfet l=180n w=180n
M3 N002 clock D VDD pfet l=180n w=540n
M4 N002 clock N003 0 nfet l=180n w=180n
M8 N003 clock_b N002 VDD pfet l=180n w=540n
M9 q N001 0 0 nfet l=180n w=180n
M10 VDD N001 q VDD pfet l=180n w=540n
M11 D q 0 0 nfet l=180n w=180n
M12 VDD q D VDD pfet l=180n w=540n
M13 N004 clock N001 0 nfet l=180n w=180n
M14 N001 clock_b N004 VDD pfet l=180n w=540n
M15 N001 clock_b D 0 nfet l=180n w=180n
M16 D clock N001 VDD pfet l=180n w=540n
M17 clock_b clock 0 0 nfet l=180n w=180n
M18 VDD clock clock_b VDD pfet l=180n w=540n



VDD VDD 0 1.8
V1 clock 0 PULSE 0 1.8 10p 50p 50p 100n 200n


.control
tran .1ns 1u
plot V(clock)+2 V(q)
.endc


.end

```
* Verification

![FrepD](https://user-images.githubusercontent.com/92792321/137956406-5b9d84fe-63e6-4ff8-b71f-9eb4acdc3e5e.PNG)

### 3.6 PLL Pre-Playout
#### Final Code
```
*__________________PLL-combined in 1 file_________________________*


XX1 f_in f_VCO/8 up down pfd_501
XX2 up down Vin_vco charge_pump
XX3 Vin_vco f_out vco_19_16
XX5 f_out N003 freq_div_2
XX6 N003 N002 freq_div_2
XX7 N002 f_VCO/8 freq_div_2
V1 f_in 0 PULSE 0 1.8 10p 60p 60p 100n 200n

.include osu018.lib

*V2 f_in 0 PULSE 0 1.8 10p 60p 60p 50n 100n
*V3 f_in 0 PULSE 0 1.8 10p 60p 60p 41.665n 83.33n

*___________LPF________________*
C1 Vin_vco 0 200p
C2 N001 0 500p
R1 Vin_vco N001 500
*______________________________*

*__________________Subcircuits_____________________*

******************PFD*******************************
.subckt pfd_501 f_clk_in f_VCO up down
XX1 N001 N005 N002 vddd 0 nand101
XX2 N002 N008 N006 vddd 0 nand101
XX3 N006 N007 N008 vddd 0 nand101
XX4 N007 N010 N011 vddd 0 nand101
XX5 N011 N009 N010 vddd 0 nand101
XX6 N013 N012 N009 vddd 0 nand101
XX7 f_clk_in N005 vddd 0 inv101
XX8 f_VCO N013 vddd 0 inv101
XX9 N002 N003 vddd 0 inv101
XX10 N003 N004 vddd 0 inv101
XX11 N009 N014 vddd 0 inv101
XX12 N014 N015 vddd 0 inv101
XX13 N004 N006 N007 N001 vddd 0 nand301
XX14 N007 N010 N015 N012 vddd 0 nand301
XX15 N012 down vddd 0 inv101
XX16 N006 N002 N009 N010 vddd 0 N007 nand401
XX17 N001 up vddd 0 inv101
V3 vddd 0 1.8
.ends pfd_501

*****************charge_pump*******************************
.subckt charge_pump UP Down CP
M7 UP_bar UP 0 0 nfet l=180n w=180n
M8 DOWN_bar Down 0 0 nfet l=180n w=180n
M12 VDD Down DOWN_bar VDD pfet l=180n w=540n
M14 VDD UP UP_bar VDD pfet l=180n w=540n
M15 UP_bar 0 N001 N001 nfet l=180n w=180n
M16 DOWN_bar 0 N004 N004 nfet l=180n w=180n
M17 N003 N004 N003 N003 nfet l=180n w=180n
M18 0 VDD VDD 0 nfet l=180n w=180n
M19 CP VDD N003 N003 nfet l=180n w=180n
M20 N003 Down 0 0 nfet l=180n w=15u
M21 N001 VDD UP_bar N001 pfet l=180n w=540n
M22 N004 VDD DOWN_bar N004 pfet l=180n w=540n
M23 N002 UP N002 N002 pfet l=180n w=540n
M24 0 0 VDD VDD pfet l=180n w=540n
M25 N002 0 CP N002 pfet l=180n w=540n
M26 VDD N001 N002 VDD pfet l=180n w=45u
V1 VDD 0 1.8
.ends charge_pump

******************VCO*******************************
.subckt vco_19_16 Vinvco osc
M7 Vp Vn 0 0 nfet l=180n w=360n
M8 Vp Vp VDD VDD pfet l=180n w=1800n
R1 Vn Vinvco 1
XU22 osc_fb Osc inv_20_10
XX3 n1 osc_fb Vp Vn cs_inv
XX16 N005 n1 Vp Vn cs_inv
XX17 N004 N005 Vp Vn cs_inv
XX18 N003 N004 Vp Vn cs_inv
XX19 N002 N003 Vp Vn cs_inv
XX20 N001 N002 Vp Vn cs_inv
XX21 osc_fb N001 Vp Vn cs_inv
V1 VDD 0 1.8
.ends vco_19_16

******************Frequency divider*******************************
.subckt freq_div_2 clock Q
M1 N004 N002 0 0 nfet l=180n w=180n
VDD VDD 0 1.8
M2 VDD N002 N004 VDD pfet l=180n w=540n
M5 N003 N004 0 0 nfet l=180n w=180n
M6 VDD N004 N003 VDD pfet l=180n w=540n
M7 D clock_b N002 0 nfet l=180n w=180n
M3 N002 clock D VDD pfet l=180n w=540n
M4 N002 clock N003 0 nfet l=180n w=180n
M8 N003 clock_b N002 VDD pfet l=180n w=540n
M9 Q N001 0 0 nfet l=180n w=180n
M10 VDD N001 Q VDD pfet l=180n w=540n
M11 D Q 0 0 nfet l=180n w=180n
M12 VDD Q D VDD pfet l=180n w=540n
M13 N004 clock N001 0 nfet l=180n w=180n
M14 N001 clock_b N004 VDD pfet l=180n w=540n
M15 N001 clock_b D 0 nfet l=180n w=180n
M16 D clock N001 VDD pfet l=180n w=540n
M17 clock_b clock 0 0 nfet l=180n w=180n
M18 VDD clock clock_b VDD pfet l=180n w=540n
.ends freq_div_2

.subckt nand101 in1 in2 out VDD GND
M1 out in1 N001 N001 nfet l=180n w=180n
M2 VDD in2 out VDD pfet l=180n w=360n
M3 VDD in1 out VDD pfet l=180n w=360n
M4 N001 in2 GND GND nfet l=180n w=360n
.ends nand101

.subckt inv101 in out VDD GND
M1 out in GND GND nfet l=180n w=180n
M2 VDD in out VDD pfet l=180n w=360n
.ends inv101

.subckt nand301 in1 in2 in3 out VDD GND
M1 out in1 N001 N001 nfet l=180n w=540n
M2 VDD in2 out VDD pfet l=180n w=360n
M3 VDD in1 out VDD pfet l=180n w=360n
M4 N001 in2 N002 N002 nfet l=180n w=540n
M5 N002 in3 GND GND nfet l=180n w=540n
M6 VDD in3 out VDD pfet l=180n w=360n
.ends nand301

.subckt nand401 in1 in2 in3 in4 VDD GND out
M1 out in1 N001 N001 nfet l=180n w=720n
M2 VDD in2 out VDD pfet l=180n w=360n
M3 VDD in1 out VDD pfet l=180n w=360n
M4 N001 in2 N002 N002 nfet l=180n w=720n
M5 N002 in3 N003 N003 nfet l=180n w=720n
M6 VDD in3 out VDD pfet l=180n w=360n
M7 N003 in4 GND GND nfet l=180n w=720n
M8 VDD in4 out VDD pfet l=180n w=360n
.ends nand401

.subckt inv_20_10 In Out
M1 Out In 0 0 nfet l=180n w=360n
M2 Out In N001 N001 pfet l=180n w=720n
V1 N001 0 1.8
.ends inv_20_10

.subckt cs_inv In Out Vp Vn
M1 N003 Vn 0 0 nfet l=180n w=360n
M4 N002 Vp N001 VDD pfet l=180n w=720n
M3 Out In N002 VDD pfet l=180n w=720n
M2 Out In N003 0 nfet l=180n w=360n
C1 Out 0 1f
V1 N001 0 1.8
.ends cs_inv


************simulation commands**************

.tran .1ns 6u
.ic V(vin_vco) = 0
.control
run
plot V(f_in)+8 V(up)+6 V(down)+4 V(Vin_vco)+2 V(f_out)

*plot V(N002)
*plot V(Vin_vco)
*plot V(f_out)
.endc
.end

```
![PLL](https://user-images.githubusercontent.com/92792321/137956601-d3bb9afb-96b7-4db9-9f44-4de76f31d4ab.PNG)
