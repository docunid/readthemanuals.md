# CoreOS on Bare Metal (Intel NUC Kit DN2820FYK)

There is some fairly decent documentation on how to install CoreOS on Bare Metal

- [Installing CoreOS Container Linux to disk](https://coreos.com/os/docs/latest/installing-to-disk.html)
- [How to perform a bare metal installation of CoreOS Linux](https://linuxconfig.org/how-to-perform-a-bare-metal-installation-of-coreos-linux)

But - as always - there are slight variations when getting your hand dirty, is this case with the Intel NUC Kit DN2820FYK. First update the BIOS of the NUC to the latest version, at the time of writing [FY0059.bio](https://downloadcenter.intel.com/download/26898/NUCs-BIOS-Update-FYBYT10H-86A-). Obviously a good idea but I suspect CoreOS will install equally fine with an older BIOS version.

Then scramble together a Live Linux Bootable Media Device. In other words create a bootable USB drive with - in this case - CoreOS on it. Just for fun because any other Live Linux will do equally good. Actually better since CoreOS is not loaded with handy tools.

- get USB drive
- get [UNetbootin](https://unetbootin.github.io/)
- get CoreOS [ISO](https://coreos.com/os/docs/latest/booting-with-iso.html)

But before we create the Bootable Media Device we must take into account that installing CoreOS to disk needs two helpers

- a container linux config file
- and an install script, located [here](https://coreos.com/os/docs/latest/installing-to-disk.html)

CoreOS favors [Ignition](https://coreos.com/ignition/docs/latest/what-is-ignition.html) over `coreos-cloudinit` hence we need a container linux config file and not a Cloud-Config. Creating a container linux config file is a two-step proces:

- write config in YAML
- transform config into json using a config transpiler

## Config Transpiler

Off course you'll need to install a config transpiler first.

Using prebuilt binaries: the Container Linux Config Transpiler command line interface, ct for short, can be downloaded from its [GitHub Releases page](https://github.com/coreos/container-linux-config-transpiler/releases). Download it, move it to `/usr/local/bin/ct` or whatever has your fancy and make it executable.

Then, write a config (see [examples](https://coreos.com/os/docs/latest/clc-examples.html)) and run `ct -in-file example.yaml -out-file example.json`, afterwards check the output [here](https://coreos.com/validate/).

For an handy eximple: only add ssh key to user core.

## Bootable Media Device

Now we need the config and the install script on the Live Linux, so before we create a bootable USB drive with UNetbootin, let us partition the drive: `diskutil partitionDisk disk2 2 MBR MS-DOS DATA 3G MS-DOS COREOS R` and move the `coreos-install` script and the ignition config `example.json` to DATA. Then create the Bootable Media Device on partition COREOS by following [these steps](https://tutorials.ubuntu.com/tutorial/tutorial-create-a-usb-stick-on-macos).

Now we boot the NUC from USB. Once we get a prompt we mount the DATA partition (`lsblk` and `mkdir <mountpoint>` and `mount /dev/... <mountpoint>` and such). Then we `coreos-install -d /dev/sda -C stable -i example.json` from DATA.

And Bob's your uncle!

Finally you can check by running `journalctl --identifier=ignition --all`. Mind you, after ssh-ing in of course because CoreOS has no password to login from the console on the Bare Metal. CoreOS is not meant to be "consoled".

## Headless

A headless Linux server is just a server that has no monitor, keyboard or mouse. Apparently `coreos-install` does not play nice with the NUC, it will not boot without a HDMI cable/monitor attached. Offcourse Bob could buy a HDMI dummy plug but where is the fun in that? It looks like the coreos-install script gambles on two monitors for grub, the GRand Universal Bootloader: `set linux_console="console=ttyS0,115200n8 console=tty0"`. Let's override that to make only ttyS0 necessary: append `set linux_console="console=ttyS0,115200n8"` to `/usr/share/oem/grub.cfg`. Reboot without monitor and presto!

## Updating the container linux config file

Ignition only runs on the first boot. Since Ignition is not typically run more than once during a machine's lifetime in a given role, the situation requiring manual systemd intervention does not commonly arise. But when it does: `sudo systemctl preset-all`.

Ignition is usually used to write your desired configuration to disk, so there is no need to run it on later boots.  If you need to perform some operation with Ignition again, you can add `coreos.first_boot=1` to the kernel boot command-line to make it run again.
