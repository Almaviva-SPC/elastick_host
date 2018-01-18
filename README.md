

Pre setup for installation of elasticsearch on linux machine

This ansible config the machine to prepare enviroment (vg file file system repo ecc)


VARIABLES

The playbook use three different groups of variables, for each host group.
These variables can be specified for each server node and data node:
* devdisk: the name of the physical device (sda,vda,or device under /dev)
* vgname: Name of the Volume Group for elasticsearch data
* lvname: Name of the logical volume for elasticsearch data
* mountpath: Data directory for elasticsearch
* formatfs: File system Type
* lvsize: size of the logical volume

These variables are common for master and data node:
* ekl_version_to_install: the ELK stack version that must be installed and configured

ROLES

The roles are executed in this order:
* config_file_hosts: populates the /etc/hosts files inside the machines specified in *hosts* file with correct hostname - ip for master_node and data_node  
* createfs: creates the vg,lv and filesystem specified for elasticsearch data; mounts the filesystem on specified folder, disables firewalld service and selinux
* ansible-elasticsearch: installs and configure elasticsearch cluster. Configures a master node and no.2 data nodes
* ansible-kibana: installs and configures kibana on master_node
* ansible-logstash installs and configures logstash collector on master node

PLAYBOOK
