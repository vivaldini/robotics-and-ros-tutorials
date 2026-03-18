# ROS 2 Tutorial
Author: Kelen Cristiane Teixeira Vivaldini - LARIS

This repository contains a step-by-step tutorial for learning ROS 2.
The material is designed for students and beginners who want to learn how to develop robotics applications using ROS 2.

### Requirements

- Ubuntu 24.04
- ROS 2 Jazzy
- Python 3
- colcon

## Tutorial Index

```bash
ros2-tutorials/
│   01_create_workspace
└── scripts/
│   └── setup_workspace.sh
│   02_create_package.md
│   03_build_package.md
│   04_ros2_nodes.md
│   ├── python_nodes/
│   └── cpp_nodes/
│   05_publisher_subscriber.md
│   06_services.md
│   07_actions.md
│   08_launch_files.md
│   09_simulation_gazebo.md

```

## 01 - Creating a ROS 2 Workspace

A workspace is the main directory where ROS 2 packages are developed and compiled.

### Step 1 – Go to the Home Directory

```bash
cd
```

### Step 2 – Create a Workspace
Create a new workspace directory. In this tutorial, we will call it ``ros2_ws``, but you can choose any name. 

```bash
mkdir ros2_ws
```

> [!NOTE]
> The suffix ``ws`` is commonly used to indicate a workspace.

### Step 3  – Enter the Workspace
Move into the workspace directory:
```bash
cd ros2_ws
```
### Step 4 – Create the Source Folder
Inside the workspace, create the src folder. This directory will contain all your ROS 2 packages.

```bash
mkdir src
```

You can verify the contents of the workspace:

```bash
ls
```
Expected output:

```bash
src
```

### Step 5 – Build the Workspace

The command used to compile ROS 2 workspace is ``colcon build``. 

```bash
colcon build
```
> [!NOTE]
> Since there are no packages yet, it will report 0 packages:
```bash
Summary: 0 packages finished [0.44s]
```

### Step 6 – Workspace Structure
After building, new folders will be created automatically. Check them using:

```bash
ls
```
Expected structure:

```bash
build  install  log  src
```

| Folder | Description |
|--------------|-----------------------------------------------------------------------------------|
| `src`        | Contains the source code of ROS 2 packages                    |
| `build`     | Temporary files generated during compilation                  |
| `install`   | Installed packages and setup scripts used to configure the environment |
| `log`       | Log files generated during the build process |## Step 7 – Install ROS Development Tools (if necessary)

### Step 7 – Source the Workspace
Enter the ``install` directory:

```bash
cd install
```

List the files:

```bash
ls
```

Example output:

```bash
COLCON_IGNORE local_setup.bash local_setup.sh local_setup.zsh setup.bash
setup.sh setup.zsh _local_setup_util_ps1.py _local_setup_util_sh.py
```

| File | Description |
|--------------|-----------------------------------------------------------------------------------|
|` setup.bash`| Sets up the environment to use all packages installed in the workspace|
|`local_setup.bash`|Similar to setup.bash, but only applies to the current workspace without extending the global ROS environment|

Activate the workspace:
```bash
source setup.bash
```

### Step 8 – Add to .bashrc

To avoid running the source command every time, you can add it to your ``.bashrc``.
Go back to the home directory and open the ``.bashrc`` file:

```bash
cd
gedit .bashrc
```

Add the following line at the end of the file:

```bash
source ~/ros2_ws/install/setup.bash
```

Save and close the file.
Now every new terminal will automatically load your ROS 2 workspace environment.

> [!IMPORTANT]
> If you do not have the development tools installed, run:

```bash
sudo apt-get install ros-dev-tools
```


# ROS 2 Package 

In ROS 2, Python packages and C++ packages have completely different architectures.

## ROS 2  Python Package 
### Step 1 - Create the Python Package
To create a package, we use the ROS 2 command line tool.

```bash
cd ~/ros2_ws/src/
ros2 pkg create py_pkg --build-type ament_python --dependencies rclpy
```

| Element  |  Description|
|--------------|-----------------------------------------------------------------------------------|
|`ros2`|   ROS 2 command line interface|
|`pkg create `|   Creates a new package|
|`py_pkg `|   Name of the package| 
|`--build-type ament_python `|   Specifies that this is a Python package|
|--dependencies rclpy `|   Adds the ROS 2 Python client library|


> [!NOTE]
> After running the command, ROS 2 will create several files and folders. You may see a license warning: ``WARNING: No license file found``. A license is only necessary if you plan to publish your code as open source.

### Step 2  -  Open the Package in VS Code
It is recommended to open Visual Studio Code from the src folder. or other preference editor
```bash
code .
```
Opening VSCode from this location helps with:
-  autocomplete
- imports
- ROS dependencies detection

### Step 3 - Python Package Structure
Your Python package will contain the following structure:

```bash
my_py_pkg/
├── my_py_pkg
│ └── __init__.py
├── resource
├── test
├── package.xml
├── setup.cfg
└── setup.py
```

| Item          | Description                                    |
| ------------- | ---------------------------------------------- |
| `my_py_pkg/`  | Folder where your Python nodes will be written |
| `__init__.py` | Makes the folder a Python module               |
| `resource/`   | Internal ROS package information               |
| `test/`       | Unit tests for the package                     |
| `package.xml`       | This is a mandatory file for every ROS 2 package.  It contains: package name, version, description, dependencies and build type                  . |
| `setup.cfg`       | Configuration file used during installation.                     |


> [!NOTE]
>  No arquivo `package.xml` você adiconar as dependências neste arquivo, ver exemplo ``<depend>rlcpy</depend>``


### Step 4 - Build the Package
Return to the workspace root directory:

```bash
cd ~/ros2_ws

```
Then build the workspace:
```bash
colcon build

```

## Step 5 - Build a Specific Package
```bash
colcon build --packages-select py_pkg

```


## ROS 2  C++ Package 
### Step 1 - Create the C++ Package in ROS 2

To create a C++ package, we use the ROS 2 command line tool.

```bash
cd ~/ros2_ws/src/
ros2 pkg create cpp_pkg --build-type ament_cmake --dependencies rclcpp
```

| Element  |  Description|
|--------------|-----------------------------------------------------------------------------------|
|`ros2`|   ROS 2 command line interface|
|`pkg create `|   Creates a new package|
|`cpp_pkg `|   Name of the package| 
|`--build-type ament_cmake `|   Specifies that this is a C++ package|
|--dependencies rclcpp `|   Adds the ROS 2 C++ client library dependencies|


> [!NOTE]
> - After running the command, ROS 2 will create several files and folders. You may see a license warning: ``WARNING: No license file found``. A license is only necessary if you plan to publish your code as open source.
> - ``rclcpp`` allows you to write C++ code that interacts with ROS 2 functionalities, such as nodes, topics, services, and actions. You can include other dependencies.

```bash
ros2 pkg create my_cpp_pkg --build-type ament_cmake --dependencies rclcpp std_msgs
```

## Step 2  -  Open the Package in VS Code
It is recommended to open Visual Studio Code from the src folder. or other preference editor
```bash
code .
```
Opening VSCode from this location helps with:
-  autocomplete
- imports
- ROS dependencies detection

### Step 3 - C++ Package Structure
The structure of a C++ package: 

```bash
my_cpp_pkg/
├── include/
│ └── my_cpp_pkg/
├── src/
├── CMakeLists.txt
├── package.xml
```


| Folder /Files         | Description                                    |
| ------------- | ---------------------------------------------- |
| `cpp_pkg/`  |Folder where your C++ nodes will be written |
| `include/`  | Header files (`.hpp` or `.h`) |
| `src/` | C++ source files (`.cpp`)               |
| `CMakeLists.txt`   | This file defines the rules to build the package. It is standard in C++ projects using CMake.  Here you define executables, libraries, dependencies, compilation rules.    |
| `package.xml`       | This is a mandatory file for every ROS 2 package.  It contains: package name, version, description, dependencies, and build type                  . |

> [!NOTE]
>  No arquivo `package.xml` você adicionar as dependências neste arquivo, ver exemplo ``<depend>rlccpp</depend>``


### Step 4 - Build the Package

Return to the workspace root directory:

```bash
cd ~/ros2_ws
```
Then build the workspace:
```bash
colcon build
```

If necessary, you can remove the folders:
```bash
rm -r build install log
```
Then go back to the workspace root and build again.


### Step 5 - Build a Specific Package
```bash
colcon build --packages-select cpp_pkg

```



# ROS 2 Node

A node is a small program that performs a specific task inside a ROS 2 application.

Nodes can be written in different programming languages, such as Python or C++.

Each node should have a single responsibility.

Instead of writing one large program, a ROS 2 application is divided into multiple nodes that communicate with each other.

Nodes are organized inside packages.
A package by itself does nothing if it is empty.
 The application's functionality comes from the nodes implemented within the packages.
```bash
application
│
├── camera_package
│   ├── camera_driver_node
│   └── image_processing_node
│
├── motion_planning_package
│   ├── motion_planning_node
│   └── path_correction_node
│
└── hardware_control_package
    ├── motor_driver_node
    └── state_publisher_node
```

## Create a ROS 2 Python Node

### Step 1 - Go to Your Package

Go to your ROS 2 workspace and enter your package:
```bash
cd ~/ros2_ws/src/py_pkg/py_pkg
```

### Step 2 - Write a simple Node Code

Open a file:
```bash
gedit my_first_simple_node.py
```

Add:
```bash
# Import the main ROS 2 Python library
import rclpy
# Import the Node class, which allows us to create ROS 2 nodes
from rclpy.node import Node

# Define the main function for this node
def main(args=None):
    # Initialize ROS 2 communications, must be called before using any ROS functionality
    rclpy.init(args=args)
    
    # Create a Node instance with the name "py_test"
    node = Node("py_test")
    
    # Use the node's logger to print an info-level message
    node.get_logger().info("Hello World")
    
    # Keep the node alive, waiting for callbacks (if any)
    rclpy.spin(node)
    
    # Shutdown ROS 2 cleanly after spinning stops
    rclpy.shutdown()

# Standard Python idiom to call main() if this file is executed directly
if __name__ == "__main__":
    main()
```

#### Step 2.1 - Make the File Executable
```bash
chmod +x my_first_simple_node.py
```
> [!NOTE]
> This command gives execution permission to the file.
>  Without it, you cannot run the script directly using ./my_first_node.py.
> - `chmod` = change file permissions
> - ` +x` = adds executable permission


#### Step 2.2 — Run the simple Node (Direct Execution)
```bash
./my_first_simple_node.py
```
```bash
[INFO] [py_test]: Hello World
```
Stop with:
```bash
Ctrl + C
```

#### Step 2.3 - Register the simple Node in setup.py
Open:
```bash
cd ~/ros2_ws/src/py_pkg
```
Edit `setup.py`:
```bash
entry_points={
    'console_scripts': [
        'py_node = py_pkg.my_first_simple_node:main',
    ],
},
```


#### Step 2.4 - Build the Workspace
```bash
cd ~/ros2_ws
colcon build --packages-select py_pkg
```

#### Step 2.5 - Source the Workspace
```bash
source install/setup.bash
```
or

```bash
source ~/.bashrc
```

#### Step 2.6 - Run the Node with ROS 2

```bash
ros2 run py_pkg py_node
```

#### Step 2.7 - Key Concepts
> [!IMPORTANT]
> There are three different names

| Type            | Where it is defined | Example                    |
|-----------------|--------------------|-----------------------------|
| Node name       | in the code        | `Node("py_test")`           |
| File name       | file name          | `my_first_simple_node.py`   |
| Executable name | setup.py           | `py_node`                   |



### Step 3 Write an OPP Node Code

Open a file:
```bash
gedit my_first_opp_node.py
```

Open the file and add:
```bash
#!/usr/bin/env python3
import rclpy
from rclpy.node import Node


class MyNode(Node):

    def __init__(self):
        super().__init__("py_test_opp")
        self.counter_ = 0
        self.get_logger().info("Hello world")
        self.create_timer(1.0, self.timer_callback)

    def timer_callback(self):
        self.get_logger().info("Hello " + str(self.counter_))
        self.counter_ += 1

def main(args=None):
    rclpy.init(args=args)
    node = MyNode()
    rclpy.spin(node)
    rclpy.shutdown()

if __name__ == "__main__":
    main()
```

> [!NOTE]
> This version uses Object-Oriented Programming (OOP), which is the recommended approach for creating ROS 2 nodes.
> - `class MyNode(Node)`: creates a custom node class
> - `super().__init__("py_test_opp")`: defines the node name
> - `self.create_timer(1.0, callback)`: executes a function every 1 second, allowing the node to run continuously without needing loops.
> - `self.counter_`: stores internal state
> - `rclpy.spin(node)`: the node will keep running indefinitely, execute callbacks (like timers, subscribers, etc.), stop only when you press `Ctrl + C`.
> This structure makes your node modular, reusable, and scalable.

#### Step 3.1 - Make the File Executable
```bash
chmod +x my_first_opp_node.py
```
> [!NOTE]
> This command gives execution permission to the file.
>  Without it, you cannot run the script directly using ./my_first_opp_node.py.
> - `chmod` = change file permissions
> - ` +x` = adds executable permission


#### Step 3.2 — Run the Node (Direct Execution)
```bash
./my_first_opp_node.py
```
```bash
[INFO] [py_test_opp]: Hello World
```
Stop with:
```bash
Ctrl + C
```

#### Step 3.3 - Register the Node in setup.py
Open:
```bash
cd ~/ros2_ws/src/py_pkg
```
Edit `setup.py`:
```bash
entry_points={
    'console_scripts': [
        'py_node_opp = py_pkg.my_first_opp_node:main',
    ],
},
```


#### Step 3.4 - Build the Workspace
```bash
cd ~/ros2_ws
colcon build --packages-select py_pkg
```

#### Step 3.5 - Source the Workspace 
```bash
source install/setup.bash
```
or

```bash
source ~/.bashrc
```

#### Step 3.6 - Run the OPP Node with ROS 2

```bash
ros2 run py_pkg py_node
```

#### Step 3.7 - Key Concepts
> [!IMPORTANT]
> There are three different names:
> - File name: `my_first_node.py`
> - Node name: `py_test_opp`
> - Executable name: `py_node_opp`


> [!NOTE]
>  They are different and independent.

| Type            | Where it is defined | Example              |
|-----------------|--------------------|----------------------|
| Node name       | in the code        | `Node("py_test_opp")`    |
| File name       | file name          | `my_first_opp_node.py`   |
| Executable name | setup.py           | `py_node_opp`            |
