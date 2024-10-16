### Catkin workspace

**1. Create a Catkin Workspace**

First, navigate to the directory where you'd like to create the Catkin workspace. 
 	
```bash
mkdir -p ~/your_ws/src
cd ~/your_ws/src
```

**Note** . your_ws will be your workspace.

**2. Initialize the Workspace**

Initialize the Catkin workspace by running the following command inside the `src` subdirectory:

```bash
cd ~/your_ws/
catkin init
catkin config --extend /opt/ros/noetic
catkin config -DCMAKE_BUILD_TYPE=Release
```

**Note**. This creates the necessary `CMakeLists.txt` in the `src` directory.

**3. Download/Create Packages**

Clone or create your ROS packages inside the `src` folder of your package. For example, you can clone a package from GitHub:

 	```bash
 	git clone https://github.com/<username>/<your_package>.git
 	```

**4. Compile the Workspace**:

To compile all the packages inside your workspace, navigate to the root of the Catkin workspace (`your_ws`) and run:
 	```bash
 	catkin build
 	```
Note. When you compile the workspace, ROS will automatically create the `build` and `devel` directories.

**5. Source the Workspace**:

After building, you need to source the `setup.bash` file in the `devel` directory to make the workspace visible to ROS. Run:

```bash
 	source devel/setup.bash
 	```
**Important**: You must re-execute this command in each new terminal session where you want ROS to recognize the packages in this workspace.

**Automating the Process**:
This process can be automated, often by including the sourcing command in the `.bashrc` file. To do this, add the following line to your `~/.bashrc` file:
 	```bash
 	echo "source ~/your_ws/devel/setup.bash" >> ~/.bashrc
 	source ~/.bashrc
 	```
### (optional) may you find some errors, so you can use the "Magic" of rosdep
```bash
cd ~/your_ws/src
rosdep install --from-paths . --ignore-src --os=ubuntu:focal -r -y

```

