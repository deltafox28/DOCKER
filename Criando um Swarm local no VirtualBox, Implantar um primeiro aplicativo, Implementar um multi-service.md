# Criando um Swarm local no VirtualBox

##
```sh
https://sempreupdate.com.br/como-instalar-o-virtualbox-6-1-no-fedora-centos-e-rhel/
https://phoenixnap.com/kb/install-virtualbox-on-ubuntu
Link de ref: https://docs.docker.com/machine/install-machine/
```

##
```sh
base=https://github.com/docker/machine/releases/download/v0.16.0 && curl -L $base/docker-machine-$(uname -s)-$(uname -m) >/tmp/docker-machine && sudo mv /tmp/docker-machine /usr/local/bin/docker-machine && chmod +x /usr/local/bin/docker-machine
```

##
```sh
docker-machine create --driver virtualbox default
docker-machine ls
docker-machine start default
```

##
```sh
for NODE in `seq 1 5`; do 
docker-machine create --driver virtualbox "node-${NODE}"
done
```

##
```sh
docker-machine ls
docker-machine start node-1 node-2 node-3 node-4 node-5
export IP=$(docker-machine ip node-1)
```

##
```sh
docker-machine ssh node-1 docker swarm init --advertise-addr $IP
export JOIN_TOKEN=$(docker-machine ssh node-1 \ docker swarm join-token worker -q)
```

##
```sh
for NODE in `seq 2 5`; do
NODE_NAME="node-${NODE}"
docker-machine ssh $NODE_NAME docker swarm join \
--token $JOIN_TOKEN $IP:2377
done
```

##
```sh
docker-machine ssh node-1 docker node promote node-2 node-3
docker-machine ssh node-1 docker node ls
```

# Implantar um primeiro aplicativo

##
```sh
docker-machine ssh node-1
```

##
```sh
CUIDADO COM A INDENTAÇÃO!
CUIDADO COM O COPIA E COLA!

vim stack.yaml

version: "3.7"
services:
   whoami:
     image: training/whoami:latest
     networks:
       - test-net
     ports: 
       - 81:8000
     deploy:
      replicas: 6
      update_config:
       parallelism: 2
       delay: 10s
      labels:
       app: sample-app
       environment: prod-south
networks:
 test-net:
  driver: overlay
```

##
```sh
https://hub.docker.com/r/training/whoami

docker stack deploy -c stack.yaml sample-stack
docker stack ls
docker service ls
```

##
```sh
docker service inspect sample-stack_whoami
docker service logs n4l77de4vwgliocue9uroquum
```

# Implementar um multi-service

##
```sh
docker-machine start node-1 node-2 node-3 node-4 node-5
docker-machine ls
docker-machine ssh node-1 docker node ls
docker-machine ssh node-1
```

##
```sh
CUIDADO COM A INDENTAÇÃO!
CUIDADO COM O COPIA E COLA!
USE UM 'IDE' PARA NÃO DESFORMATAR!


vi pet-stack.yaml

version: "3.7"
services:
   web:
     image: fundamentalsofdocker/ch11-web:2.0
     networks:
       - pets-net
     ports:
       - 3000:3000
     deploy:
       replicas: 3
   db:
     image: fundamentalsofdocker/ch11-db:2.0
     networks:
       - pets-net
     volumes:
       - pets-data:/var/lib/postgresql/data

networks:
 pets-net:
  driver: overlay

volumes:
 pets-data:
```

##
```sh
docker stack deploy -c pet-stack.yaml pets
docker stack ps pets
curl localhost:3000/pet
docker stack rm pets
```














