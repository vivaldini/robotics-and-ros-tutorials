# ROS 2 Jazzy Jalisco

This guide describes how to build **ROS 2 Jazzy Jalisco** from source.

Official documentation: [https://docs.ros.org/en/jazzy/index.html](https://docs.ros.org/en/jazzy/index.html)

## Supported Platforms:

- [Ubuntu Linux 24.04](https://github.com/vivaldini/robotics-and-ros-tutorials/blob/main/Tools-Tutorials/Ubuntu/Ubuntu24.04.3.md)
- [Windows 10](https://docs.ros.org/en/jazzy/Installation/Alternatives/Windows-Development-Setup.html)
- [RHEL-9/Fedora](https://docs.ros.org/en/jazzy/Installation/Alternatives/RHEL-Development-Setup.html)
- [macOS](https://docs.ros.org/en/jazzy/Installation/Alternatives/macOS-Development-Setup.html)

> [!IMPORTANT] 
>### In the RMA-DC/UFSCar course, Ubuntu 24.04 will be used.

## 1. System setup

Open a terminal. You can do this by pressing `Ctrl + Alt + T` or searching for "Terminal" in your application menu.

### 1.1 Set locale
Make sure you have a locale that supports UTF-8. If you are in a minimal environment (such as a docker container), the locale may be something minimal like POSIX. We test with the following settings. However, it should be fine if you’re using a different UTF-8 supported locale.

```bash
locale  
```
If needed:

```bash
sudo apt update && sudo apt install locales
sudo locale-gen en_US en_US.UTF-8
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
export LANG=en_US.UTF-8
```
Verify settings

```bash
locale
```
### 1.2  Enable required repositories
You will need to add the ROS 2 apt repository to your system.
First, ensure that the [Ubuntu Universe repository](https://help.ubuntu.com/community/Repositories/Ubuntu) is enabled.

```bash
sudo apt install software-properties-common
sudo add-apt-repository universe
```

Install ROS 2 repository configuration:
The [ros-apt-source](https://github.com/ros-infrastructure/ros-apt-source/ packages provide keys and apt source configuration for the various ROS repositories.
Installing the ros2-apt-source package will configure ROS 2 repositories for your system. Updates to repository configuration will occur automatically when new versions of this package are released to the ROS repositories.

```bash
sudo apt update && sudo apt install curl -y
export ROS_APT_SOURCE_VERSION=$(curl -s https://api.github.com/repos/ros-infrastructure/ros-apt-source/releases/latest | grep -F "tag_name" | awk -F\" '{print $4}')
curl -L -o /tmp/ros2-apt-source.deb "https://github.com/ros-infrastructure/ros-apt-source/releases/download/${ROS_APT_SOURCE_VERSION}/ros2-apt-source_${ROS_APT_SOURCE_VERSION}.$(. /etc/os-release && echo ${UBUNTU_CODENAME:-${VERSION_CODENAME}})_all.deb"
sudo dpkg -i /tmp/ros2-apt-source.deb
```

### 1.3 Install development tools

```bash
sudo apt install python3-colcon-common-extensions
```

```bash
sudo apt update && sudo apt install -y \
  python3-flake8-blind-except \
  python3-flake8-class-newline \
  python3-flake8-deprecated \
  python3-mypy \
  python3-pip \
  python3-pytest \
  python3-pytest-cov \
  python3-pytest-mock \
  python3-pytest-repeat \
  python3-pytest-rerunfailures \
  python3-pytest-runner \
  python3-pytest-timeout \
  ros-dev-tools
```

## 2. Build ROS 2

### 2.1 Create a workspace and clone all repos:

```bash
mkdir -p ~/ros2_jazzy/src
cd ~/ros2_jazzy
vcs import --input https://raw.githubusercontent.com/ros2/ros2/jazzy/ros2.repos src

```

### 2.2 Install dependencies

ROS 2 packages are built on frequently updated Ubuntu systems. It is always recommended that you ensure your system is up to date before installing new packages.

```bash
sudo apt upgrade
```
Initialize rosdep:
```bash
sudo rosdep init
rosdep update
```
Install dependencies:
```bash
rosdep install --from-paths src --ignore-src -y --skip-keys "fastcdr rti-connext-dds-6.0.1 urdfdom_headers"
```

> [!NOTE]
> If you’re using a distribution that is based on Ubuntu (like Linux Mint) but does not identify itself as such, you’ll get an error message like Unsupported OS [mint]. In this case append --os=ubuntu:noble to the above command.

```bash
--os=ubuntu:noble
```


## 3 Environment setup - ROS 2

After building ROS 2 from source, you must properly configure your environment before using it.

- `local_setup.bash` → Loads only that specific workspace
- `setup.bash` → Loads the full environment (including underlay workspaces)

### 3.1 Ensure No Other ROS is Sourced

> [!IMPORTANT]
> ### Ensure No Other ROS is Sourced
> Before building or sourcing your workspace, verify that no other ROS version is active.

Check environment variables:

```bash
printenv | grep -i ROS
```

Check  your `.bashrc`:
```bash
grep -i ros ~/.bashrc
```
If you see something like:

```bash
source /opt/ros/`<ros_distro>/setup.bash
```
This means another ROS version is being sourced automatically.

Comment or remove that line before proceeding.


### 3.2 Build the code in the workspace

Build the code inside your workspace:

```bash
cd ~/ros2_jazzy/
colcon build --symlink-install

```
[!IMPORTANT]
 You must run `colcon build` every time you modify the source code of a package.
ROS 2 does not automatically recompile your code.
 Any changes in `.cpp`, `.py`, `CMakeLists.txt`, or `package.xml` require rebuilding the workspace.


> [!NOTE]
> If a package fails and prevents a successful build, you can skip it:

```bash
colcon build --symlink-install --packages-skip image_tools intra_process_demo
```

### 3.3 Source the Workspace (Manual Method)

To use ROS 2 Jazzy in the current terminal:

```bash
source ~/ros2_jazzy/install/local_setup.bash
```

If you open a new terminal, you must run this command again.

When sourced, ROS 2 sets environment variables such as:
- `ROS_DISTRO`
- `AMENT_PREFIX_PATH`
- `LD_LIBRARY_PATH`
- `PYTHONPATH`
**3.4 Jazzy the Default ROS Version (Optional)

If ROS 2 Jazzy is your main installation, add it to your .bashrc:


```bash
echo "source ~/ros2_jazzy/install/setup.bash" >> ~/.bashrc
source ~/.bashrc
```
Now every new terminal will automatically load ROS 2 Jazzy.
>[!NOTE]
> If you need to switch to another ROS version later, remove the line manually from `.bashrc`.
To edit `.bashrc`:

```bash
gedit ~/.bashrc
```

## 4. Test - Installation
After building and sourcing your workspace, test the installation using the demo nodes.

### Terminal 1
Open a new terminal to launch ROS, source the setup file, and then run a C++ talker:
```bash
. ~/ros2_jazzy/install/local_setup.bash
ros2 run demo_nodes_cpp talker
```
> [!IMPORTANT]
> ### Understanding the Command
`ros2 run <package_name> <node_name>`
This command corresponds to the specific application you want to run. In this example, demo_node_cpp is the package name and talker is the node name.

### Terminal 2
Open another terminal to launch ROS, source the setup file, and then run a Python listener:
```bash
. ~/ros2_jazzy/install/local_setup.bash
ros2 run demo_nodes_py listener
```
> [!TIP]
> **Expected Result**
>
> - The **talker** publishing messages
> - The **listener** receiving: `I heard: ...`
>
> This confirms that your ROS 2 installation is working correctly.

Done!! 
For detailed information, access [ROS 2 Jaszzy Jalisco](https://docs.ros.org/en/jazzy/index.html) and [Tutorials](https://docs.ros.org/en/jazzy/Tutorials.html)



