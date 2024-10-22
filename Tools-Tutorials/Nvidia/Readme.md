### Nvidia graphics driver

**1. Install graphics driver** 

If the drivers are available, install them using the `ubuntu-drivers` command (a tool for managing system-recommended drivers). Run the following commands:

```bash
sudo apt-get update
sudo apt-get install ubuntu-drivers-common
sudo ubuntu-drivers autoinstall
```

This will install the recommended drivers for your graphics card.

> [!NOTE]
> If you have an NVIDIA graphics card. You also can check that NVIDIA drivers are available for your graphics card by visiting the NVIDIA website (https://www.nvidia.com/Download/index.aspx). If needed, you might have to replace it with a supported version.

**2. Update NVIDIA Driver:** 

If your current NVIDIA driver version is not supported, you'll need to update it. For example, if your driver version is nvidia-driver-510-server, use the appropriate version number and execute the following commands:

 ```bash 
sudo apt install nvidia-driver-X  # Replace X with your supported version
sudo reboot
```
After installing the drivers, you can verify if the graphics card has been correctly recognized using the following command:

```bash
nvidia-smi
```

**3. Restart the system:**
Remember to restart your system if required after installing the drivers.

```bash
sudo reboot
```
