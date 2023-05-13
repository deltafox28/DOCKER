# Usando o Docker com Shell in a Box

## Shell In A Box é um emulador de terminal baseado em web que permite aos usuários acessar ambientes de shell de linha de comando usando um navegador da web. Ele fornece uma interface fácil de usar que pode ser acessada de qualquer lugar com uma conexão à internet, sem a necessidade de nenhum software ou plug-ins adicionais.
```sh
https:/​/​github.​com/​shellinabox/shellinabox
```

## --rm: essa opção informa o Docker para remover o contêiner automaticamente quando ele for encerrado.
--name shellinabox: define o nome do contêiner como "shellinabox".
-p 4200:4200: mapeia a porta 4200 do contêiner para a porta 4200 do host, permitindo que o Shell In A Box seja acessado através do navegador da web em http://localhost:4200.
-e SIAB_USER=vitor: define o nome de usuário do Shell In A Box como "vitor".
-e SIAB_PASSWORD=123456: define a senha do Shell In A Box como "123456".
-e SIAB_SUDO=true: define a permissão de sudo no Shell In A Box como "true", permitindo que o usuário execute comandos com privilégios de superusuário.
-v pwd/dev:/usr/src/dev: cria um volume do Docker para o diretório dev no host, que é montado no diretório /usr/src/dev no contêiner. Isso permite que o usuário acesse e edite arquivos no diretório dev do host a partir do Shell In A Box.
sspreitzer/shellinabox:latest: especifica a imagem Docker a ser usada para criar o contêiner, nesse caso a imagem "sspreitzer/shellinabox:latest".
```sh
docker container run --rm \
--name shellinabox \
-p 4200:4200 \
-e SIAB_USER=vitor \
-e SIAB_PASSWORD=123456 \
-e SIAB_SUDO=true \
-v `pwd`/dev:/usr/src/dev \
sspreitzer/shellinabox:latest


https://52.14.178.179:4200/ (acessar pelo browser)
```

# Trabalhando com a rede de bridge

## docker network ls: lista todas as redes disponíveis no Docker.
docker network inspect bridge: exibe informações detalhadas sobre a rede padrão do Docker, chamada "bridge".
docker network create --driver bridge sample-net: cria uma nova rede chamada "sample-net" usando o driver de rede "bridge".
docker network inspect sample-net | grep Subnet: exibe o endereço IP da sub-rede atribuído à rede "sample-net".
docker network create --driver bridge --subnet "10.1.0.0/16" test-net: cria uma nova rede chamada "test-net" com uma sub-rede especificada como "10.1.0.0/16".
docker container run --name c1 -it --rm alpine:latest /bin/sh: cria e inicia um novo contêiner chamado "c1" a partir da imagem "alpine:latest" e abre um shell interativo dentro dele.
docker container inspect c1: exibe informações detalhadas sobre o contêiner "c1", incluindo sua configuração de rede e informações de IP.
```sh
docker network ls
docker network inspect bridge
docker network create --driver bridge sample-net
docker network inspect sample-net | grep Subnet
docker network create --driver bridge --subnet "10.1.0.0/16" test-net
docker container run --name c1 -it --rm alpine:latest /bin/sh
docker container inspect c1
```

## ip addr: exibe informações detalhadas sobre as interfaces de rede configuradas no sistema.
ip addr show eth0: exibe informações detalhadas sobre a interface de rede "eth0", incluindo seu endereço IP, máscara de sub-rede e estado.
ip route: exibe a tabela de roteamento do sistema, que contém informações sobre como pacotes de rede devem ser encaminhados entre redes e hosts. Isso inclui informações sobre o gateway padrão, rotas específicas para outras redes e muito mais.
```sh
ip addr 
ip addr show eth0
ip route
```

## docker container run --name c2 -d alpine:latest ping 127.0.0.1: cria e inicia um novo contêiner chamado "c2" a partir da imagem "alpine:latest" e executa o comando "ping 127.0.0.1" dentro dele. Esse comando faz com que o contêiner fique em execução em segundo plano (em modo "detached").
docker container inspect --format "{{.NetworkSettings.IPAddress}}" c2: exibe o endereço IP atribuído ao contêiner "c2". A opção --format especifica que queremos exibir apenas o endereço IP e não todas as informações do contêiner.
docker network inspect bridge: exibe informações detalhadas sobre a rede padrão do Docker, chamada "bridge". Isso inclui informações sobre os contêineres conectados a ela, as configurações de IP e gateway da rede, e muito mais.
```sh
docker container run --name c2 -d alpine:latest ping 127.0.0.1
docker container inspect --format "{{.NetworkSettings.IPAddress}}" c2
docker network inspect bridge
```

## Esse comando cria um novo contêiner chamado "c3" a partir da imagem "alpine:latest" e executa o comando "ping 127.0.0.1" dentro dele. No entanto, ele especifica a opção --network para conectar o contêiner à rede "test-net" que foi criada anteriormente. Isso significa que o contêiner "c3" será capaz de se comunicar com outros contêineres na mesma rede "test-net" usando seus nomes de host ou endereços IP internos, em vez de se comunicar com outros contêineres na rede padrão do Docker. O uso de redes personalizadas no Docker é uma maneira comum de isolar e segmentar aplicativos e serviços em diferentes ambientes ou redes virtuais.
```sh
docker container run --name c3 -d --network test-net \
alpine:latest ping 127.0.0.1
```

## Esse comando cria um novo contêiner chamado "c4" a partir da imagem "alpine:latest" e executa o comando "ping 127.0.0.1" dentro dele. Ele também especifica a opção --network para conectar o contêiner à rede "test-net" que foi criada anteriormente. Dessa forma, o contêiner "c4" será capaz de se comunicar com outros contêineres na mesma rede "test-net" usando seus nomes de host ou endereços IP internos, desde que eles também estejam conectados à mesma rede.
```sh
docker container run --name c4 -d --network test-net \
alpine:latest ping 127.0.0.1
```

## Esse comando exibe informações detalhadas sobre a rede "test-net" do Docker, incluindo suas configurações, contêineres conectados e outras informações relevantes. A saída inclui informações sobre os contêineres que estão conectados à rede, seus endereços IP e outras configurações de rede, bem como informações sobre a própria rede, como seu endereço IP e máscara de sub-rede. Isso pode ser útil para solucionar problemas de rede em contêineres ou para obter uma visão geral das configurações de rede do Docker.
```sh
docker network inspect test-net
```

## Ao executar o comando docker container exec -it c3 /bin/sh, você acessa o shell do contêiner "c3". Em seguida, ao executar o comando ping c4, você tenta fazer um ping para o contêiner "c4" usando seu nome de host.
```sh
docker container exec -it c3 /bin/sh
/ # ping c4
/ # ping 172.18.0.3
/ # ping c2
```

## Ao executar o comando ping: bad address 'c2', o erro indica que não é possível resolver o nome de host "c2" em um endereço IP válido, pois o contêiner "c2" não está na mesma rede do contêiner "c3".
```sh
ping: bad address 'c2'
/ # ping 172.17.0.3
```

## O comando docker container rm -f $(docker container ls -aq) remove todos os contêineres em execução e também os contêineres parados no seu sistema.

O comando docker network rm sample-net remove a rede "sample-net" do seu sistema. Certifique-se de que não haja mais contêineres conectados a essa rede antes de removê-la.

O comando docker network rm test-net remove a rede "test-net" do seu sistema. Certifique-se de que não haja mais contêineres conectados a essa rede antes de removê-la.

O comando docker network prune --force remove todas as redes que não estão sendo usadas por nenhum contêiner. Certifique-se de que não haja mais contêineres conectados a essas redes antes de removê-las. Esse comando pode ser útil para limpar redes que não estão mais em uso no seu sistema.
```sh
docker container rm -f $(docker container ls -aq)
docker network rm sample-net
docker network rm test-net
docker network prune --force
```

# O host e a rede nula

##
```sh
docker container run --rm -it --network host alpine:latest /bin/sh
/ # ip addr show enp0s3
/ # ip route
```

##
```sh
A rede nula

docker container run --rm -it --network none alpine:latest /bin/sh
/ # ip addr show eth0
/ # ip route
```

##
```sh
docker network create --driver bridge test-net
```

##
```sh
docker container run --name web -d \
--network test-net nginx:alpine
```

##
```sh
docker container run -it --rm --network container:web \
alpine:latest /bin/sh
/ # wget -qO - localhost
```

##
```sh
docker container rm --force web
docker network rm test-net
```



