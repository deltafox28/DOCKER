# Criando um Swarm local no VirtualBox

##
```sh
https://sempreupdate.com.br/como-instalar-o-virtualbox-6-1-no-fedora-centos-e-rhel/(Este link é um guia passo a passo para instalar o VirtualBox 6.1 no Fedora, CentOS e Red Hat Enterprise Linux (RHEL). O VirtualBox é uma ferramenta de virtualização que permite criar e executar máquinas virtuais em um sistema operacional hospedeiro.)

https://phoenixnap.com/kb/install-virtualbox-on-ubuntu(Este é um guia semelhante ao anterior, mas destinado à instalação do VirtualBox no sistema operacional Ubuntu. O Ubuntu é uma distribuição popular do Linux e o VirtualBox pode ser usado para criar máquinas virtuais nele.)

Link de ref: https://docs.docker.com/machine/install-machine/(Este é um link de referência para a documentação oficial do Docker. Ele fornece informações sobre como instalar o Docker Machine, uma ferramenta que permite criar e gerenciar máquinas virtuais com Docker instalado nelas. O Docker é uma plataforma de contêiner que permite empacotar, distribuir e executar aplicativos em contêineres isolados.)
```

## base=https://github.com/docker/machine/releases/download/v0.16.0
Esta parte define a variável base com a URL base para o download do binário do Docker Machine. Neste caso, está apontando para a versão 0.16.0 do Docker Machine.
curl -L $base/docker-machine-$(uname -s)-$(uname -m) >/tmp/docker-machine
Este comando usa o curl para fazer o download do binário do Docker Machine. A URL completa é construída com base no sistema operacional ($(uname -s)) e na arquitetura do sistema ($(uname -m)).
O parâmetro -L é usado para seguir redirecionamentos, caso ocorram.
O redirecionamento de saída >/tmp/docker-machine redireciona o conteúdo baixado para o arquivo /tmp/docker-machine.
sudo mv /tmp/docker-machine /usr/local/bin/docker-machine
Este comando move o arquivo do Docker Machine baixado para o diretório /usr/local/bin/. A utilização do sudo garante que o comando seja executado com privilégios de superusuário.
chmod +x /usr/local/bin/docker-machine
Este comando adiciona permissão de execução ao arquivo do Docker Machine, permitindo que ele seja executado como um programa.
```sh
base=https://github.com/docker/machine/releases/download/v0.16.0 && curl -L $base/docker-machine-$(uname -s)-$(uname -m) >/tmp/docker-machine && sudo mv /tmp/docker-machine /usr/local/bin/docker-machine && chmod +x /usr/local/bin/docker-machine
```

## docker-machine create --driver virtualbox default
Este comando cria uma nova máquina virtual chamada "default" usando o driver "virtualbox" no Docker Machine. O driver "virtualbox" permite que o Docker Machine crie a máquina virtual usando o VirtualBox como provedor de virtualização. Após a execução deste comando, uma nova máquina virtual será criada e configurada.
docker-machine ls
Este comando lista todas as máquinas virtuais gerenciadas pelo Docker Machine. Ao executar este comando, você verá uma tabela que mostra o nome, o driver, o estado (se a máquina virtual está ativa ou não) e outros detalhes relevantes de cada máquina virtual.
docker-machine start default
Este comando inicia a máquina virtual chamada "default" que foi criada anteriormente. Se a máquina virtual estiver desligada, esse comando a iniciará. Isso é necessário antes de executar comandos Docker na máquina virtual.
```sh
docker-machine create --driver virtualbox default
docker-machine ls
docker-machine start default
```

## O comando que você compartilhou é um loop em bash que utiliza o Docker Machine para criar várias máquinas virtuais com o nome "node-N" (onde N varia de 1 a 5) usando o driver "virtualbox". Cada iteração do loop cria uma nova máquina virtual.
```sh
for NODE in `seq 1 5`; do 
docker-machine create --driver virtualbox "node-${NODE}"
done
```

## Após a criação das máquinas virtuais, você pode executar os seguintes comandos para listar as máquinas virtuais, iniciar todas elas, e exportar o endereço IP da máquina "node-1":
```sh
docker-machine ls
docker-machine start node-1 node-2 node-3 node-4 node-5
export IP=$(docker-machine ip node-1)
```

## docker-machine ssh node-1 docker swarm init --advertise-addr $IP
Esse comando executa uma conexão SSH com a máquina virtual "node-1" usando o Docker Machine. Em seguida, ele executa o comando docker swarm init na máquina virtual.
O parâmetro --advertise-addr $IP especifica o endereço IP para o qual outros nós do Swarm devem se comunicar com o nó "node-1".
O comando docker swarm init inicia a configuração de um novo cluster Swarm do Docker na máquina virtual "node-1".
export JOIN_TOKEN=$(docker-machine ssh node-1 \ docker swarm join-token worker -q)
Esse comando também executa uma conexão SSH com a máquina virtual "node-1" usando o Docker Machine. Em seguida, ele executa o comando docker swarm join-token worker -q na máquina virtual para obter o token de junção para os nós trabalhadores (workers) do Swarm.
O token de junção é armazenado na variável de ambiente "JOIN_TOKEN" para uso posterior.
```sh
docker-machine ssh node-1 docker swarm init --advertise-addr $IP
export JOIN_TOKEN=$(docker-machine ssh node-1 \ docker swarm join-token worker -q)
```

## O comando que você compartilhou é um loop em bash que utiliza o Docker Machine para adicionar nós trabalhadores (workers) ao cluster Swarm do Docker. Ele itera de 2 a 5, adicionando cada nó ao cluster. Com esse loop, cada nó de 2 a 5 será adicionado como um nó trabalhador (worker) ao cluster Swarm. 
```sh
for NODE in `seq 2 5`; do
NODE_NAME="node-${NODE}"
docker-machine ssh $NODE_NAME docker swarm join \
--token $JOIN_TOKEN $IP:2377
done
```

## docker-machine ssh node-1 docker node promote node-2 node-3
Esse comando executa uma conexão SSH com a máquina virtual "node-1" usando o Docker Machine. Em seguida, ele executa o comando docker node promote na máquina virtual para promover os nós "node-2" e "node-3" no cluster Swarm.
A promoção de nós no cluster Swarm significa que esses nós serão elegíveis para executar tarefas e serviços no cluster. Eles passarão de nós trabalhadores (workers) para nós gerenciadores (managers).
docker-machine ssh node-1 docker node ls
Esse comando executa uma conexão SSH com a máquina virtual "node-1" usando o Docker Machine. Em seguida, ele executa o comando docker node ls na máquina virtual para listar os nós presentes no cluster Swarm.
O comando docker node ls retorna informações sobre todos os nós no cluster, incluindo seu ID, nome, estado, disponibilidade, papel (worker ou manager) e outras informações relevantes.
```sh
docker-machine ssh node-1 docker node promote node-2 node-3
docker-machine ssh node-1 docker node ls
```

# Implantar um primeiro aplicativo

## Ao executar o comando docker-machine ssh node-1, você está iniciando uma conexão SSH com a máquina virtual chamada "node-1" usando o Docker Machine. Esse comando permite que você acesse a linha de comando da máquina virtual diretamente.
```sh
docker-machine ssh node-1
```

## Este arquivo YAML define uma pilha de serviços com uma única definição de serviço chamado "whoami". Aqui estão os principais componentes desse arquivo:
version especifica a versão da sintaxe do arquivo Compose.
services define a configuração do serviço "whoami".
image define a imagem Docker a ser usada para o serviço "whoami".
networks especifica a rede a ser atribuída ao serviço. Neste caso, é a rede "test-net".
ports mapeia a porta 81 do host para a porta 8000 do serviço "whoami".
deploy contém configurações de implantação adicionais para o serviço.
replicas define o número de réplicas (instâncias) do serviço a serem implantadas, neste caso, 6 réplicas.
update_config define a configuração de atualização do serviço.
parallelism especifica quantas réplicas podem ser atualizadas simultaneamente, neste caso, 2 réplicas.
delay define um atraso de 10 segundos entre as atualizações das réplicas.
labels atribui rótulos personalizados ao serviço para fins de identificação e filtragem.
networks define a configuração da rede "test-net", usando o driver de rede "overlay".
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

## O comando docker stack deploy -c stack.yaml sample-stack é utilizado para implantar uma pilha de serviços no Docker Swarm usando um arquivo de configuração YAML. Nesse caso, o arquivo de configuração é o stack.yaml e a pilha de serviços será denominada sample-stack. Já os comandos docker stack ls e docker service ls são utilizados para listar informações sobre as pilhas e serviços implantados no Docker Swarm, respectivamente.
```sh
https://hub.docker.com/r/training/whoami

docker stack deploy -c stack.yaml sample-stack
docker stack ls
docker service ls
```

## O comando docker service inspect sample-stack_whoami é usado para obter informações detalhadas sobre um serviço específico no Docker Swarm. No seu caso, o serviço é chamado sample-stack_whoami. Já o comando docker service logs n4l77de4vwgliocue9uroquum é utilizado para visualizar os logs de um serviço específico no Docker Swarm. Nesse caso, o serviço é identificado pelo seu ID, que é n4l77de4vwgliocue9uroquum.
```sh
docker service inspect sample-stack_whoami
docker service logs n4l77de4vwgliocue9uroquum
```

# Implementar um multi-service

## Os comandos que você compartilhou são relacionados à inicialização das máquinas virtuais Docker Machine, listagem dos status das máquinas virtuais e acesso SSH à máquina virtual "node-1".
```sh
docker-machine start node-1 node-2 node-3 node-4 node-5
docker-machine ls
docker-machine ssh node-1 docker node ls
docker-machine ssh node-1
```

## Este arquivo YAML define uma pilha de serviços para uma aplicação de pets com um serviço web e um banco de dados. Aqui estão os principais componentes desse arquivo:
version especifica a versão da sintaxe do arquivo Compose.
services define a configuração dos serviços da pilha.
web define o serviço web.
image define a imagem Docker a ser usada para o serviço web.
networks especifica a rede a ser atribuída ao serviço. Neste caso, é a rede "pets-net".
ports mapeia a porta 3000 do host para a porta 3000 do serviço web.
deploy contém configurações de implantação adicionais para o serviço web.
replicas define o número de réplicas (instâncias) do serviço web a serem implantadas, neste caso, 3 réplicas.
db define o serviço de banco de dados.
image define a imagem Docker a ser usada para o serviço de banco de dados.
networks especifica a rede a ser atribuída ao serviço. Neste caso, é a rede "pets-net".
volumes define o volume a ser montado para persistir os dados do banco de dados.
networks define a configuração da rede "pets-net", usando o driver de rede "overlay".
volumes define o volume chamado "pets-data" que será usado para persistir os dados do banco de dados.
Esse arquivo YAML pode ser usado com ferramentas como o Docker Compose para criar e implantar a pilha de serviços definida.
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

## docker stack deploy -c pet-stack.yaml pets
Esse comando é usado para implantar uma pilha de serviços no Docker Swarm usando um arquivo de configuração YAML.
O arquivo de configuração YAML utilizado é o pet-stack.yaml.
A pilha de serviços será denominada pets.
Após a execução desse comando, a pilha de serviços será criada e configurada com base nas definições no arquivo YAML.
docker stack ps pets
Esse comando lista os serviços e suas respectivas tarefas (tasks) dentro da pilha pets.
Ele exibe informações como o nome do serviço, a ID da tarefa, o nó onde a tarefa está sendo executada e o status atual da tarefa.
curl localhost:3000/pet
Esse comando é usado para enviar uma solicitação HTTP GET para o serviço web em execução na porta 3000 do host local.
Nesse caso, está sendo feita uma solicitação para a rota /pet.
A resposta retornada pelo serviço web será exibida no terminal.
docker stack rm pets
Esse comando é utilizado para remover a pilha de serviços chamada pets do Docker Swarm.
Após a execução desse comando, todos os serviços, tarefas e recursos associados à pilha pets serão removidos do cluster.
```sh
docker stack deploy -c pet-stack.yaml pets
docker stack ps pets
curl localhost:3000/pet
docker stack rm pets
```














