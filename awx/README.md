# Tutorial on AWX 

## Install on Ubuntu

## install Docker on ubuntu 24.04
```shell
sudo apt update
sudo apt install -y curl apt-transport-https ca-certificates software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
sudo apt install docker-ce -y
sudo systemctl status docker
sudo usermod -aG docker $USER
# log out log in again
docker ps # check
```

## install docker-compose
```shell
sudo curl -L "https://github.com/docker/compose/releases/download/v2.30.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/bin/docker-compose
sudo chmod +x /usr/bin/docker-compose 
docker-compose version 
```

## install docker + docker-compose with python venv and Ansible
```shell
sudo apt update
sudo apt -y install python3-venv
cd jenkins-pic/
python3 -m venv venv
source venv/bin/activate
pip3 install wheel
pip3 install ansible
pip3 install setuptools
ansible-playbook -i inventory install_docker_ubuntu.yml --limit local
sudo curl -L "https://github.com/docker/compose/releases/download/v2.32.4/docker-compose-linux-x86_64" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
docker-compose --version
#pip3 install "cython<3.0.0" wheel && pip3 install pyyaml==5.4.1 --no-build-isolation
#pip3 install docker-compose
```

## install on kubernetes kind
```shell
wget https://github.com/kubernetes-sigs/kind/releases/download/v0.27.0/kind-linux-amd64
mv kind-linux-amd64 kind
chmod +x kind
sudo mv kind /usr/local/bin/kind
kind version # should be  version 0.27.0
```
## install kubectl
```shell
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/kubectl
# add in .bashrc
alias ks='kubectl'
source <(kubectl completion bash | sed s/kubectl/ks/g)
# check 
kubectl version
```

## Create a cluster
```shell
cd /home/ubuntu/jenkins-pic/kind
kind create cluster --name awx --config kind-config-cluster.yml
ks version # should be version  v1.32.2+
ks get nodes # see one controle-plane and 3 workers
```

## install AWX
```shell
cd
git clone https://github.com/ansible/awx-operator.git
cd awx-operator/
git checkout tags/2.19.1
git log --oneline  # HEAD should be on tag 2.19.1 #hash dd37ebd
export VERSION=2.19.1
```

### Manually create file kustomization.yaml
```yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
resources:
  # Find the latest tag here: https://github.com/ansible/awx-operator/releases
  - github.com/ansible/awx-operator/config/default?ref=2.19.1
  - awx-demo.yml
# Set the image tags to match the git version from above
images:
  - name: quay.io/ansible/awx-operator
    newTag: 2.19.1

# Specify a custom namespace in which to install AWX
namespace: awx
```
```ks create ns awx```
```ks apply -k . ```  # run twice(??) this command

Wait 15 minutes
And check with ```ks get pod -A``` # all K8s objects should be running, completed 

## User AWX
username admin
Password uses the command below
```shell
kubectl get secret -n awx  awx-demo-admin-password -o jsonpath="{.data.password}" | base64 --decode ; echo
```
## Web access
```
kubectl port-forward -n awx service/awx-demo-service 30540:80 --address='0.0.0.0' &
```
access to AWX with http://<ip>:30540


## Troubleshooting to prevent job template failure in AWX
```shell
echo fs.inotify.max_user_watches=655360 | sudo tee -a /etc/sysctl.conf
echo fs.inotify.max_user_instances=1280 | sudo tee -a /etc/sysctl.conf
echo fs.file-max = 2097152 | sudo tee -a /etc/sysctl.conf
sudo sysctl -p
```

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
python3 -m pip install -U pip
pip3 install wheel   # install wheel permissions
pip3 install ansible
pip3 install docker   # library python pour ansible
pip3 install docker-compose # pour les containers d'AWX
```
# Add credential file
Change host_port value at line 67  to 31111 in the inventory file
```shell
# creer un fichier
vi vars.yml
# add these lines
admin_password: 'adminpass'
pg_password: 'pgpass'
secret_key: 'mysupersecret'
# do Esc and :wq
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
(ansible) bash-4.4# pip3 install natsort
exit
```

## Ou installer la derniere version stable d'AWX
```
git clone -b 23.2.0 https://github.com/ansible/awx.git
```
See tools/docker-compose/_sources/secrets/






