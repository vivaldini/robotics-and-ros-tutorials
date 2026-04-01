# 2D LiDAR D500 - Setup and Usage Guide


## 1. Workspace Setup (if needed)

> [!NOTE]
> This package can be installed in **any ROS 2 workspace** (not necessarily `~/ros2_ws`).
> Just make sure that:
> - The workspace is built
> - The workspace is sourced before running ROS 2 commands

If you **do not already have a ROS 2 workspace**, create one:

```bash
mkdir -p ~/ros2_ws/src
cd ~/ros2_ws
colcon build
source install/local_setup.bash
```

> [!TIP]
>For advanced configurations (e.g., Nav2 integration), check the official repository.

##  2. Clone the Repository

Go to the src folder of your workspace and clone the package:

```bash
cd ~/ros2_ws/src/
git clone -b jazzy https://github.com/Myzhar/ldrobot-lidar-ros2.git
```

> !NOTE]
> The branch `jazzy` is used in this example. If you are working with a different ROS 2 distribution (e.g., Humble, Iron), make sure to replace the branch name accordingly.


## 3. Configure Device Permissions (udev Rules)

Run the script to allow the system to detect the LiDAR device properly:

```bash
cd ~/ros2_ws/src/ldrobot-lidar-ros2/scripts/
./create_udev_rules.sh
```

##  4. Install System Dependencies

Install required system libraries:

```bash
sudo apt install libudev-dev
cd ~/ros2_ws/
rosdep install --from-paths src --ignore-src -r -y
```

## 5. Build the Workspace

Compile the workspace:

```bash
colcon build --symlink-install --cmake-args=-DCMAKE_BUILD_TYPE=Release
```

After building, source the workspace again:

```bash
source install/local_setup.bash
```

## 6. Running the LiDAR

To launch RViz2 and visualize the LiDAR data:
```bash
ros2 launch ldlidar_node ldlidar_rviz2.launch.py
```

## 7. Troubleshooting

If the visualization does not start correctly, try the manual lifecycle steps.

### Step 1: Launch the node
```bash
ros2 launch ldlidar_node ldlidar_bringup.launch.py
```
### Step 2: Configure
```bash
ros2 lifecycle set /ldlidar_node configure
```
###Step 3: Activate
```bash
ros2 lifecycle set /ldlidar_node activate

