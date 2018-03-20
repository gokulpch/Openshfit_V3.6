# Openshfit (3.6) Installation and Using GitHub Hooks with Openshift for S2I based Continuous Delivery

##### Version Information

* OS: CentOS Linux release 7.4
* Docker version: Docker version 1.13.1
* Openshift Release: 3.6
* Openshift ansible: https://github.com/openshift/openshift-ansible.git
* Openshift ansible branch: origin/release-3.6

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

3. Create Inventory file. Use "sample_openshift_ad_inventory.yaml", add hosts as applicable and change the osm_default_subdomain to one of your worker nodes

4. Bootstrap ansible with "host_bootstrap_openshift.yaml" with the inventory file above

```
ansible-playbook host_bootstrap_openshift.yaml -i sample_openshift_ad_inventory.yaml

AWS:

ansible-playbook host_bootstrap_openshift.yaml -i sample_openshift_ad_inventory.yaml --key-file key.pem
```

5. Clone Openshift Ansible and checkout the required version_branch

```
git clone https://github.com/openshift/openshift-ansible.git
cd openshift-ansible
git checkout origin/release-3.6
```

6. Run Openshift ansible with the inventory created above

```
ansible-playbook -c paramiko -i sample_openshift_ad_inventory.yaml openshift-ansible/playbooks/byo/config.yml

AWS:

ansible-playbook -c paramiko -i sample_openshift_ad_inventory.yaml openshift-ansible/playbooks/byo/config.yml --key-file key.pem
```
* This takes 15-20 Minutes for completion

7. Create a Admin password for logging into the UI

```
sudo htpasswd -b /etc/openshift/openshift-passwd admin test123
```

8. Add a policy for role Admin

```
oadm policy add-cluster-role-to-user cluster-admin admin
```

9. Allow all Docker Images

```
oc adm policy add-scc-to-group anyuid system:authenticated
```

10. Access UI https://<master_ip>:8443




11. From Console create test pods and service to check the functionality of router

```
oc create -f hello-pods.json
oc create -f hello-service.json
```

10. Install Ultrahook

   a. Install Ruby-Gem
   
   ```
   #!/bin/bash

   echo "Starting Installation..."

   # Install dependencies
   yum install gcc-c++ patch readline readline-devel zlib zlib-devel libyaml-devel libffi-devel openssl-devel make bzip2   autoconf automake libtool bison iconv-devel ruby-devel libxml2 libxml2-devel libxslt libxslt-devel git

   # Install Ruby from rvm
   \curl -sSL https://get.rvm.io | bash -s stable --ruby

   #Enable rvm in current shell
   source /usr/local/rvm/scripts/rvm

   #Install Bundler
   gem install bundler

   echo "Installation is completed now that was easy :)"
```

  b. Install Ultrahook
  
  ```
  gem install ultrahook -v 0.1.4
  ```

  c. Configure api_key
  
  ```
  echo "api_key: vaGhHaKT7Z1t0oid4b40Ijbtqgzj2xSl" > ~/.ultrahook
  ```






  ```
  [centos@ip-172-00-91-12 ~]$ ultrahook githook https://ec2-51-13-4-21.us-west- 1.compute.amazonaws.com:8443/oapi/v1/namespaces/github-build/buildconfigs/webapp/webhooks/7a10a789137beb54/generic &
[1] 95480
[centos@ip-172-100-92-11 ~]$ Authenticated as gokulpr
Forwarding activated...
http://githook.gokulpr.ultrahook.com -> https://ec2-54-183-94-212.us-west-1.compute.amazonaws.com:8443/oapi/v1/namespaces/github-build/buildconfigs/webapp/webhooks/7a10a789137beb54/generic
```



```
git clone https://github.com/gokulchandrap/Sample_PHP.git
[Sample_PHP]$ cat index.php
<?php
echo "<br>Welcome to OpenShift!!";
echo "<br>This is Deployment2";
echo "<br>############### .    Deployment_Name: Red .  ###################";

git add, commit and push
```






 
