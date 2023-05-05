# Criacao de imagens interativas 1:

##  cria e inicia um novo contêiner Docker a partir da imagem Alpine versão 3.10, com um shell interativo.
Depois de entrar no contêiner, o comando "apk update && apk add iputils" é usado para atualizar os repositórios de pacotes do Alpine e, em seguida, instalar o pacote "iputils", que contém o utilitário "ping".
Em seguida, o comando "ping fb.com" é executado para testar a conectividade com o site do Facebook. Esse comando envia pacotes ICMP para o servidor fb.com e imprime as respostas recebidas. 
Depois disso, o comando "exit" é executado para sair do shell dentro do contêiner e encerrar o contêiner:
```sh
docker container run -it \
	--name sample \
	alpine:3.10 /bin/sh
/ #apk update && apk add iputils
/ #ping fb.com
/ #exit
```

##  é usado para listar todos os contêineres Docker, incluindo aqueles que estão parados ou foram criados anteriormente:
```sh
docker container ls -a | grep sample
```

## é usado para listar todos os contêineres Docker, incluindo aqueles que estão parados ou foram criados anteriormente:
```sh
docker container ls -a | grep sample
```

## é usado para criar uma nova imagem Docker a partir de um contêiner existente "sample" e salvar as alterações feitas no sistema de arquivos como uma nova imagem com o nome "my-alpine":
```sh
docker container commit sample my-alpine
```

##  é usado para listar todas as imagens Docker armazenadas localmente no seu sistema:
```sh
docker image ls
```

## " é usado para exibir o histórico de camadas que foram criadas na construção da imagem Docker "my-alpine".
Esse comando exibe informações sobre cada camada, incluindo o ID da camada, o comando usado para criar a camada, o tamanho da camada e a data em que a camada foi criada:
```sh
docker image history my-alpine
```

## Remocao de imagens e containers:
```sh
docker rmi <id-da-imagem> (remove uma imagem)
docker rmi $(docker images -q) (remove todas as imagens)
docker stop $(docker ps -a -q) (Parar todos os contêineres em execução)
docker rm $(docker ps -a -q) (Excluir todos os contêineres parados)
docker rm <nome> (remove um container)
```

