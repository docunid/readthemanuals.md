# CoreOS on Bare Metal (Intel NUC Kit DN2820FYK)
There is some fairly decent documentation on how to install CoreOS on Bare Metal

- [Installing CoreOS Container Linux to disk](https://coreos.com/os/docs/latest/installing-to-disk.html)
- [How to perform a bare metal installation of CoreOS Linux](https://linuxconfig.org/how-to-perform-a-bare-metal-installation-of-coreos-linux)

But - as always - there are slight variations when getting your hand dirty, is this case with the Intel NUC Kit DN2820FYK. First update the BIOS of the NUC to the latest version, at the time of writing [FY0059.bio](https://downloadcenter.intel.com/download/26898/NUCs-BIOS-Update-FYBYT10H-86A-). Obviously a good idea but I suspect CoreOS will install equally fine with an older BIOS version.

Then scramble together a Live Linux Bootable Media Device. In other words create a bootable USB drive with - in this case - CoreOS on it. Just for fun because any other Live Linux will do equally good. Actually better since CoreOS is not loaded with handy tools.

- get USB drive
- get [UNetbootin](https://unetbootin.github.io/)
- get [ISO](https://coreos.com/os/docs/latest/booting-with-iso.html)

But before we create the Bootable Media Device we must take into account that installing CoreOS to disk needs two helpers
- a container linux config file
- an install script, located [here](https://coreos.com/os/docs/latest/installing-to-disk.html)

CoreOS favors [Ignition](https://coreos.com/ignition/docs/latest/what-is-ignition.html) over `coreos-cloudinit` hence we need a container linux config file. And creating that is a two-step proces:
- write config in YAML
- transform into json using a config transpiler

## Config Transpiler
Off course you'll need to install a config transpiler first. Using prebuilt binaries: the Container Linux Config Transpiler command line interface, ct for short, can be downloaded from its [GitHub Releases page](https://github.com/coreos/container-linux-config-transpiler/releases). Download it, move it to `/usr/local/bin/ct` or whatever has your fancy and make it executable.

Write an config (see [examples](https://coreos.com/os/docs/latest/clc-examples.html) for syntax and more) and run `ct -in-file example.yaml -out-file example.json`. Then check the output [here](https://coreos.com/validate/). For eximple only add ssh key to user core.

## Bootable Media Device
We need the config and the install script on the Live Linux, so before we create a bootable USB drive with UNetbootin, let us partition the drive: `diskutil partitionDisk disk2 2 MBR MS-DOS DATA 3G MS-DOS COREOS R` and move coreos-intall script and example.json to DATA. Then create the Bootable Media Device on COREOS by following [these steps](https://tutorials.ubuntu.com/tutorial/tutorial-create-a-usb-stick-on-macos).

Now we boot the NUC from USB. Once we get a prompt we mount the DATA partition (`lsblk` and `mkdir <mountpoint>` and `mount /dev/... <mountpoint>` and such). Then we `coreos-install -d /dev/sda -C stable -i example.json` from DATA.

And Bob's your uncle!

Finally you can check by running `journalctl --identifier=ignition --all` after ssh-ing in off course.
