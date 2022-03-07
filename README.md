# In-Memory-DAC-Operations-Inside-8T-SRAM-Cells
## Contents
[Abstract](#abstract)<br>
[Keywords](#keywords)<br>
[Tools Used](#tools-used)<br>
1. [Introduction](#1-introduction)<br>
2. [8T SRAM Cell For In-Memory DAC](#2-8t-sram-cell-for-in-memory-dac)<br>
3. [Simulation Results](#3-simulation-results)<br>
   - 3.1. [Netlist](#31-netlist)<br>
   - 3.2. [Reference Waveforms](#32-reference-waveforms)<br>
   - 3.3. [Simulated Waveforms](#33-simulated-waveforms)<br>
4. [Acknowledgement](#acknowledgement)<br>
5. [References](#references)<br>
## Abstract
The von-Neumann computing methodology dominates most of the today's processor architecture. In such architecture, program and data are decoupled from each other leading to massive latency as well as energy consumption due to frequent shuttling back and forth of data between physically separate memory and computing cores also known as bottleneck. Digital to analog convertor (DAC) is one of the frequently used block inside any processing core which needs to interface analog and digital world. In this paper, we will design an 8T SRAM based in-memory DAC convertor to overcome the von-Neumann bottleneck.
<br>[🠉 Back to Top](#contents)
## Keywords
bottleneck, DAC, SRAM, von-Neumann
<br>[🠉 Back to Top](#contents)
## Tools Used
[eSim](https://esim.fossee.in/home) (previously known as Oscad / FreeEDA) is a free/libre and open source EDA tool for circuit design, simulation, analysis and PCB design. It is an integrated tool built using free/libre and open source software such as [KiCad](http://www.kicad-pcb.org/), [Ngspice](http://ngspice.sourceforge.net/), [Verilator](https://www.veripool.org/verilator/), [makerchip-app](https://pypi.org/project/makerchip-app/), [sandpiper-saas](https://pypi.org/project/sandpiper-saas/) and [GHDL](http://ghdl.free.fr/). eSim is released under GPL.
![eSim](https://user-images.githubusercontent.com/100511409/157074547-e9c855cf-ddaa-41e1-a44f-8071950f172b.jpg)<br>
Fig. 1. FOSSEE eSim software.
<br>[🠉 Back to Top](#contents)
## 1. Introduction
Most of the recent day processors works on von-Neumann architecture [1](#references). In von-Neumann architecture program and data units are spatially, separately stored at different locations on the chip, leads to significantly high delay and energy consumption due to frequent data transfer between physically separated memory and computing cores. This problem is further exacerbated for data intensive applications such as in AI/ML, DSP processors, etc. By enabling computations within memory, significant improvements, both in energy efficiency and throughput are expected [2](#references). Digital to analog convertor (DAC) is a key block to these applications to interface analog signal with digital processors/algorithms. So, in this work, we will design an in-memory DAC inside 8T SRAM cell.
<br>[🠉 Back to Top](#contents)
## 2. 8T SRAM Cell For In-Memory DAC
The 8T SRAM cell is shown in Fig. 2. It has same write operations as that of 6T SRAM cells, but for read operations different port RBL is used by activating RWL line. An 8T-SRAM, without modifying its basic circuit structure, can behave as a digital to analog converter (DAC), without affecting the bits stored in the SRAM cell. Consider an array of 4 cells connected as shown in Fig. 2. Under normal memory operations, the source terminal of M7 is grounded but for DAC operation SL (source line) of same row are all connected to _vin_. Thus, the current flowing through RBL is proportional to _vin_, and also to the conductance of transistor M1 and M2 in Fig. 2. When logic ‘1’ is stored in memory cell, conductance of M1 is very high but when logic ‘0’ is stored in memory cell, the conductance of M1 is almost negligible, i.e., it does not conducts at all. Another parameter on which current will depend are (W/L) ratio of mosfet M1 and M2 present in read port of the 8T-SRAM cell. It is well known that current through enhancement mosfet is directly proportional to its (W/L) ratio. Hence, properly sizing the (W/L) ratios of mosfet M1 and M2 in each column we can obtained our proposed digital to analog converter.<br>
For our proposed structure of 8T-SRAM array cells, current _I_ will be sum of currents through RBL of each cell. Now consider that the ratio of (W/L) of mosfet M1 and M2 used in column 1 through 4 is 8: 4: 2: 1 as shown in Fig. 1 (we will keep the sizes of mosfet M1 and M2 same in each column). Hence, their conductance will be in the same ratio, since conductance will be directly proportional to (W/L) ratio. This will generate output current _I_, which will be proportional to the analog equivalent of digital bits stored inside SRAM. _I_ = _K_(2<sup>3</sup>b<sub>3</sub>+2<sup>2</sup>b<sub>2</sub>+2<sup>1</sup>b<sub>1</sub>+2<sup>0</sup>b<sub>0</sub>), where _K_ is some constant.
![Schematic](https://user-images.githubusercontent.com/100511409/157077638-687b5181-f9a2-4a4a-a796-9710683ec4b7.jpg)<br>
Fig. 2. Schematic of in-memory DAC. The designing of 6TSRAM in makerchip is discussed in upcoming sections.
<br>[🠉 Back to Top](#contents)
## 3. Simulation Results
First, the 6T SRAM were designed and simulated using the verilog HDL on makerchip plugin integrated in eSim as shown in Fig. 3 and Fig. 4 below. The 6T SRAM design contains input data line-- din (data-in), control line-- wen (write-enable) and output line-- q (cell output) as can be observed from verilog file.
![eSim_MakerChip](https://user-images.githubusercontent.com/100511409/157078390-9bc3a44d-3d9d-4aad-866b-332e63c5d125.PNG)<br>
Fig. 3. Loading of top module verilog file in makerchip integrated in eSim.
![MakerChip](https://user-images.githubusercontent.com/100511409/157078492-f89c1abb-96ec-4936-8819-355aac9db590.PNG)<br>
Fig. 4. Logic verification of 6T SRAM cell in makerchip.
The detailed verilog file is discussed below:
```
module sram6T(din, wen, q);
input din, wen;
output reg q;
always @(din, wen) begin
    if(wen == 1) q = din;
end
endmodule
```
### 3.1. Netlist
### 3.2. Reference Waveforms
### 3.3. Simulated Waveforms
<br>[🠉 Back to Top](#contents)
## Acknowledgement
<br>[🠉 Back to Top](#contents)
## References
[1] 
[2]
<br>[🠉 Back to Top](#contents)
