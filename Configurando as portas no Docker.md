# Configurando as portas no Docker

## O primeiro comando docker container run --name web -P -d nginx:alpine cria um novo container com o nome web, baseado na imagem nginx:alpine e em modo de execução destacado (-d). A opção -P publica todas as portas expostas pelo container para portas aleatórias no host.

O segundo comando docker container port web exibe as portas publicadas do container web. Isso mostra a porta aleatória que o Docker atribuiu à porta padrão do servidor web (80) dentro do container.

O terceiro comando docker container inspect web | grep HostPort exibe o número da porta do host em que o Docker publicou a porta do container web. A opção grep HostPort filtra a saída do comando docker container inspect para exibir apenas a porta do host atribuída pelo Docker.
```sh
docker container run --name web -P -d nginx:alpine
docker container port web
docker container inspect web | grep HostPort
```

##
```sh
docker container ls
docker container run --name web2 -p 8080:80 -d nginx:alpine
docker container ls
```



























