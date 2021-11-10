# Install glusterfs on Centos
## set cluster inventory 
Change all IP address in the file inventory_cluster to reflect your network settings
There are a host named leader and 2 slaves 
```shell script
[leader]
leader01 ansible_host=172.81.180.34 ansible_ssh_user=centos ansible_ssh_private_key=/home/centos/.ssh/id_rsa
[slave]
slave01 ansible_host=172.81.178.44 ansible_ssh_user=centos ansible_ssh_private_key=/home/centos/.ssh/id_rsa
slave02 ansible_host=170.75.164.12 ansible_ssh_user=centos ansible_ssh_private_key=/home/centos/.ssh/id_rsa
``` 
