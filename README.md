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
