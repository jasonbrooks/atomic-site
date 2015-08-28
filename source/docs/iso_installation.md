Installing an Atomic Host from an ISO image
===========================================

This document shows how to install an Atomic Host from an ISO image, either on bare metal or on a virtual machine. The installation is guided by the **Anaconda** tool that is also used in Fedora, CentOS and Red Hat Enterprise Linux. If you are familiar with the Anaconda GUI, you will find the installation process very similar.

## Getting the Installation Image and Creating Media

Download a [Fedora Atomic](http://download.fedoraproject.org/pub/fedora/linux/releases/22/Cloud_Atomic/x86_64/iso/Fedora-Cloud_Atomic-x86_64-22.iso) or [CentOS Atomic](http://cloud.centos.org/centos/7/atomic/images/CentOS-Atomic-Host-7-Installer.iso) iso image file and, if necessary, use it to create installation media. For example, if you run the GNOME desktop, you can use the *Write to disk* capability of the Nautilus file browser to create an installation DVD. Alternatively, you can write the installation ISO image to a USB device with the `dd` command.

These procedures are described in the [Fedora Installation guide](https://docs.fedoraproject.org/en-US/Fedora/22/html/Installation_Guide/sect-preparing-boot-media.html) in the section called *Preparing Boot Media*. 

## Installing an Atomic Host with the Anaconda Installer

For a bare metal system, disconnect any drives which you do not need for the installation, power on your computer system and insert the installation media you created from the ISO image. Then, power off the system with the boot media still inside, and power on the system again. Note that you might need to press a specific key or combination of keys to boot from the media or configure your system's *Basic Input/Output System* (BIOS) to boot from the media.

For a virtual machine installation, consult the documentation for your virtualization software to learn how specifically to attach the Atomic Host ISO image to your VM and boot from it.

Once your system has completed booting, the boot screen is displayed: 

*NOTE: The screens and docs below reference Fedora, but equally apply to CentOS-based hosts.*

![Fedora Atomic Boot Screen](boot_screen1.png "Fedora Atomic Boot Screen")

The boot media displays a graphical boot menu with three options:

- *Install Fedora 22* - Choose this option to install Fedora Atomic onto your computer system using the graphical installation program. 

- *Test this media & install Fedora 22* -  With this option, the integrity of the installation media is tested before installing Fedora Atomic onto your computer system using the graphical installation program. This option is selected by default.

- *Troubleshooting* - This item opens a menu with additional boot options. From this screen you can launch a rescue mode for Fedora Atomic, or run a memory test. Also, you can start the installation in the basic graphics mode as well as boot the installation from local media.

If no key is pressed within 60 seconds, the default boot option runs. Press *Enter* to proceed. 

After media check, you are directed to the welcome screen, where you can choose the language of the installation:

![Fedora Atomic Welcome Screen](welcome_screen1.png "Fedora Atomic Welcome Screen")

Select your language of preference and press *Continue*. The *INSTALLATION SUMMARY* screen appears. It is the central menu for configuring localization, software, and system settings for your installation. Items that are not yet configured are marked with orange warning sign. 

![Fedora Atomic Installation Summary](installation_summary1.png "Fedora Atomic Installation Summary")

This menu allows you to configure your installation in the order you choose. Each menu item leads to an individual configuration screen. For description of these sub-screens, see the corresponding sections of the Fedora Installation guide:

- [*Time & Date*](https://docs.fedoraproject.org/en-US/Fedora/22/html/Installation_Guide/sect-installation-gui-date-and-time.html) - lets you configure date, time, time zone, and NTP (Network Time Protocol).
- [*Keyboard*](https://docs.fedoraproject.org/en-US/Fedora/22/html/Installation_Guide/sect-installation-gui-keyboard-layout.html) - here you can select keyboard layouts to use on your system.
- [*Language Support*](https://docs.fedoraproject.org/en-US/Fedora/22/html/Installation_Guide/sect-installation-gui-language-support.html) - this screen provides language options for your installation, it is identical to the welcome screen depicted above.
- [*Installation Destination*](https://docs.fedoraproject.org/en-US/Fedora/22/html/Installation_Guide/sect-installation-gui-storage-partitioning.html) - allows you to select storage devices and set partitioning. Note that it is recommended to use the default partitioning configured in the boot.iso image.
- [*Network & Hostname*](https://docs.fedoraproject.org/en-US/Fedora/22/html/Installation_Guide/sect-installation-gui-network-configuration.html) - provides a network management interface.

Once you configured all required settings, the orange warning signs disappear and you can start the installation by pressing *Begin Installation*.

While the installation proceeds, you can specify the user settings:

![Fedora Atomic Final Screen](final_screen1.png "Fedora Atomic Final Screen")

[Select a root password](https://docs.fedoraproject.org/en-US/Fedora/22/html/Installation_Guide/sect-installation-gui-root-password.html) and [create a user](https://docs.fedoraproject.org/en-US/Fedora/22/html/Installation_Guide/sect-installation-gui-create-user.html) if desired. When the installation is finished, you are prompted to reboot the system by pressing *Reboot*.

## Next Steps

Once you're up and running, and logged into your Atomic Host, you can:

### Run a docker container

```
# docker run fedora /bin/echo "hello world"
```

### Update your host

```
# atomic host upgrade
# systemctl reboot
```

### Configure an Atomic cluster

Atomic hosts are either used in a standalone configuration to run Docker containers or in an orchestrated cluster to run Kubernetes pods. You can use the [Getting Started Guide](/docs/gettingstarted) to set up a cluster.




