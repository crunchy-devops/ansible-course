## Ce code a ete fait au debut de la formation
```shell
sudo apt update
sudo apt install -y python3.8-venv docker.io python3-pip
sudo usermod -aG docker $USER # change user , it should be in docker group 
python3 -V # check python version
```
### Installation d'Ansible
Fork et clone ce repository git clone https://github.com/crunchy-devops/ansible-course.git  
```shell
cd ansible-course
python3 -m venv venv # set up the module venv in the directory venv
source venv/bin/activate # activate the python virtualenv
python3 -V # vous etes sur la version 3.8.10 de python
pip3 install --upgrade pip # evite les erreurs sur la module de cryptographie
pip3 install wheel # set for permissions purpose
pip3 install ansible # install ansible
pip3 install sqlalchemy # install access to a postgres database from python if necessary
pip3 install psycopg2-binary # driver for postgres  if necessary
pip3 install natsort # for sorting alphanum required for filter code 
pip3 install requests # extra packages 
ansible --version  # check version number , should be the latest 2.13.12+
```

## Install  and compile Python 3.6.14
```shell
wget https://www.python.org/ftp/python/3.6.14/Python-3.6.14.tgz
tar -zxvf Python-3.6.14.tgz
cd Python-3.6.14
sudo apt-get update
sudo apt-get install -y build-essential checkinstall 
sudo apt-get install libreadline-gplv2-dev libncursesw5-dev libssl-dev libsqlite3-dev tk-dev libgdbm-dev libc6-dev libbz2-dev
./configure --enable-optimization --prefix=/opt/python3614
make -j2
make test
sudo make install
cd /opt
cd python3614/
./python3.6 -V
```
## Set a virtualenv for python 3.6.14
```shell


```



