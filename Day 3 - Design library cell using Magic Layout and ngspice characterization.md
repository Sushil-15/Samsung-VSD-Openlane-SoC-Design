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

* Here. in this PMOS, the arrow points outwards. Normally, it had a circle presenton the line marking it as PMOS.

(Lecture Videos)

![image](https://github.com/user-attachments/assets/231e003d-92c5-4c4a-b065-c0d04bd8d162)

**NMOS**

* In this NMOS, the arow points inwards.
 
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

Different w/l (width/length) ratios of PMOS can have changes on the output waveform. (Usually PMOS is bigger tan NMOS)

### Switching Threshold

(Lecture Videos)

![image](https://github.com/user-attachments/assets/d87830aa-c277-486f-bfad-b27ae7c8c8f7)

* Switching Threshold is the point where V<sub>in</sub> = V<sub>out</sub>
We find this by plotting the input and output voltages on a graph (Vin nad time on the axis ond the Vout as the actual plotted line/curve according to its correspondng to its values at different times), drawing a 45 degree line from the origin, and then findint the point where it intersects.

(Lecture Videos)

![image](https://github.com/user-attachments/assets/0719b00b-6d26-4fcf-96d8-03473785e542)

Switching threshold is the intersection of the lines.
* In this area both the NMOS and PMOS are in the saturation region. This can lead to short circuit as both NMOS and PMOS are ON.
* *It is also observed that different sizes of the MOSFETS inflence the switching threshold. This observation is important later.*

(Lecture Videos)

![image](https://github.com/user-attachments/assets/56efd70a-d3cf-444b-81f8-7cfbd165fd90)

If Vin = Vout means gate voltage = drain voltage, Vgs = Vds
* the currents of the NMOS and PMOS will be the same, however the directions will be different.
* I<sub>dsP</sub> = I<sub>dsN</sub> (The currents of the NMOS and the PMOS are the same (but act in opposite directons)).
* Pulse definition in spice deck: V <location based on nodes> pulse <start voltage> <end voltage> <shift(delay in start)> <rise time> <fall time> <pulse width> <width of complete cycle>

(Lecture Videos)

![image](https://github.com/user-attachments/assets/807ae7ff-cc9b-4995-a8cc-8813ebe985ea)

![image](https://github.com/user-attachments/assets/6aae2906-6ac8-4772-a4be-09f3086a326e)

This is the spice deck definition for the pulse and the corresponding pulse.

Once we enter ngspice with the pulse definition for analysis, it shows you a transient analysis option along with the dc analysis option. This is because the pulse acts like alternating (AC) current.

(Lecture Videos)

![image](https://github.com/user-attachments/assets/aebea3c4-0f18-4873-b772-ebe9650ad105)

Delay(rise or fall): Out-In(at 50% w.r.t. time)

Image of the transient analysis.

(Lecture Videos)

![image](https://github.com/user-attachments/assets/bb4f84d4-e3c7-41f3-9fde-2d80be354afd)

On zooming in to 50% Vin

(Lecture Videos)

![image](https://github.com/user-attachments/assets/2199f3e8-c495-4048-9289-07a79d7bd496)

The values of the voltages at any pint in the ngspice graph can be found by simply clicking.

We can also calculate the value of rise and fall delays with different sizes of MOSFET's

(Lecture Videos)

![image](https://github.com/user-attachments/assets/ecf76529-261e-4087-ba93-3c9149ab0c03)

### Performing ngspice analysis using OpenLANE

1) Clone [vsdcelldesign repo by Nickson Jose](https://github.com/nickson-jose/vsdstdcelldesign)

 * Copy the repository link

![image](https://github.com/user-attachments/assets/ecea849f-f85c-4789-bba1-b1f4661dd13b)

 * Go to `Desktop/work/tools/openlane_working_dir/openlane/`

```bash
cd Desktop/work/tools/openlane_working_dir/openlane/
```
![image](https://github.com/user-attachments/assets/c9b80c9c-ed5d-4d47-a935-7885dc4719f1)

 * Clone the repo using `git clone`
```bash
git clone https://github.com/nickson-jose/vsdstdcelldesign.git
```
![image](https://github.com/user-attachments/assets/c64e26c8-6034-4cb2-acd9-cbd242d812b6)

 * Open the new directory `vsdstdcelldesign`

```
cd vsdstdcelldesign
```
![image](https://github.com/user-attachments/assets/842a6b24-3e71-4ba6-b156-0a7387c69918)

The inverter mag file

![image](https://github.com/user-attachments/assets/a7fae211-9e18-4d0d-997f-79a9415c7cd5)

2) Copying magic tech file to current directory.

 * Get working directory location by runnning `pwd`
```bash
pwd
```
![image](https://github.com/user-attachments/assets/baae5983-d76f-4178-a9d5-0f6dbdf564f6)

 * Open tech file location in new terminal tab
```bash
cd Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/
```
![image](https://github.com/user-attachments/assets/eefe1d55-9a31-4b2d-880a-45edd19f6390)

This is the file we need to copy

![image](https://github.com/user-attachments/assets/24a82137-c9d5-4aa8-9d66-42f7363b81a7)

 * Copy the file to `vsdstdcelldesign`
```
cp sky130A.tech /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign/
```
![image](https://github.com/user-attachments/assets/2bc288a3-d812-49c8-a250-bdd72e5fa2bb)

The .tech file has been copied

![image](https://github.com/user-attachments/assets/98e27fde-5a02-434d-b77f-ee57d67ac81e)

 * Open the inverter mag file using the tach file we just copied
```bash
magic -T sky130A.tech sky130_inv.mag &
```
![image](https://github.com/user-attachments/assets/e9f2abee-e336-4419-9c2c-081aabd25c6b)

> The -T stands for technology
> & is used to free the comand prompt

Screenshot of `sky130_inv.mag` opened in magic

![image](https://github.com/user-attachments/assets/c858f2b2-5df9-4101-a48e-6b7a1f3fd676)

## Section 2: 16 mask CMOS creation proscess

1) Select a substrate - The part on which everything is fabricated on. The doping level should be less than well doping.
2) Creating active region(pockets) for transistors - We keep gaps and isolation between each and every pocket to avoid interference.
 * Grow a layer of silicon dioxide (insulator) on the substrate.
 * Then deposit a later of Silicon Nitride on it.
 * After that, deposit a layer of photoresist on top of the Silicon Nitride layer.
 * Add a protectional layer of Mask 1 on the area where the pockets are to be made. UV light is shined on top of the mask.

(Lecture Videos)

![image](https://github.com/user-attachments/assets/cc9b8631-7eb2-49d6-b854-f2a920ed42e2)

  * Wash out the unmasked area in developing solution.

(Lecture Videos)

![image](https://github.com/user-attachments/assets/9a5c5850-e216-436b-878c-9cc72817fca9)

  * Remove the Mask and etch off the Silicon Nitride that is not protected by photoresist.
  * Remove all the photoresist chemically.

![image](https://github.com/user-attachments/assets/3c0a6410-a99a-4635-8569-07c9a7f2c158)

  * Grow the Silicon di-oxide once more by placing it in an oxidation furnace. You will observe that the area under the silicon dioxide remains untouched. The area under the edges of the silicon di-oxide though, is not protected due to strong growth. This creates isolation areas between pockets. This proscess is known as LOCOS(Local oxidation of Silicon).

(Lecture Videos)

![image](https://github.com/user-attachments/assets/09c3f19b-329b-408d-b7bb-9376a3bd6c74)

![image](https://github.com/user-attachments/assets/086f7cf4-6d2d-477b-8048-8337fe6d63db)

  * The silicon Nitride is stripped using hot phosphoric acid.

(Lecture Videos)

![image](https://github.com/user-attachments/assets/795d783c-139f-43a8-8f56-651e7b5c5149) <br>
‎ 
3) N-well and P-well formation:

 * Photoresist is added again and a portion is protected by Mask 2.
	* Mask 2 in layout.
‎ 
(Lecture Videos)

![image](https://github.com/user-attachments/assets/a85fda01-3afb-4827-b19f-4e169188b302) <br>
‎ * It is again subjected to UV light and wahed. The mask is removed and then subjected to Boron in a process called ion-implantation to form a P-well.(High energy, about 200keV is needed). The oxide layer is damaged.

(Lecture Videos)
	 
![image](https://github.com/user-attachments/assets/c0e5b3e2-0c86-44be-b14a-432592052e12) <br>
‎
	* The same step is done using Mask 3 to form a N-well except this time we replace bororn with phosphorus.
 
(Lecture Videos)

![image](https://github.com/user-attachments/assets/b4e0984e-a73c-43cd-9be4-e3f279ca68a3) <br>
‎
	* We now have a N-well and a P-well.
 
(Lecture Videos)

![image](https://github.com/user-attachments/assets/aaa8bd60-f975-4adf-9bc0-fd1dd8df1e79) <br>
‎
	* We then do drive-on diffusion and diffuse the wells to about half the substrates height by placing it in a high temprature furnace.(This is known as theh twin tub proscess)
 
(Lecture Videos)

![image](https://github.com/user-attachments/assets/3d76c7aa-404c-46df-a547-1736b75b87cb) <br>

4) Formation of gate: 
	* The doping concentration and oxide capacitance impact the threshold voltage.
	* We repeat the steps done with mask 2 again with mask 4.

(Lecture Videos)

![image](https://github.com/user-attachments/assets/3e0046ae-ca90-45ae-aeef-8f1d6939cfd4) <br>
‎
	* We again put it under the effect of boron with leser energy to create a smaller P-well with doping concentration adjusted to meet requirements.

(Lecture Videos)

![image](https://github.com/user-attachments/assets/bd9bdafd-4c9d-414f-ba0a-bf9f6e46c392) <br>
‎
 	* Same thing is one with mask 5, similar to the steps done with mask 3. This time, arsenic can be used in place of phosphorus. The oxide layer will be damaged.

(Lecture Videos)

![image](https://github.com/user-attachments/assets/d0b05ff9-9b06-418b-b050-bd3c7bbf4473) <br>
‎
 	* The original damaged oxide is then etched/stripped using dilut hydrofluric (HF) solution.

(Lecture Videos)

![image](https://github.com/user-attachments/assets/19186b03-f06c-4f5e-89fd-eb1c2ca2b4c4) <br>
‎
	* Then, it is regrown to give high quality oxide. The oxide thickness (corresponding to oxide capacitance) is maintained as per required threshold voltage.

(Lecture Videos)

![image](https://github.com/user-attachments/assets/f545a9bd-9994-4e9b-a872-d2107495dfb7) <br>
‎
	* After this, a polysilicon layer is deposited on the oxide layer. We then dope it with N-type ion implants for low gate resistance.

(Lecture Videos)

![image](https://github.com/user-attachments/assets/a493d527-f9ac-4e24-a1e0-5cbc80b0550c) <br>

 * Another photoresist layer is deposited and shaped as shown in figure with the help of mask 6 (polysilicon). Then mask 6 is removed followed by the photoresist. Also given below is how mask 6 looks in a loyout.

(Lecture Videos)

![image](https://github.com/user-attachments/assets/78927ccd-0a2c-4792-ba8f-b3f608237e08) <br>

![image](https://github.com/user-attachments/assets/b7b1da0b-3e4f-4e85-83cf-ecd8a75c3bf3) <br>
	
![image](https://github.com/user-attachments/assets/1ca5a125-359b-463a-8d0c-b921d6a74f97) <br>
	
5) Lightly doped drain formation:
	* This is done mainly for 2 reasons-
		i) Hot electron effect: when we decrease the size of the chip, the electric feild value becomes high as we dont usually change the power supply. The high energy carriers break Si-Si bonds. It might also break the 2eV barrier between the Si conduction band and the Silicon-dioxide condution band and get into the oxide layer.

		ii) Short channel effect: for short channels, it becomes harder for the gate to control the source and the drain current.

	* Build a photoresist layer over the N-well using Mask 7. Then we implant phosphorus in our P-well to create a N- implant. The energy is also regulated to make sure the N-implant does  not penetrate deep into the well. Remove the photoresist.

(Lecture Videos)

![image](https://github.com/user-attachments/assets/a41f311f-7b37-4e38-b24e-5890360e22f1) <br>
‎ 
	* Do the same thing on the other side using Boron and Mask 8 to create a P- implant.

(Lecture Videos)

![image](https://github.com/user-attachments/assets/dc859621-2273-49ac-afb0-c1cac15837db) <br>
‎ 
	 * ***Side wall spacers:*** Deposit a thick layer of Silicon Nitride or Silicon di-oxide. Then perform anisotropic plasma etching. This remoed all the silicon di-oxide, oxide layer and nitride except for where the polysilicon is.

(Lecture Videos)

![image](https://github.com/user-attachments/assets/fe3e99b0-04ea-4371-876a-ccc0b6a0001e) <br>

![image](https://github.com/user-attachments/assets/eabf7c1c-9bf3-4e70-82ee-dc4a00532413) <br>
‎
 	* The side wall spacers help in making sure that some of the N- and P- dopings are preserved.
 
6) Source and drain formtion:
	* We add a thin oxide layer to prevent ions from interfering with the P-substrate (chanelling).

(Lecture Videos)

![image](https://github.com/user-attachments/assets/269c2333-b864-4cc4-9dcf-dc8e99cfc89c) <br>

 * Steps done to create ligtly dope drains are repeated.
 * The N- and P- implants are still present thanks to the polysilicon and the side wall spacers.

(Lecture Videos)

![image](https://github.com/user-attachments/assets/e6973f1f-d9d6-4dc5-9d84-6c2a953b82a2) <br>

![image](https://github.com/user-attachments/assets/b608089c-79e8-47dc-a8d0-54b6903e4b03) <br>

 * Annealing is done in high temprature. The implants will go deeper into the wells.

7) Steps to form contacts and interconnects(local)
	* Remove the thin screen oxide layer.
	* Titanium (low resistance) is deposited on the wafer surface using *sputtering*.
	* In *sputtering* titanium metal is bombarded with argon gas. The titanium atoms will get sputtered out and get deposited onto the substrate.

(Lecture Videos)

![image](https://github.com/user-attachments/assets/2479e875-19fe-4645-b03d-afed9e3d47f0) <br>
‎
 	* Wafer is heated at tempratures of 650-700°C in N<sub>2</sub> ambient for about 60s. This results in Titanium Silicon di-oxide(low resistant contacts)

(Lecture Videos)

![image](https://github.com/user-attachments/assets/27e22e87-1d13-4ec7-b548-d1a24345adf9) <br>
‎ 
	* TiN is also formed due to nitrogen ambient. It is used only for local communication.
	* Polysilicon placed with help of mask 11 as so. Mask is removed.

(Lecture Videos)

![image](https://github.com/user-attachments/assets/f5002e59-04f5-4dcc-8416-f3e7efd013ba) <br>
‎
	* Excess TiN is etched using RCA cleaning which consists of-

(Lecture Videos)

![image](https://github.com/user-attachments/assets/f8009e39-c85c-4617-b95f-f3e8e4f4c2bb) <br>

![image](https://github.com/user-attachments/assets/ebcf630a-b33c-4264-9b33-0485bd901635) <br>

8) Higher metal level formation:
	* Now we deposit a thick layer of SiO<sub>2</sub> doped with phosphorus or boron. This is done to give a protection against mobile sodium ions(phosphorus). Doping with boron reduces temperature.

(Lecture Videos)

![image](https://github.com/user-attachments/assets/90bd4ad4-f0f6-4ccf-8f7a-5f1364146278) <br>
‎
	* Chemical Mechanical Polishing(CMP) is done to flatten out wafer surface.

(Lecture Videos)

![image](https://github.com/user-attachments/assets/dbb05cde-60ba-4356-a295-0183c5b6a7f1) <br>
‎
	* Next we use mask 12 and photoresist on areas where we do not need contact holes. We also etch off the SiO<sub>2</sub> in places where we need the contact holes. We remove the photoresist.

(Lecture Videos)

![image](https://github.com/user-attachments/assets/82dcbb8a-0863-4f62-93e7-77e0b2dc5637) <br>
‎
	* We then deposit a thin layer of Titanium Nitride.
	* We then deposit a blanket tungsten layer on top of the TiN. It serves as a good interconnect between the bottom and top layers.

(Lecture Videos)

![image](https://github.com/user-attachments/assets/02359a99-db2f-4341-b6d7-ca8f9434dc52) <br>
‎
	* We again do CMP to remove excess Tungsten and planarise the wafer surface.
	* Next we add an aluminium layer to take it to the top layer. We remove unnecesarry parts using mask 13 and photoresist as shown in diagram. Plasma etch out the remaining Al. Remove photoresist.

(Lecture Videos)

![image](https://github.com/user-attachments/assets/ec1443ad-f6af-423f-ae58-7955f27b5344) <br>
‎
	* We now already have 2 matal layers. To make another one, we deposit more Silicon di-oxide and perform CMP to make the surface planar.
	* Using mask 14, define contact holes.

(Lecture Videos)

![image](https://github.com/user-attachments/assets/7d41d948-fa02-43ea-8952-2d6d89622643) <br>
‎
	* Deposit a small layer of tin, and a layer of tungsten, before removing excess using CMP.

(Lecture Videos)

![image](https://github.com/user-attachments/assets/013ddf38-32ee-4c6d-aa2a-131b4de02439) <br>
‎
	* Using mask 15, make another deposit of Al interconnects over the tungsten as shown in diagram. This layers interconnects should be thicker than the ones on the prepious layer.

(Lecture Videos)

![image](https://github.com/user-attachments/assets/2bd51079-1375-4066-aed9-ba82c121bb31) <br>
‎
	* Deposit the top layer of dielectric (Silicon Nitride or Silicon dioxide) to protect the chip.

(Lecture Videos)

![image](https://github.com/user-attachments/assets/4932d211-3282-4bf5-9bf2-330a05c4edbb) <br>

 * Finally use mask 16 to drill contat holes and bring contacts outside the chip.

(Lecture Videos)

![image](https://github.com/user-attachments/assets/94dff425-11de-475a-9d13-7de4d78da520)
