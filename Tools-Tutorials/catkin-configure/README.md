# Catkin workspace

Ensure that `catkin` is installed. You can install it using the following command:

```bash
   sudo apt-get install python3-catkin-tools
```

## 1. Create a Catkin Workspace

First, navigate to the directory where you'd like to create the Catkin workspace. 

> [!NOTE]
> You can use any name for your workspace; just replace 'your_ws' with 'your_workspace_name' in the commands. For example, if you want to name your workspace 'my_workspace,' you would replace 'your_ws' with 'my_workspace' in the commands.
>
> 
```bash
mkdir -p ~/your_ws/src
cd ~/your_ws/src
```


## 2. Initialize the Workspace

Initialize the Catkin workspace by running the following command inside the `src` subdirectory:

```bash
cd ~/your_ws/
catkin init
catkin config --extend /opt/ros/noetic
catkin config -DCMAKE_BUILD_TYPE=Release
```

> [!NOTE]
> This creates the necessary `CMakeLists.txt` in the `src` directory.

## 3. Download/Create Packages

Clone or create your ROS packages inside your package's `src` folder. For example, you can clone a package from GitHub:

```bash
git clone https://github.com/<username>/<your_package>.git
```

## 4. Compile the Workspace

To compile all the packages inside your workspace, navigate to the root of the Catkin workspace (`your_ws`) and run:
```bash
catkin build
```

> [!NOTE]
> When you compile the workspace, ROS will automatically create the `build` and `devel` directories.
> You can also use `catkin_make` here if you prefer. Check the better command for you (packages used).

## 5. Source the Workspace

After building, you need to source the `setup.bash` file in the `devel` directory to make the workspace visible to ROS. Run:

```bash
 	source devel/setup.bash
```
> [!Important]
> You must re-execute this command in each new terminal session where you want ROS to recognize the packages in this workspace.

## 6. Environment package Initialization

To automatically initialize the ROS environment every time you open a new terminal, add the following line to the end of your `~/.bashrc` file:

```bash
echo "source ~/your_ws/devel/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

## (optional) may you find some errors, so you can use the "Magic" of rosdep

```bash
cd ~/your_ws/src
rosdep install --from-paths . --ignore-src --os=ubuntu:focal -r -y
source ~/.bashrc
```

