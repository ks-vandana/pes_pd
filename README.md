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
cd ../
cp sky130A.tech ../OpenLane/vsdstdcelldesign/
```
To view the layout of the inverter, use the following command
```
magic -T sky130A.tech sky130_inv.mag &
```
The version of magic on my system is 8.3.105 and to view the layout you need to have the file sky130A.tech which can be downloaded from [https://github.com/praharshapm/vsdmixedsignalflow/blob/master/sky130A.tech](https://github.com/praharshapm/vsdmixedsignalflow/blob/master/sky130A.tech). This file must be downloaded into the **vsdstdcelldesign** folder to run the above command.

![image](https://github.com/ks-vandana/pes_pd/blob/main/DAY%203/Images%20part%201/inverter_magic.png)

![image](https://github.com/ks-vandana/pes_pd/blob/main/DAY%203/Images%20part%201/magic_layout.png)

## Inception of Layout and CMOS fabrication process
16 mask CMOS process

1) Selecting a substrate: The substrate is on which we fabricate the entire design. We usually choose a P-type substrate. Substrate doping should be less than well doping. 
2) Create an active region for transistors: We place 40nm of Si02 on the P substrate. Then we deposit a layer of Si3N2 of thickness 80nm. Then we deposit 1um of photoresist. We use **mask1** and then do photolithography to obtain the active regions. We then etch out Si3N2 and chemically remove the photoresist. When we place the above in an oxidation furnace, we notice a growth in SiO2. This process is called “LOCOS” or “Local Oxidization of Silicon”. We then etch out the remaining Si3N2. 
3) N-well and P-well formation: We deposit photoresist and use **mask2**. The area exposed is washed away after photolithography. P-well is formed using boron in the process called ion implantation. We then use **mask3** and wash away the area exposed after photolithography. N-well is formed using phosphorous during ion implantation. We then take the entire substrate into a high-temperature furnace to drive in the diffusion to form clear wells. In N-well we will create PMOS and in the P-well we will create NMOS. 
4) Formation of ‘gate’: 2 important terms for gate formation are doping concentration and oxide capacitance. We then use **mask4** and then use boron with less energy than the one used to make the P-well to get the desired doping concentration. Similarly we use **mask5** for the N-well region. The original oxide is then stripped and new high-quality oxide is regrown. We then deposit 0.4um polysilicon layer and dope the gate with N-type ion implants. We then use **mask6** and remove exposed areas. This gives us 2 gate terminals. 
5) Lightly doped drain formation: For PMOS, we require P+, P-, and N. For NMOS, we require N+, N-, and P. The reason for this is the hot electron effect and short channel effect. We use **mask7** and deposit phosphorous to make N- implant. Next, we use **mask8** and deposit boron to make a P- implant. Then we deposit a thick layer of Si02 or Si3N4 and do plasma anisotropic etching to make side-wall spacers. 
6) Source and drain formation: Thin screen oxide is applied to avoid channeling during implants. We use **mask9** and then expose the structure to arsenic to make N+ implants. We then use **mask10** and then expose the structure to boron to make P+ implants. We then place the structure in a high-temperature furnace to perform annealing. 
7) Steps to form contacts and interconnects (local): We first remove the thin screen oxide. We then deposit titanium on the wafer surface using sputtering (hitting titanium with argon gas makes particles of titanium sputter out). The wafer is then heated in N2 ambient for 60 seconds and we get TiSi2 and TiN. We then use **mask11** and then etch out the TiN using RCA cleaning. This creates local TiN connections.
8) Higher-level metal formation: We deposit a thick layer of SiO2 doped with phosphorous or boron. We then do chemical mechanical polishing for planarizing the wafer structure. Next, we use **mask12** and drill contact holes. We remove the photoresist and deposit TiN. Next, we deposit blanket tungsten and then do chemical metal polishing. We then put a layer of aluminium, use **mask13**, and etch out the aluminium. SiO2 is deposited and CMP is done. **mask14** is used to drill contact holes and the above process is repeated. We then use **mask15** and use Si3N2 to protect the chip. Finally, we use **mask16** to open the contact holes. 

![image](https://github.com/ks-vandana/pes_pd/blob/main/DAY%203/Images%20part%202/nmos_inv.png)

![image](https://github.com/ks-vandana/pes_pd/blob/main/DAY%203/Images%20part%202/pmos_inv.png)

LEF file gives us information on the metal layer. 

To know the logical function of any design, we first extract the spice and then perform simulations on it. Use the following commands in the tkcon window to extract the spice netlist.
```
pwd
extract all
ext2spice cthresh 0 rthresh 0
ext2spice
```

![image](https://github.com/ks-vandana/pes_pd/blob/main/DAY%203/Images%20part%202/tkcon_inv.png)


## Sky130 Tech File Labs

### LAB 1
The order in which we can see the pins in the spice file is drain, gate, source and substrate. We need to make a few changes
This is the original spice file 
```txt
* SPICE3 file created from sky130_inv.ext - technology: sky130A

.option scale=10000u
.subckt sky130_inv A Y VPWR VGND
M1000 Y A VPWR VPWR pshort w=37 l=23
+  ad=1443 pd=152 as=1517 ps=156
M1001 Y A VGND VGND nshort w=35 l=23
+  ad=1435 pd=152 as=1365 ps=148
C0 A VPWR 0.07fF
C1 VPWR Y 0.11fF
C2 A Y 0.05fF
C3 Y VGND 0.24fF
C4 VPWR VGND 0.59fF
ends
```
After changes, this is the spice file
```txt
* SPICE3 file created from sky130_inv.ext - technology: sky130A

.option scale=0.01u
.include /home/vandana/OpenLane/vsdstdcelldesign/libs/pshort.lib
.include /home/vandana/OpenLane/vsdstdcelldesign/libs/nshort.lib
//.subckt sky130_inv A Y VPWR VGND
M1000 Y A VPWR VPWR pshort_model.0 w=37 l=23
+  ad=1443 pd=152 as=1517 ps=156
M1001 Y A VGND VGND nshort_model.0 w=35 l=23
+  ad=1435 pd=152 as=1365 ps=148
VDD VPWR 0 3.3V
VSS VGND 0 0V
Va A VGND PULSE(0V 3.3V 0 0.1ns 0.1ns 2ns 4ns)

C0 A VPWR 0.07fF
C1 VPWR Y 0.11fF
C2 A Y 0.05fF
C3 Y VGND 0.24fF
C4 VPWR VGND 0.59fF
//.ends
.tran 1n 20n

.control
run
.endc
.end
```
We then run the simulation using this command
```
ngspice sky130_inv.spice
```

### LAB 2
```
plot y vs time a
```
This gives us the transient analysis of the inverter.

![image](https://github.com/ks-vandana/pes_pd/blob/main/DAY%203/Images%20part%203/transient_analysis_inv.png)

We need to find time for rise transition (output from 20% to 80%), fall transition (output from 80% to 20%), cell fall delay and cell rise delay. 

1. Rise transistion: 20% of 3.3V is 0.66V and 80% of 3.3V is 2.64V. We will now locate this on the graph. Everytime we select a point on the graph, we can see its x and y value in the terminal. Difference between x0 at 20% and x0 at 80% is 0.042n seconds.

![image](https://github.com/ks-vandana/pes_pd/blob/main/DAY%203/Images%20part%203/rise_20%25.png)

![image](https://github.com/ks-vandana/pes_pd/blob/main/DAY%203/Images%20part%203/rise_80%25.png)

![image](https://github.com/ks-vandana/pes_pd/blob/main/DAY%203/Images%20part%203/rise_transistion.png)

3. Fall transistion: Difference between x0 at 80% and x0 at 20% is 0.028n seconds.

![image](https://github.com/ks-vandana/pes_pd/blob/main/DAY%203/Images%20part%203/fall_20%25.png)

![image](https://github.com/ks-vandana/pes_pd/blob/main/DAY%203/Images%20part%203/fall_80%25.png)

![image](https://github.com/ks-vandana/pes_pd/blob/main/DAY%203/Images%20part%203/fall_transistion.png)

5. Cell fall delay: 50% of 3.3V is 1.65V. Difference between input x0 at 50% and output x0 at 50% is  seconds.
  
![image](https://github.com/ks-vandana/pes_pd/blob/main/DAY%203/Images%20part%203/cell_fall.png)

![image](https://github.com/ks-vandana/pes_pd/blob/main/DAY%203/Images%20part%203/cell_fall_coords.png)

7. Cell rise delay: Difference between input x0 at 50% and output x0 at 50% is  seconds.
  
![image](https://github.com/ks-vandana/pes_pd/blob/main/DAY%203/Images%20part%203/cell_rise.png)

![image](https://github.com/ks-vandana/pes_pd/blob/main/DAY%203/Images%20part%203/cell_rise_coords.png)

### LAB 3
Use the website ![http://opencircuitdesign.com/magic/](http://opencircuitdesign.com/magic/) to explore the documentation of magic. We are going to focus on DRC. 

### LAB 4
Use this command to set up the lab files needed
```
wget https://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz
tar xfz drc_tests.tgz
cd drc_tests/
```
Use the next command to invoke magic
```
magic -d XR
```
![image](https://github.com/ks-vandana/pes_pd/blob/main/DAY%203/Images%20part%203/magic.png)

### LAB 5
Go to files and open the **met3.mag**. TYping **drc why** gives us the DRC rule associated with what is seen on screen

![image](https://github.com/ks-vandana/pes_pd/blob/main/DAY%203/Images%20part%203/drc_met3.png)

### LAB 6
This lab we will correct the incorrect depictions of DRC. First we open poly but typing **load poly** in the tkcon window. As we can see poly.9 is incorrect.

![image](https://github.com/ks-vandana/pes_pd/blob/main/DAY%203/Images%20part%203/incorrect_poly.png)

To rectify this we will look at sky130A.tech.
Add the following lines after line 5178
```
spacing xhrpoly,uhrpoly,xpc allpolynonres 480 touching_illegal \
	"xhrpoly/uhrpoly resistor spacing to diffusion < %d (poly.9)"
```
Add the following lines after line 4815
```
spacing npres allpolynonres 480 touching_illegal \
	"poly.resistor spacing to N-tap < %d (poly.9)"
```
Then type the following commands in tkcon windown to see the change
```
tech load sky130A.tech
drc check
```
![image]() corrected poly drc

### LAB 7
To fix poly and diff and tap drc, make the following changs to the sky130A.tech file. SUbstitute the floowing lines in 4814 and 4815
```
spacing npres alldiff 480 touching_illegal \
	"poly.resistor spacing to N-tap < %d (poly.9)"
```

### LAB 9
To fix nwell errors write the foolowings lines after line 4728 in sky130A.tech
```
variants (full)
cifmaxwidth nwell_untapped 0 bend_illegal \
	"Nwell missing tap (nwell.4)"
variants *
```
Add the following afetr line 1239
```
templayer nwell_tapped
bloat -all nsc nwell
 
templayer nwell_untapped nwell
and-not nwell_tapped
```

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
