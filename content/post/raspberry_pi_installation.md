+++
author = "Neha Joshi"
title = "Raspberry pi installation"
date = 2020-05-24T18:05:08+05:30
draft = true
description = "A guide to raspberry pi installation"
tags = [
    "Raspberry Pi",
]
+++

The model of Raspberry Pi which I have is "Raspberry Pi 2 Model B". This is a pretty old model and we have many new versions of Raspberry Pi available today. I love ubuntu and since we have the "Ubuntu MATE" (which uses an extremely lightweight desktop environment) designed to be suitable for Raspberry Pi’s ARM architecture, I have installed that. Lets begin with the installation steps:
#### 1. Download the Ubuntu Mate image from the website: 
> https://ubuntu-mate.org/ports/raspberry-pi/

after checking which model of Raspberry Pi you have. Also, make sure that you download the image which is specifically for the Raspberry Pi.

#### 2. Extract the image
The downloaded package will be a .xz file. Extract the same using whichever unzip utility you have. I used 7-zip on my win-10 64-Bit setup.

#### 3. Format the USB
- [-] I would recommend minimum 32 GB of USB stick for the installation. You can later add on more USB/micro-SD card and use it as a data partition. Format this USB before you go ahead with anything. On my Win10-64 setup, I just right clicked on the USB in the explorer and selected "format" option.
- [-] In case if the USB was flashed before, then you will not be able to see its entire volume. To format such a USB properly, plug in the bootable USB drive when you running Windows and then type “diskmgmt. msc” in Run box to start Disk Management. Right click the bootable drive and select “Format”. Delete any partitions in that USB if you see those, and then proceed to format.

#### 4. Download the utility for flashing the image on your USB
Download Balena Etcher utility from the site for uyour setup:
>https://www.balena.io/etcher/

#### 5. Flash "Ubuntu MATE" image on your device
The Balena etcher utility is very simple to use.
- [-] Select the image
- [-] Select the USB where you want to flash
- [=] Select "Flash"
And you are done with the flashing!

#### 6. Boot the "Ubuntu Mate" on your Raspberry Pi
- [-] Many times people are mistaken about the step 5 above, thinking that its just burning the ISO image for installation. But the step 5 will give you a properly installed Ubuntu Mate USB, ready for booting once its inserted into the Raspberry Pi. The file system will automatically resize to occupy the unallocated space of the USB.
- [-] Go ahead and insert the USB in your Raspberry Pi. Since I had "Raspberry Pi 2 Model B", I used a USB 2.0. 
- [-] Power on the Raspberry Pi.

#### 7. Setting up the "Ubuntu Mate" on your Raspberry Pi
Now this was the most challenging step for me, since I did not have a spare keyboard at my home. I connected the HDMI cable of my smart TV and the USB mouse to the Raspberry Pi. I could see the "Ubuntu Mate" boot up and saw the desktop. Now, to do anything, I had to type on it. however, I didnt have the keyboard :D

Hacks:
- [1] I created a temporary open hotspot via my mobile and connected my Raspberry Pi to that hotspot using the mouse.
- [2] Next, I came to know this very late, but apparently, every Raspberry Pi comes with an inbuilt hostname "raspberrypi" and a pre-created user "pi". 
- [3] I connected my laptop to the same open hotspot and did an ssh to: pi@raspberrypi pwd: raspberry (you can change it later)
- [4] Next, I installed a soft keyboard on the Raspberry Pi
- [-] ==== sudo apt-get install matchbox-keyboard
- [5] The next challenge was to launch that keyboard (I didn't seem to find any way to launch it using just the mouse). So I wrote a shell script to launch it and placed it on the desktop :D
- [6] Double click on the shell script and that launched the soft keyboard! Now I had everything that I needed.
So I disconnected the Raspberry Pi from the open hotspot and re-connected it to a secure wi-fi (now I could add the password using the soft keyboard)

#### 8. Using the Raspberry Pi
I do not see much use in accessing the UI on the Raspberry Pi. But in case if you still want it, you can install the x11vnc on the Raspberry Pi, and connect to it remotely. However, I mostly ssh to the Raspberry Pi and use it.

#### 9. Mount and configure an extra data partition
- [-] Insert another USB to the Raspberry Pi which you want to use ass a data partition
- [-] You will need to partition the USB using fdisk.
- |===== 1. fdisk /dev/\<device\>
- |===== 2. To add a new partition. Type "n" and press enter.
- |===== 3. Select extended partition and add the necessary info to create it.
- |===== 4. More details of this can be found on: https://help.ubuntu.com/community/InstallingANewHardDrive 
- [-] Create mount point: I created a directory called "workspace" under /media/pi
- [-] Add an entry to the /etc/fstab, so that the device is always mounted upon reboot. My entry looked like this:
- |===== /dev/sda5     /media/pi/workspace   ext3   rw,user,auto,exec 0       0
- [-] Mount the device using "mount /media/pi/workspace". From next reboot, the space will be pre-mounted and you wont need to do this step.
- [-] Check the working of the following utilities and you will feel better equipped to do this whole step:
- |===== 1. fdisk
- |===== 2. parted
- |===== 3. lsblk

And your very own Raspberry Pi setup is ready for use! 

I am using it currently for writing Rust programs. I am teaching myself Rust :) So stay tuned for some blogs on **Rust programming** in the future!

Happy Coding!