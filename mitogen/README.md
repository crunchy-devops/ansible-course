# TP sur mitogen 

## Utilisation de docker 
```shell
git switch mitogen 
cd mitogen
docker build -t mitogen .
cd ..
docker run -d --name mitogen-example -v .:/tmp/ansible-course mitogen
docker exec -it mitogen-example /bin/bash
/root/.local/bin/ansible local -i inventory_mitogen -m setup 
```
## Test de performance sur 3 machines remote


