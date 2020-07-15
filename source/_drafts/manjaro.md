---
title: Manjaru Architect
date: 2020-05-17 18:43:00
updated: 2020-05-29 22:07:00
categories:
draft: true
  - Tutorials
tags:
  - terminal
  - linux distro
---

# Manjaro Architect

##  Intro

Manjaro-Architect 1.3k is a CLI (or actually TUI) net-installer, which means it does not need or provide a (real) graphical interface but uses a console or terminal menu to download all packages for the target system from the internet during installation rather than extracting a compressed ISO image.


Compared to traditional unpack-installation with a graphical installer like Calamares this has some apparent advantages:


  -  The install media can be very small. In this case the download is only around 500MB. Also since no video-drivers are needed for the text-only environment it is unlikely that you will run into problems booting the M-A ISO on any hardware.
  -  The packages installed to the resulting target will be the latest available on the chosen branch and no matter how old your install media might be, you will not need to update the fresh install.
  -  Like that the manjaro-architect ISO will basically never be outdated. Even the installer itself will be updated to its current version when you start the launcher.
  -  The same install media can be used to install any desktop environment - even those DEs and WMs not maintained as an edition by Manjaro.
  -  Options for customization are basically unlimited. You are free to choose your kernel(s), drivers, desktop environment (or none) and any other packages and you will have many more and very detailed ways of configuring the target install to your liking.
  -  At the same time, if you prefer, you can use mhwd’s automatic driver installation and access the pre-configured iso-profiles of Manjaro’s supported editions to install an environment of identical configuration with what you would get by unsquashing one of our ISOs.

On the other hand, installation with this tool

  -  requires more time for configuration and download of packages.
  -  requires some understanding of a Linux system and you will need to make several decisions on your own rather than someone else has made them for you beforehand.
  -  does not provide a live environment of the resulting deskop. You will see what you get when you get it - WYNSIWYG


##  Install preconfigured Manjaro-bspwm

Once you have booted the M-A ISO you will be greeted by this login screen


![shot1](https://i.imgur.com/hrcPBEZ.png)


You can login as user manjaro or as root, in any case using password manjaro.
Then start the launcher by typing: setup


Now manjaro-architect-launcher will look for an active internet connection and then download and install the latest available version of the installer. If no network is available you will land on this screen with nmtui where you have the opportunity to connect:


![shot2](https://i.imgur.com/4L7mw6q.png)


After that you can choose between several languages for the installer. Some translations are not complete, yet, but in theory, these are available at the moment:
Brasilian Portuguese, Danish, Dutch, English, French, German, Hungarian, Italian, Polish, Portuguese, Russian and Spanish.

Some more system checks in the background and you will reach the main menu:

![shot3](https://i.imgur.com/k6NJE9q.png)


where our first required step will be:


### Prepare Installation

![shot4](https://i.imgur.com/tCmxzYG.png)


### Set Virtual Console

At this point M-A will already have chosen a keyboard layout (vconsole) for the CLI environment automatically based on your language choice. If the expected default works for you, meaning you have for example chosen German and you are using a German QWERTZ keyboard, you can skip this menu entry or just open it briefly and it will show you the current configuration so you can decide if you want to keep it.

![shot5](https://i.imgur.com/yAZYK8x.png)

NOTE: The same setting will later be used for the Console configuration of the target install - it is however independent of the keyboard layout for the Desktop Environment, which is a different thing (xkbmap) and will be defined in a later step.

### List Devices

Here you can list your available harddrives and other storage devices if you like. You can skip this step safely.

### Partition Disk

In case you haven’t already prepared the needed partitions for your install in advance, this step offers tools to partition storage devices with a choice of CLI tools (cfdisk, cgdisk, fdisk, gdisk or parted) and you also have an option to automatically partition your selected disk. In that case M-A will create a separate boot partition of 512MB next to a root partition of the remaining disk space for you.
Additionally this submenu includes an option to securely clean your disk using the tool wipe.
In our example we will choose automatic partitioning to keep it simple.

Submenus 1.4. and 1.5. deal with LUKS Encryption and LVM. Use those if you like and if you know what you’re doing.
A dedicated tutorial about LVM on LUKS with m-a can be found here 1.9k.
In our example anyway we will just skip these steps.

### Mount Partitions

Here you will be asked to specify which partitions to mount and how, starting with /root. We’ll select the bigger one of our partitions that have been auto-created in the preceeding step and use filesystem ext4, which is common for a Linux system. On the next screen we are offered a list of different mount options. If you don’t know of any special needs, just leave the default and confirm with [OK], or else choose what you need.
Next step is about SWAP. We haven’t created a separate swap partition but we still have the option to use a swap file. In that case we can specify its size on the next screen. Or just don’t use swap and continue with ‘None’. Or of course you can choose another existing partition here to be mounted as swap by the final install - sharing a swap partition with another system on your computer is also an option of course. Just select it here like any other partition.
A /boot partition (if needed) should be mounted as filesystem vfat.
If you decide to mount more partitions you will be asked to specify their respective mountpoints (/home /opt /var …).

### Configure Installer Mirrorlist

As you already know we will need to fetch quite a lot of data from the net very soon. So it will be good to optimise our download a little.
The ISO’s live script will already have tried to find a mirror in your region. To rank available mirrors by speed, to switch to a different branch than the default ‘stable’ or to tweak pacman in other ways by editing its config files you can use this submenu.
I recommend you use at least 1.7.3. Rank Mirrors by Speed.

The entry 1.8. Refresh Pacman Keys will normally not be needed.

Back to Main Menu we will now in our case select option 2. Install Desktop System and begin with

## Install Manjaro Desktop

After the package database has been updated we will be asked to select at least one Manjaro kernel.
Navigate to the desired list entry and select it with the Space key.
It makes sense to choose an alternative kernel here already, so we won’t need to install a backup kernel later manually. In this example lets choose the latest available LTS kernel linux414 and as a ‘cutting edge’ alternative linux416.
The base-devel group will be needed if you want to use the AUR in your installed system. If you are not sure that your selected profile actually provides base-devel and yaourt, just select the entry here together with the kernel(s).
(Duplicate packages will automatically be removed from the list later, so you don’t need to worry about that…)

![shot7](https://i.imgur.com/qZ1ypX7.png)

Next you will see a selection of all the available kernel extramodules. Since for my wireless card I will need the broadcom-wl module, I will select that one here so the matching driver packages for both my selected kernels will be installed. The rest of the listed modules I will not need, so I can leave those deselected.

After that we are presented with the selection of available preconfigured desktop environments, where we can select bspwm for our example installation.

The next screen offers the opportunity to select any additional packages you might want installed on your system that would else not be included, or you can just skip this with Esc.

Next, in case the chosen profile offers full and minimal (with a limited selection of pre-installed packages) versions, you will also be presented with this choice.

After that we can relax for a little while and watch M-A doing its job. It will create a list of needed packages and download and install them for us…

![shot8](https://i.imgur.com/q10AgWn.png)

In our case the download is about 600MB. Depending on your internet connection and the available selected mirror it will take a little while to get everything we need…

Once all our packages have been installed successfully, Manjaro’s hardware detection tool mhwd will install needed network drivers automatically. After that you will see this dialog about video-drivers installation:

![shot9](https://i.imgur.com/YCx2iPx.png)


While options 1 and 2 will find the matching free or proprietary video-drivers for your hardware automatically, option 3 will let you choose manually exactly which driver you would like to install.
Entry 4 will install mhwd’s complete list of free video-drivers. This option is intended for installation on a removable drive to later be used on varying hardware. Typical usecase would be when you want to create your own manjaro-architect install media as explained in detail in @Chrysostomus’ tutorial 452.

Our system is now already installed, but still there are some important things we need to do:
Back to the ‘Install Desktop System’ menu we proceede with

### Install Bootloader

Here you can choose between ‘grub’ or ‘grub + os-prober’.
You will like to use os-prober in case more than one system is to be found on your computer. If the Manjaro system you are installig right now is the only one or if you use a custom GRUB configuration, GRUB alone will do. The device where GRUB will be installed can be selected in the next step. Likely just one device will be listed.
If a system including a bootloader is already present on another partition of your computer you may want to not install a bootloader at all and skip this menu entry altogether.

Most of the steps in the next submenu 2.3. Configure Base are also important if you want to end up with a functional system! :wink:

### Generate FSTAB

The fstab file specifies where exactly on your drive(s) the partitions to be used by your system are located.
If you don’t know anything about the options available here, just use number 3 Device UUID, which is the current standard that would be used by the graphical installer.

### Set Hostname

Here you can enter what will elsewhere be called ‘computer-name’.

### Set System Locale

The so-called ‘locale’ defines the language to be used in the target environment and will also play a role in a DE’s automatic selection of a default currency, measure units and other regional “oddities” …

### Set Desktop Keyboard Layout

As said before, here you can select the keyboard layout to be used by the graphical environment.

### Set Timezone and Clock

In my case I select ‘Europe’ first, then ‘Vienna’ and confirm the selected timezone Europe/Vienna.
Next, since I am not dual-booting with Windows I can say ‘yes’ to using UTC.

### Set Root Password

This step is needed (!), even when you are going to use the same password for root as for your only user. Just type it in twice, also for Root.

### Add New User(s)

Enter a user name (lower case letters only), choose the default shell (zsh, bash or fish) and provide the password(s)!

With this, we are in fact DONE!
If you like you can double check the installed configuration using 2.5. Review Configuration Files one level up in the menu tree, which will look for available config files in the new system and offer the opportunity to open and edit them with text editor nano:


![shoot1](https://i.imgur.com/xgYEG6C.png)

or even use 2.7 Chroot into Installation to adjust or fix things in the fresh install.

But now let’s go back to the Main Menu and hit 6. Done !!

Before quitting, the installer will perform a final check of some vital parts of the installation and will tell us if maybe we skipped or forgot something important, like if we didn’t define a root password or not install a bootloader or the likes.

We will finally also have the choice to save the installation log to the target’s root directory.

In case something doesn’t work out as expected this will later be helpful to find out what might have gone wrong…

But now let’s reboot, shall we?


Similar to the process demonstrated here manjaro-architect can of course alternatively be used to install a bare CLI base system without any desktop environment (menu entry 3. Install CLI System) or a custom desktop to your own liking without Manjaro pre-configuration via 4. Install Custom System.
manjaro-architect also has a dedicated 5. System Rescue submenu which can be used to repair a borked Manjaro installation. However, these use-cases are not part of this tutorial.
Similar to the process demonstrated here manjaro-architect can of course alternatively be used to install a bare CLI base system without any desktop environment (menu entry 3. Install CLI System) or a custom desktop to your own liking without Manjaro pre-configuration via 4. Install Custom System.
manjaro-architect also has a dedicated 5. System Rescue submenu which can be used to repair a borked Manjaro installation. However, these use-cases are not part of this tutorial.

```bash
Overview of the menu structure in current version manjaro-architect 0.9.11:

Main Menu
|
├── Prepare Installation
|   ├── Set Virtual Console
|   ├── List Devices
|   ├── Partition Disk
|   ├── LUKS Encryption
|   ├── Logical Volume Management
|   ├── Mount Partitions
|   ├── Configure Installer Mirrorlist
|   |   ├── Edit Pacman Configuration
|   |   ├── Edit Pacman Mirror Configuration
|   |   └── Rank Mirrors by Speed
|   |
│   └── Refresh Pacman Keys
|
├── Install Desktop System
│   ├── Install Manjaro Desktop
│   ├── Install Bootloader
│   ├── Configure Base
|   │   ├── Generate FSTAB
|   │   ├── Set Hostname
|   │   ├── Set System Locale
|   │   ├── Set Desktop Keyboard Layout
|   │   ├── Set Timezone and Clock
|   │   ├── Set Root Password
|   │   └── Add New User(s)
|   │
│   ├── System Tweaks
|   │   ├── Enable Automatic Login
|   │   ├── Enable Hibernation
|   │   ├── Performance
|   |   │   ├── I/O Schedulers
|   |   │   ├── Swap Configuration
|   |   │   └── Preload
|   |   │
|   │   ├── Security and systemd Tweaks
|   |   │   ├── Amend journald Logging
|   |   │   ├── Disable Coredump Logging
|   |   │   └── Restrict Access to Kernel Logs
|   |   │
|   │   └── Restrict Access to Kernel Logs
|   │
│   ├── Review Configuration Files
│   └── Chroot into Installation
|
├── Install CLI System
│   ├── Install Base Packages
│   ├── Install Bootloader
│   ├── Configure Base
|   │   ├── Generate FSTAB
|   │   ├── Set Hostname
|   │   ├── Set System Locale
|   │   ├── Set Timezone and Clock
|   │   ├── Set Root Password
|   │   └── Add New User(s)
|   │
│   ├── Install Custom Packages
│   ├── System tweaks
|   │   └── ... (see 'Install Desktop System')
|   │
│   ├── Review Configuration Files
│   └── Chroot into Installation
|
├── Install Custom System
│   ├── Install Base Packages
│   ├── Install Unconfigured Desktop Environments
|   │   ├── Install Display Server
|   │   ├── Install Desktop Environment
|   │   ├── Install Display Manager
|   │   ├── Install Networking Capabilities
|   │   |   ├── Display Wireless Devices
|   │   |   ├── Install Wireless Device Packages
|   │   |   ├── Install Network Connection Manager
|   │   |   └── Install CUPS / Printer Packages
|   │   |
|   │   └── Install Multimedia Support
|   │       ├── Install Sound Driver(s)
|   │       ├── Install Codecs
|   │       └── Accessibility Packages
|   │
│   ├── Install Bootloader
│   ├── Configure Base
|   |   └── ... (see 'Install Desktop System')
|   │
│   ├── Install Custom Packages
│   ├── System Tweaks
|   |   └── ... (see 'Install Desktop System')
|   │
│   ├── Review Configuration Files
│   └── Chroot into Installation
|
└── System Rescue
    ├── Install Hardware Drivers
    │   ├── Install Display Drivers
    │   └── Install Network Drivers
    |
    ├── Install Bootloader
    ├── Configure Base
    |   └── ... (see 'Install Desktop System')
    │
    ├── Install Custom Packages
    ├── Remove Packages
    ├── Review Configuration Files
    ├── Chroot into Installation
    ├── Data Recovery
    │   ├── Clonezilla
    │   └── Photorec
    │
    └── View System Logs
        ├── Dmesg
        ├── Pacman log
        ├── Xorg log
        └── Journalctl

```
