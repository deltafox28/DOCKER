# Configurando as portas no Docker

## O primeiro comando docker container run --name web -P -d nginx:alpine cria um novo container com o nome web, baseado na imagem nginx:alpine e em modo de execução destacado (-d). A opção -P publica todas as portas expostas pelo container para portas aleatórias no host.
O segundo comando docker container port web exibe as portas publicadas do container web. Isso mostra a porta aleatória que o Docker atribuiu à porta padrão do servidor web (80) dentro do container.
O terceiro comando docker container inspect web | grep HostPort exibe o número da porta do host em que o Docker publicou a porta do container web. A opção grep HostPort filtra a saída do comando docker container inspect para exibir apenas a porta do host atribuída pelo Docker.
```sh
docker container run --name web -P -d nginx:alpine
docker container port web
docker container inspect web | grep HostPort
```

## O primeiro comando docker container ls lista todos os containers em execução no host.
O segundo comando docker container run --name web2 -p 8080:80 -d nginx:alpine cria um novo container com o nome web2, baseado na imagem nginx:alpine e em modo de execução destacado (-d). A opção -p é usada para mapear a porta 80 do container para a porta 8080 do host.
O terceiro comando docker container ls lista novamente todos os containers em execução no host. Dessa vez, ele deve exibir dois containers: web e web2, ambos em execução em segundo plano.
```sh
docker container ls
docker container run --name web2 -p 8080:80 -d nginx:alpine
docker container ls
```

# Aplicação monolítica no Docker

## O comando "git clone" é usado para fazer uma cópia de um repositório Git para o seu ambiente local. No caso específico do comando "git clone https://github.com/Mazuco/Linux.git", você está clonando o repositório chamado "Linux" hospedado no GitHub para o seu ambiente local.
Após clonar o repositório, o próximo comando é "cd Linux/e-shop/monolith/", que permite navegar para o diretório "Linux/e-shop/monolith/" no seu sistema de arquivos. Esse comando é usado para entrar no diretório que você acabou de clonar.
Em seguida, temos o comando "pip3 install -r requirements.txt". Isso instala as dependências necessárias para o projeto contidas no arquivo "requirements.txt". O "pip3" é um gerenciador de pacotes do Python.
Por fim, temos "export FLASK_APP=main.py". Essa linha define a variável de ambiente "FLASK_APP" e atribui a ela o valor "main.py". A variável de ambiente é usada pelo Flask, um framework web em Python, para identificar qual arquivo contém a aplicação principal (o ponto de entrada) do projeto.
```sh
git clone https://github.com/Mazuco/Linux.git
cd Linux/e-shop/monolith/
pip3 install -r requirements.txt
export FLASK_APP=main.py
```

## 
O comando "flask run" é usado para iniciar a aplicação Flask. Após executar esse comando no diretório do projeto, o servidor web embutido do Flask será iniciado e a aplicação começará a ser executada.
Já o comando "curl localhost:5000/catalog?category=bicycles" é uma solicitação HTTP feita através da ferramenta cURL. Ele está fazendo uma solicitação GET para o endpoint "/catalog" da aplicação Flask, passando o parâmetro "category" com o valor "bicycles". Essa solicitação é feita ao servidor local em "localhost" na porta 5000.
```sh
flask run
curl localhost:5000/catalog?category=bicycles
```

## O comando "vim /etc/hosts" é usado para editar o arquivo de hosts do sistema. O arquivo "/etc/hosts" é um arquivo de texto no sistema operacional Linux que mapeia nomes de host para endereços IP.
No exemplo fornecido, a linha "127.0.0.1 acme.com" está sendo adicionada ao arquivo de hosts. Isso significa que o nome de host "acme.com" está sendo associado ao endereço IP loopback "127.0.0.1", que é o endereço IP padrão usado para se referir à própria máquina local.
```sh
vim /etc/hosts
127.0.0.1 acme.com
```

## Ao executar o comando "ping acme.com", você está enviando pacotes ICMP (Internet Control Message Protocol) para o nome de host "acme.com" a fim de verificar a conectividade com o servidor associado a esse domínio. O comando "ping" envia pacotes para o endereço IP correspondente ao nome de host e aguarda a resposta do servidor.
Quanto ao comando "python3 main.py", ele é usado para executar o arquivo "main.py" usando a versão 3 do Python. Esse comando presumivelmente executa o aplicativo principal do projeto, conforme definido no arquivo "main.py". A execução desse comando iniciará o servidor Flask e sua aplicação estará disponível na porta configurada (geralmente a porta 5000) para receber solicitações HTTP.
```sh
ping acme.com
python3 main.py
```

## Define a imagem base como "python:3.7-alpine". O Alpine é uma distribuição Linux leve, que é útil para imagens Docker menores em tamanho.
Define o diretório de trabalho dentro do contêiner como "/app". Isso é o local onde o aplicativo será copiado.
Copia o arquivo "requirements.txt" para o diretório de trabalho no contêiner.
Executa o comando "pip install -r requirements.txt" para instalar as dependências listadas no arquivo "requirements.txt".
Copia todo o conteúdo local (do host) para o diretório de trabalho no contêiner.
Expõe a porta 5000 do contêiner para que possa ser acessada externamente.
Define o comando "python main.py" para ser executado quando o contêiner for iniciado.
Após criar ou editar o arquivo "Dockerfile", você pode construir uma imagem Docker com o comando "docker image build -t acme/eshop:1.0 .". Esse comando irá construir uma imagem Docker com a tag "acme/eshop:1.0", usando o contexto atual (o diretório em que você está executando o comando). A imagem resultante terá o nome "acme/eshop" e a versão "1.0".
```sh
vim Dockerfile

FROM python:3.7-alpine
WORKDIR /app
COPY requirements.txt ./
RUN pip install -r requirements.txt
COPY . .
EXPOSE 5000
CMD python main.py

docker image build -t acme/eshop:1.0 .
```

## A opção "--rm" remove automaticamente o contêiner quando ele for encerrado.
A opção "-it" permite interação com o terminal, tornando possível visualizar logs e interagir com o contêiner.
A opção "--name eshop" define o nome do contêiner como "eshop".
A opção "-p 5000:5000" mapeia a porta 5000 do contêiner para a porta 5000 do host, permitindo acessar o aplicativo Flask que está sendo executado dentro do contêiner.
O último argumento "acme/eshop:1.0" especifica o nome da imagem Docker e sua versão.
```sh
docker container run --rm -it \
--name eshop \
-p 5000:5000 \
acme/eshop:1.0
```

## 
Se você configurou a entrada "127.0.0.1 acme.com" no arquivo de hosts (/etc/hosts) como mencionado anteriormente, você pode usar o comando curl para enviar solicitações HTTP para "acme.com" na porta 5000.
O comando "curl http://acme.com:5000/catalog?category=bicycles" envia uma solicitação GET para o endpoint "/catalog" com o parâmetro "category" definido como "bicycles". Ele retorna a resposta do servidor para essa solicitação.
O comando "curl http://acme.com:5000/checkout" envia uma solicitação GET para o endpoint "/checkout". Ele também retorna a resposta do servidor para essa solicitação.
No entanto, o comando "cd catalog" não está relacionado ao uso do cURL. "cd catalog" é um comando do sistema de arquivos que permite entrar no diretório "catalog" no seu sistema. 
```sh
curl http://acme.com:5000/catalog?category=bicycles
curl http://acme.com:5000/checkout
cd catalog
```

## Executar o comando docker image build -t acme/catalog:1.0 . para construir a imagem Docker. Esse comando usa o Dockerfile presente no diretório atual (representado por "."). Ele cria uma imagem com o nome "acme/catalog" e a tag "1.0".
Em seguida, execute o comando docker run --rm -it --name catalog -p 3000:3000 acme/catalog:1.0 para iniciar um contêiner com a imagem recém-criada. Esse comando inicia o contêiner com o nome "catalog", expõe a porta 3000 do contêiner para a porta 3000 do host e remove o contêiner automaticamente quando ele é encerrado (-rm).
Por fim, você pode usar o comando curl http://acme.com:3000/catalog?type=bicycle para enviar uma solicitação GET para o endpoint "/catalog" do aplicativo que está sendo executado no contêiner. O parâmetro "type" é definido como "bicycle". Isso retornará a resposta do servidor para essa solicitação.
```sh
docker image build -t acme/catalog:1.0 .
docker run --rm -it --name catalog -p 3000:3000 acme/catalog:1.0
curl http://acme.com:3000/catalog?type=bicycle
```

## --rm: Remove automaticamente o contêiner quando ele for encerrado.
-d: Executa o contêiner em modo detached, em segundo plano.
--name catalog: Define o nome do contêiner como "catalog".
--label traefik.enable=true: Ativa o uso do Traefik como proxy reverso para o contêiner.
--label traefik.port=3000: Especifica a porta interna do contêiner na qual o aplicativo está escutando (no caso, porta 3000).
--label traefik.priority=10: Define a prioridade do roteamento no Traefik (quanto menor o número, maior a prioridade).
--label traefik.http.routers.catalog.rule="Host(\"acme.com\") && PathPrefix(\"/catalog\")": Define a regra de roteamento para o contêiner no Traefik. Essa regra especifica que o domínio é "acme.com" e o prefixo do caminho é "/catalog". Portanto, as solicitações para "acme.com/catalog" serão direcionadas para esse contêiner.
```sh
docker container run --rm -d \
--name catalog \
--label traefik.enable=true \
--label traefik.port=3000 \
--label traefik.priority=10 \
--label traefik.http.routers.catalog.rule="Host(\"acme.com\")
&& PathPrefix(\"/catalog\")" \
acme/catalog:1.0
```

## --rm: Remove automaticamente o contêiner quando ele for encerrado.
-d: Executa o contêiner em modo detached, em segundo plano.
--name eshop: Define o nome do contêiner como "eshop".
--label traefik.enable=true: Ativa o uso do Traefik como proxy reverso para o contêiner.
--label traefik.port=5000: Especifica a porta interna do contêiner na qual o aplicativo está escutando (no caso, porta 5000).
--label traefik.priority=1: Define a prioridade do roteamento no Traefik (quanto menor o número, maior a prioridade).
--label traefik.http.routers.eshop.rule="Host(\"acme.com\")": Define a regra de roteamento para o contêiner no Traefik. Essa regra especifica que o domínio é "acme.com". Portanto, as solicitações para "acme.com" serão direcionadas para esse contêiner.
```sh
docker container run --rm -d \
--name eshop \
--label traefik.enable=true \
--label traefik.port=5000 \
--label traefik.priority=1 \
--label traefik.http.routers.eshop.rule="Host(\"acme.com\")" \
acme/eshop:1.0
```

## -d: Executa o contêiner em modo detached, em segundo plano.
--name traefik: Define o nome do contêiner como "traefik".
-p 8080:8080: Mapeia a porta 8080 do contêiner para a porta 8080 do host, permitindo acessar a interface de administração do Traefik.
-p 80:80: Mapeia a porta 80 do contêiner para a porta 80 do host, permitindo redirecionamento de tráfego HTTP.
-v /var/run/docker.sock:/var/run/docker.sock: Monta o socket do Docker dentro do contêiner, permitindo que o Traefik se comunique com o Docker e obtenha informações sobre os serviços em execução.
traefik:v2.0: Especifica a imagem do Traefik e sua versão.
--api.insecure=true: Habilita a interface de API do Traefik sem segurança (apenas para fins de teste ou desenvolvimento).
--providers.docker: Habilita o provedor do Docker no Traefik, permitindo que ele descubra e configure automaticamente os serviços que estão sendo executados no Docker.
```sh
docker run -d \
--name traefik \
-p 8080:8080 \
-p 80:80 \
-v /var/run/docker.sock:/var/run/docker.sock \
traefik:v2.0 --api.insecure=true --providers.docker
```

## Este comando envia uma solicitação GET para o endpoint "/catalog" com o parâmetro "type" definido como "bicycles". Ele retorna a resposta do servidor para essa solicitação.
```sh
curl http://acme.com/catalog?type=bicycles
curl http://acme.com/checkout
```

## O comando abaixo é utilizado para remover os contêineres Docker chamados "traefik", "eshop" e "catalog":
```sh
docker container rm -f traefik eshop catalog
```
