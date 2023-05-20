# Criação de imagens com Docker Compose

## Baixe o binário do Docker Compose da versão desejada do GitHub Releases:
Defina as permissões corretas para o binário.
Crie um link simbólico para que o comando docker-compose esteja disponível no caminho de busca:
Verifique se a instalação foi bem-sucedida:
```sh
https://github.com/docker/compose/releases
curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
docker-compose --version
https://docs.docker.com/compose/install/
```

## Clonar o repositório Git:
Entrar no diretório do projeto:
Construir os serviços usando o Docker Compose:
Fazer upload das imagens para o Docker Hub:
```sh
git clone https://github.com/Mazuco/Linux.git
cd Linux/docker_compose/
docker-compose build
https://hub.docker.com/u/fundamentalsofdocker
```

## docker-compose up:
Inicia todos os serviços definidos no arquivo docker-compose.yml.
Os logs dos serviços são exibidos no console.
O comando fica em execução e os serviços continuam sendo executados.
docker-compose up -d:
Inicia todos os serviços definidos no arquivo docker-compose.yml em segundo plano (modo detached).
Os logs dos serviços não são exibidos no console.
O comando retorna imediatamente e os serviços continuam sendo executados em segundo plano.
docker-compose ps:
Lista todos os contêineres gerenciados pelo Docker Compose.
Exibe o status atual de cada serviço, como "up" (em execução) ou "exited" (parado).
docker-compose down:
Para todos os serviços e remove os contêineres associados.
Os volumes definidos no arquivo docker-compose.yml não são removidos.
docker-compose down -v:
Para todos os serviços, remove os contêineres associados e também remove os volumes definidos no arquivo docker-compose.yml.
docker-compose -p meu-app up:
Inicia todos os serviços definidos no arquivo docker-compose.yml, usando o nome do projeto "meu-app" em vez do nome do diretório.
Útil quando você deseja executar várias instâncias separadas do mesmo projeto.
```sh
docker-compose up
docker-compose up -d
docker-compose ps
docker-compose down
docker-compose down -v
docker-compose -p meu-app up
docker-compose down
```

##
```sh
E vamos alterar o arquivo "vim docker-compose.yml":
version: "2.4"
services:
  web:
    image: fundamentalsofdocker/ch11-web:2.0
    build: web
    ports:
      - 3000
  db:
    image: fundamentalsofdocker/ch11-db:2.0
    build: database
    volumes:
      - pets-data:/var/lib/postgresql/data

volumes:
  pets-data:
```

## O campo build foi atualizado para usar a chave context para especificar o diretório de contexto para a construção das imagens. Assume-se que os diretórios web e database estão presentes no mesmo diretório que o arquivo docker-compose.yml.
A porta 3000 foi mantida para o serviço web, indicando que ele será exposto na porta 3000.
O volume pets-data foi definido para o serviço db, especificando que o diretório /var/lib/postgresql/data dentro do contêiner será montado no volume pets-data.
```sh
docker-compose up -d
docker-compose up -d --scale web=3
```

## O comando docker-compose ps é usado para listar os serviços em execução definidos no arquivo docker-compose.yml e exibir o status de cada serviço. Certifique-se de executar o comando no mesmo diretório em que o arquivo docker-compose.yml está localizado.
Quanto ao comando curl -4 localhost:32768, ele é usado para fazer uma solicitação HTTP para o endereço localhost na porta 32768. O -4 é usado para forçar a utilização do protocolo IPv4.
```sh
docker-compose ps
curl -4 localhost:32768
```

# Criando um Docker Swarm

## O comando docker swarm init é usado para iniciar um cluster do Docker Swarm, que é uma ferramenta de orquestração de contêineres nativa do Docker. Ao executar esse comando em uma máquina, ela se torna o nó de controle principal do cluster e pode gerenciar outros nós.
```sh
docker swarm init
```

## O comando docker swarm join --token <join-token> <IP address>:2377 é usado para adicionar um nó (node) ao cluster do Docker Swarm. O <join-token> é um token de junção que é gerado quando você inicializa o swarm e o <IP address> é o endereço IP do nó de controle (manager) que você deseja se juntar.
Aqui está como usar o comando docker swarm join:
No nó que deseja adicionar ao swarm, abra um terminal ou console de comandos.
Execute o seguinte comando, substituindo <join-token> pelo token de junção apropriado e <IP address> pelo endereço IP do nó de controle que você deseja se juntar:
```sh
docker swarm join --token <join-token> <IP address>:2377
```

## 
O comando docker node ls é usado para listar todos os nós (nodes) em um cluster do Docker Swarm. Ele fornece informações sobre os nós, como ID, nome, status, disponibilidade, tipo de nó e versão do Docker.
O comando docker node inspect <node-id> é usado para obter informações detalhadas sobre um nó específico em um cluster do Docker Swarm. Ele fornece informações sobre o nó, incluindo sua configuração, endereços IP, recursos, tarefas em execução e muito mais.
```sh
docker node ls
docker node inspect pxb5lt3k8okl7n730wdkr9qn2
```

