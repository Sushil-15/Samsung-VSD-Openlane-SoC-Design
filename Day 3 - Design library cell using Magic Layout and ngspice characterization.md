## Section 1: Labs for CMOS Inverter ngspice Simulations

### Performing floorplan with equidistant and non equidistant pin placement

* OpenLANE allows us to make changes to the design variables on the go (while the flow is running).
* We will now try this out.

Run all the steps done until floorplan.

```bash
# Changes the directory to the one containing the OpenLANE files.
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
![image](https://github.com/user-attachments/assets/8bff71ec-e81c-40ef-b4e3-0a801266626c)

```bash
run_synthesis
# runs the synthesis step in OpenLANE
```
![image](https://github.com/user-attachments/assets/00002ff0-0a00-47f6-8eb8-7a646b537a1a)

```bash
run_floorplan
# Runs the floorplan stepD in OpenLANE
```
![image](https://github.com/user-attachments/assets/e72d250a-ac8c-46f2-aebd-03f1fb173dfe)

Open a new terminal window and run the following to see the floorplan results in magic.

```bash
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/31-01_18-27/results/floorplan/
```
![image](https://github.com/user-attachments/assets/678f760c-d3b0-475e-9678-52bf448bb623)

```bash
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
```
![image](https://github.com/user-attachments/assets/8d73ba73-6bca-4a39-8040-aa178d140d64)

Image of result opened in magic.
![image](https://github.com/user-attachments/assets/bd2d320e-567a-4dda-8494-b63817c45030)

Pins are equidistant
![image](https://github.com/user-attachments/assets/bd481f3e-43ea-4ce4-b851-cf4710da649a)

Set the `env(FP_IO_MODE)` variable to 2 to decrease tendency of pins to be equidistant.
```bash
set ::env(FP_IO_MODE) 2
```
![image](https://github.com/user-attachments/assets/d49d98f9-8cca-417e-94bd-c75988d683b2)

Run floorplan again to see changes.
```bash
run_floorplan
```
![image](https://github.com/user-attachments/assets/b78a292c-7715-4311-a23a-727d83cec469)

Open a new terminal window and run the following to see the new floorplan results in magic.

```bash
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/31-01_18-27/results/floorplan/
```
![image](https://github.com/user-attachments/assets/678f760c-d3b0-475e-9678-52bf448bb623)

```bash
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
```
![image](https://github.com/user-attachments/assets/c7424933-ed5c-4834-a0df-64c492d68fd0)

Image of result opened in magic.
![image](https://github.com/user-attachments/assets/73cd7347-6241-4222-ab53-9e8a98ded4ef)

Pins are n longer equididtant (There is a huge gap in the middle)
![image](https://github.com/user-attachments/assets/0e80b5f0-935d-4e02-9268-a20736e64b7a)

### Spice Simulations

**PMOS**

* In a PMOS, the arrow points outwards.

(Lecture Videos)

![image](https://github.com/user-attachments/assets/231e003d-92c5-4c4a-b065-c0d04bd8d162)

**NMOS**

* In a NMOS, the arroe points inwards.
 
(Lecture Videos)

![image](https://github.com/user-attachments/assets/382de0de-a60e-4422-b8bc-ca73b24a4568)

**Output Load Capacitor**

(Lecture Videos)

![image](https://github.com/user-attachments/assets/f337a38a-c047-4d1f-9758-79263e182695)

**Creation of spice Deck**
* We start from a netlist with component connectivity.

(Lecture Videos)

![image](https://github.com/user-attachments/assets/c9f94883-7ea1-4864-962f-e6cfa9d2bc38)

* We then define the component values. This includes input and supply voltage, load values, MOSFET values.

(Lecture Videos)

![image](https://github.com/user-attachments/assets/8d2d17b1-14c4-4c60-b2ac-27e665b92cc6)

* Then we identify *nodes*.
* *Nodes* are 2 points lying on either side of a component.

(Lecture Videos)

![image](https://github.com/user-attachments/assets/f8d87ac8-72f8-49f7-8dc2-3d3af7ce4a48)

* We name nodes and define MOSFETs by the name, followed by the nodes around it in the order drain-gate-source-substrate. Then we decribe it as NMOS or PMOS. This is folllowed by the width and length.

(Lecture Videos)

![image](https://github.com/user-attachments/assets/28c2da0e-71dd-4fc3-843d-bab5cd6b1304)

* We describe load and Voltage sources in a similar manner.

(Lecture Videos)

![image](https://github.com/user-attachments/assets/075a7473-dac9-48f3-85d2-92b1ee1a3419)

* We then give simulation commands.
* The order (essentially the syntax) is Vcc <from> <to> <in steps of>

(Lecture Videos)

![image](https://github.com/user-attachments/assets/e13d1929-cee2-4dd6-8a31-ac7738c31a78)

* We then show the location of the model file which has all the technological parameters of everything in the netlist.

(Lecture Videos)

![image](https://github.com/user-attachments/assets/c73c6e71-83c7-4ba1-a3a9-f6facd18ad87)

Different w/l (width/length) ratios of PMOS can have changes on the output waveform.

### Switching Threshold

(Lecture Videos)

![image](https://github.com/user-attachments/assets/d87830aa-c277-486f-bfad-b27ae7c8c8f7)

* Switching Threshold is the point where V<sub>in</sub> = V<sub>out</sub>

(Lecture Videos)

![image](https://github.com/user-attachments/assets/0719b00b-6d26-4fcf-96d8-03473785e542)

Switching threshold is the intersection of the lines.
* In this area both the NMOS and PMOS are in the saturation region.

(Lecture Videos)

![image](https://github.com/user-attachments/assets/56efd70a-d3cf-444b-81f8-7cfbd165fd90)

If Vin = Vout means gate voltage = drain voltage, Vgs = Vds
* the currents of the NMOS and PMOS will be the same, however the directions will be different.
* I<sub>dsP</sub> = I<sub>dsn</sub>
* Pulse definition in spice deck: V <location based on nodes> pulse <start voltage> <end voltage> <shift(delay in start)> <rise time> <fall time> <pulse width> <width of complete cycle>

(Lecture Videos)

![image](https://github.com/user-attachments/assets/807ae7ff-cc9b-4995-a8cc-8813ebe985ea)

Delay(rise or fall): Out-In(at 50% w.r.t. time)

(Lecture Videos)

![image](https://github.com/user-attachments/assets/1891d848-73a1-495b-8882-5fdd498b0d71)



