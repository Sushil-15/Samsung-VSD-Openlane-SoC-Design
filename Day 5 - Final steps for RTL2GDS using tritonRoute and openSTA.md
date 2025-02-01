LAB TASKS

> (This part of the documentation has been taken from my documentation for this same course from a little earlier (https://github.com/Sushil15-ai/OpenLANE-Sky130))

1) Perform generation of Power Distribution Network (PDN) and explore the PDN layout.

Commands to perform all necessary stages up until now

```bash
cd Desktop/work/tools/openlane_working_dir/openlane
```
```bash
docker
```
```bash
./flow.tcl -interactive
```
```bash
package require openlane 0.9
```
```bash
prep -design picorv32a
```
![image](https://github.com/user-attachments/assets/7d421419-3558-4dd0-a580-59c90fc6f65a)

```bash
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs
```
```bash
set ::env(SYNTH_STRATEGY) "DELAY 3"
```
```bash
set ::env(SYNTH_SIZING) 1
```
![image](https://github.com/user-attachments/assets/a818c691-1b8b-4ad9-a4e5-447967d7f6f1)

```bash
run_synthesis
```
![image](https://github.com/user-attachments/assets/a65d2357-a50d-437e-a0dd-313580d30790)

Alternative for running floorplan
```bash
init_floorplan
place_io
tap_decap_or
```
![image](https://github.com/user-attachments/assets/a77c8c90-61a9-490f-a8ef-6fe47685e3d1)

Running placement
```bash
run_placement
```
![image](https://github.com/user-attachments/assets/924b4ef7-1132-4f8c-89e0-c18c2d597fe4)

```bash
unset ::env(LIB_CTS)
```
Running Clock Tree Synthesis (CTS)
```bash
run_cts
```
![image](https://github.com/user-attachments/assets/00632efe-e8f3-4a60-b652-98f840f84ff7)

Generate the PDN
```bash
gen_pdn 
```
![image](https://github.com/user-attachments/assets/febb1b6c-392d-45a9-a5ca-2659ee02bb84)

Commands to load PDN def in magic in another terminal

```bash
# Change directory to path containing pdn def file
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/21-01_15-14/tmp/floorplan/
```
Open in magic
```bash
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read 14-pdn.def &
```
![image](https://github.com/user-attachments/assets/4b1e2fee-907a-449d-9a02-e0f352ceb041)

![image](https://github.com/user-attachments/assets/0d7bb155-6633-4c2d-897c-333cffdd7781)


2) Perfrom detailed routing using TritonRoute and explore the routed layout.

Commands to run routing

```bash
# Check value of 'CURRENT_DEF'
echo $::env(CURRENT_DEF)

# Check value of 'ROUTING_STRATEGY'
echo $::env(ROUTING_STRATEGY)

# Command for detailed route using TritonRoute
run_routing
```
![image](https://github.com/user-attachments/assets/7302a471-0274-4d20-bc13-ff9b02a1446a)

![image](https://github.com/user-attachments/assets/23d58ea2-dad1-4504-944d-0d794695e6d0)

*Commands to load routed def in magic in another terminal*

Change directory to the one that contains the routing .def
```bash
# Change directory to path containing routed def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/21-01_15-14/results/routing/
```
Open in magic.
```bash
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.def &
```
![image](https://github.com/user-attachments/assets/ed9f0259-8f60-4ff0-b79d-44e04200cf01)

![image](https://github.com/user-attachments/assets/2f5f214c-39fd-465a-b445-422618a214dd)


3) Post-Route parasitic extraction using SPEF extractor.

*Commands for SPEF extraction using external tool (not nescessary in new openlane)*
Github link: https://github.com/HanyMoussa/SPEF_EXTRACTOR
```bash
# Change directory to the one where you have cloned the github repo
cd Desktop/work/tools/SPEF_EXTRACTOR
```
```bash
# Extract the spef
python3 main.py /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/21-01_15-14/tmp/merged.lef /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/21-01_15-14/results/routing/picorv32a.def
```

4) Post-Route OpenSTA timing analysis with the extracted parasitics of the route.

Commands to be run in OpenLANE flow to do OpenROAD timing analysis with integrated OpenSTA in OpenROAD

```tcl
# Command to run OpenROAD tool
openroad
```
Read the .lef file we have created.
```bash
read_lef /openLANE_flow/designs/picorv32a/runs/21-01_15-14/tmp/merged.lef
```
![image](https://github.com/user-attachments/assets/3f0a8003-2e65-47dc-a77e-588ee7be1640)

read the .def file we have created.
```bash
read_def /openLANE_flow/designs/picorv32a/runs/21-01_15-14/results/routing/picorv32a.def
```
![image](https://github.com/user-attachments/assets/7fccc71b-e8f7-4ed6-aa01-1f266ed0007a)

```bash
write_db pico_route.db
```
```bash
read_db pico_route.db
```
Read the verilog file created in the synthesis step.
```bash
read_verilog /openLANE_flow/designs/picorv32a/runs/21-01_15-14/results/synthesis/picorv32a.synthesis_preroute.v
```
```bash
read_liberty $::env(LIB_SYNTH_COMPLETE)
```
```bash
link_design picorv32a
```
![image](https://github.com/user-attachments/assets/b24542d6-77a0-4315-9f8b-6963a18634a1)

```bash
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc
```
![image](https://github.com/user-attachments/assets/5f525373-71af-4f7f-8108-b0672aff4707)

```bash
set_propagated_clock [all_clocks]
```
Read the spef file we just extracted(not nescessary in new openlane)
```bash
read_spef /openLANE_flow/designs/picorv32a/runs/21-01_15-14/results/routing/picorv32a.spef
```
![image](https://github.com/user-attachments/assets/3136e686-5ee2-4268-9d17-d33321c22939)

```bash
report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4
```
![image](https://github.com/user-attachments/assets/feda064d-96d5-4bd0-b376-99414d8b57ec)

Exit the openlane flow.
```bash
exit
```
![image](https://github.com/user-attachments/assets/814cc98a-6ec0-490e-9950-131dbe1ec4f4)
