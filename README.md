# VLSI PHYSICAL DESIGN

## COURSE DETAILS

<details>

<summary><b> DAY 1 - Inception of open-source EDA, OpenLANE and Sky130 PDK </b></summary>

+ How to talk to computers
  - Introduction to QFN-48 Package, chip, pads, core, die and IPs
  - Introduction to RISC-V
  - From Software Applications to Hardware
+ SoC design and OpenLANE
  - Introduction to all components of open-source digital asic design
  - Simplified RTL2GDS flow
  - Introduction to OpenLANE and Strive chipsets
  - Introduction to OpenLANE detailed ASIC design flow
+ Get familiar to open-source EDA tools
  - OpenLANE Directory structure in detail
  - Design Preparation Step
  - Review files after design prep and run synthesis
  - OpenLANE Project Git Link Description
  - Steps to characterize synthesis results
  
</details>

<details>

<summary><b> DAY 2 - Good floorplan vs bad floorplan and introduction to library cells </b></summary>

+ Chip Floor planning considerations
  - Utilization factor and aspect ratio
  - Concept of pre-placed cells
  - De-coupling capacitors
  - Power planning
  - Pin placement and logical cell placement blockage
  - Steps to run floorplan using OpenLANE
  - Review floorplan files and steps to view floorplan
  - Review floorplan layout in Magic
+ Library Binding and Placement
  - Netlist binding and initial place design
  - Optimize placement using estimated wire-length and capacitance
  - Final placement optimization
  - Need for libraries and characterization
  - Congestion aware placement using RePlAce
+ Cell design and characterization flows
  - Inputs for cell design flow
  - Circuit design step
  - Layout design step
  - Typical characterization flow
+ General timing characterization parameters
  - Timing threshold definitions
  - Propagation delay and transition time
  
</details>

<details>

<summary><b> DAY 3 - Design library cell using Magic Layout and ngspice characterization </b></summary>

+ Labs for CMOS inverter ngspice simulations
  - IO placer revision
  - SPICE deck creation for CMOS inverter
  - SPICE simulation lab for CMOS inverter
  - Switching Threshold Vm
  - Static and dynamic simulation of CMOS inverter
  - Lab steps to git clone vsdstdcelldesign
+ Inception of Layout and CMOS fabrication process
  - Create Active regions
  - Formation of N-well and P-well
  - Formation of gate terminal
  - Lightly doped drain (LDD) formation
  - Source and drain formation
  - Local interconnect formation
  - Higher level metal formation
  - Lab introduction to Sky130 basic layers layout and LEF using inverter
  - Lab steps to create std cell layout and extract spice netlist
+ Sky130 Tech File Labs
  - Lab steps to create final SPICE deck using Sky130 tech
  - Lab steps to characterize inverter using sky130 model files
  - Lab introduction to Magic tool options and DRC rules
  - Lab introduction to Sky130 pdk's and steps to download labs
  - Lab introduction to Magic and steps to load Sky130 tech-rules
  - Lab exercise to fix poly.9 error in Sky130 tech-file
  - Lab exercise to implement poly resistor spacing to diff and tap
  - Lab challenge exercise to describe DRC error as geometrical construct
  - Lab challenge to find missing or incorrect rules and fix them

</details>

<details>

<summary><b> DAY 4 - Pre-layout timing analysis and importance of good clock tree </b></summary>

+ Timing modelling using delay tables
  - Lab steps to convert grid info to track info
  - Lab steps to convert magic layout to std cell LEF
  - Introduction to timing libs and steps to include new cell in synthesis
  - Introduction to delay tables
  - Delay table usage Part 1
  - Delay table usage Part 2
  - Lab steps to configure synthesis settings to fix slack and include vsdinv
+ Timing analysis with ideal clocks using openSTA
  - Setup timing analysis and introduction to flip-flop setup time
  - Introduction to clock jitter and uncertainty
  - Lab steps to configure OpenSTA for post-synth timing analysis
  - Lab steps to optimize synthesis to reduce setup violations
  - Lab steps to do basic timing ECO
+ Clock tree synthesis TritonCTS and signal integrity
  - Clock tree routing and buffering using H-Tree algorithm
  - Crosstalk and clock net shielding
  - Lab steps to run CTS using TritonCTS
  - Lab steps to verify CTS runs
+ Timing analysis with real clocks using openSTA
  - Setup timing analysis using real clocks
  - Hold timing analysis using real clocks
  - Lab steps to analyze timing with real clocks using OpenSTA
  - Lab steps to execute OpenSTA with right timing libraries and CTS assignment
  - Lab steps to observe impact of bigger CTS buffers on setup and hold timing
  
</details>

<details>

<summary><b> DAY 5 - Final steps for RTL2GDS using tritonRoute and openSTA </b></summary>

+ Routing and design rule check (DRC)
  - Introduction to Maze Routing and Lee's algorithm
  - Lee's Algorithm conclusion
  - Design Rule Check
+ Power Distribution Network and routing
  - Lab steps to build power distribution network
  - Lab steps from power straps to std cell power
  - Basics of global and detail routing and configure TritonRoute
+ TritonRoute Features
  - TritonRoute feature 1 - Honors pre-processed route guides
  - TritonRoute Feature2 & 3 - Inter-guide connectivity and intra- & inter-layer routing
  - TritonRoute method to handle connectivity
  - Routing topology algorithm and final files list post-route
  
</details>

## COURSE WORK

<details>

<summary><b> DAY 1 </b></summary>

## How to talk to computers
Any board is the processor/SoC with interfaces. Packages have predefined pins and these are connected to the chip using wires. Packages have components like pads (signals going inside the chip or out of the chip go through the pads), core (where all the digital logic sits),die, foundry IPs (like SRAM, ADC, DAC, PLL) and macros (like the SoC and SPI)

## SoC design and OpenLANE
For digital ASIC design we require RTL IPs, EDA tools and PDK data. PDK is the interface between the FAB and the designers. It contains process design rules, device models, digital standard cell libraries, IO libraries and much more. To go from RTL to GDS we need to follow the following steps – synthesis, floor/power planning, placement, clock tree synthesis, routing and sign off.

Synthesis – converts RTL into a circuit of components from the standard cell library.

Floor and power planning – partition the chip die between different system building blocks and place the IO pads or define the dimensions, pin locations and routing tracks.

Placement - place the cells on the floorplan rows. We have global and detailed placement.

Clock tree synthesis – create clock distribution network with minimum clock skew.

Routing – Implement interconnects using the available metal layers. We have global and detailed routing.

Sign off – DRC, LVS and STA

OpenLane can be used to harden macros and chips. RTL synthesis is done by yosys and abc. Yosys converts to netlist and abc maps it to a technology library. OpenROAD is used to perform place and route. 
Shown below is the OpenLane design flow

![image](https://github.com/ks-vandana/pes_pd/blob/main/DAY%201/OpenLane_design_flow.png)

## Get familiar to open-source EDA tools 
We are going to see the results of synthesis on OpenLane for a predefined design file **picorv32a**.

After installation is successful run the following commands
```
cd OpenLane
sudo make mount
./flow.tcl -interactive
```
After the previous command we enter in the interactive mode of OpenLane which lets us execute the design flow step by step. We will start with synthesis and check the reports.
```
package require openlane 0.9
prep -design <file_name>
run_synthesis
```

![image](https://github.com/ks-vandana/pes_pd/blob/main/DAY%201/run_synthesis.png)

It is required to give the command **package require openlane 0.9** every time we open the interactive mode.
On running the command **run_synthesis** we can check our designs folder to find a report.
```
cd ../OpenLane/designs/picorv32a/runs/RUN_2023.09.09_18.41.06/reports/synthesis/
```
**RUN_2023.09.09_18.41.06** is the most recent run that was performed. 
Navigate to the above folder and open **1-synthesis.AREA_0.stat.rpt**

![image](https://github.com/ks-vandana/pes_pd/blob/main/DAY%201/run_synthesis_report.png)

From this report we can see that the total number of cells = 10104 and the number of sky130_fd_sc_hd_dfxtp_2 (this is the standard cell for d flip flop) = 1596. Thus the ratio of cells which are d flip flops is 0.157

</details>


<details>

<summary><b> DAY 2 </b></summary>

## Chip Floor planning considerations
1) Define the width and height of the core and die: we first begin with a netlist. Calculate the area occupied by the netlist on a silicon wafer. Inside the die, we have a core, where we place our digital logic. A die is a small semiconductor material specimen on which the fundamental circuit is fabricated. Utilization factor = area occupied by the netlist/total area of the core. Aspect ratio = height/width.

2) Define the location of preplaced cells: Some cells perform certain tasks and can be re-instantiated multiple times like memory, clock gating cells, comparator, mux, and more. These IPs have user-defined locations and hence are placed on the chip before placement and routing which is why we refer to them as preplaced cells.  The location of these cells is not modified during the PNR stages.

3) surround pre-placed cells with decoupling capacitors: VDD takes care of the transition from 0 to 1. VSS takes care of the transition from 1 to 0. To connect VDD or VSS to the circuit we require wires, Since wires have physical dimensions, they will have resistance and inductances. These will reduce the supplied voltages. If the voltage drop is not in the noise margin range, the signal will be in an undefined area. So we can't guarantee that signal is 1 or 0. To prevent this, we add decoupling capacitors in parallel with the circuit. Every time the circuit switches, current is drawn from the capacitors. The RL network is used to replenish the charge in the capacitor.

4) Power planning: When we have a bus of n bits and some logical operation must be done on it, the lines of the bus will either discharge or charge. When multiple capacitors discharge to VSS, the voltage of VSS might increase (ground bounce). When multiple capacitors charge to VDD, the voltage of VDD might decrease (voltage droop). Thus instead of power coming from one source, if it comes from multiple sources, we can avoid signals going into the undefined area.

5) Pin placement: All input pins are on the left-hand side and all the output pins are on the right-hand side.

6) Logical cell placement blockage: We block the area occupied by the pins to prevent the PNR tool from placing logical blocks where the pins are present. 

If installation of OpenLane was a local installation, use the following commands to set up **magic** and **PDKs**

For magic:
```
sudo apt-get update
sudo apt-get install magic
```
For PDK: 
```
git clone https://github.com/RTimothyEdwards/open_pdks.git git_open_pdks
cd ~/git_open_pdks
./configure --enable-sky130-pdk --with-sky130-variants=all --prefix=/home/<unixusername>
make
make install
```
In this lab we are going to see the floorplan of our previously synthesized design picorv32a. Use the following commands.
```
cd OpenLane
sudo make mount
./flow.tcl -interactive
package require openlane 0.9
prep -design <file_name>
run_synthesis
run_floorplan
```
![image](https://github.com/ks-vandana/pes_pd/blob/main/DAY%202/openlane_floorplan.png)

Once floorplan is complete we need to open it in magic to view the floorplan.
```
cd ../OpenLane/designs/picorv32a/runs/<most_recent_run>/results/floorplan/
magic -T ../git_open_pdks/sky130/magic/sky130.tech lef read ../OpenLane/designs/picorv32a/runs/<most_recent_run>/tmp/merged.nom.lef def read picorv32.def &
```
![image](https://github.com/ks-vandana/pes_pd/blob/main/DAY%202/view_floorplan.png)

Once magic opens, we can see the cell. Use **s** and then **v** to center the floorplan. Use **z** to zoom in.

![image](https://github.com/ks-vandana/pes_pd/blob/main/DAY%202/floorplan.png)

![image](https://github.com/ks-vandana/pes_pd/blob/main/DAY%202/pins%20in%20floorplan.png)

![image](https://github.com/ks-vandana/pes_pd/blob/main/DAY%202/cells%20in%20floorplan.png)

![image](https://github.com/ks-vandana/pes_pd/blob/main/DAY%202/standard%20cells%20in%20floorplan.png)


## Library Binding and Placement
This part discusses about placement and the steps involved in it.
Placement and routing
1) Bind netlist with physical cells: The library consists of various flavors of standard cells and their respective timing information and required conditions. It also contains the width and height of each cell.
2) Placement: We place the cells in the available space of the floorplan as close as possible to their respective input and output pins.
3) Optimize placement: This is the stage where we estimate wire length and capacitance and based on that, we place repeaters or buffers. This will prevent the loss of signals. The drawback is that area increases but signal integrity is maintained.

In this lab we are going to see the placement of our previously floorplanned design picorv32a. Use the following commands.
```
cd OpenLane
sudo make mount
./flow.tcl -interactive
package require openlane 0.9
prep -design <file_name>
run_synthesis
run_floorplan
run_placement
```
To view the placement we need to invoke **magic** using the following commands
```
cd ../OpenLane/designs/picorv32a/runs/<most_recent_run>/results/placement/
magic -T ../git_open_pdks/sky130/magic/sky130.tech lef read ../OpenLane/designs/picorv32a/runs/<most_recent_run>/tmp/merged.nom.lef def read picorv32.def &
```

![image](https://github.com/ks-vandana/pes_pd/blob/main/DAY%202/view_placement.png)

This opens up magic where we can see the global placement of the standard cells

![image](https://github.com/ks-vandana/pes_pd/blob/main/DAY%202/placement.png)

![image](https://github.com/ks-vandana/pes_pd/blob/main/DAY%202/placement_std_cells.png)

## Cell design and characterization flows
Cell design flow has 3 parts – inputs, design steps, and outputs. 
Inputs to design a standard cell are PDKs which contain DRC and LVS rules, SPICE models, library, and user-defined specs. SPICE model parameters are the parameters that we get from the foundary. 
Design steps are circuit design, layout design and characterization.
 Circuit design has 2 steps – implement the design and model the NMOS and PMOS to meet the requirements. 
Layout design requires us to get the PMOS and NMOS network graphs from the network. Next we obtain the Euler’s path and get the stick diagram of the layout. We then convert the stick diagram into a layout while adhering to the inputs obtained from previous stages.  
Characterization gives us timing, noise and power information.
Output of circuit design is circuit description language (CDL). Output of layout design is GDS, LEF and extracted spice netlist. Output of characterization is timing, noise, power .libs and functionality of the circuit. 

Characterization flow has the following steps
1) Read the model files
2) Read the extracted SPICE netlist
3) Recognize the behaviour of the circuit
4) Read the models of the sub-circuits
5) Attach the necessary power sources
6) Apply the stimulus
7) Provide necessary output capacitances
8) Provide necessary simulation commands
9) 
Feed in the outputs of the 8 steps to the characterization software tool called GUNA. It will give output as a model that contains timing, noise, power .libs and functionality of the circuit. 

## General timing characterization parameters
Timing threshold definitions are – slew_low_rise_thr,  slew_high_rise_thr, slew_low_fall_thr, slew_high_fall_thr, in_rise_thr, in_fall_thr, out_rise_thr, and out_fall_thr. 
Propagation delay uses in_rise_thr, in_fall_thr, out_rise_thr, and out_fall_thr. Transition time for low to high uses slew_low_rise_thr and  slew_high_rise_thr. Transition time for high to low uses slew_low_fall_thr and slew_high_fall_thr


</details>

<details>

<summary><b> DAY 3 </b></summary>

## Labs for CMOS inverter ngspice simulations
SPICE deck is the connectivity information of the netlist, the inputs necessary, tap points etc. We need to define the component values like W/L ratios, capacitance values, VDD and VSS values. Next we need to identify and name the nodes. 
CMOS circuits are very robust. The parameter that define the robustness of CMOS is switching threshold: This is the point where Vin = Vout = Vm. When we compare 2 circuits where the 1st one has Wn < Wp and the 2nd one has Wn != Wp, Vm2>Vm1. At Vm, Idsp = -Idsn. 

Use the following commands to git clone the required repo
```
cd OpenLane
git clone https://github.com/nickson-jose/vsdstdcelldesign.git
cd vsdstdcelldesign
```
Then find the sky130.tech file and move to the above directory
```
cd ../git_open_pdks/sky130/magic/
cp sky130.tech ../OpenLane/vsdstdcelldesign/
```
To view the layout of the inverter, use the following command
```
magic -T sky130.tech sky130_inv.mag &
```
The version of magic on my system is 8.3.105 and to view the layout you need to have the file sky130A.tech which can be downloaded from [https://github.com/praharshapm/vsdmixedsignalflow/blob/master/sky130A.tech](https://github.com/praharshapm/vsdmixedsignalflow/blob/master/sky130A.tech). This file must be downloaded into the **vsdstdcelldesign** folder to run the above command.

![image](https://github.com/ks-vandana/pes_pd/blob/main/DAY%203/inverter_magic.png)

![image](https://github.com/ks-vandana/pes_pd/blob/main/DAY%203/magic_layout.png)

## Inception of Layout and CMOS fabrication process


## Sky130 Tech File Labs



</details>

<details>

<summary><b> DAY 4 </b></summary>

## Timing modelling using delay tables


## Timing analysis with ideal clocks using openSTA


## Clock tree synthesis TritonCTS and signal integrity


## Timing analysis with real clocks using openSTA

</details>


<details>

<summary><b> DAY 5 </b></summary>

## Routing and design rule check (DRC)


## Power Distribution Network and routing


## TritonRoute Features

</details>
