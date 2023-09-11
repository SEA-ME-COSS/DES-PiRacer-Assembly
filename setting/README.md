# Setting

---

# Piracer Assembly Guide

This document provides a basic guide on how we assembled the PiRacer. All the components were provided by [Waveshare](https://www.waveshare.com/), and this guide is based on their [PiRacer Assembly Manual](https://www.waveshare.com/wiki/PiRacer_Assembly_Manual).

## Before You Start

Before starting this guide, you will need the following components:

1. Raspberry Pi (in our case, we used the Raspberry Pi 4 Model B)
2. PiRacer kit
3. Micro SD card (with Ubuntu desktop OS installed)
4. Additional sensors or components as needed

## Assembly Steps

1. Prepare all components from the PiRacer Kit.
2. Start assembling the main body of the PiRacer. For detailed assembly process, please refer to the [Waveshare guide](https://www.waveshare.com/wiki/PiRacer_Assembly_Manual).
3. Connect the Raspberry Pi to the piracer's servo using 6pin gpio and connect all the necessary wires. Check Raspberry Pi and piracer's GPIO map.
4. Finally, insert the Micro SD card into the Raspberry Pi to complete the assembly.

## Precautions

- All assembly procedures should be done with the power off to avoid electric shock.
- Please assemble the components with care. Incorrect assembly could damage the components.
- Please keep the components safe and turn off the power when not in use.

# **Software Setting**

## Install

Tested on the following Hardware:

- Raspberry Pi 4 Model B 4GB

Tested on the following distributions:

- Rasbian Lite(Server) 64-Bit : latest version but update raspi-firmware version 5.15.84

### Download Raspbian

: **[Rpi-Imager](https://www.raspberrypi.com/software/)**

You can download Raspberry Pi Imager in link. You can choose raspbian version in imager. We choose Rasbian Lite 64-Bit.

Among the parts to be careful of, the USB erase function in Imager cannot initialize the file system, so you need to download the OS from Imager after initializing the USB with a program that initializes the file system.

### Basic Wi-Fi setting

If you know the Wi-Fi ssid and access password, you can add the following to the "wpa_supplicant.conf" file.

Change my_ssid to your wifi's ssid and my_password to your own Wi-Fi access password.

You can edit the "wpa_supplicant.conf" file with the command below.

```bash
sudo nano /etc/wpa_supplicant/wpa_supplicant.conf
```

```bash
network={
	ssid="my_ssid"
	psk="my_password"
}
```

If the ssid of my wireless router is iptime123 and the connection password is abcd1111, the final wpa_supplicant.conf file will look like the following.

```bash
pi@raspberrypi:~ $ cat /etc/wpa_supplicant/wpa_supplicant.conf
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=GB

network={
	ssid="iptime123"
	psk="abcd1111"
}
```

After confirming that the file was saved normally, reboot with the command below.

```bash
sudo reboot
```

When the reboot is complete, check if the Wi-Fi connection is normal with the command below. If the IP is assigned to the wlan0 item, it is normally connected. (IP varies depending on your router settings.)

```bash
pi@raspberrypi:~ $ ifconfig
wlan0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.1.6  netmask 255.255.255.0  broadcast 192.168.1.255
```

### **Update the Raspberrypi Firmware**

[Rpi-Firmware](https://github.com/raspberrypi/rpi-firmware)

When update firmware, you need to choose stable version. We choose 5.15.84. You can check the version in commits.

```bash
sudo apt-get update
sudo apt-get dist-upgrade
sudo reboot
sudo rpi-update (firmware_version)
sudo reboot
```

### Install Dependencies

```bash
sudo apt update
    sudo apt install \
        gcc \
        v4l-utils \
        i2c-tools \
        raspi-config \
        python3-dev \
        python3-setuptools \
        libopencv-dev \
        python3-pip \
    sudo apt upgrade
    sudo reboot
```

### Enable i2c and camera

Use the **raspi-config** tool to enable the following peripherals:

- i2c: Interface Options > I2C
- Camera: Interface Options > Camera

Afterwards, reboot:

```bash
sudo reboot
```

### Install Piracer-py package

```bash
cd ~
    git clone https://github.com/dongdongO/SEA-ME_Embedded.git
    cd DES_PiRacer-Assembly/examples

    pip install piracer-py
```

## Examples

The following examples will show the basic functionality of the piracer-py package.

We didn't use virtual environment because of the PATH issues.
