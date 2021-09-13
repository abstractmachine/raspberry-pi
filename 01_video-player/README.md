# Raspberry-Video-Player
This is a bare-bones tutorial for installing a Raspberry Pi video player from scratch.

## Why Should I Care?
Sometimes you want to play a video in a loop in an exhibition, for example, and you are not using a television or beamer that allows for direct playback of your video from a USB-Key. Or perhaps you want more control over how video plays on your computer, and/or you do not like the way in which the current monitor/beamer loops your video. There are many different reasons to use a small Raspberry Pi computer to play back your video, but these are two scenarios we often encounter in the world of art & design exhibitions.

Raspberry Pi's are cheap, open-source hardware, that allow a lot of control over how your media can be played.

Since these are small computers, they might not be able to handle full 8k quality video, in which case you might have to use a larger computer, or a dedicated 4k or 8k media player that are getting cheaper and cheaper.

While there is a beginner-friendly graphical user interface for Raspberry OS, we will be using the bare-bones, console-only version called Raspberry Pi Lite. If you have ever wanted to dip your toes into how to control computers using the console, this might be a good project given that this document is based around a specific use case scenario with a limited number of steps to acheive the final goal. Based on this experience, you might feel more comfortable moving on to more complex projects using various ilk of Raspberry Pi, or other Linux-based systems.

## Hardware
We have tested this tutorial with a Raspberry PI 3b & 4b, but given that we're using only basic pre-installed functions, it should (theoretically at least) work on a standard Raspberry PI Zero.

- [Raspberry Pi Zero](https://www.raspberrypi.org/products/raspberry-pi-zero/)
- [Raspberry Pi 3A+](https://www.raspberrypi.org/products/raspberry-pi-3-model-a-plus/)
- [Raspberry Pi 4b](https://www.raspberrypi.org/products/raspberry-pi-4-model-b/)

You will also need:

- An SD card (cf. below)
- A Keyboard (during setup)

## SD Card
We start with a blank SD card for the Raspberry OS system. You will also place your video on this card in a later step. Given that we are using Raspberry OS Lite, we can get away with tiny sizes (2GB, 4GB, ...) for the SD Card. Remember to have enough space for both the OS itself and your video(s).

- SD Card Tests for Raspberry OS on Tom's Hardware
  - [Raspberry PI Micro SD Cards](https://www.tomshardware.com/best-picks/raspberry-pi-microsd-cards)
- Current cards we've been using:
  - [SanDisk MAX ENDURANCE microSDHC](https://www.amazon.fr/dp/B084CJ96GT)
  - [SanDisk Ultra MicroSDHC](https://www.amazon.fr/gp/product/B073K14CVB)

## Raspberry OS
1. Download the [Raspberry Pi Imager](https://www.raspberrypi.org/software/) to install Raspberry OS onto your SD Card. You can download and install your operating system directly from this tool:
  - <https://www.raspberrypi.org/software/operating-systems/>
2. Open the Raspberry Pi Imager software and `CHOOSE OS` to select the operating system you wish to install. We are going to choose the 'Lite' version. Select `Raspberry PI OS (other)` > `Raspberry Pi OS Lite (32-bit)`
3. Insert your blank SD Card into your computer and select it under `Storage`. Make sure it matches the size of your card (2Gb, 4Gb, 8Gb, 16Gb, 32Gb, 64Gb, etc) and that you are not erasing another external hard disk or flash card currently connected to your computer 
4. `Write` the `Raspberry Pi Lite` operating system to your card

## Turn On
- Install SD Card
- Plug in an HMDI monitor before powering on
- Power on the Raspberry computer using a compatible USB-C adapter. There is a bug with the RPI 4's USB-C power plug design, so be careful to use not just any adapter. There are cheap adapters out there
- Plug in a USB Keyboard

## Configure PI
On first boot, the PI should resize the volume to match the card size.

- login: `pi`
- password: `raspberry`

Note: on French keyboards `q` and `a` keys will be inverted. We'll fix keyboard configuration in next steps
  
Tip: if you can't see your command line because of overscan (cf. below), you can reposition your cursor to better see your login prompt. Once your computer has finished its startup sequence, type `clear` and `{enter}` a few times. We'll fix the over/underscan problem in a few steps.

### raspi-config
When you get to the command line, open `sudo raspi-config` to start configuring your installation

### Password
This password is for your Raspberry itself. You should change this base password.
- `1` System > `S3` Password > enter a unique password

### Autologin
- `1` System > `S5` Boot/Auto-login > `B2` Console Autologin

You can instead choose `B1` if you want to stick to safely logging into your Raspberry by hand, but in this case you will have to retype your password at each startup, which is not very practical for an installation.

### SSH (optional)
SSH allows you to connect to this Raspberry computer from another computer over the terminal, for example in order to copy files onto the device.

- `3` Interface > `P2` SSH > `YES` (make sure you set a good password in the previous steps)

### Wifi (optional)
If you wish to upload videos to your Raspberry over SSH, and if your Raspberry also has wifi, connect to your Wifi Router with your `login` + `password`:

- `1` System > `S1` Wireless LAN > configure your wifi

### Display
- `2` Display Options > `D2` Underscan > `YES/NO` (depending on your monitor's handling of the HDMI signal)

`Finish` to exit this menu to the command line

### Overscan Problems (optional)
If you're still having problems with overscan black lines surrounding your screen, open the nano text editor, and modify the system startup (`boot`) configuration file:

- `sudo nano /boot/config.txt`

Remove `#` symbols to make sure `#disable_overscan=0` (yes, this is the de-activation of a negative, yes all this is impossibly inelegant) and un-comment to change values of `overscan_left=40`, etc. I have sometimes used negative values in the past. More recent monitors do not usually need overscan and so you can `disable_overscan=1`.

Quit the `nano` text editor by typing the `{ctrl}` + `x` keys > `y` + `{enter}`. And then reboot from the command line with `sudo reboot now`. You might need to `clear` and `{enter}` to see your command prompt.

## Bluetooth Keyboard (optional)
I like using the Logitech K380 keyboards because they allow easy switching between various devices: Pi, Mac, Windows, iPadOS, etc. They come in pink, as well as other, less important, colors. They (finally) have all the colors in the French keyboard layout: [Logitech K380 Multi-Device Keyboard](https://www.logitech.com/fr-ch/product/multi-device-keyboard-k380?crid=27). These are very handy in complex art and design installations I do that often make it difficult to access the ports. 

To pair a bluetooth keyboard, you need to put your keyboard into pairing mode, then to go into `bluetoothctl`, `scan` the bluetooth controller for available bluetooth devices identifiers, find your device, turn off scan, and then `pair`, `trust`, and `connect` your device.

During the Logitech keyboard `pair` process you need to enter a `######` + `{enter}` keyboard combination that is random each time. Follow the instructions from the command line.

```
sudo bluetoothctl
[bluetooth]# power on
[bluetooth]# agent on
[bluetooth]# default-agent
[bluetooth]# scan on
[bluetooth]# scan off
[bluetooth]# pair XX:XX:XX:XX:XX
[bluetooth]# trust XX:XX:XX:XX:XX
[bluetooth]# connect XX:XX:XX:XX:XX
[bluetooth]# exit
```

## Copy Video to Raspberry
To be continued...

### Copy File via USB Key
Plug your USB Key into your Raspberry Pi.

To be continued...

### Copy File via SSH
To be continued...

## Video Format
Your video should be encoded in h264 format.

More info required on encoding h264 correctly... To be continued...

## Test Video Playback
Before we create an auto-start routine to automatically play the video on each startup, we should first check to see if it plays correctly on the device.

Make sure we're in the `home` folder:

```
$ cd
```

If your file is named `sequence-a.h264` and is located in your home folder, simply type the following command to play it fullscreen with automatic looping activated:

```
$ omxplayer --loop sequence-a.h264
```

If all went well you should see your video playing in a loop.

To stop playback, type `ctrl` + `c` on your keyboard.

If you want to play your video sideways for whatever reason, here is 

```
omxplayer --loop /home/pi/video.mp4 --orientation 90
```

If you want to play the video upside-down, use `--orientation 180`.

## Auto-start Your Video After System Startup
We want our video to play on its own. We are going to use `systemd` to auto-start this playing this video at each startup. `systemd` is a means for starting various automatic background or forground processes on your Raspberry. To access `systemd`, you need to creat a script and then load that script into `systemctl`.

Cf. <https://www.raspberrypi.org/documentation/linux/usage/systemd.md>

Create an `appname.service` text file using `nano` or whatever text editor you prefer, containing the following instructions. I use the command `nano appname.service` to create these types of text files directly on my Raspberry, but you can also use `ssh` from another computer if you want to use [VS Code](https://code.visualstudio.com) or whatever code editor you prefer. You can also easily create and access remote text files via the `ssh` extension for VS Code.

To be continued...