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



























