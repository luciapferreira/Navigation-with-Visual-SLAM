# Navigation with Visual SLAM

Autonomous maze navigation using ORB-SLAM3 and ROS2. A simulated robot builds a 2D occupancy grid map from a monocular camera feed using ORB-SLAM3, then navigates autonomously to goal poses using NAV2.

Built as the final project for the Bachelor's in Computer Science and Engineering at the University of Beira Interior (2024).

---

## Demo

> *Screenshots or a video recording of the simulation running would go here.*

---

## How It Works

1. **Simulation** — a differential-drive robot with a monocular camera is spawned in a Gazebo environment defined via URDF/XACRO
2. **Mapping** — ORB-SLAM3 runs in monocular mode, processing camera frames to build a sparse 3D point cloud
3. **Grid conversion** — a custom C++ node uses PCL to filter the point cloud and project it into a 2D occupancy grid map
4. **Navigation** — NAV2 uses the occupancy grid for path planning and autonomous goal navigation; `twist_mux` arbitrates between the autonomous planner and manual joystick teleoperation

---

## Repository Structure

```
├── auto_navigation/        # NAV2 configuration and navigation launch files
├── bombubi/                # Robot URDF/XACRO model and Gazebo simulation launch
├── camera_publisher/       # ROS2 node that publishes camera frames
├── camera_subscriber/      # ROS2 node that subscribes to camera frames
├── command_publisher/      # Velocity command publisher
├── command_subscriber/     # Velocity command subscriber
├── orbslam3/               # ORB-SLAM3 integration node
└── pointcloud_to_grid_map/ # C++ pipeline: point cloud → 2D occupancy grid
```

---

## Requirements

- Ubuntu 22.04
- ROS2 Humble
- Gazebo (classic)
- ORB-SLAM3 — [installation guide](https://github.com/UZ-SLAMLab/ORB_SLAM3)
- PCL (Point Cloud Library)
- NAV2
- `twist_mux`

---

## Running

Open 3 separate terminals. Source your ROS2 workspace in each.

**Terminal 1 — Launch the simulation:**
```bash
ros2 launch bombubi launch_sim.launch.py
```

**Terminal 2 — Start ORB-SLAM3 and map generation:**
```bash
ros2 launch orbslam3 pointcloud_to_gridmap.launch.py
```

**Terminal 3 — Start autonomous navigation:**
```bash
ros2 launch auto_navigation navigation_launch.py
```

> **Note:** ORB-SLAM3 requires the robot to move around the environment before the map has enough coverage for NAV2 to plan paths. Drive the robot manually with the joystick first, then set navigation goals once the map is populated.

---

## Known Limitations

- Monocular ORB-SLAM3 is sensitive to low-texture surfaces and can lose tracking in featureless areas
- The point cloud to occupancy grid conversion involves manual scale tuning — results may vary depending on the environment
- The project was developed and tested in 2024 and has not been retested since; dependency version mismatches are possible

---

## Built With

- [ROS2 Humble](https://docs.ros.org/en/humble/)
- [ORB-SLAM3](https://github.com/UZ-SLAMLab/ORB_SLAM3)
- [Gazebo Classic](https://classic.gazebosim.org/)
- [NAV2](https://navigation.ros.org/)
- [PCL](https://pointclouds.org/)
