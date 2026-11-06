# Software Outline

The software folder of the Nocturna Design Package documentation organizes our software components to best highlight its architecture design while also providing descriptions of critical functions and variables as they are mapped into the ROS 2 Framework design philosophy.

## Architecture

![Architecture-Visual](../additional_documentation_NF/imgs/architecturePNG.png)

Figure: Architectural visualization of the software layers. From Li et al (2026). "Autonomous Navigation System for Library Service Robot Based on Unitree Go2 Edu."  [https://doi.org/10.48550/arXiv.2606.03340]

## Software Folder Structure

The `software_folder_` repository is organized into modular ROS 2 packages. Each functional objective is cleanly isolated into its own directory containing dedicated source code, configuration parameters, and launch files, making the project structure intuitive and easy to navigate:

```text 
software_folder_/
├── bat_SLAM/                
│   ├── CMakeLists.txt
│   ├── package.xml
│   ├── launch/
│   │   └── slam.launch.py
│   ├── config/
│   │   └── slam_params.yml
│   └── src/
│       ├── slam_manager.cpp
│       ├── map_manager.cpp
│       └── localization_manager.cpp
│
├── bat_autonomy/              
│   ├── CMakeLists.txt
│   ├── package.xml
│   ├── config/
│   │   └── autonomy_params.yaml
│   └── src/
│       ├── mission_manager.cpp
│       ├── exploration_manager.cpp
│       ├── navigation_manager.cpp
│       ├── obstacle_monitor.cpp
│       └── return_manager.cpp
│
├── bat_heatmap/             
│   ├── CMakeLists.txt
│   ├── package.xml
│   ├── config/
│   │   └── heatmap_params.yaml
│   └── src/
│       ├── heatmap_generator.cpp
│       ├── sighting_aggregator.cpp
│       └── heatmap_manager.cpp
│
├── bat_communication/        
│   ├── CMakeLists.txt
│   ├── package.xml
│   ├── config/
│   │   └── communication_params.yaml
│   └── src/
│       ├── command_handler.cpp
│       ├── status_publisher.cpp
│       └── data_interface.cpp
│
├── bat_wildlife_detection/    
│   ├── CMakeLists.txt
│   ├── package.xml
│   ├── config/
│   │   └── detection_params.yaml
│   └── src/
│       ├── wildlife_detector.cpp
│       ├── wildlife_classifier.cpp
│       ├── detection_manager.cpp
│       └── wildlife_localizer.cpp
│
├── bat_data_logging/         
│   ├── CMakeLists.txt
│   ├── package.xml
│   ├── config/
│   │   └── logging_params.yaml
│   └── src/
│       ├── data_logger.cpp
│       ├── sighting_recorder.cpp
│       └── database_manager.cpp
│       └── mission_report.cpp
│
└── system_launch/             
    ├── CMakeLists.txt
    ├── package.xml
    ├── launch/
    │   ├── system.launch.py
    │   ├── sensors.launch.py
    │   └── robot.launch.py
    └── config/
        └── system_params.yaml
```

## Project Software Packages


| Project Objective | Proposed ROS 2 Package | Core Responsibility |
| :--- | :--- | :--- |
| SLAM | `bat_SLAM` | Mapping, localization, and map management |
| Heatmap | `bat_heatmap` | Wildlife activity heatmap generation |
| Autonomy | `bat_autonomy` | Exploration, navigation, and mission planning |
| User Communication | `bat_communication` | User commands, system status, and visualization |
| Wildlife Detection & Classification | `bat_wildlife_detection` | Wildlife recognition and classification |
| Data Collection & Documentation | `bat_data_logging` | Recording, organizing, and retrieving collected data |


---


## Objective 1 – SLAM


| Proposed File | Description |
| :--- | :--- |
| `slam_manager.cpp` | Coordinates the SLAM process and monitors mapping and localization status |
| `map_manager.cpp` | Handles saving, loading, and managing previously generated maps |
| `localization_manager.cpp` | Manages localization against an existing map and reports localization status |
| `slam.launch.py` | Launches the selected SLAM software and its supporting nodes |
| `slam_params.yml` | Stores SLAM configuration parameters |


### Detailed File Breakdown


#### `slam_manager.cpp`
* **Key Functions:**
  * `initialize_slam_node()` – Sets up ROS 2 parameters, publishers, and subscribers.
  * `lifecycle_state_callback()` – Monitors active SLAM node states.
  * `publish_status()` – Broadcasts live mapping status to the system.
* **Key Variables:**
  * `bool is_localized_` – Tracks whether the rover has successfully localized.
  * `std::string current_state_` – Holds the current operating status string.


#### `map_manager.cpp`
* **Key Functions:**
  * `save_map_service_callback()` – Handles requests to save the current occupancy grid to disk.
  * `load_map_service_callback()` – Loads a previously saved map file.
* **Key Variables:**
  * `std::string map_storage_path_` – Directory path where maps are saved.
  * `nav_msgs::msg::OccupancyGrid cached_map_` – In-memory storage for the map data.


#### `localization_manager.cpp`
* **Key Functions:**
  * `particle_cloud_callback()` – Processes particle filters for localization tracking.
  * `check_convergence()` – Evaluates localization accuracy and convergence.
* **Key Variables:**
  * `double covariance_threshold_` – Error limit used to determine valid localization.
  * `int particle_count_` – Number of active particles used in the filter.


---


## Objective 2 – Autonomous Navigation


| Proposed File | Description |
| :--- | :--- |
| `mission_manager.cpp` | Coordinates autonomous missions and transitions between operating modes |
| `exploration_manager.cpp` | Selects unexplored areas for autonomous exploration |
| `navigation_manager.cpp` | Sends navigation goals to the navigation system and monitors their execution |
| `obstacle_monitor.cpp` | Monitors obstacles and navigation hazards using available sensor data |
| `return_manager.cpp` | Coordinates returning to a designated starting location or previously mapped area |
| `autonomy_params.yaml` | Stores exploration, navigation, and mission configuration parameters |


### Detailed File Breakdown


#### `mission_manager.cpp`
* **Key Functions:**
  * `state_machine_tick()` – Updates high-level mission state logic periodically.
  * `transition_to_state()` – Handles valid transitions between operating modes.
  * `emergency_stop_callback()` – Instantly halts operations upon safety triggers.
* **Key Variables:**
  * `enum MissionState` – Defines operational states (`IDLE`, `EXPLORING`, `RETURNING`, `ERROR`).
  * `MissionState current_state_` – Tracks the active state of the rover.


#### `exploration_manager.cpp`
* **Key Functions:**
  * `frontier_detection_algorithm()` – Scans the occupancy grid for unexplored boundaries.
  * `select_best_frontier()` – Evaluates and chooses the optimal target coordinate for mapping.
  * `publish_next_goal()` – Sends the chosen exploration goal to the navigation stack.
* **Key Variables:**
  * `double min_frontier_size_` – Minimum cluster size required to consider an area valid.
  * `double sensor_fov_` – Field of view parameter used during frontier evaluation.


#### `navigation_manager.cpp`
* **Key Functions:**
  * `send_goal_to_nav2()` – Interface method dispatching goals to the Nav2 action server.
  * `goal_response_callback()` – Processes acceptance or rejection of sent navigation goals.
  * `feedback_callback()` – Monitors ongoing progress and distance remaining to target.
  * `result_callback()` – Handles successful arrival or failure/recovery states.
* **Key Variables:**
  * `bool goal_in_progress_` – Flag indicating whether an active navigation goal is running.
  * `int retry_count_` – Tracks failed path planning attempts for recovery behavior.


#### `obstacle_monitor.cpp`
* **Key Functions:**
  * `pointcloud_callback()` – Processes real-time LiDAR and depth camera point cloud frames.
  * `evaluate_safety_margins()` – Checks distance between the rover and nearby obstacles.
  * `trigger_emergency_stop()` – Activates safety halts when obstacles breach thresholds.
* **Key Variables:**
  * `double min_safe_distance_` – Closest allowable approach distance (meters).
  * `bool hazard_flag_` – Boolean flag indicating an immediate collision hazard.


#### `return_manager.cpp`
* **Key Functions:**
  * `initiate_return_sequence()` – Triggers the autonomous return-to-home routine.
  * `calculate_path_home()` – Computes a safe return trajectory back to the starting point.
* **Key Variables:**
  * `geometry_msgs::msg::Pose home_pose_` – Stores the initial starting coordinates.
  * `double low_battery_threshold_` – Battery percentage trigger for automatic return.


---


## Objective 3 – Heatmap Generation


| Proposed File | Description |
| :--- | :--- |
| `heatmap_generator.cpp` | Generates wildlife activity heatmaps using recorded sighting locations |
| `sighting_aggregator.cpp` | Groups and processes wildlife detections by location and frequency |
| `heatmap_manager.cpp` | Manages heatmap generation, updates, and retrieval |
| `heatmap_params.yaml` | Defines heatmap resolution, spatial boundaries, and other settings |


### Detailed File Breakdown


#### `heatmap_generator.cpp`
* **Key Functions:**
  * `compute_kernel_density()` – Applies Gaussian kernel density estimation to sighting points.
  * `render_heatmap_image()` – Converts density calculations into a visual raster layer.
  * `publish_heatmap_grid()` – Broadcasts the generated heatmap grid to the ROS 2 network.
* **Key Variables:**
  * `double bandwidth_` – Spatial smoothing parameter for density estimation.
  * `float grid_resolution_` – Resolution of the output occupancy grid cells.


#### `sighting_aggregator.cpp`
* **Key Functions:**
  * `add_sighting_to_cluster()` – Groups incoming detection events into spatial clusters.
  * `spatial_binning()` – Aggregates wildlife sightings by geographic coordinates.
  * `get_density_metrics()` – Computes activity frequency metrics per zone.
* **Key Variables:**
  * `std::map<std::string, int> species_counts_` – Tracks cumulative counts per animal species.
  * `double clustering_radius_` – Radius threshold used to bin nearby sightings.


#### `heatmap_manager.cpp`
* **Key Functions:**
  * `update_timer_callback()` – Triggers periodic background updates for live heatmaps.
  * `export_heatmap_service_callback()` – Handles requests from the UI to retrieve map data.
* **Key Variables:**
  * `rclcpp::TimerBase::SharedPtr update_timer_` – Timer controlling refresh intervals.
  * `std::string export_path_` – File path for exporting saved heatmap artifacts.
---


## Objective 4 – User Communication


| Proposed File | Description |
| :--- | :--- |
| `command_handler.cpp` | Receives and processes user commands, such as starting or stopping a mission |
| `status_publisher.cpp` | Publishes robot status, mission progress, and system information |
| `data_interface.cpp` | Provides access to generated maps, heatmaps, and wildlife records for the user interface |
| `communication_params.yaml` | Defines communication settings and interface parameters |


### Detailed File Breakdown


#### `command_handler.cpp`
* **Key Functions:**
  * `command_subscription_callback()` – Receives incoming operator commands from the user interface.
  * `validate_command()` – Checks command parameters and permissions before execution.
  * `dispatch_command()` – Routes verified commands to the appropriate robot subsystems.
* **Key Variables:**
  * `std::string last_received_command_` – Stores the text or ID of the most recent command.
  * `bool command_lockout_` – Safety flag preventing overlapping or conflicting commands.


#### `status_publisher.cpp`
* **Key Functions:**
  * `publish_telemetry()` – Broadcasts battery levels, speed, and GPS location periodically.
  * `mission_progress_callback()` – Tracks and publishes current mission progress percentage.
  * `system_health_check()` – Monitors node statuses and broadcasts error warnings.
* **Key Variables:**
  * `rclcpp::Publisher<std_msgs::msg::String>::SharedPtr status_pub_` – Publisher handle for system status updates.
  * `double publish_frequency_` – Rate at which status messages are broadcasted (Hz).


#### `data_interface.cpp`
* **Key Functions:**
  * `serve_map_data()` – Packages and transmits occupancy grid maps to the UI dashboard.
  * `serve_heatmap_data()` – Formats activity heatmaps for frontend visualization.
  * `query_wildlife_records()` – Retrieves and forwards logged wildlife sighting records.
* **Key Variables:**
  * `std::string ui_bridge_topic_` – ROS 2 topic name used for communication with the frontend.
  * `int max_payload_size_` – Maximum data chunk size permitted for streaming records.


---


## Objective 5 – Wildlife Detection & Classification


| Proposed File | Description |
| :--- | :--- |
| `wildlife_detector.cpp` | Processes camera images to identify potential wildlife |
| `wildlife_classifier.cpp` | Determines the animal category or species using a trained classification model |
| `detection_manager.cpp` | Coordinates detection results, confidence thresholds, and event publication |
| `wildlife_localizer.cpp` | Associates wildlife detections with positions in the robot's map |
| `detection_params.yaml` | Stores model settings, confidence thresholds, and detection parameters |


### Detailed File Breakdown


#### `wildlife_detector.cpp`
* **Key Functions:**
  * `image_callback()` – Receives and processes live camera frames from the OAK-D S2 sensor.
  * `run_bounding_box_detection()` – Executes neural network inference to isolate potential targets.
  * `publish_raw_detections()` – Broadcasts detected bounding box coordinates.
* **Key Variables:**
  * `sensor_msgs::msg::Image latest_frame_` – Stores the current camera frame in memory.
  * `float detection_confidence_` – Minimum threshold required to register a detection.


#### `wildlife_classifier.cpp`
* **Key Functions:**
  * `classify_crop()` – Feeds cropped bounding box image data into the species classifier model.
  * `load_model_weights()` – Loads trained model weights from storage during initialization.
* **Key Variables:**
  * `std::string model_file_path_` – File path to the classification model weights.
  * `std::vector<std::string> class_labels_` – List of recognizable animal species categories.


#### `detection_manager.cpp`
* **Key Functions:**
  * `filter_duplicates()` – Eliminates overlapping redundant bounding box outputs.
  * `evaluate_thresholds()` – Filters results against confidence and area criteria.
  * `publish_confirmed_event()` – Broadcasts verified wildlife sighting events to the system.
* **Key Variables:**
  * `int max_detections_per_frame_` – Upper limit on processed targets per cycle.
  * `bool event_logging_active_` – Flag governing whether triggers are dispatched to the logger.


#### `wildlife_localizer.cpp`
* **Key Functions:**
  * `project_pixel_to_3d()` – Combines bounding box pixels with depth data to find 3D camera-frame coordinates.
  * `transform_to_map_frame()` – Converts local camera coordinates into global map coordinates using TF2.
* **Key Variables:**
  * `geometry_msgs::msg::Point global_sighting_pose_` – Resulting map-fixed position of detected wildlife.
  * `tf2_ros::Buffer tf_buffer_` – Transform buffer used for coordinate frame conversions.


---


## Objective 6 – Data Collection & Documentation


| Proposed File | Description |
| :--- | :--- |
| `data_logger.cpp` | Records wildlife sightings and relevant mission data |
| `sighting_recorder.cpp` | Stores detection records, images, timestamps, classifications, and locations |
| `database_manager.cpp` | Manages storage and retrieval of recorded wildlife information |
| `mission_report.cpp` | Generates summaries of completed exploration missions |
| `logging_params.yaml` | Configures data storage paths, logging settings, and recording options |


### Detailed File Breakdown


#### `data_logger.cpp`
* **Key Functions:**
  * `log_event_callback()` – Listens for incoming mission and wildlife telemetry triggers.
  * `write_to_disk()` – Streams active log data sequentially to the onboard NVMe SSD storage.
* **Key Variables:**
  * `std::string log_file_path_` – Destination directory for active mission logs.
  * `bool continuous_logging_` – Flag governing whether background session logging is enabled.


#### `sighting_recorder.cpp`
* **Key Functions:**
  * `capture_sighting_bundle()` – Packages timestamp, classification label, and GPS/map coordinates together.
  * `save_snapshot_image()` – Writes captured camera frames to persistent storage when wildlife is detected.
* **Key Variables:**
  * `std::string image_output_dir_` – Local folder path where captured animal sighting images are saved.
  * `int total_sightings_recorded_` – Counter tracking total wildlife events logged in the current session.


#### `database_manager.cpp`
* **Key Functions:**
  * `insert_record()` – Adds new wildlife sighting entries into the local database.
  * `query_database()` – Retrieves past sighting records based on time ranges or species filters.
* **Key Variables:**
  * `std::string db_connection_string_` – Connection parameters for the local database instance.
  * `bool db_initialized_` – Status flag confirming successful database connection.


#### `mission_report.cpp`
* **Key Functions:**
  * `compile_mission_summary()` – Aggregates statistics, total area covered, and total sightings at mission end.
  * `generate_pdf_report()` – Creates a formatted document outlining completed exploration metrics.
* **Key Variables:**
  * `double mission_duration_` – Total elapsed time from mission start to completion.
  * `int unique_species_count_` – Count of distinct animal types identified during the run.


---


## Supporting Packages – System Launch & Configuration


| Proposed File | Description |
| :--- | :--- |
| `system.launch.py` | Launches the complete B.A.T. Mobile software system |
| `sensors.launch.py` | Starts the required sensor drivers and interfaces |
| `robot.launch.py` | Starts the Unitree robot interface and required robot-side components |
| `system_params.yaml` | Stores shared system configuration parameters |


### Detailed File Breakdown


#### `system.launch.py`
* **Key Functions:**
  * `generate_launch_description()` – Orchestrates the startup sequence for all major software objectives and nodes.
  * `include_launch_descriptions()` – Imports and nests subsidiary launch files (sensors, robot, SLAM, autonomy).
* **Key Variables:**
  * `std::string namespace_` – Defines the shared namespace for system-wide nodes.
  * `bool enable_logging_` – Global flag controlling system-wide telemetry logging at startup.


#### `sensors.launch.py`
* **Key Functions:**
  * `start_lidar_driver()` – Initializes the LiDAR sensor driver node and pointcloud publisher.
  * `start_camera_node()` – Launches the OAK-D S2 camera driver and stereo depth processors.
  * `start_imu_driver()` – Starts the inertial measurement unit data stream.
* **Key Variables:**
  * `int sensor_publish_rate_` – Target frequency for sensor data acquisition (Hz).
  * `std::string frame_id_` – Base coordinate frame ID used for sensor transformations.


#### `robot.launch.py`
* **Key Functions:**
  * `start_unitree_interface()` – Initializes low-level communication drivers for the Unitree robot base.
  * `start_motor_controllers()` – Connects to motor interfaces (such as ZLAC8015D controllers).
* **Key Variables:**
  * `std::string robot_model_` – Identifies the active hardware platform configuration.
  * `double control_frequency_` – Core hardware feedback and command cycle rate.


#### `system_params.yaml`
* **Key Parameters:**
  * `robot_hardware_version` – Specifies the hardware revision ID for the rover.
  * `global_frame` – Defines the primary map frame (`map` vs `odom`).
  * `tf_broadcast_frequency` – Sets the rate for coordinate transform broadcasting.
 
---



## Code Attribution & Commenting Standards

> **Note on Implementation:** The templates below establish the standardized documentation and commenting conventions that all team members will enforce across repository source files. Because code development is ongoing, these structural formats are documented here as our formal project policy to ensure complete transparency regarding code provenance, authorship, and modifications.

To ensure clarity between original team work and external integrations, all source files follow these commenting rules:
1. **File Headers:** Every file includes a header block stating authorship, original source URLs (if third-party), and date of creation.
2. **Third-Party Identification:** Any code sourced from external ROS 2 packages or tutorials explicitly notes its origin in the file header.
3. **Modification Notes:** Any adapted snippet or sample code is marked with explicit inline comments detailing *how* and *why* it was modified for the B.A.T. Mobile platform.
   
* **For Team-Authored Files:**
  ```cpp
  /**
   * @file file_name.cpp
   * @brief Brief description of file responsibility.
   * @author B.A.T. Mobile Senior Design Team
   * @date YYYY-MM-DD
   * 
   * @copyright Copyright (c) 2026 B.A.T. Mobile Team. All rights reserved.
   *            This file is original work developed entirely by team members.
   */

* **For Third-Party or Adapted Files:**
  ```cpp
  /**
   * @file file_name.cpp
   * @brief Brief description of file responsibility.
   * @origin Adapted from [Name of Open-Source Project / Tutorial] (URL)
   * @modified_by B.A.T. Mobile Senior Design Team
   * @modification_date YYYY-MM-DD
   * 
   * @note THIRD-PARTY NOTICE: This file incorporates or builds upon open-source 
   *       code. See inline modification notes for specific adaptations.
   */