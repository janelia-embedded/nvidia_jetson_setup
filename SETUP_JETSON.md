# JETSON_SETUP


## JetPack

Setup host PC with Xubuntu 16.04:

<http://cdimage.ubuntu.com/xubuntu/releases/16.04/release/>

On host PC, download JetPack 3.2 (requires registration and login):

<https://developer.nvidia.com/embedded/jetpack>

```shell
cd ~/Downloads
chmod +x JetPack-L4T-3.2-x64_b196.run
./Jetpack-L4T-3.2-linux-x64_b196.run
```

Select Development Environment: Jetson TX1, TX2, or TX2i

Choose Full Install to select everything, then switch to Custom Install.

### Host - Ubuntu

Install all host options.

### Target - Jetson

Set everything to install, then set the following to "no action":

1) Compile CUDA Samples
2) VisionWorks Pack

Automatically resolve dependency conflicts.

Accept All Terms and Conditions.

Install on host PC.

Connect Jetson to the network.

Select Network Layout.

Select Network Interface on host PC.

## Jetson

### Put Jetson board in Force USB Recovery Mode

To place system in Force USB Recovery Mode:

1. Power down the device. If connected, remove the AC adapter from the
   device. The device must be powered OFF, and not in a suspend or
   sleep state.

2. Connect the Micro-B plug on the USB cable to the Recovery (USB
   Micro-B) Port on the device and the other end to an available USB
   port on the host PC.

3. Connect the power adapter to the device. Press and release power button.

4. With the system powered on:

5. Press and hold the RECOVERY FORCE button.

6. While depressing the RECOVERY FORCE button, press and release the
   RESET button.

7. Wait 2 seconds and release the RECOVERY FORCE button.

8. Press "ctrl-alt-t" to open a new terminal.

9. Type "lsusb" then press "Enter" to make sure NVidia Corp. device is listed.

10. Type "exit" then press "Enter" to close new terminal.

### Install on Jetson Board

Press enter on host PC to install on Jetson board in recovery mode.

### Jetson Setup

1. Connect a USB keyboard to the USB Type A connector of your device.

2. Connect an HDMI-compatible display to the HDMI connector on your
   device.

3. Connect the AC adapter supplied in your kit to the power connector
   of your device. Use the supplied AC adapter since it is
   appropriately rated for your device.

4. Plug the power adapter into an appropriately rated electrical
   outlet.

5. The system should power on. If not, press and release the power
   button on the device.

6. When prompted, enter nvidia for both the user name and password.

### Update and Install Xubuntu

Press <ctrl><alt>t to open a terminal in Unity.

```shell
# Update the needed repositories for the ROS on the Jetson
# Configure repositories
sudo apt-get update
sudo apt-get dist-upgrade
sudo apt-get install xubuntu-desktop
sudo apt-get purge ubuntu-desktop unity*
sudo apt-get autoremove
sudo apt-get autoclean
sudo dpkg-reconfigure tzdata
sudo hwclock -w
sudo apt-get install build-essential
sudo reboot
```

### Maximize Performance

#### Maximizing CPU/GPU

```shell
sudo ~/jetson_clocks.sh
```

#### Modify extlinux.conf

The steps above will allow you to stream your USB3 camera at a maximum image
size of 2 MB. To acquire images greater than 2 MB in resolution, add the
following to the APPEND line:

usbcore.usbfs_memory_mb=1000

to this file:

/boot/extlinux/extlinux.conf

### Arduino Interface

#### Install Arduino IDE

```shell
sudo apt-get install arduino
sudo usermod -aG dialout $USER
```

Download Linux udev rules:

<https://www.pjrc.com/teensy/td_download.html>

```shell
sudo cp ~/Downloads/49-teensy.rules /etc/udev/rules.d/
```

### Install Camera Drivers

#### Spinnaker

##### Prerequisites

```shell
sudo apt-get install libusb-1.0-0
```

##### Download Spinnaker SDK

Requires login.

<https://www.ptgrey.com/support/downloads>

Select ARM64. Save file.

```shell
mkdir ~/spinnaker
mv ~/Downloads/spinnaker* ~/spinnaker/
cd ~/spinnaker
tar -xzvf spinnaker-<version>_arm.tar.gz
cd spinnaker-<version>_arm
sudo sh install_spinnaker_arm.sh
```

#### FlyCapture

##### Prerequisites

```shell
sudo apt-get install libraw1394-11 libgtkmm-2.4-1v5 libglademm-2.4-1v5 libgtkglextmm-x11-1.2-dev libgtkglextmm-x11-1.2 libusb-1.0-0
```

##### Download FlyCapture SDK

Requires login.

<https://www.ptgrey.com/support/downloads>

Select ARM64. Save file.


```shell
mkdir ~/flycapture
mv ~/Downloads/flycapture* ~/flycapture/
cd ~/flycapture
tar -xzvf flycapture-<version>_arm.tar.gz
cd flycapture-<version>_arm
sudo ./flycap2-conf
sudo mkdir /usr/lib/flycapture
sudo cp ./lib/libflycapture* /usr/lib/
sudo mkdir /usr/include/flycapture
sudo cp -r ./include/* /usr/include/flycapture/
sudo reboot
```
