### Nvidia graphics driver

To check and configure a graphics card correctly on an Ubuntu 20.04 system, you can follow these steps:

**1.1: Check if you have a graphics card.**

You can use the following command to list information about your graphics card:

```bash
sudo update-pci ids
lspci | grep -i vga
```

This command will display information about your graphics card if you have one. 

> [!NOTE]
> `lspci` command lists PCI devices, including their manufacturer and model information. When you execute the `lspci | grep -i vga` command, you filter the list to display only information related to the graphics card.

If a graphics card is installed in your system, the output of the command typically looks something like this:

```
01:00.0 VGA compatible controller: NVIDIA Corporation GP106 [GeForce GTX 1060 6GB] (rev a1)
```
In this example, the manufacturer is "NVIDIA Corporation," and the model is "GeForce GTX 1060 6GB."  

**1.2. Check graphics card compatibility:**

To ensure smooth performance, verify the appropriate driver of your graphics card. 

Open a terminal and enter the following command to list available drivers:
```bash
   ubuntu-drivers devices
```

**1.3 Install graphics driver** 

If the drivers are available, install them using the `ubuntu-drivers` command (a tool for managing system-recommended drivers). Run the following commands:

```bash
sudo apt-get update
sudo apt-get install ubuntu-drivers-common
sudo ubuntu-drivers autoinstall
```

This will install the recommended drivers for your graphics card.

> [!NOTE]
> If you have an NVIDIA graphics card. You also can check that NVIDIA drivers are available for your graphics card by visiting the NVIDIA website (https://www.nvidia.com/Download/index.aspx). If needed, you might have to replace it with a supported version.

**1.4 Update NVIDIA Driver:** 

If your current NVIDIA driver version is not supported, you'll need to update it. For example, if your driver version is nvidia-driver-510-server, use the appropriate version number and execute the following commands:

 ```bash 
sudo apt install nvidia-driver-X  # Replace X with your supported version
sudo reboot
```
After installing the drivers, you can verify if the graphics card has been correctly recognized using the following command:

```bash
nvidia-smi
```

**1.5 Restart the system:**
Remember to restart your system if required after installing the drivers.

```bash
sudo reboot
```
