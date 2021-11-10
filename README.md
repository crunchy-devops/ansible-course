# Ansible-course
Complete Ansible course on all topics such as create module, create filter,use dynamic inventories, AWX and much more 

##Installation du bac a sable   ---  Ansible  ----

Connectez-vous a la VM fournie lors du debut du cours

### Eviction du malware kinsing
```shell
./evict_malware.sh
```

### Pre-requis pour ubuntu 20.04 pour installer un virtualenv, Ansible et Docker
```shell
sudo apt update   # update all packages
sudo apt -y install sshpass # allow using ssh with a password
sudo apt -y install python3-venv
#fork and clone ---> git clone https://github.com/crunchy-devops/ansible-course.git
cd ansible-course
python3 -m venv venv # set up the module venv in the directory venv
source venv/bin/activate # activate the python virtualenv
pip3 install wheel # set for permissions purpose
pip3 install ansible # install ansible
pip3 install requests # extra packages 
ansible --version  # check version number , should be the latest 2.10.9+
cp inventory_template inventory
ansible-playbook -i inventory install_docker_ubuntu.yml --limit local # run a playbook
# Fermer votre IDE et le demarrer a nouveau pour que les changements soient appliques
cd ansible
source venv/bin/activate # activate the python virtualenv
docker ps 
```

### Installation de portainer pour verifier les resultats de nos tps
```shell
docker run -d --name portainer -p 29000:9000 -v /var/run/docker.sock:/var/run/docker.sock portainer/portainer -H unix:///var/run/docker.sock 
```

