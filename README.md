# Openshfit (3.6) Installation and Using GitHub Hooks with Openshift for S2I based Continuous Delivery

##### Version Information

OS: CentOS Linux release 7.4
Docker version: Docker version 1.13.1
Openshift Release: 3.6
Openshift ansible: https://github.com/openshift/openshift-ansible.git
Openshift ansible branch: origin/release-3.6

##### Demo Topology and Components

1. One Master and Two Workers nodes
2. Multi-Tenant OVS Plugin for Networking
3. xip.io for Wildcard DNS
4. Ultrahook to enable Github communication to private hosts

##### Openshift Installation

1. If using on AWS use: https://github.com/gokulpch/Container_Platform_Cloudformation/blob/master/Openshift_1Master_2Worker_Cformation.yaml

This creates required VPC, Subnet, IGW, Security Groups and 3 EC2 Instances (1 Mater and 2 Workers)

2. Install Ansible on the Master 

```
yum install ansible

# ansible --version
ansible 2.4.2.0
  config file = /etc/ansible/ansible.cfg
  configured module search path = [u'/root/.ansible/plugins/modules', u'/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python2.7/site-packages/ansible
  executable location = /bin/ansible
  python version = 2.7.5 (default, Aug  4 2017, 00:39:18) [GCC 4.8.5 20150623 (Red Hat 4.8.5-16)]
```












 
