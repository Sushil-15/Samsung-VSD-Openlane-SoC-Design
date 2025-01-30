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

>.def stands for design exchange format

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
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/30-01_19-22/results/placement/
```
![image](https://github.com/user-attachments/assets/eb9be4bb-72b3-46a1-b813-9fd0bc99f3ed)

```bash
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```
![image](https://github.com/user-attachments/assets/e9bff1cf-8243-4bc6-9e93-12557d003cac)

Screenshots of def file opened in magiczv
