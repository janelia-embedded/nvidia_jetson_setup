# JETSON_SETUP TX1


## JetPack

Setup host PC with Xubuntu 14.04:

<http://cdimage.ubuntu.com/xubuntu/releases/14.04/release/>

On host PC, download JetPack 3.1 (requires registration and login):

<https://developer.nvidia.com/embedded/jetpack>

```shell
cd ~/Downloads
chmod +x JetPack-L4T-3.1-x64.run
./Jetpack-L4T-3.1-linux-x64.run
```

Select Development Environment: Jetson TX1

Choose Custom Install.

### Host - Ubuntu

Make sure VisionWorks and OpenCV for Tegra are not installed.

### Target - Jetson TX1

Set everything to install, then set the following to "no action":

1) Compile CUDA Samples
2) PerfKit
3) TensorRT
4) Multimedia API package
5) cuDNN Package
6) OpenCV for Tegra
6) VisionWorks Pack

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

#### Build Kernel and ttyACM Module

```shell
cd ~/git
git clone https://github.com/jetsonhacks/buildJetsonTX1Kernel.git
cd buildJetsonTX1Kernel
./getKernelSources.sh
```

In the kernel configuration editor:

Double-Click General Setup > Local version - append to kernel release

Type: ACM

Click Edit > Find

Type: ACM

Click 'USB Modem (CDC ACM) support' until it becomes a check mark.

Click File > Save

Click File > Quit

```shell
cd ~/git/buildJetsonTX1Kernel
./makeKernel.sh
./copyImage.sh
sudo rm /usr/src/source_release.tbz2
sudo rm -rf /usr/src/kernel
sudo reboot
```

### Install Camera Driver Spinnaker SDK

#### Prerequisites

```shell
sudo apt-get update
sudo apt-get install build-essential
```

##### Ubuntu 16.04

```shell
sudo apt-get install libusb-1.0-0
```

#### Download Spinnaker SDK

<https://www.ptgrey.com/support/downloads>

Select ARM64.


```shell
mkdir ~/spinnaker
mv ~/Downloads/spinnaker* ~/spinnaker/
cd ~/spinnaker
tar xvfz spinnaker-<version>_arm.tar.gz
cd spinnaker-<version>_arm
sudo sh install_spinnaker_arm.sh


sudo ./flycap2-conf
sudo cp ./lib/libspinnaker* /usr/lib/
sudo mkdir /usr/include/spinnaker
sudo cp -r ./include/* /usr/include/spinnaker/
sudo reboot
```
