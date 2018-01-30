
INSTALLATION OF ELASTICSEARCH, KIBANA AND LOGSTASH ON LINUX MACHINES

This ansible playbook prepares the enviroment (vg file file system repo ecc) and install an ELK stack with a master elasticsearch node with kibana and logstash collector and two data elasticsearch nodes.


HOSTS

the hosts affected by the playbook are written in *hosts* file.


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
* elastic_Cluster: the name of the elasticsearch cluster
* elasticsearch_master_is_datanode: if true the elasticsearch master node is added in the list of hosts logstash will connect to. When true, the value of *node.data* in prepare.yml is set to true too. In this way the host is considered either a master node or a data node
* master_node: the hostname of elasticsearch master node
* data_node1: the hostname of the first elasticsearch data node
* data_node2: the hostname of the second elasticsearch data node


ROLES

The roles are:
* config_file_hosts: populates the /etc/hosts files inside the machines specified in *hosts* file with correct hostname - ip for master node and data nodes  
* createfs: creates the vg,lv and filesystem specified for elasticsearch data; mounts the filesystem on specified folder, disables firewalld service and selinux
* ansible-elasticsearch: installs and configure elasticsearch cluster; there are two different kind of installation that can be used: master installation and node installation  
* ansible-kibana: installs and configures kibana
* ansible-logstash installs and configures logstash collector


PLAYBOOK

The playbook *prepare.yml* configures the machines in *hosts* file in this way:

- connects to machines of group 'elk_host_to_discovery' and retrieves ip addresses (like user "centos" and sudo privilege)
- starts role *config_file_hosts* and writes correct entries in all files /etc/hosts in those machines, as follows:
            node1_hostname ip
            node2_hostname ip
            node3_hostname ip
- connects to machines of group 'elastic_server_master' (like user "centos" and sudo privilege)  
- starts role *createfs*
- starts role *ansible-elasticsearch*. It uses master installation and configure the master node that work like a cluster controller. All values used for this installation are specified with variables or with default values inside /ansible-elasticsearch/defaults/main.yml
- starts role *ansible-kibana*
- starts role *ansible-logstash*
- connects to machines of group 'elastic_server_node' (like user "centos" and sudo privilege)
- starts role *createfs*
- starts role *ansible-elasticsearch*. It now uses node installation to configure the cluster nodes that actively store elasticsearch data. All values used for this installation are specified with variables or with default values inside /ansible-elasticsearch/defaults/main.yml, as for the master installation.
