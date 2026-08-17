# My Command Storge 

i write tha commands I've learn in Lpic1 by Jady in this file 
you can use it for good an i hope it useful for you :)

## commad 
*Shows all file and directoris*
``` bash
ls
```
### options
- **-l this option lists the files in the repo**
- **-t sort's items by time** 
- **-r reverse order while sorting** 
- **-h human readable mode** 
- **-a show all in the dir** 
- **-R list subdirectories recursively** 
- *I most use the -ltrh and -ltrha*

## commad 
*To move in to a directory*
``` bash
cd
```


## commad 
*Shows all connected usb's*
``` bash
lsusb
```

## commad 
*PC information*
``` bash
lspci
uname 
```

## commad 
*Shows info of the all disk's*

``` bash
lsblk
```

## commad 
*Shows information of hardwere*
``` bash
lshw
```

## commad 
*Shows all Modules in the kernel*
``` bash
lsmod
```

## commad 
*Remove a modules from kernel*
``` bash
rmmod
```

## commad 
*Add a module to kernel (absolute path)*
``` bash
insmod
```

## commad 
*Add a module to kernel (name only)*
``` bash
modprobe
```

## commad 
*Shows Boot Log's*
``` bash
dmesg
```

## commad 
*Shows system log note*
``` bash
journalctl
```
### options
- **-r Reverse output so that the newest entries are displayed first.**
- **-f Show only the most recent journal entries**
- **-e Immediately jump to the end of the journal inside the implied pager tool.**
- **-n Show the most recent journal events and limit the number of events shown.**
- **-k Show only kernel messages.**
- **-b Show messages from a specific boot.**
- **-p Filter output by message priorities or priority ranges.** *you need to read more for this.*
- **-u Show messages for the specified systemd unit UNIT**
- **--list-boots Show a tabular list of boot numbers (relative to the current boot), their IDs, and the timestamps of the first and last message pertaining to the boot.**
- **--vaccum-time removes archived journal files older than the specified timespan.**
- **--vaccum-size removes the oldest archived journal files until the disk space they use falls below the specified size.**
- **--vaccum-file leaves only the specified number of separate journal files.**
- **-D Takes a directory path as argument.**
- **-xefu *that's on you***

## commad 
*To poweroff the PC*
``` bash
shutdown
poweroff
```
### options
**shutdown -r  it's reboot the machine at the seted time/timer.**

## commad 
*To shutdown the Cpu only*
``` bash
halt
```

## commad 
*To reboot the system*
``` bash
reboot
```

## commad 
*to sand a message to the all open terminal's*
``` bash
wall
```

## commad 
*message manager*
``` bash
mesg
```

## commad 
*Shows who is logged on*
``` bash
who
```

## commad 
*To list all units*
``` bash
systemctl list-units
```

## commad 
*Shows information of one unit*
``` bash
systemctl cat (your target)
```

## commad 
*Shows the status of a unit*
``` bash
systemctl status (your target)
```

## commad 
*Shows the contents in a file*
``` bash
cat
```

## commad 
*Shows status of all unit's*
``` bash
systemctl status
```

## commad 
*For chenge your status (mode you using it )*
``` bash
systemctl isolate rescue
systemctl isolate emergency
systemctl isolate graphical
```

## commad 
*Shows status of free space in your disk*
``` bash
free
```

## commad 
*Shows info of virtual RAM*
``` bash
swapon
```

## commad 
*Shows the active program's*
``` bash
top
htop
```

## commad 
*Shows the content in the binry file's (elf files)*
``` bash
readelf
```

## commad 
*Shows the info / location of the command's*
``` bash
type
```

## commad 
*Shows the corent addres (in the directory's)*
``` bash
pwd
```

## commad 
*for print a text the teminal (it use to write a text in to a file and ...)*
``` bash
echo
```

## commad 
*Shows manual of commands*
``` bash
man (your command)
```

## commad 
*to create a variable*
``` bash
(your variable name)=(you value)   MyName=Reza
```

## commad 
*Shows all variable's in PC (OS)*
``` bash
env
```

## commad 
*Shows the location of a command *
``` bash
which (your command)
```

## commad 
*show the location of binary, cource, .... of a command*
``` bash
whereis
```

## commad 
**
``` bash

```
