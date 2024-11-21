# Dual Boot Installation Guide: Windows and Proxmox VE

This guide details how to remove an existing Linux installation (Xubuntu), clean the GRUB bootloader, and install **Proxmox VE** alongside Windows in a dual-boot setup.

## Prerequisites

- **Backup Your Data**: Ensure all important files from both Windows and Xubuntu are backed up.
- **Bootable USB**: Have a USB drive (8GB or more) ready for the Proxmox VE installer.
- **Proxmox ISO**: Download the Proxmox VE ISO from [Proxmox VE Downloads Page](https://www.proxmox.com/en/downloads).

## Remove Xubuntu and GRUB Bootloader

### Boot into Windows

Restart your PC and select **Windows** from the GRUB menu.

### Remove GRUB and Restore Windows Bootloader

1. Open **Command Prompt** as Administrator:
	Press `Windows + R`, type `cmd`, and press `Ctrl + Shift + Enter`.
	
3. Run the following commands one by one to remove GRUB and restore the Windows bootloader:
	```sh
	bootrec /fixmbr
	bootrec /fixboot
	bootrec /scanos
	bootrec /rebuildbcd
	```
4.  Restart your PC:
	Your computer should now boot directly into **Windows**.

### Delete Xubuntu Partitions

1.  Open **Disk Management** in Windows:
	Press Windows + X and select **Disk Management**.
	
2.  Identify and delete Xubuntu-related partitions:
	Look for partitions labeled **ext4**, **Linux swap**, or unrecognized file systems.
	Right-click on these partitions and choose **Delete Volume**.

## Install Proxmox VE

### Download and Create a Bootable Proxmox VE USB

1.  Download the Proxmox VE ISO from the [official site](https://www.proxmox.com/en/downloads).

3.  Create a bootable USB using a tool like **Rufus** or **Balena Etcher**:
	-  Select the Proxmox ISO.
	- Choose your USB drive.
	- For Rufus, set:
		Partition scheme: **MBR** for BIOS or **GPT** for UEFI.

### Boot into the Proxmox Installer

1.  Insert the bootable USB and restart your PC.
2.  Access your system’s boot menu (press **F2**, **DEL**, or **F12** during startup).
3.  Select the USB drive and boot from it.

### Install Proxmox VE

1.  When the installer appears, choose **Install Proxmox VE**.

3.  Select the **unallocated space** left by Xubuntu (do **not** overwrite the Windows partition).

4.  Follow the installation wizard:
	- Set the **root password**.
	- Configure the network (assign a static IP address if needed).

5.  Finish the installation:
	Once complete, Proxmox will prompt you to remove the USB and reboot.

## Configure Dual Boot (Windows + Proxmox VE)

### Check GRUB Menu

After Proxmox boots, check if the GRUB menu shows both **Proxmox VE** and **Windows**.

### Update GRUB if Windows Is Missing

1.  Log into Proxmox via the terminal (or SSH).

2.  Run the following command:
	```sh
	update-grub
	```
3.  Restart your system:
	The GRUB menu should now show both **Windows** and **Proxmox VE**.

## Access Proxmox VE Web Interface

1.  On a different device or from Windows, open a web browser.

2.  Enter the Proxmox server address:
	```sh
	https://<proxmox-server-ip>:8006
	```
3.  Log in with the root account and password you set during installation.