# configurando containeres:

## O comando "export" é usado para definir variáveis de ambiente. Quando executado no shell interativo do contêiner Alpine, o comando não apresentará nenhuma saída, pois não há nenhuma variável de ambiente definida atualmente. 
```sh
export
docker container run --rm -it alpine /bin/sh

/ # export
```

## O comando export | grep LOG_DIR procura por todas as variáveis de ambiente que contenham o texto "LOG_DIR". Se o contêiner estiver configurado corretamente, ele exibirá a variável de ambiente "LOG_DIR" na saída. No exemplo que você apresentou, o contêiner está configurado para definir a variável de ambiente "LOG_DIR" com o valor "/var/log/my-log" usando a opção --env no comando docker container run. 
```sh
$ docker container run --rm -it \
--env LOG_DIR=/var/log/my-log \
alpine /bin/sh

/ # export | grep LOG_DIR
```

## LOG_DIR: Define o diretório de log como "/var/log/my-log".
MAX_LOG_FILES: Define o número máximo de arquivos de log como "5".
MAX_LOG_SIZE: Define o tamanho máximo de cada arquivo de log como "1G".
Essas variáveis de ambiente podem ser usadas pelos processos em execução no contêiner para controlar o comportamento do log. O valor dessas variáveis pode ser acessado dentro do contêiner como variáveis de ambiente normais, por exemplo, $LOG_DIR, $MAX_LOG_FILES, $MAX_LOG_SIZE.
O comando export é usado para definir variáveis de ambiente que serão utilizadas no shell atual e em quaisquer shells filho criados a partir dele. Quando você executa export | grep LOG, está filtrando as variáveis de ambiente definidas no shell atual que contêm a string "LOG".
```sh
docker container run --rm -it \
--env LOG_DIR=/var/log/my-log \
--env MAX_LOG_FILES=5 \
--env MAX_LOG_SIZE=1G \
alpine /bin/sh
/ # export | grep LOG
```

## Este comando cria um diretório ~/fod/teste e entra nele. Depois, ele abre o editor de texto Vim para criar um arquivo chamado development.config. Em seguida, o conteúdo do arquivo é adicionado, definindo as variáveis de ambiente LOG_DIR, MAX_LOG_FILES e MAX_LOG_SIZE.
```sh
$ mkdir -p ~/fod/teste && cd ~/fod/teste

vim development.config

LOG_DIR=/var/log/my-log
MAX_LOG_FILES=5
MAX_LOG_SIZE=1G
```

## No exemplo anterior, o comando docker container run cria e executa um contêiner a partir da imagem alpine, com as variáveis de ambiente definidas no arquivo development.config usando a opção --env-file. Em seguida, o comando sh executa o shell dentro do contêiner e o comando export exibe as variáveis de ambiente definidas dentro do contêiner.
Em seguida, o Dockerfile é criado para definir as variáveis de ambiente em tempo de construção da imagem. A imagem alpine:latest é usada como base para a criação da nova imagem. As variáveis de ambiente LOG_DIR, MAX_LOG_FILES e MAX_LOG_SIZE são definidas usando a instrução ENV.
```sh
$ docker container run --rm -it \
--env-file ./development.config \
alpine sh -c "export"

# cd /fod/teste

# vim Dockerfile

FROM alpine:latest
ENV LOG_DIR=/var/log/my-log
ENV MAX_LOG_FILES=5
ENV MAX_LOG_SIZE=1G 
```

## Os comandos que você executou criaram uma imagem Docker personalizada com base na imagem Alpine, com variáveis de ambiente definidas para o diretório de log, o número máximo de arquivos de log e o tamanho máximo do arquivo de log. Em seguida, você iniciou dois contêineres dessa imagem, um com as variáveis de ambiente padrão e outro substituindo os valores padrão pelas novas variáveis de ambiente. Você usou o comando export para listar todas as variáveis de ambiente definidas no contêiner e o comando grep para filtrar apenas as variáveis relacionadas aos logs.
```sh
$ docker image build -t my-alpine .

$ docker container run --rm -it \
my-alpine sh -c "export | grep LOG"

$ docker container run --rm -it \
--env MAX_LOG_SIZE=2G \
--env MAX_LOG_FILES=10 \
my-alpine sh -c "export | grep LOG"
```


## 
Nesse comando, estamos construindo uma imagem Docker chamada my-node-app-test a partir do Dockerfile no diretório atual (.), usando a opção --build-arg para passar a variável de build BASE_IMAGE_VERSION com o valor 12.7-alpine. Essa variável pode ser usada dentro do Dockerfile com a sintaxe ${BUILD_ARG_NAME}. Por exemplo, se tivermos ARG BASE_IMAGE_VERSION no Dockerfile, podemos usar ${BASE_IMAGE_VERSION} para fazer referência ao valor passado pela opção --build-arg.
```sh
$ docker image build \
--build-arg BASE_IMAGE_VERSION=12.7-alpine \
-t my-node-app-test .
```

