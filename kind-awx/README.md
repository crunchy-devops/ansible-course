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
## install kind 




## Start a cluster with kubernetes image version
Check on docker hub to get a valid image version
```shell
kind create cluster --name=kube --config kind-config.yml --image kindest/node:v1.22.7  # recent 1.29.2
```

## install kubectl 
```shell
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt )/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
kubectl version --client
alias k='kubectl'
```

## install awx kubernetes operator 
```shell
sudo apt update
sudo apt -y install git build-essential
git clone https://github.com/ansible/awx-operator.git
export NAMESPACE=awx
k create ns ${NAMESPACE}
k config set-context --current --namespace=$NAMESPACE
cd awx-operator/
sudo apt -y install curl jq
RELEASE_TAG=`curl -s https://api.github.com/repos/ansible/awx-operator/releases/latest | grep tag_name | cut -d '"' -f 4`
echo $RELEASE_TAG
git checkout $RELEASE_TAG
make deploy
k get pods
```

## Install AWX ansible
```yaml
cat <<EOF | kubectl create -f -
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: static-data-pvc
  namespace: awx
spec:
  accessModes:
    - ReadWriteOnce
  storageClassName: local-path
  resources:
    requests:
      storage: 5Gi
EOF
```
```shell
k get pvc -n awx
vim awx-deploy.yml
```

```yaml
---
apiVersion: awx.ansible.com/v1beta1
kind: AWX
metadata:
  name: awx
spec:
  service_type: nodeport
  projects_persistence: true
  projects_storage_access_mode: ReadWriteOnce
  web_extra_volume_mounts: |
    - name: static-data
      mountPath: /var/lib/projects
  extra_volumes: |
    - name: static-data
      persistentVolumeClaim:
        claimName: static-data-pvc
```

```shell
k apply -f awx-deploy.yml
```