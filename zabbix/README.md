# Zabbix 5.3+
## Installation
```shell
cd
wget https://repo.zabbix.com/zabbix/5.4/ubuntu/pool/main/z/zabbix-release/zabbix-release_5.4-1+ubuntu20.04_all.deb
sudo dpkg -i zabbix-release_5.4-1+ubuntu20.04_all.deb
sudo apt update
sudo apt install zabbix-server-pgsql zabbix-frontend-php php7.4-pgsql zabbix-apache-conf zabbix-sql-scripts zabbix-agent
# Installation de postgresql
docker run -d --name db -e POSTGRES_PASSWORD=password  -v /opt/postgres:/var/lib/postgresql/data \
  -p 5432:5432  systemdevformations/docker-postgres12  
  
```
## Creation de la base de donnees
```shell
# Connection a la base postgres
CREATE DATABASE zabbix;
CREATE ROLE zabbix WITH LOGIN ENCRYPTED PASSWORD 'zabbix';
```
## Peupler la base de donnees
```shell
cd /usr/share/doc/zabbix-sql-scripts/postgresql
sudo -s
docker cp create.sql.gz db:/tmp/create.sql.gz
# dans le container db avec portainer
adduser zabbix 
su - zabbix 
cd /tmp
zcat create.sql.gz | psql zabbix
```

## Changer le password pour le daemon zabbix_server 
```shell
cd /etc/zabbix/zabbix_server.conf
# change 
DBPassword=zabbix
```
## restart all processes
```shell
sudo systemctl restart zabbix-server zabbix-agent apache2
sudo systemctl enable zabbix-server zabbix-agent apache2
```
## Installer le GUI en PHP
Completer l'installation avec un navigateur
```http://<ip>/zabbix```







