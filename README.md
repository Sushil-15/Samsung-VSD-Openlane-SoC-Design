# Samsung-VSD-Openlane-SoC-Design
> Level 3 of a course offered by VSD-IAT, in collaboration with Samsung using Openlane and the SKY 130 pdk.

## Overview of OpenLANE flow
* **Synthesis**
  * Generating gate-level netlist (yosys).
  * Performing cell mapping (abc).
  * Performing pre-layout STA (OpenSTA).
* **Floorplanning**
  * Defining the core area for the macro as well as the cell sites and the tracks (init_fp).
  * Placing the macro input and output ports (ioplacer).
  * Generating the power distribution network (pdn).
* **Placement**
  * Performing global placement (RePLace).
  * Perfroming detailed placement to legalize the globally placed components (OpenDP).
* **Clock Tree Synthesis (CTS)**
  * Synthesizing the clock tree (TritonCTS).
* **Routing**
  * Performing global routing to generate a guide file for the detailed router (FastRoute).
  * Performing detailed routing (TritonRoute)
* **GDSII Generation**
  * Streaming out the final GDSII layout file from the routed def (Magic).
(Info credit: [VSD Standard Cell Design Repo](https://github.com/nickson-jose/vsdstdcelldesign))

(Lecture Videos)

![image](https://github.com/user-attachments/assets/5c550fea-0bf8-4bb9-87f1-fbe714045e6d)

# Acknowledgements

* [Kunal Ghosh](https://github.com/kunalg123), Co-founder, VSD Corp. Pvt. Ltd.
* [Nickson P Jose](https://github.com/nickson-jose), Physical Design Engineer, Intel Corporation.
* [R. Timothy Edwards](https://github.com/RTimothyEdwards), Senior Vice President of Analog and Design, efabless Corporation.
