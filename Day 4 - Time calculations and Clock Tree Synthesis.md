## Section 1: Timing Modelling using Delay Tables

### Creation of lef file

We need only certain things like oundaries, power and ground rails, and the inputs and outputs datab for the PnR step that is performed by OpenLANE.
The mag file has a lot of information. Most of this information is not needed for placement and routing. therefore we create a .lef file containing only the things we need for the PnR step (boundaries, power and ground rails, and the input and output data).

However, there are certain things we need to take care of.
* The input and output ports must lie at the intersection of the horizontal and vertical tracks.
* The width of the standard cell should be in odd multiples of the horizontal track pitch.
* The height of the standard cell should be in even multiples of the vertical track pitch.

The track info will be present in `Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/openlane/sky130_fd_sc_hd/tracks.info`

(Image of tracks.info)

![image](https://github.com/user-attachments/assets/191d481f-3e07-4935-90a5-7ad10b9c5f0d)

Tracks are used in the routing stage. Routs are the leftovers of metal layers.

`grid` command syntax in tkcon
```bash
grid [xSpacing [ySpacing [xOrigin yOrigin]]]
```
Info from tracks.info
```bash
li1 X 0.23 0.46
li1 Y 0.17 0.34
```

Adjust grid according to info from tracks.info
```bash
 grid 0.46um 0.34um 0.23um 0.17um
```
New grid

![image](https://github.com/user-attachments/assets/b250a33e-c6b9-47e9-b9b0-71d337eb20e6)

There is at least 1 intersection for a port

![image](https://github.com/user-attachments/assets/5b5e0c20-ee94-4adb-b3aa-0c41e0f35250)
![image](https://github.com/user-attachments/assets/0bfeee1c-d23c-43fd-89c5-c5b3410b6358)

So the 1st requirement is satisfied (The input and output ports must lie at the intersection of the horizontal and vertical tracks).

The dimensions of the standard cell is shown as the white box.

![image](https://github.com/user-attachments/assets/bad75a11-4657-49e1-b991-f29f06ef317b)

It is clearly seen that the width of the standard cell is 3 grid boxes.
So the 2nd requirement is also satisfied (The width of the standard cell should be in odd multiples of the horizontal track pitch).

![image](https://github.com/user-attachments/assets/28877f79-5168-465d-9359-c1ec5ed93abe)

It is observed that the height of the standard cell is 8 grid boxes.
So the 3rd requirement is also satisfied (The height of the standard cell should be in odd multiples of the vertical track pitch).

Ports are defined as pins while extracting the lef file.

We insert text to name the ports by clicking text under the edit tab in magic.

![image](https://github.com/user-attachments/assets/81836680-83f6-4c97-a500-d99e46221e7c)

Set ports following the below diagram (it is already done for us) 

(Lecture Videos)

![image](https://github.com/user-attachments/assets/51ca39d9-6f2f-4cc3-9e91-b925cb0ab750)

Save the file and give it a name
```bash
save sky130_vsdinv.mag
```
![image](https://github.com/user-attachments/assets/2fdd76e7-af88-42f4-b327-15e173ddc69f)

```bash
# Open the created file
magic -T sky130A.tech sky130_vsdinv.mag &
```
![image](https://github.com/user-attachments/assets/731eee8e-84a7-4322-88c5-ff73141f33b0)

Create lef file
```bash
lef write
```
![image](https://github.com/user-attachments/assets/26ecdf98-b488-4142-afba-10f326ad993a)

On opening lef file

![image](https://github.com/user-attachments/assets/e3d750cd-e2bf-4f0b-a29a-125f9e25cc13)

![image](https://github.com/user-attachments/assets/67f9f30e-fe42-49eb-bd24-a1ef8be2b104)

### Inserting Inverter lef file in Picorv32a flow

Copy the lef file to the picorv32a src folder. This is done to ensure that all our design files are in the same folder.
```bash
cd Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign
```
![image](https://github.com/user-attachments/assets/3e10db38-72c2-478a-a99f-2466477290d7)

```bash
cp sky130_vsdinv.lef /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src
```
![image](https://github.com/user-attachments/assets/3ad810c9-6409-4e8d-b6ec-dada67b64c88)

```bash
# Copy library files to src folder
cp libs/sky130_fd_sc_hd__* ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/
```
![image](https://github.com/user-attachments/assets/e7ea30bf-d40a-4172-9f77-874b788155fe)

Modify ` /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/config.tcl` from this..

![image](https://github.com/user-attachments/assets/d260e45a-1de1-4ead-aaf8-3533ef90d827)

to this..

![image](https://github.com/user-attachments/assets/6bcce34b-fea6-4e7c-b1e9-70531af10ec3)

by adding the following.

```bash
set ::env(LIB_SYNTH) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"
set ::env(LIB_FASTEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__fast.lib"
set ::env(LIB_SLOWEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__slow.lib"
set ::env(LIB_TYPICAL) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"
```
```bash
# This tells the flow to include an extra lef
set ::env(EXTRA_LEFS) [glob $::env(OPENLANE_ROOT)/designs/$::env(DESIGN_NAME)/src/*.lef]
```
Now we run the OpenLANE flow with these variables
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
prep -design picorv32a -tag 31-01_18-27 -overwrite
# This command makes sure that future commands follow the design specifications as set in picorv32a
# The tags at the end keep all your work in a single directory instead of creating multiple directories each time you run the OpenLANE flow
```
![image](https://github.com/user-attachments/assets/f2000eb0-2ab6-436e-a5d6-b2d7b5d2a149)

```bash
# Include lefs. This isfrom the vsdstdcelldesign repo
set ::env(EXTRA_LEFS) [glob $::env(OPENLANE_ROOT)/designs/$::env(DESIGN_NAME)/src/*.lef]
```
![image](https://github.com/user-attachments/assets/012871ed-49e6-41a3-941d-b37fbe539f32)

```bash
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
```
![image](https://github.com/user-attachments/assets/45877482-4ecc-4154-a01a-68cbed08e562)

```bash
add_lefs -src $lefs
```
![image](https://github.com/user-attachments/assets/e02218d1-ed7f-4ef2-8e94-3711f1270f19)

```bash
run_synthesis
# runs the synthesis step in OpenLANE
```
![image](https://github.com/user-attachments/assets/aa3b3fc3-6926-4135-af61-6d7cb5685dde)

Instances of our custom cell

![image](https://github.com/user-attachments/assets/b6c7cbb0-8d8c-4847-9b3a-9b67551764bc)

![image](https://github.com/user-attachments/assets/7a75ac9b-ff38-4334-adcb-e2c8e68a06f8)

`tns -711.59`
`wns -23.89`
`Chip area for module '\picorv32a':` 147712.918400

![image](https://github.com/user-attachments/assets/a8a137d3-025d-48c3-8a4b-4f1b887dd6eb)

View value of `$::env(SYNTH_STRATEGY)`

```bash
echo $::env(SYNTH_STRATEGY) 
```
![image](https://github.com/user-attachments/assets/01484871-e3e3-4b7b-aa15-ba775e78aa4b)

Change variables hpoing to improve slack.
```bash
 set ::env(SYNTH_STRATEGY) 1
```
![image](https://github.com/user-attachments/assets/edd2c77b-3a31-4054-a41c-76e7ec3b895d)

```bash
set ::env(SYNTH_SIZING) 1
```
![image](https://github.com/user-attachments/assets/84bc0902-1e94-441f-a474-4c376f6cb926)

```bash
run_synthesis
# Running synthesis withchanges
```
![image](https://github.com/user-attachments/assets/40172eeb-6d8b-4323-b48a-44547e1048fe)

![image](https://github.com/user-attachments/assets/fab44ab4-1d92-442c-952a-ee4c6d49f489)

`tns -711.59`
`wns -23.89`

```bash
run_floorplan
# Runs the floorplan stepD in OpenLANE
```
![image](https://github.com/user-attachments/assets/7f253eed-ab7a-4bce-8179-4a90ee0bd319)

** ERROR!!! **

![image](https://github.com/user-attachments/assets/774efdb2-726f-456d-af74-9d307168e110)

![image](https://github.com/user-attachments/assets/4286e1ea-5267-4548-96d4-23dacc17cc2c)

Since we are facing unexpected un-explainable error while using `run_floorplan` command, we can instead use the following set of commands available based on information from `Desktop/work/tools/openlane_working_dir/openlane/scripts/tcl_commands/floorplan.tcl` and also based on Floorplan Commands section in `Desktop/work/tools/openlane_working_dir/openlane/docs/source/OpenLANE_commands.md`

![image](https://github.com/user-attachments/assets/db781ee2-6dbf-4562-a583-ac0df2af9138)

![image](https://github.com/user-attachments/assets/d6565c1d-1ee7-49df-9d9e-955407c7f5cd)

```bash
# Follwing commands are alltogather sourced in `run_floorplan` command according to `OpenLANE_commands.md`. So we wil now run them directly without making use of the `run_floorplan` command
init_floorplan
place_io
tap_decap_or
```
![image](https://github.com/user-attachments/assets/bda41909-a2c3-4ec2-8ab1-cdf04baa0093)

![image](https://github.com/user-attachments/assets/a2ddf435-5a89-43fd-8194-18e5dce49071)

![image](https://github.com/user-attachments/assets/84f9b029-ed4b-4d4c-824f-466daf73a22b)

```bash
run_placement
# Runs the placement step in OpenLANE
```
![image](https://github.com/user-attachments/assets/c762eb91-eab1-41ca-bcd8-7728306f0634)

![image](https://github.com/user-attachments/assets/5fcc8bee-6712-43c7-bc48-4558d4993cff)

Now we have to check weather or cell has been included in the flow.

```bash
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/31-01_18-27/results/placement/
```
![image](https://github.com/user-attachments/assets/30a8f87b-21ba-4860-baee-8c69d6a94d05)

Load it in magic
```bash
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```
![image](https://github.com/user-attachments/assets/e8bd1bd6-ca96-46f4-a173-be0faec3f22c)

![image](https://github.com/user-attachments/assets/a67d46c0-81e0-4f9d-b8ae-c64880c159f9)

Our cell

![image](https://github.com/user-attachments/assets/b5e341bb-a4a9-426b-b7ee-81608d1eafaa)

After running `expand` in tkcon

![image](https://github.com/user-attachments/assets/5aef22c1-4295-4c2b-ab12-503670e11e76)

### Running STA

Create new file in OpenLANE directory named `pre_sta.conf`

```
set_cmd_units -time ns -capacitance pF -current mA -voltage V -resistance kOhm -distance um

read_liberty -max /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/sky130_fd_sc_hd__slow.lib

read_liberty -min /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/sky130_fd_sc_hd__fast.lib

read_verilog /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/31-01_18-27/results/synthesis/picorv32a.synthesis.v

link_design picorv32a

read_sdc /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/my_base.sdc

report_checks -path_delay min_max -fields {slew trans net cap input_pin}
report_tns
report_wns
```
Create `my_base.sdc` for STA analysis in `openlane/designs/picorv32a/src` directory based on the file `openlane/scripts/base.sdc`

```
set ::env(CLOCK_PORT) clk
set ::env(CLOCK_PERIOD) 24.73
#set ::env(SYNTH_DRIVING_CELL) sky130_vsdinv
set ::env(SYNTH_DRIVING_CELL) sky130_fd_sc_hd__inv_8
set ::env(SYNTH_DRIVING_CELL_PIN) Y
set ::env(SYNTH_CAP_LOAD) 17.653
set ::env(IO_PCT) 0.2
set ::env(SYNTH_MAX_FANOUT) 6

create_clock [get_ports $::env(CLOCK_PORT)]  -name $::env(CLOCK_PORT)  -period $::env(CLOCK_PERIOD)
set input_delay_value [expr $::env(CLOCK_PERIOD) * $::env(IO_PCT)]
set output_delay_value [expr $::env(CLOCK_PERIOD) * $::env(IO_PCT)]
puts "\[INFO\]: Setting output delay to: $output_delay_value"
puts "\[INFO\]: Setting input delay to: $input_delay_value"

set_max_fanout $::env(SYNTH_MAX_FANOUT) [current_design]

set clk_indx [lsearch [all_inputs] [get_port $::env(CLOCK_PORT)]]
#set rst_indx [lsearch [all_inputs] [get_port resetn]]
set all_inputs_wo_clk [lreplace [all_inputs] $clk_indx $clk_indx]
#set all_inputs_wo_clk_rst [lreplace $all_inputs_wo_clk $rst_indx $rst_indx]
set all_inputs_wo_clk_rst $all_inputs_wo_clk

# correct resetn
set_input_delay $input_delay_value  -clock [get_clocks $::env(CLOCK_PORT)] $all_inputs_wo_clk_rst
#set_input_delay 0.0 -clock [get_clocks $::env(CLOCK_PORT)] {resetn}
set_output_delay $output_delay_value  -clock [get_clocks $::env(CLOCK_PORT)] [all_outputs]

# TODO set this as parameter
set_driving_cell -lib_cell $::env(SYNTH_DRIVING_CELL) -pin $::env(SYNTH_DRIVING_CELL_PIN) [all_inputs]
set cap_load [expr $::env(SYNTH_CAP_LOAD) / 1000.0]
puts "\[INFO\]: Setting load to: $cap_load"
set_load  $cap_load [all_outputs]
```
Now with these 2 files run STA

Open a new terminal
```bash
cd Desktop/work/tools/openlane_working_dir/openlane
```
Run the sta according to the details given in the file pre_sta.conf
```bash
sta pre_sta.conf
```
![image](https://github.com/user-attachments/assets/f7b6be40-87ae-49ba-9571-45782f50417d)

![image](https://github.com/user-attachments/assets/e577e3c1-711d-419f-ae6a-942eefa104ef)

