# Construindo uma imagem do zero:

## cria um novo diretório chamado "fod" dentro do diretório inicial do usuário (~), em seguida, cria um novo diretório chamado "test" dentro do diretório "fod" recém-criado.
Em seguida, ele navega para dentro do diretório "test". O sinalizador "-p" permite criar diretórios aninhados em uma única etapa, mesmo que os diretórios pai ainda não existam:
```sh
mkdir -p ~/fod/test && cd ~/fod/test
```

## Essas instruções especificam que a imagem base é o CentOS 7 e que o comando yum install -y wget deve ser executado durante a construção da imagem para instalar o utilitário wget.
O comando docker image build -t my-centos . cria uma nova imagem Docker a partir do diretório atual (onde o arquivo Dockerfile está localizado) e atribui a tag my-centos a ela. Isso usa o arquivo Dockerfile por padrão.
O comando docker image build -t my-centos -f Dockerfile . é semelhante, mas especifica explicitamente que o arquivo Dockerfile deve ser usado para construir a imagem.
Finalmente, o comando vim hello.c abre o editor de texto vim e cria um novo arquivo chamado hello.c. O conteúdo do arquivo é um programa simples em C que imprime "Hello, world!" quando é executado:
```sh
vim Dockerfile
 FROM centos:7
 RUN yum install -y wget
docker image build -t my-centos . (criar a imagem)
docker image build -t my-centos -f Dockerfile . (indicar o arquivo docker)
vim hello.c 
#include <stdio.h>
 int main (void)
 {
	printf ("Hello, world!\n");
	return 0;
 }
```

## O comando "vim Dockerfile" é utilizado para abrir o editor de texto Vim e criar/abrir o arquivo Dockerfile, que é um arquivo de texto utilizado para definir as instruções de construção de uma imagem Docker. 
O conteúdo do arquivo contém as instruções que serão utilizadas pelo Docker para construir uma imagem personalizada. No exemplo dado, 
o Dockerfile começa com a instrução "FROM alpine:3.7", que define a imagem base a ser utilizada, seguida por várias instruções que criam e compilam um programa "hello.c".
O arquivo é salvo e então é construída a imagem Docker com o comando "docker image build -t hello-world .". As próximas etapas incluem a construção de uma imagem menor, 
a criação de um diretório de backup e a utilização dos comandos "docker image save" e "docker image load" para salvar e carregar imagens Docker em arquivos tar:
```sh
vim Dockerfile 
 FROM alpine:3.7
 RUN apk update && \
 apk add --update alpine-sdk
 RUN mkdir /app
 WORKDIR /app
 COPY . /app
 RUN mkdir bin
 RUN gcc -Wall hello.c -o bin/hello
 CMD /app/bin/hello
docker image build -t hello-world .
docker image ls | grep hello-world
vim Dockerfile
 FROM alpine:3.7 AS build
 RUN apk update && \
 apk add --update alpine-sdk
 RUN mkdir /app
 WORKDIR /app
 COPY . /app
 RUN mkdir bin
 RUN gcc hello.c -o bin/hello
 FROM alpine:3.7
 COPY --from=build /app/bin/hello /app/hello
 CMD /app/hello
docker image build -t hello-world-small .
docker image ls | grep hello-world
mkdir backup
docker image save -o ./backup/my-alpine.tar my-alpine 
docker image load -i ./backup/my-alpine.tar 
```

