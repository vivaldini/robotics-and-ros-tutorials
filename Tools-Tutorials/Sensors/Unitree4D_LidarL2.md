#  Unitree4D LiDAR L2

Guide for setting up and running the Unitree L2 LiDAR with ROS 2.

## 📖 Table of Contents
- [1. Initial Setup](#1-initial-setup)
- [2. Point-LIO ROS 2 Package](#2-point-lio-ros-2-package)


## Features
- Ethernet configuration for LiDAR communication
- SDK build instructions
- ROS 2 driver setup
- Integration with Point-LIO (SLAM)

## References
- [Download Unitree L2](https://www.unitree.com/download/L2)
- [Unitree github page](https://github.com/unitreerobotics/unilidar_sdk2)


## 1.Initial Setup

### 1.1. Configure the Ethernet Interface

To configure the Unitree L2 LiDAR, follow these steps:

#### Step 1.1.1. Check network interfaces
        
        ```bash
        ip a
        ```
#### Step 1.1.2. Identify your Ethernet interface

Example: enp44s0

#### Step 1.1.3. Create a Netplan configuration file
        
        ```bash
        sudo gedit /etc/netplan/99-lidar-config.yaml
        ```

Add the following content (replace with your interface name):
        
        ```bash
        network:
          version: 2
          ethernets:
            enp44s0: # <-- Change to your correct interface name!
              dhcp4: false
              addresses:
                - 192.168.1.2/24
              routes:
                - to: default
                  via: 192.168.1.1
        ```
        
#### Step 1.1.4. Set file permissions
        
        ```bash
        sudo chmod 644 /etc/netplan/99-lidar-config.yaml
        ```

#### Step 1.1.5 Apply configuration
        
        ```bash
        sudo netplan apply
        ```
        
#### Step 1.1.6 Test communication with the LiDAR
        
        ```bash
        ping 192.168.1.62
        ```
        
### 1.2. Workspace Setup (if needed)

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
> For advanced configurations (e.g., Nav2 integration), check the official repository.

### 1.3. Clone the Repository

Go to the src folder of your workspace and clone the package (ROS2 do GitHub da Unitree):

	```bash
	cd ~/ros2_ws/src/
	git clone https://github.com/unitreerobotics/unilidar_sdk2
	```

### 1.4. Install System Dependencies

Install required system libraries:

	```bash
	sudo apt update
	sudo apt install libpcl-dev
	rosdep install --from-paths src --ignore-src -r -y
	```

### 1.5. Build the SDK

Compile the workspace:

```bash
	cd ~/ros2_ws/src/unilidar_sdk2/unitree_lidar_sdk
	mkdir -p build
	cd build
	cmake .. && make -j2
```
Remove the ROS1 package (important):
	
```bash
	cd ~/ros2_ws/src/unilidar_sdk2
	rm -rf unitree_lidar_ros
```

### 1.6 Build the ROS 2 Workspace

```bash
	cd ~/ros2_ws/
	colcon build --symlink-install --cmake-args=-DCMAKE_BUILD_TYPE=Release
	source install/local_setup.bash
```


### 1.7. Test the SDK:
    
```bash
    ../bin/example_lidar_udp
```
    
Expected output:

```bash
 [UDPHandler] create udp socket success.
[UDPHandler] bind udp port success. port 6201.
[UDPHandler] set udp recv timeout success. 1 sec.
Unilidar initialization succeed!
set Lidar work mode to: 0
```

    
### 1.8 Run the ROS 2 Driver
    
```bash
    source install/setup.bash
    ros2 launch unitree_lidar_ros2 launch.py
```
    

## 2. Pacote Point Lio ROS2

The package **Point-LIO: Robust High-Bandwidth Lidar-Inertial Odometry** has a ROS 2 adaptation for the Unitree L2.


### Step 2.1. Install Dependencies
> [!NOTE]
> Replace jazzy with your ROS 2 distribution if needed)
    
```bash
    sudo apt-get install ros-jazzy-pcl-ros
    sudo apt-get install ros-jazzy-pcl-conversions
    sudo apt-get install ros-jazzy-visualization-msgs
```
    
### Step 2.2. Clone and Build
 

```bash
    	cd ~/ros2_ws/src/
        git clone https://github.com/dfloreaa/point_lio_ros2.git
        cd ~/ros2_ws
        colcon build --symlink-install
        source install/setup.bash
```
    
### Step 2.3. Run the System

Start the LiDAR first:

```bash
    cd ~/ros2_ws/src/
    source install/setup.bash
    ros2 launch unitree_lidar_ros2 launch.py
```
Then run Point-LIO:
    
```bash
    cd ~/ros2_ws/src/
    source install/setup.bash
    ros2 launch point_lio mapping_unilidar_l2.launch.py
```
