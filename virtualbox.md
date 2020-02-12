# Documentation for Virtualbox

***

## How to enlarge .vdi (virtual drive) space

1. Firstly, you have to turn off your virtual machine (turn off, not just suspend!)

2. Then open "Settings" for your virtual machine

3. In "Settings" go to "Storage" and select the .vdi drive

4. Copy the location of .vdi file

5. In the console run

```bash
$ VBoxManage modifyhd <path_to_vdi_file> --resize <required_size>
```

- **<required_size>** => is in megabytes (so for 20GB use **20480** value; for the calculation you can use this website https://www.convertunits.com/from/GB/to/MB)

6. Then you have to log into virtual machine and use some "Partition Manager" software to resize the main drive (e.g. /dev/sda1 for linux systems)