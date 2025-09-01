# Kubuntu 24 install notes

## megaraid-sas

So it seems K24 uses kernel 6.8 whereas K22 is 5.5 and something changed between to screw up the megaraid-sas drivers.  
To fix it needs the kernel options `intel_iommu=on iommu=pt`.  They can be added manually or applied to grub.  

### grub changes to support kernel options
The best fix is to add the above to `/etc/default/grub` in the `GRUB_CMDLINE_LINUX` option, which gets inserted into the main `10_linux` triggered startup options.  However note, it is not added to `30_os_probe` based options.  

### storcli

So `storcli` and `storcli64` seem to have replaced the old `tw` tool.  The command structure is as clunky as ever, but help seems to be much better, than was (it gives a prompt of all possible commands for a context, with examples; so if its not there you can't do it).  

One thing I don't remember from tw days, is you need to `sudo` to use it, otherwise it can't even see the controller.  

#### Useful storcli commands:-

1) `sudo storcli /c0 show all`            # Show everything for the main c0 controller   
2) `sudo storcli /c0 show bootdrive`      # Show which device is the one to boot from, when the bios boots from the raid controller
3) `sudo storcli /c0/v0 set bootdrive=on` # Select v0 (the volume assigned to the d0 raid array at time of writing) as bootable
4) `sudo storcli /cx/e68/s1 set jbod`     # If you connect a fresh drive, enable jbod mode to see it
5) `sudo storcli /cx/eall/s1 del jbod`    # If you subsequently want to use it, in a drive set, you need remove jbod mode first
6) `sudo storcli /c0/e68/s1 help`         # As long as you use the full path, not `eall` you can get context relevant commands using help
7) `sudo storcli /c0/d0 show`             # To get details on an existing drive set.  Notice the Dg Arr and Row columns to see if a disk is missing.  
8) `sudo storcli /c0/e68/s1 insert dg=0 array=0 row=0` # To insert the unused disk, in slot 1, into the 0th Drive set, in the 0:0 location
9) `sudo storcli /c0/eall/s1 show rebuild` # See how much longer before all data has been copied to the new disk, promoting the Drive set from Degraded to Online
9) `sudo storcli /c0/eall/sall show rebuild` # Show how long left, before the new disk has been rebuild with the old data

##### When things break:-
1) `sudo storcli /c0/e68/s3 set good`       # Will change a disk from UBad to UGood
2) `sudo storcli /c0/fall import`           # Will scan available UGood disks, to identify and Raid arrays, that are otherwise inaccessible
3) `sudo storcli /c0/e68/s3 set online`     # Will add a UGood disk, back to the now available raid array, making it degraded but available

##### When you've increased the size of the disks in the array
1) `sudo storcli /c0/v0 start expand size=Full`  # To tell the controller to increate the VDisk size, to fill the disks

## nvidia drivers

It seems a new tool has appeared called `ubuntu-drivers` that manages some 3rd Party drivers.  `sudo ubuntu-drivers install nvidia:570` seemed to be the required command to have it setup the nvidia drivers properly

Before I got to that point, It seems something in the toolset of the `dkms` package was wrong, so when `nvidia-dkms-570` was installed, it would attempt to run a command using `/bin/sh -c <cmd>`, piping output to a `.h` file, where the `<cmd>` itself was a call to `/bin/sh`, but without quoting the command.  Meaning it would simply open a shell up, using that file as output, and so hang.  
I was in the process of digging for where it was calling the conftest.sh script, to quote that call, but then it started working.  
