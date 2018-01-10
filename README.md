
Pre setup for installation of elasticsearch on linux machine

This ansible config the machine to prepare enviroment (vg file file system repo ecc)

The playbook use this variable
* devdisk: the name of the physical device (sda,vda,or device under /dev)
* vgname: Name of the Volume Group for elasticsearch data
* lvname: Name of the logical volume for elasticsearch data
* mountpath: Data directory for elasticsearch
* formatfs: File system Type
