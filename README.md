# Ansible-course
Complete Ansible course on all topics such as create module, create filter, use dynamic inventory, AWX and much more 

##Set up a sandbox   ---  Ansible  ----

Connectez-vous a la VM controller fournie lors du debut du cours

### Eviction du malware kinsing - **A FAIRE SI NECESSAIRE** - 
```shell
./evict_malware.sh
```

### Pre-requis pour installer un virtualenv, Ansible et Docker sur Ubuntu 20.04
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
pip3 install sqlalchemy # install access to a postgres database from python
pip3 install psycopg2-binary # driver for postgres 
pip3 install natsort # for sorting alphanum 
pip3 install requests # extra packages 
ansible --version  # check version number , should be the latest 2.12.1+
cp inventory_template inventory
ansible-playbook -i inventory install_docker_ubuntu.yml --limit local # run a playbook
# Fermer votre IDE et le demarrer a nouveau pour que les changements soient appliques
cd ansible-course 
source venv/bin/activate # activate the python virtualenv
docker ps 
```

### Installation de portainer pour verifier les resultats de nos tps
```shell
docker run -d --name portainer -p 20000:9000 -v /var/run/docker.sock:/var/run/docker.sock portainer/portainer -H unix:///var/run/docker.sock 
```

### Mise en place des containers et du fichier inventory
Demarrer des containers pour simuler plusieurs machines.
```shell script
docker run -d --name target1 systemdevformations/ubuntu_ssh:v2  
docker run -d --name target2 systemdevformations/centos_ssh:v5 
docker run -d --name target3 --env ROOT_PASSWORD=Passw0rd systemdevformations/alpine-ssh:v1   
```
Verifier si les containers sont presents  
Faire un ```docker ps | grep systemdevformation ```

```shell script
CONTAINER ID        IMAGE                               COMMAND                  CREATED             STATUS                    PORTS                  NAMES
f5034036dc56        systemdevformations/alpine-ssh:v1   "/entrypoint.sh"         11 hours ago        Up 11 hours               22/tcp                 target3
c714f0b92509        systemdevformations/centos_ssh:v5   "/usr/bin/supervisor…"   23 hours ago        Up 23 hours (unhealthy)   22/tcp                 target2
6051c68c1712        systemdevformations/ubuntu_ssh:v2   "/usr/sbin/sshd -D"      23 hours ago        Up 23 hours               22/tcp                 target1              target1  
```  
et retrouver l'adresse IP des containers, et notez les
 ```shell script
docker network inspect bridge
```
Modifier le fichier inventory en fonction des adresses IP fournies pendant le cours (les VMs remote)       

### Creer un cle ssh et la deployer sur les machines remote
Dans votre home directory,  faire  
```ssh-keygen -t rsa -b 4096 ```  
Valider les parametres par defaut en tapant **enter** a chaque etape
sans passphrase  
Et  
```ssh-copy-id ubuntu@<remote_id_address>```    
Verifier l'acces ssh vers ces machines   

et faire la commande Ansible Ad-Hoc pour verifier si votre fichier inventory est correct.    
```ansible all -m ping -i inventory```  

Faire ensuite  les **Ad-Hoc commandes** suivantes une a une lentement:

```shell
# add apt-get install apt-transport-https in ubuntu OS 
ansible ubuntu -m apt -a "upgrade=yes update_cache=yes cache_valid_time=86400" -b -i inventory
ansible ubuntu -m apt -a "name=elinks state=latest" -i inventory
ansible ubuntu -m apt -b -a "name=elinks state=latest" -i inventory
ansible ubuntuvm -m apt -b -a "name=elinks state=latest" -i inventory
ansible all --list-hosts -i inventory
ansible target2 -i inventory -m setup
ansible all -m setup -a "filter=ansible_default_ipv4"  -i inventory
ansible all -m setup -a "filter=ansible_distribution"  -i inventory 
ansible all -m setup -a "filter=ansible_distribution_version"  -i inventory 
ansible all -m command -a "df -h" -i inventory
ansible ubuntu -m file -a "dest=/home/ubuntu/testfile state=touch" -i inventory 
```
### Presentation des groupes de hosts
Mettre a jour les adresses IP dans le fichier inventory_children sans en modifier
la structure.

### Premier script YAML
Dans la directory ansible-course, editez le fichier ansible_ping.yml, et etudiez le code.

### Premieres commandes ansible-playbook
 ```shell script
ansible-playbook  -i inventory_children ansible_ping.yml  --limit ubuntuvm
ansible-playbook  -i inventory_children ansible_pin.yml  --limit slave
```




