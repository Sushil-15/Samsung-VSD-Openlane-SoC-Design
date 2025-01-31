## Section 1-Chip Floor planning considerations

### Defining Width and Height of the Chip

(Lecture videos)

![image](https://github.com/user-attachments/assets/aa53f632-8a83-447c-bd18-48fc034137e8)

* Let us begin with a _netlist_.
* **Netlist:** A netlist is a list of all the components (like flip-flops, gates) and their connections in a circuit.
(Lecture videos)

![image](https://github.com/user-attachments/assets/3f96a19c-3ccf-47ad-bff9-0189384c7941)

* FF - Flip Flops
* A1, O1 - AND Gate and OR Gate
* **Gates:** We assign a size (width and height) to each gate in the netlist, creating a unique box for each one.

(Lecture videos)

![image](https://github.com/user-attachments/assets/05be10b8-3128-4af4-8dea-4d5a13d17426)

* **Cell Areas:** We estimate the size of each cell in the netlist and calculate its area.

(Lecture videos)

![image](https://github.com/user-attachments/assets/b098e3c6-5679-4dc1-95d2-5369b10fe569)

![image](https://github.com/user-attachments/assets/a9287ce7-e226-4893-95b9-1f0bdc27ca02)

* **Core Placement:** We arrange all the logic cells (gates, flip-flops) inside the central area called core.

(Lecture videos)

![image](https://github.com/user-attachments/assets/7c95b04f-4892-436e-993e-79026e240a65)

![image](https://github.com/user-attachments/assets/a4197965-a4ce-4665-ab1f-c82f8c8c304c)

* **Utilization Factor (UF):** This measures how efficiently the core area is used by the logic cells.
* UF is calculated as the area occupied by the **netlist** divided by the total core area. In real chips, UF is typically around 0.5-0.6.
* UF = Ar. occupied by netlist/total area of core.
* **Aspect Ratio:** This is the ratio of the core's height to its width. A value of 1 indicates a square core.
 
### Preplaced Cells

* **Preplaced cells** are groups of predefined logic gates that are treated as a single unit.
* To create a preplaced cell, we first partition or divide the **netlist** into smaller functional blocks or parts. We then create boxes around each part.

(Lecture videos)

![image](https://github.com/user-attachments/assets/c51824ba-67a8-49ed-96c2-cf8ca51bab68)

* Then we *extend* the input and output io pins such that they are jutting out of the box.

(Lecture videos)

![image](https://github.com/user-attachments/assets/8d59cb64-34df-431f-bf25-f0374b832599)

* **Blackboxing:** We hide the internal details of each block by treating it as a black box with a specific number of input and output pins.

(Lecture videos)

![image](https://github.com/user-attachments/assets/15d24e48-52b2-4ee3-861e-01bfdd482c76)

* Afetr that, we separate the blackboxed parts. This allows each small block to have its own input and output pins, enabling it to be used independently.
(Lecture videos)

![image](https://github.com/user-attachments/assets/65e6a9c4-fc5f-493d-98c5-52ea84a03452)

* The arrangement of these IP's in the chip is known as **floorplanning**. These blocks have user defined locations and are placed before the automated placement and routing phase. Hence they are known as preplaced cells.

### Defining Locattions of Pre-placed Cells

* **Preplaced Cell Locations:** Input pins are typically placed on one side of the chip (left or bottom), and output pins are placed on the opposite side (right or top).
*  Preplaced cells are positioned closer to the input side for better connectivity. Their location after this step will remain untouched.

(Lecture videos)

![image](https://github.com/user-attachments/assets/133a020b-ae42-40f2-b4a6-03bfe1062574)

### Decoupling Capacitors

* Real-world (Practical) wires have resistance, which can influence the voltage difference (voltage drop) between different parts of the circuit.

(Lecture videos)

![image](https://github.com/user-attachments/assets/2f5f89c5-73ce-4872-aa5f-bbe2f0129a9b)

* This may lead the voltage difference applied between the IP's to reduce.
* If this reduction is high, then the voltage will not be enough to trigger a logic 1 like it should and will lie in the undefined region.

(Lecture videos)

![image](https://github.com/user-attachments/assets/0dcf107f-67e1-4625-85ec-84d7f6f30fd8)

* So to ensure that the piece of circut always has enough of a voltage drop, we use a decoupling capacitor.
* **Decoupling capacitors** are used to maintain a stable voltage supply for the circuit. They ae large capacitors that are *filled* with charge.
* They act as tiny batteries that can quickly release stored energy to compensate for voltage fluctuations.
* They charge when the circuit they are supposed to power is not in use.

(Lecture videos)

![image](https://github.com/user-attachments/assets/b2a6fbc0-fc31-41d2-b18f-2e3cc35fcadb)

* Decoupling capacitors are placed near pre-placed cells throughout the core area of the chip.

(Lecture videos)

![image](https://github.com/user-attachments/assets/93f22b9c-9b7f-4f76-8c42-9e5e18990d05)

### Power Planning

* **Ground Bounce**- It's a sudden change in the ground voltage due to simultaneous switching (dumping of current) of many components (capacitors). If the bounce is very high then the ground can enter into an undefined state.
* **Voltage Drop**- This occurs when a single power source is tasked with recharging multiple capacitors. It's a decrease in voltage along the power supply lines due to the immense amount of current drawn by the circuit. This leads to the power line to experiance a voltage drop, leading the voltage difference to go into the undefined region.
* The above problems are caused by an overloaded power line/ground line. This can be fixed by having multiple power and ground lines throughout the chip.
(The below image is representing a 16-bit bus when set as input for an inverter)


(Lecture videos)

![image](https://github.com/user-attachments/assets/6c51746e-1f4e-40d8-bc12-abb8f53d8a9b)

![image](https://github.com/user-attachments/assets/93d99c70-2bb4-476e-8b3b-76a1bd3ade9f)

![image](https://github.com/user-attachments/assets/aec37dcc-85cf-4204-b0ef-1bf010c0ae51)

(Power is coming from a single source)

![image](https://github.com/user-attachments/assets/6b60e995-83d0-438e-b377-6b5e061fe6c7)

(Power is coming from multiple sources. In this system, the power line's and ground line's are arranged in the form of a mesh)

![image](https://github.com/user-attachments/assets/0939b3c0-109e-40e5-b091-6857d1830c9d)

### Pin Placement

* The goal is to *optimize* pin placement for better performance.
* Input and output pins are placed close to their corresponding pre-placed cells to minimize signal path lengths.
* Clock pins (input and output) are placed directly opposite each other to minimize resistance.
* Pre-placed cells are placed close together whenever possible to reduce the number of decoupling capacitors needed.

(Lecture videos)

![image](https://github.com/user-attachments/assets/b4ba35f1-e906-461b-b29f-affc1c4101a3)

### Logical Cell Placement and Blockage

* The space between the core and the die is filled with special blockages.
* These blockages reserve the area for pin placements and prevent the automated placement and routing tool from accidentally placing cells there.

(Lecture videos)

![image](https://github.com/user-attachments/assets/d0f00886-808c-4d11-aeb6-8eed0b778539)

### Running Floorplan in OpenLANE

View the adjustable variables by opening `README.md` in `/Desktop/work/tools/openlane_working_dir/openlane/configuration`
```bash
cd /Desktop/work/tools/openlane_working_dir/openlane/configuration
```
```bash
less README.md
```

![image](https://github.com/user-attachments/assets/3219b093-5dc0-4ffe-8c0b-aae779d9f45a)

Some of the Variables:

`FP_CORE_UTIL` : FP indicates Floorplan. The core utilization percentage. (The default is 50)

`FP_ASPECT_RATIO` : The core's aspect ratio (height / width). (Default is 1 giving a square shape)

`FP_IO_HMETAL` : The metal layer on which to place the io pins horizontally (top and bottom of the die).(Default: `4`)

`FP_IO_VMETAL` : The metal layer on which to place the io pins vertically (sides of the die)

`FP_PDN_*****` : Power Distribution Network variables.

`PL_TARGET_DENSITY` : It tells us how spread the cells would be on the core area. PL indicates placement.

Open `floorplan.tcl`
```bash
less floorplan.tcl
```
![image](https://github.com/user-attachments/assets/e2e9b6ed-ca99-407f-9743-b06ae625fa30)

It shows some of the set values for floorplan.

> `env` means environment variable
> 
> If `env(FP_IO_MODE)` is 1, the pin position will be equidistant.

Preferences for settings and variables for floorplan
1. openlane/designs/<design>/sky130A_sky130_fd_sc_hd_config.tcl
2. openlane/designs/<design>/config.tcl
3. openlane/configuration/floorplan.tcl

Edit `openlane/designs/picorv32a/config.tcl` as per requirement.
![image](https://github.com/user-attachments/assets/21b138bf-c90c-4297-bebc-2725069a8e5f)

Edit `openlane/configuration/floorplan.tcl` as per requirement
![image](https://github.com/user-attachments/assets/0cfc43c7-0776-48c1-bc41-5e258b44305a)

> In openlane flow `env(FP_IO_VMETAL)` and `env(FP_IO_VMETAL)` is one more than what you specify.

We set different values of `env(FP_CORE_UTIL)`, `env(FP_IO_VMETAL)` and `env(FP_IO_VMETAL)` in `openlane/designs/<design>/config.tcl` and `openlane/configuration/floorplan.tcl` for experimentation as shown in vids.

After setting variables and settings, run floorplan (initiate OpenLANE flow again if nescessary)
```bash
run_floorplan
```
![image](https://github.com/user-attachments/assets/5383e93e-ec82-41a9-8160-528c27d0c0f9)

Screenshot of successfull floorplan execution

![image](https://github.com/user-attachments/assets/5a925e25-b108-4f71-8af8-5ffc2f845504)

Screenshot of IOplacer.log

![image](https://github.com/user-attachments/assets/625b0f9c-21f7-45e2-8b67-a98398c122df)

This is due to nothing being mentioned in `openlane/designs/picorv32a/sky130A_sky130_fd_sc_hd_config.tcl` and therefore this overrode both the other files having higher priority.

![image](https://github.com/user-attachments/assets/8b741fc8-f6da-4ba1-b825-88ec2a602c47)

> .def stands for design exchange format

Open `/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/30-01_19-22/results/floorplan/picorv32a.floorplan.def` to get `DIEAREA`
```bash
cd /Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/30-01_19-22/results/floorplan
```
```bash
less picorv32a.floorplan.def
```
Screeenshot of `picorv32a.floorplan.def` with `DIEAREA`

![image](https://github.com/user-attachments/assets/734df186-3b8d-4489-819b-086cfdeed916)

UNITS DISTANCE MICRONS 1000 ;

> According to this statement, 1000 database units make up one micron.

`DIEAREA` ( 0 0 ) ( 660685 671405 ) ;

Area = length * breeadth
     = (660685-0) * (671405-0)
     = 443587212425 database units square
     = 443587.212425 microns square

Now we have to open the def fine in a more understandable visual representation. This can be done using magic.
```bash
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/30-01_19-22/results/floorplan/
```
![image](https://github.com/user-attachments/assets/f59c5a82-5530-4b68-a5fd-2e51bed74328)

```bash
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
```
![image](https://github.com/user-attachments/assets/bbd7c5a1-231d-424c-b243-723d4e19e19d)

Screenshots of def file opened in magic.

![image](https://github.com/user-attachments/assets/4fe38b35-d6c4-41e2-86ec-08aca6782c81)

Commands:

`S`- Select object that the cursor is hovering over.

`V`- Fit to screen

`Z`- Zoom

Screenshot showing placement of the cells.(After zooming in)

![image](https://github.com/user-attachments/assets/057c3265-03a5-472c-a146-75c09b8b5b71)

Sceenshot showing the pins. The decap cells are also seen.s One of the pins have been selected using `S`.

![image](https://github.com/user-attachments/assets/84ed6404-7cc4-4555-8fe9-a96d1e7ae9a3)

Run `what` in the tkcon window (the one with the diagram is the layout window and the smaller one with the text is the tkcon window) to get info on the pin.

```bash
what
```
![image](https://github.com/user-attachments/assets/00d04daa-fee1-4c6c-8dc8-ce1bcb42052c)

It in shown that the pin is in metal 3 as per the settings for horizontal metal (HMETAL)

Select vertical metal pin

![image](https://github.com/user-attachments/assets/5030e5e0-91e0-4b80-8068-5f4f25b8268d)

On running `what` it shows that the pin is in metal 2, as set in the settings.

![image](https://github.com/user-attachments/assets/68b07d6b-6eb6-4a3c-a21c-f2b2e5b8606a)

Tap cell selected.

![image](https://github.com/user-attachments/assets/c4f14ee8-5883-4a40-8183-2f60205e7dfe)

If we go to the lower left corner, we will see a lot of overlapping text.

![image](https://github.com/user-attachments/assets/49ba4592-3949-42e0-9c5f-ceb4e3bd4601)

If we select one of them, we will see that is is a gate. In the screenshot below, I have selected a buffer gate.

![image](https://github.com/user-attachments/assets/56564fe1-31a8-4f1a-ad5f-1b62c015f819)

## Section 2: Library and Placement
	
### Libraries
* A **netlist** is a list of all the components (like flip-flops, gates) and their connections in a circuit. 
* Below is a netlist.

(Lecture videos)

![image](https://github.com/user-attachments/assets/2a6b9bf3-863a-43d6-9bd2-59d7900891b7)

* We take all the cells in the netlist and place them in a shelf known as the **library**.

(Lecture videos)

![image](https://github.com/user-attachments/assets/9763eb63-1245-422c-83b9-bdcb94cd10d1)

* A **library** contains detailed information about each cell, including:

    * Dimensions (size)
    * Delay characteristics (how long it takes for a signal to pass through the cell)
    * Environmental requirements (e.g., voltage range)
    * Threshold voltage (minimum voltage required for the cell to switch states)
    * Below is a picture of libraries with same cells but different properties.

(Lecture Videos)

![image](https://github.com/user-attachments/assets/8ba23fb2-e97d-45a6-86f0-900a7a9ac8a3)

### Placement 

Placement is the stage where we estimte wire length and capacitance and, based on that, place repeaters, logic and Flip Flops. The placement process involves these steps:

   1. **Pre-placed cell placement:**  Placing specially designated cells at predefined locations.
   2. **Standard cell placement:**  Placing the remaining cells based on their connectivity to pins and other cells. Cells are positioned closer to the pins they connect with to minimize the wirelength. 
   3. **Repeater insertion:** If the distance between a cell and its connected pin is too large, buffers or repeaters are inserted to ensure reliable signal transmission.

(Lecture videos)
       
![image](https://github.com/user-attachments/assets/985d4d79-e5d3-4f11-94da-d4d727f6715e)

![image](https://github.com/user-attachments/assets/c81cc697-8fbb-4293-89df-7458fa971426)

### Goals for placement:

* Cutting down on wire length
* Making sure placement is legal (cells don't overlap)

### ***Typical IC Design Flow***

1) **Logic Synthesis**
 * This is the first step in converting RTL to hardware.
 * Gives output as an arrangement of gates and flip flops to represent origional function described by the RTL.

(Lecture videos)

![image](https://github.com/user-attachments/assets/50c83343-d018-418f-8338-c6f93fc1424b)

2) **Floorplanning**
 * Takes imput from logic synthesis step.
 * Defines the height of the core and die based on the number of cells utilised.

(Lecture Videos)

![image](https://github.com/user-attachments/assets/6724b883-2bdb-48ee-83fd-b2c5c3be7516)


3) **Placement**
 * In this stage we arrange the cells in an optimised position.
 * The cells are placed close to where they are logically connected to reduce wire length.
 * This is done to get optimal timing.

(Lecture Videos)

![image](https://github.com/user-attachments/assets/64ccefc9-a59f-444d-b309-519fad6a3a3c)

4) **Clock Tree Synthesis**
 * This is the step of coordinationg all the clocks reaching all the cells (with minimum delay).
 * This is done by makig sure all the clock delays are the same.

(Lecture Videos)

![image](https://github.com/user-attachments/assets/d58ebd56-2d21-41c2-9a82-f6435399182f)

5) **Routing**
 * This is the proscess of connecting cells with least amount of wire.
 * One of the popular methods to do this is the maze routing method.

(Lecture Videos)

![image](https://github.com/user-attachments/assets/01a83ee7-a6b3-412e-a0ff-1a4fcd0c1827)

6) **Static Timing Analysis**
 * We calculate setup time, hold time, and the maximum frequency of the circuit.
 * We also do other timing calculations and tweak the values so as to get no errors.

(Lecture Videos)

![image](https://github.com/user-attachments/assets/2776f700-bcb8-41d7-9c29-52264209f1a7)

### Running Placement in OpenLANE

Run all the steps done until floorplan.
```bash
cd Desktop/work/tools/openlane_working_dir/openlane
```
![image](https://github.com/user-attachments/assets/fbb23858-d0df-4cec-9b94-52053b694459)

```bash
docker
```
![image](https://github.com/user-attachments/assets/02bb0199-cbe7-4bde-aa7a-82261d6e6e67)

```bash 
./flow.tcl -interactive
# The -interactive flag opens flow.tcl in interactive mode (step by step)
```
![image](https://github.com/user-attachments/assets/2836052d-1cc9-4851-8981-7d44253c4e6b)

```bash
package require openlane 0.9
```
![image](https://github.com/user-attachments/assets/c2c4679b-d038-422d-9aac-ec5a1d4036ce)

```bash
prep -design picorv32a
# This command makes sure that future commands follow the design specifications as set in picorv32a
```
![image](https://github.com/user-attachments/assets/68478e34-6d5d-43a3-a8bf-b575bf362338)

```bash
run_synthesis
# runs the synthesis step in OpenLANE
```
![image](https://github.com/user-attachments/assets/a6ac0369-3596-4e33-a3b6-e395cd8c270e)

```bash
run_floorplan
# Runs the floorplan stepD in OpenLANE
```
![image](https://github.com/user-attachments/assets/1d983c03-d250-4151-8eea-95d230d022fa)

```bash
run_placement
# Runs the placement step in OpenLANE
```
![image](https://github.com/user-attachments/assets/84a5e585-59c5-47db-98ec-ce7e398f92d7)

Screenshot of sucessfull completion of the placement step.

![image](https://github.com/user-attachments/assets/02d80c11-109f-4e03-9250-6d7d28e60ee2)

Open a new terminal window and run the following to see the placement in magic.

```bash
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/31-01_17-13/results/placement/
```
![image](https://github.com/user-attachments/assets/d281148c-518f-4cda-9269-9bb72f1bc124)

```bash
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```
![image](https://github.com/user-attachments/assets/86a8b480-7d90-48f7-9a24-c788756fba1e)

Screenshots of placement.def in magic.

![image](https://github.com/user-attachments/assets/93745fa7-25f2-4bbd-8ca9-12cc84fc7e5f)

The stadard cells that were present in the lower left corner in floorplan.def have been placed onto the chip.

Screenshot of the standard cells being placed. (Zoom in using `Z`)

![image](https://github.com/user-attachments/assets/102e3dc2-26e6-43d9-8276-67fa5f898454)

> Note that normally the Power-Ground Network gets created during floorplan but in OpenLANE it is created after CTS.
> 
________________________________________________________________________________________________
## Section 3 - Cell design and characterization flows

![image](https://github.com/user-attachments/assets/3c88277d-24df-479f-88f6-178e379db640)

## Section 4 - Timing threshold definitions
_________________________________________________________________________________________________


