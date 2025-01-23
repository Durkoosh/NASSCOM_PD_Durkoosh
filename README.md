# NASSCOM_PD_Durkoosh
Digital VLSI SoC Design &amp; Planning - VSDIAT 2 week Course

## Table of Content 
- [Overview](#overview)
- [Prerequisite](#Prerequisite)
- [Introduction](#Introduction)
- [Day - 1 Inception of Open-Source EDA, OpenLane and Sky130 PDK](#day---1-inception-of-open-source-eda-openlane-and-sky130-pdk)
- [Day - 2 Good Floor plan Vs Bad floorplan & intro to Library Cells](#day---2-good-floorplan-vs-bad-floorplan-and-introduction-to-library-cells)
- [Day - 3 Design Library Cells using Magic Layout and Ngspice Characterization](#day--3-design-library-cell-using-magic-layout-and-ngspice-charcterization)
- [Day - 4 Pre-Layout Timing Analysis and Importance of good Clock Tree](#day-4-pre-layout-timing-analysis-and-importance-of-good-clock-tree)
- [Day - 5 Final Steps in RTL2GDS using tritonRoute and openSTA](#day-5--final-steps-in-rtl2gds-using-tritonroute-and-opensta)
- [Acknowledgement](#acknowledgement)
- [References](#references)

## Day - 1 Inception of Open-Source EDA, OpenLane and Sky130 PDK

  - To run 'picorv32a' design synthesis using OpenLANE flow and generate necessary outputs.
  - Calculate the Flop ratio.

```       
Flop Ratio = Number of D Flip Flops / Total Number of Cells
```
```
Percentage of DFF's = Flop Ratio * 100
```

#### Implementation 

Commands to invoke the OpenLANE flow and perform synthesis
 
 ```# Change directory to openlane flow directory
cd Desktop/work/tools/openlane_working_dir/openlane

# alias docker='docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) efabless/openlane:v0.21'
# Since we have aliased the long command to 'docker' we can invoke the OpenLANE flow docker sub-system by just running this command
docker

# Now that we have entered the OpenLANE flow contained docker sub-system we can invoke the OpenLANE flow in the Interactive mode using the following command
./flow.tcl -interactive

# Now that OpenLANE flow is open we have to input the required packages for proper functionality of the OpenLANE flow
package require openlane 0.9

# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis

# Exit from OpenLANE flow
exit

# Exit from OpenLANE flow docker sub-system
exit


 ```

Screenshots of the above commands ran

![Screenshot 2025-01-21 230747](https://github.com/user-attachments/assets/eaaeda12-d47b-454e-89a8-21faea1212bd)

![Screenshot 2025-01-21 230739](https://github.com/user-attachments/assets/2cac9d0b-30ad-4560-a390-5e413fefcd69)

![Screenshot 2025-01-21 233011](https://github.com/user-attachments/assets/14d1f1cc-5b61-47bd-a1e1-2fbd0e8a2d5a)

Screenshots of synthesis statistics report file with required values to calculate flop ratio.

![Screenshot 2025-01-22 000947](https://github.com/user-attachments/assets/e90fea7d-010d-4156-8449-a794a8717aef)

Calculation of Flop Ratio and DFF% from synthesis statistics report file

```
Flop Ratio = 1613/14876 = 0.108429685
```
```
Percentage of DFF's = 0.108429685 * 100 = 10.84296854%

```

## Day - 2 Good Floorplan Vs Bad Floorplan and Introduction to Library Cells

Implementation :

- Run 'picorv32a' design floorplan using OpenLANE flow and generate necessary outputs.
- Calculate the die area in microns from the values in floorplan def.
- Load generated floorplan def in magic tool and explore the floorplan.
- Run 'picorv32a' design congestion aware placement using OpenLANE flow and generate necessary outputs.
- Load generated placement def in magic tool and explore the placement.
  
```
Area of die in microns = Die width in microns * Die height in microns
```
#### - Run 'picorv32a' design floorplan using OpenLANE flow and generate necessary outputs.

Commands to invoke the OpenLANE flow and perform floorplan

```bash
# Change directory to openlane flow directory
cd Desktop/work/tools/openlane_working_dir/openlane

# alias docker='docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) efabless/openlane:v0.21'
# Since we have aliased the long command to 'docker' we can invoke the OpenLANE flow docker sub-system by just running this command
docker
```
```tcl
# Now that we have entered the OpenLANE flow contained docker sub-system we can invoke the OpenLANE flow in the Interactive mode using the following command
./flow.tcl -interactive

# Now that OpenLANE flow is open we have to input the required packages for proper functionality of the OpenLANE flow
package require openlane 0.9

# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis

# Now we can run floorplan
run_floorplan
```

Screenshot of floorplan run

![Screenshot 2025-01-22 022709](https://github.com/user-attachments/assets/6be6f95e-4919-4b08-8fce-3533b225c40d)

#### - Calculate the die area in microns from the values in floorplan def.

Screenshot of contents of floorplan def

![Screenshot 2025-01-22 085634](https://github.com/user-attachments/assets/8658e00a-a3ce-4b5d-9af7-e0e81012f549)

According to floorplan def

```
1000 Unit Distance = 1 Micron
Die width in unit distance = 660685 - 0 = 660685
Die height in unit distance = 671405 - 0 = 671405
Distance in microns = Value in Unit Distance/1000
Die width in microns = 660685/1000 = 660.685 Microns
Die height in microns = 671405/1000 = 671.405 Microns
Area of die in microns = 660.685 * 671.405 = 443587.212425 Square Microns
```


Commands to load floorplan def in magic in another terminal

```bash
# Change directory to path containing generated floorplan def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/05-01_18-38/results/floorplan/

# Command to load the floorplan def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
```

Screenshots of floorplan def in magic

![Screenshot 2025-01-22 093017](https://github.com/user-attachments/assets/44fd2ea0-37eb-4393-b645-57cfb34e41a7)

Equidistant placement of ports

![Screenshot 2025-01-22 093440](https://github.com/user-attachments/assets/811e55c3-f4e2-4159-a79d-2eb8d289eb79)

Port layer as set through config.tcl

![Screenshot 2025-01-22 093848](https://github.com/user-attachments/assets/a71f2c2a-6856-451e-9e35-052d7af337a3)

![Screenshot 2025-01-22 093905](https://github.com/user-attachments/assets/81a44d73-7937-418b-a9c1-5b62b51aa41d)


Command to run placement

```tcl
# Congestion aware placement by default
run_placement
```

Screenshots of placement run

![Screenshot 2025-01-23 215141](https://github.com/user-attachments/assets/32a32588-e71e-4868-8cbb-e3eed29040dc)

#### 5. Load generated placement def in magic tool and explore the placement.

Commands to load placement def in magic in another terminal

```bash
# Change directory to path containing generated placement def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/05-01_18-52/results/placement/

# Command to load the placement def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```

Screenshots of floorplan def in magic

![Screenshot 2025-01-23 221505](https://github.com/user-attachments/assets/bf0937f2-a310-424e-90ee-9c11779ef8b3)

Standard cells legally placed 

![Screenshot 2025-01-23 220900](https://github.com/user-attachments/assets/3c0060c8-4cd9-47e5-9a26-3161334ef9eb)

Commands to exit from current run

```tcl
# Exit from OpenLANE flow
exit

# Exit from OpenLANE flow docker sub-system
exit
```

## Day - 3 Design Library Cells using Magic Layout and Ngspice Characterization

### Implementation

- Clone custom inverter standard cell design from github repository: [Standard cell design and characterization using OpenLANE flow](https://github.com/nickson-jose/vsdstdcelldesign).
- Load the custom inverter layout in magic and explore.
- Spice extraction of inverter in magic.
- Editing the spice model file for analysis through simulation.
- Post-layout ngspice simulations.
- Find problem in the DRC section of the old magic tech file for the skywater process and fix them.

#### - Clone custom inverter standard cell design from github repository

```bash
# Change directory to openlane
cd Desktop/work/tools/openlane_working_dir/openlane

# Clone the repository with custom inverter design
git clone https://github.com/nickson-jose/vsdstdcelldesign

# Change into repository directory
cd vsdstdcelldesign

# Copy magic tech file to the repo directory for easy access
cp /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech .

# Check contents whether everything is present
ls

# Command to open custom inverter layout in magic
magic -T sky130A.tech sky130_inv.mag &
```

#### - Load the custom inverter layout in magic and explore.

Screenshot of custom inverter layout in magic

![Screenshot 2025-01-23 223808](https://github.com/user-attachments/assets/dc434c18-e4b8-49d6-aa37-f2d6e451f610)

NMOS and PMOS identified

![Screenshot 2025-01-23 223927](https://github.com/user-attachments/assets/5c880a64-8e70-45ff-8193-3e06d7447d1f)

![Screenshot 2025-01-23 224111](https://github.com/user-attachments/assets/ff1d79b9-0735-47be-9257-e5c4c646a77d)

Output Y connectivity to PMOS and NMOS drain verified

![Screenshot 2025-01-23 224242](https://github.com/user-attachments/assets/0a6fd3bf-926f-4808-99a6-26a5a5a7026d)

PMOS source connectivity to VDD (here VPWR) verified

![Screenshot 2025-01-23 224316](https://github.com/user-attachments/assets/4ea74927-04f2-4ce0-8f43-e696ad9d8752)

NMOS source connectivity to VSS (here VGND) verified

![Screenshot 2025-01-23 224348](https://github.com/user-attachments/assets/f5e864ac-81db-4008-a1f0-a846cf6dbf98)

Deleting necessary layout part to see DRC error

![Screenshot 2025-01-23 224731](https://github.com/user-attachments/assets/e74a611a-3cd7-4770-aa95-2d8c22318f46)


#### 3. Spice extraction of inverter in magic.

Commands for spice extraction of the custom inverter layout to be used in tkcon window of magic

```tcl
# Check current directory
pwd

# Extraction command to extract to .ext format
extract all

# Before converting ext to spice this command enable the parasitic extraction also
ext2spice cthresh 0 rthresh 0

# Converting to ext to spice
ext2spice
```

Screenshot of tkcon window after running above commands

![Screenshot 2025-01-23 225035](https://github.com/user-attachments/assets/213fa60a-c1b7-4200-a62f-c225586fafda)

Screenshot of created spice file

![Screenshot 2025-01-23 225143](https://github.com/user-attachments/assets/d71c4d72-d779-484c-acda-73b9c2cc2191)

#### - Editing the spice model file for analysis through simulation.
Measuring unit distance in layout grid






