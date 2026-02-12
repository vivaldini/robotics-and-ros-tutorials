# Configure your ROS Environment for ROS Noetic

You can follow the installation instructions:

## 1. Install the Robot Operating System (Noetic)

1.1 Open a terminal. You can do this by pressing `Ctrl + Alt + T` or searching for "Terminal" in your application menu.

1.2 Use the following commands to add the ROS repository and install the full desktop version:

```bash
curl https://ctu-mrs.github.io/ppa-unstable/add_ros_ppa.sh | bash

sudo apt install ros-noetic-desktop-full
```

Now that you have installed the ROS Noetic, configure your environment.


## 2. Initialize ROS Environment

In the terminal, run the following command to initialize the ROS environment:

```bash
source /opt/ros/noetic/setup.bash
```

> [!NOTE]
> This command will load the necessary environment variables for ROS Noetic.

> [!WARNING]
> You must run this command in every new shell you open to access the ROS commands unless you add this line to your ".bashrc." 

```bash
echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc
```

### Check `.bashrc` 
To open and check/edit your `.bashrc` file manually, you can use any text editor and add the command

```bash
gedit ~/.bashrc
```

Add the following line to the end of the file:

```bash
source /opt/ros/noetic/setup.bash
```

Save the file and exit the text editor.

Make sure you have ROS Noetic installed on your system. Check your version in a terminal.

```bash
rosversion -d
```

## 3. Set up your Catkin Workspace

You can use the Catkin build system to manage packages in ROS Noetic. To create a Catkin workspace, follow this [tutorial](https://github.com/vivaldini/robotics-and-ros-tutorials/edit/main/Tools-Tutorials/catkin-configure/README.md).


## 4. Create a ROS Package

To start developing with ROS Noetic, you can create a package within your workspace. Replace `my_package` with the name of your package:

```bash
cd ~/your_ws/src
catkin_create_pkg my_package std_msgs rospy roscpp
```

In this example, `std_msgs`, `rospy`, and `roscpp` are package dependencies. 

> [!WARNING]
> Make sure to add any other dependencies your package may have.


## 5. Build the Workspace

After creating your package, compile the workspace using Catkin:

```bash
cd ~/your_ws
catkin build
```


Done!! 
For detailed information, access ROS Noetic and Catkin documentation [ROS Noetic Tutorials](http://wiki.ros.org/noetic/Tutorials)

