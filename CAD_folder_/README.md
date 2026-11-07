# Nocturna — CAD Models 

## Overview

This folder contains the Computer-Aided Design (CAD) files used for the development of our project **Nocturna**, an autonomous quadrupedal robotic system designed for nighttime wildlife monitoring, environmental mapping, and wildlife activity analysis.

Nocturna utilizes the **Unitree Go2 EDU** as its primary form of mobility, integrating a **Hesai XT16 LiDAR** and a **Luxonis OAK-D Pro W stereo camera** for environmental perception.

The CAD models serve as references for component dimensions, sensor placement, mechanical integration, and the development of custom mounting solutions.

## CAD Components
- Unitree Go2 EDU
- Hesai XT16 LiDAR 
- Luxonis OAK-D Pro W 
- NVIDIA Jetson 

## Purpose of CAD Files

The CAD models in this directory support the following engineering activities:

- **Mechanical Integration:** Evaluate the placement and attachment of sensors and additional components on the Unitree Go2 EDU.
- **Sensor Mounting:** Develop mounting brackets and assemblies for the Hesai XT16 and OAK-D Pro W.
- **Spatial Analysis:** Evaluate component dimensions, clearance, and potential mechanical interference.
- **Field-of-View Evaluation:** Assess sensor positioning to minimize obstructions and improve environmental coverage.
- **Design Documentation:** Provide mechanical references for engineering design reviews, assembly planning, and future design modifications.

## File Formats

**STEP (.stp / .step)**

STEP files are standardized 3D CAD exchange files used to represent mechanical components and assemblies. These files can be imported into compatible CAD software, including FreeCAD, SolidWorks, Autodesk Fusion, and Onshape.

## Design Considerations

Mechanical integration must account for the following:

1. Sensor mounting stability during quadrupedal movement.
2. Potential interference with the robot's legs and moving joints.
3. Adequate clearance and unobstructed sensor fields of view.
4. Additional payload mass and its effect on balance and mobility.
5. Protection of sensors and electrical connections during outdoor operation.
6. Accessibility for maintenance, assembly, and future modifications.

All component dimensions, mounting interfaces, and mechanical compatibility must be verified against the physical hardware and manufacturer specifications before fabrication.

## File Management

Large CAD models are managed using **Git Large File Storage (Git LFS)** to accommodate files exceeding GitHub's standard file-size limit. 

To download the complete CAD models after cloning the repository, ensure Git LFS is installed and run:

`git lfs install`

`git lfs pull`
