# Tutorial on AWX 

## install AWX version using tar.gz file
```shell
sudo apt install git wget
wget https://github.com/ansible/awx/archive/17.1.0.tar.gz
tar -zxvf 17.1.0.tar.gz # this version is suitable for Docker
cd awx-17.1.0
cd installer
```
## Setup the python virtualenv 
```shell
python3 -m venv venv 
source venv/bin/activate  # activate the virtual env 
sudo python3 -m pip install -U pip
sudo pip3 install wheel   # install wheel permissions
sudo pip3 install ansible
sudo pip3 install docker   # library python pour ansible
sudo pip3 install docker-compose # pour les containers d'AWX
```
# Add credential file 
```shell
vi vars.yml
# add these lines
admin_password: 'adminpass'
pg_password: 'pgpass'
secret_key: 'mysupersecret'
do Esc and :wq
ansible-playbook -i inventory install.yml -e @vars.yml
```

## AWX Tutorial
Create a user  
Create a team  
Create credentials, one source control  
and SSH Machine Type, copy your private ssh key    
Create a projet 
Create an inventory  
Create a Job Template  
Execute the Job Template  

## Note: 
## Add a custom python import in AWX container
```shell 
docker exec -it awx_task /bin/bash
source /var/lib/awx/venv/ansible/bin/activate
(ansible) bash-4.4# pip install natsort
exit
```







