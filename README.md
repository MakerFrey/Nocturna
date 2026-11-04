![Custom Nocturna Project Logo](docs/imgs/NocturnaPNG.png)

# Nocturna Robot Platform

Project Repository for Nocturna, a student-driven UTSA ECE Senior Design team project. Nocturna will be configured for fully autonomous outdoor navigation in ultra low-light environments with a wildlife detection and classification pipeline for building time-stamped wildlife population heatmaps. The Nocturna worked on in this repository is built on the **Unitree Go2 EDU Plus Hesai XT16** quadruped robot with additional changes made to the robot for the target environment. These additions will include ultra low light stereo vision module, an additional Jetson Orin compute module with 8 GB of unified memory, and a fitted platform for configuring the additional sensors and modules. 


##  Preview

<!-- For images produced from this project -->

##  Hardware Requirements

<!-- For hardware required for this project -->

##  Software Requirements

<!-- For software required for this project -->


## Structure

```text
Nocturna/
├── .gitignore/
├── .vscode/
├── docs/
|   ├── architecture/ 
|   ├── bom/
|   └── imgs/
├── README.md
├── CHANGLELOG.md
└── nocturna/  
`   ├── README.md            
    ├── hw/ 
    |   ├── cad/
    |   |   ├── sensors/
    |   |   └── unitree/
    |   └── electrical/
    |       ├── pcb/
    |       └── schematic/
    └── sw/ 
        ├── ground_station/
        |   ├── client/
        |   └── foxglove/
        ├── sim/
        |   ├── isaac/
        |   └── vision/
        └── src/
            ├── autonomy/
            ├── common/
            └── vision/
```

##  Workflow

<!-- To-Do After Further Along the Expected Work -->

## Authors 

*    [Nicolas Frey](https://github.com/MakerFrey)
*    [Derrick Tong](https://github.com/DerrickT0ng)
*    [Lawrence Lee](https://github.com/Lawrence850479)
*    [Roberto Navajiro](https://github.com/robertonava08)
*    [Jack Crawford](https://github.com/jcrawford14200)
