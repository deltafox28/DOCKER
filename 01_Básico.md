# Instalando docker:

## Esse comando é usado para remover o Docker e todos os seus componentes do sistema operacional Linux:
```sh
sudo apt-get remove docker docker-engine docker.io containerd runc
```

## é usado para atualizar a lista de pacotes disponíveis nos repositórios do sistema operacional Linux:
```sh
sudo apt-get update
```

## Esse comando é utilizado para instalar pacotes e dependências necessárias para configurar repositórios de pacotes que utilizam o protocolo HTTPS, além de algumas ferramentas adicionais:
```sh
sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common
```

## Esse comando é usado para baixar a chave GPG (GNU Privacy Guard) do repositório oficial do Docker e adicioná-la à lista de chaves confiáveis do sistema operacional Linux. A chave GPG é usada para verificar a autenticidade dos pacotes baixados do repositório do Docker:
```sh
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

## Esse comando é usado para verificar a impressão digital (fingerprint) da chave GPG do repositório oficial do Docker, garantindo que a chave tenha sido baixada corretamente e não tenha sido comprometida ou alterada por terceiros:
```sh
sudo apt-key fingerprint 0EBFCD88
```

## Esse comando é usado para adicionar o repositório do Docker ao sistema operacional Linux. Ele configura o gerenciador de pacotes APT (Advanced Package Tool) para acessar o repositório do Docker ao instalar ou atualizar pacotes:
```sh
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```

## Esse comando é usado para atualizar a lista de pacotes disponíveis nos repositórios configurados no sistema operacional Linux. Ele verifica se há novas versões de pacotes disponíveis e atualiza a lista de pacotes disponíveis no seu sistema operacional:
```sh
sudo apt update
```

## Esse comando é usado para instalar o Docker no sistema operacional Linux. Ele instala os pacotes docker-ce e docker-ce-cli que contêm os componentes do Docker, e o pacote containerd.io que é usado para gerenciar os containers no Docker:
```sh
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

## Esse comando é usado para verificar se a instalação do Docker foi concluída com sucesso no seu sistema operacional Linux. Ele executa uma imagem de teste chamada "hello-world" que é baixada do Docker Hub e imprime uma mensagem simples na tela:
```sh
sudo docker run hello-world
```

## Esse comando é usado para iniciar o serviço do Docker no sistema operacional Linux, caso ele não tenha sido iniciado automaticamente durante a inicialização do sistema. O Docker é executado como um serviço do sistema operacional e precisa ser iniciado antes de ser usado:
```sh
sudo systemctl start docker
```

## Esse comando é usado para verificar se a instalação do Docker foi concluída com sucesso no seu sistema operacional Linux. Ele executa uma imagem de teste chamada "hello-world" que é baixada do Docker Hub e imprime uma mensagem simples na tela:
```sh
sudo docker run hello-world
```

# Aprenda a Iniciar um Container Docker:

## Esse comando é usado para listar as imagens Docker que foram baixadas e armazenadas no seu sistema operacional Linux. As imagens são a base para a criação de containers Docker:
```sh
sudo docker images
```

## Esse comando é usado para listar apenas os IDs das imagens Docker que estão armazenadas no sistema operacional Linux, em vez de listar todas as informações das imagens:
```sh
sudo docker images -q 
```

## Esse comando é usado para baixar a imagem do Ubuntu do Docker Hub para o seu sistema operacional Linux. O Docker Hub é um repositório de imagens Docker públicas e privadas:
```sh
docker pull ubuntu
```

## Esse comando é usado para iniciar um container Docker a partir da imagem do Ubuntu baixada anteriormente:
```sh
docker run ubuntu
```

## Esse comando é usado para criar um novo container Docker a partir da imagem do Ubuntu e nomeá-lo como "MyContainer". O parâmetro -it é usado para abrir um shell interativo dentro do container e o comando bash é usado para iniciar um shell do Bash dentro do container:
```sh
docker run --name MyContainer -it ubuntu bash
```

## Esse comando é usado para listar todos os containers Docker que estão em execução ou que já foram executados no seu sistema operacional Linux, juntamente com informações sobre eles, como o seu nome, ID, status, imagem, porta mapeada e tempo de execução:
```sh
sudo docker ps -a
```

## parar e iniciar o container:
```sh
sudo docker start MyContainer
sudo docker stop MyContainer
```

## Esse comando é usado para mostrar os processos em execução dentro do container Docker chamado "MyContainer". Ele exibe uma lista dos processos ativos, juntamente com informações como ID do processo, usuário que iniciou o processo, tempo de execução e uso de CPU e memória:
```sh
sudo docker top MyContainer
```

## Esse comando é usado para anexar o terminal do host ao terminal interativo do container chamado "MyContainer". Isso permite que você interaja diretamente com o shell dentro do container:
```sh
sudo docker attach MyContainer
```

## Esse comando é usado para mostrar estatísticas em tempo real sobre o uso de recursos de todos os containers em execução no seu sistema Docker. Ele exibe informações sobre o uso de CPU, memória, uso de rede e uso de disco de cada container:
```sh
docker stats
```

## Esse comando é usado para parar (encerrar) um container Docker em execução chamado "MyContainer". Ele envia um sinal de encerramento (SIGKILL) para o container, que interrompe imediatamente a execução do container:
```sh
sudo docker kill MyContainer
```

# Instalando uma imagem:

## 
Esse comando é usado para mostrar a versão atual do Docker instalada no seu sistema operacional Linux, juntamente com informações sobre a versão do cliente e do servidor:
```sh
docker version
```

## Esse comando é usado para criar e executar um container Docker a partir da imagem Alpine e executar o comando echo "Hello World" dentro do container:
```sh
docker container run alpine echo "Hello World"
```

## O comando começa com o comando docker container run, seguido pelo nome da imagem que será usada para criar o container, que neste caso é "centos". Em seguida, o comando ping -c 5 127.0.0.1 é passado como parâmetro para o container, que será executado dentro do container quando o container for iniciado:
```sh
docker container run centos ping -c 5 127.0.0.1
```

# Listando containers:

## Esse comando é usado para instalar o pacote jq no sistema operacional Linux. O jq é uma ferramenta de linha de comando que é usada para analisar e manipular dados JSON:
```sh
apt install jq
```

## Esse comando é usado para criar e executar um container Docker em segundo plano (detached mode) a partir da imagem "fundamentalsofdocker/trivia:ed2" e definir o nome do container como "trivia":
```sh
docker container run -d --name trivia fundamentalsofdocker/trivia:ed2
```

## ele é usado para listar todos os containers, inclusive aqueles que não estão em execução no momento:
```sh
docker container ls -l
```

## Esse comando é usado para remover um container Docker com o nome "trivia", forçando sua remoção mesmo que ele esteja em execução:
```sh
docker rm -f trivia
```

## Esse comando é usado para listar os containers Docker que estão em execução no momento.
```sh
docker container ls 
docker container ls -a (Esse comando é usado para listar todos os containers Docker, incluindo aqueles que não estão em execução no momento)
docker container ls -q (Esse comando é usado para listar apenas os IDs dos containers Docker que estão em execução no momento)
docker container rm -f $(docker container ls -a -q) (exclui todos os containers)
docker container ls -h (ajuda)
```

# Parando e iniciando containers:

## Esse comando é usado para criar e executar um container Docker em segundo plano (detached mode) a partir da imagem "fundamentalsofdocker/trivia:ed2" e definir o nome do container como "trivia":
```sh
docker container run -d --name trivia fundamentalsofdocker/trivia:ed2
```

## 
Esse comando é usado para exportar uma variável de ambiente chamada CONTAINER_ID, que recebe o ID do container Docker que foi criado a partir da imagem "fundamentalsofdocker/trivia:ed2" e recebeu o nome "trivia":
```sh
export CONTAINER_ID=$(docker container ls -a | grep trivia | awk '{print $1}')
```

## Esse comando é usado para parar o container Docker cujo ID foi armazenado na variável de ambiente CONTAINER_ID:
```sh
docker container stop $CONTAINER_ID
```

## Esse comando é usado para iniciar o container Docker que foi criado a partir da imagem "fundamentalsofdocker/trivia:ed2" e recebeu o nome "trivia":
```sh
docker container start trivia
```

## Esse comando é usado para remover um container Docker específico com base em seu ID:
```sh
docker container rm <container ID>
```

## Esse comando é usado para remover um container Docker específico com base em seu ID. O comando começa com o comando docker container rm, seguido do ID do container a ser removido:
```sh
docker container rm <container name>
```

## Esse comando é usado para visualizar informações detalhadas sobre um container Docker específico. O comando começa com o comando docker container inspect, seguido do nome ou ID do container a ser inspecionado:
```sh
docker container inspect trivia
```

## O comando começa com docker container inspect para obter as informações do container, seguido pela opção -f para especificar o formato de saída como uma string de modelo Go. O modelo Go usado neste comando é {{json .State}}, que extrai informações sobre o estado do container em formato JSON:
```sh
docker container inspect -f "{{json .State}}" trivia | jq .
```







































































