# Install AWX on kind kubernetes


## install docker
```shell

    4  sudo apt update 
    5  sudo apt -y install sshpass
    6  sudo apt -y install python3-venv
    7  git clone  https://github.com/crunchy-devops/ansible-course.git
    8  cd ansible-course/
    9  python3 -m venv venv
   10  source venv/bin/activate
   11  pip3 install wheel
   12  pip3 install ansible
   13  ansible --version 
   14  ansible-playbook -i inventory install_docker_ubuntu.yml --limit local
   15  exit
   16  cd
   17  docker ps

```

## Start a cluster with kubernetes image version
Check on docker hub to get a valid image version
```shell
kind create cluster --name=kube --config kind-config.yml --image kindest/node:v1.22.7
```