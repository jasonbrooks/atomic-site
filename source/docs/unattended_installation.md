## Unattended Installation

Kickstart installations offer a means to automate the installation process, either partially or fully. Kickstart files contain answers to all questions normally asked by the installation program, such as what time zone do you want the system to use, how should the drives be partitioned or which packages should be installed. 

The installer ISO contains embedded content, and thus works offline.

However, to do a PXE installation, you must download the content
dynamically.  We strongly recommend mirroring the OSTree repository,
rather than having each machine contact the upstream provider.

So, for unattended installation you need to specify your own kickstart
configuration and load it from the boot prompt as described below. Use
the `ostreesetup` command in your custom configuration, which is what
configures anaconda to consume the rpm-ostree generated content.

For example, create *atomic-ks.cfg* file with the following content:

    lang en_US.UTF-8
    keyboard us
    timezone America/New_York
    zerombr
    clearpart --all --initlabel
    autopart
    # SSH keys are better than passwords, but this is a simple example
    rootpw --plaintext atomic

    # NOTICE: This will download the content from upstream; this will be very slow.
    # Create your own OSTree repo locally and mirror the content instead.
    ostreesetup --nogpg --osname=fedora-atomic --remote=fedora-atomic --url=https://dl.fedoraproject.org/pub/fedora/linux/atomic/22/ --ref=fedora-atomic/f22/x86_64/docker-host

There are several other options you can specify in the kickstart file, see *Kickstart Options* in the [Fedora Installation guide](https://docs.fedoraproject.org/en-US/Fedora/22/html/Installation_Guide/sect-boot-options-kickstart.html). 

After creating the configuration file, you have to ensure that it will be available during the installation. Place the file on hard drive, network, or removable media as described in the [Fedora Installation guide](https://docs.fedoraproject.org/en-US/Fedora/22/html/Installation_Guide/chap-kickstart-installations.html#sect-kickstart-making-available).

Start the installation as described above. On the boot screen, press *Esc* to view the *boot:* prompt. At this prompt, specify the path to the kickstart configuration file:

    boot: linux inst.ks=kickstart_location 

The *linux* keyword is used here to specify the installation program image file to be loaded. The *inst.ks* option specifies the kickstart configuration file, replace *kickstart_location* with a path or URL of *atomic-ks.cfg*. See the [Fedora Installation guide](https://docs.fedoraproject.org/en-US/Fedora/22/html/Installation_Guide/chap-kickstart-installations.html#sect-kickstart-installation-starting) for more information on how to start a kickstart installation.

With the kickstart file described above, the installation proceeds automatically until Anaconda prompts you to reboot the system to complete the installation. 

## Updating and Reverting Fedora Atomic

NOTE: If you've used a different `ostreesetup` URL or reference, you'll want to make sure you set the appropriate repository to get updates.  If the `ostreesetup` URL is where you'd like to receive updates, you can skip the `rebase` section.

To receive updates for your Fedora Atomic installation, specify the location of the remote OSTree repository. Execute:

    # ostree remote add --set=gpg-verify=false fedora-atomic http://dl.fedoraproject.org/pub/alt/fedora-atomic/repo/

Here, *fedora-atomic* is used as a name for the remote repository. The URL is stored in the */etc/ostree/remotes.d/fedora-atomic.conf* configuration file. Today, it is required to disable GPG verification for the remote repository to update Fedora Atomic successfully. Therefore the *--set=gpg-verify=false* option above. Alternatively, you can disable GPG by appending `gpg-verify=false` to */etc/ostree/remotes.d/fedora-atomic.conf*.

Now you are able to update your system with the following command:

    # rpm-ostree rebase fedora-atomic:

The `rebase` command, that is an extension of the `upgrade` command, switches to a newer version of the current tree. In the above syntax, *fedora-atomic:* stands for the full branch name (in this case *fedora-atomic/rawhide/x86_64/server/docker-host*). Reboot the system to start the updated version of Fedora Atomic:
   
    # systemctl reboot

To determine what version of the operating system is running, execute:

    # atomic host status

To revert to a previous installation, execute the following commands:

    # atomic host rollback
    # systemctl reboot

<!---
## Uninstalling Fedora Atomic

To remove Fedora Atomic from your computer, you must remove its boot loader information from your master boot record (MBR) and remove any partitions that contain the operating system. Please do not forget to back up any data you want to keep before proceeding.

The removal process varies depending on whether Fedora Atomic is the only operating system installed, or whether the computer is configured to dual-boot Fedora Atomic and another operating system. Fedora Installation guide describes both the [stand-alone](http://docs.fedoraproject.org/en-US/Fedora/20/html/Installation_Guide/ch-x86-uninstall.html#sn-x86-uninstall-single) and [dual-boot](http://docs.fedoraproject.org/en-US/Fedora/20/html/Installation_Guide/sn-x86-uninstall-dual.html) case for Fedora, and these instructions are applicable to Fedora Atomic too.

-->
