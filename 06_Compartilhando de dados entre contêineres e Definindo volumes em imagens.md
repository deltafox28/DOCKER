# Compartilhando de dados entre contêineres:

## O comando "docker container run" é usado para criar e executar um novo contêiner Docker a partir da imagem "alpine". O contêiner é iniciado em modo iterativo e conectado ao terminal local usando a opção "-it". Ele é nomeado como "writer" usando a opção "--name".
A opção "-v shared-data:/data" monta o volume "shared-data" no diretório "/data" dentro do contêiner.
Dentro do contêiner, o comando "echo 'I can create a file' > /data/sample.txt" cria um arquivo chamado "sample.txt" no diretório "/data" do volume "shared-data" montado. O conteúdo do arquivo é "I can create a file".
```sh
$ docker container run -it --name writer \
-v shared-data:/data \
alpine /bin/sh

/ # echo "I can create a file" > /data/sample.txt
```

## é usado para criar e executar um novo contêiner Docker a partir da imagem "ubuntu:20.04". O contêiner é iniciado em modo iterativo e conectado ao terminal local usando a opção "-it". Ele é nomeado como "reader" usando a opção "--name".
A opção "-v shared-data:/app/data:ro" monta o volume "shared-data" no diretório "/app/data" dentro do contêiner com a opção "ro", que significa que o volume é montado como somente leitura.
Dentro do contêiner, o comando "ls -l /app/data" lista o conteúdo do diretório "/app/data", que é o diretório montado do volume "shared-data".
No entanto, como o volume foi montado como somente leitura, não será possível criar, modificar ou excluir arquivos dentro do diretório "/app/data".
```sh
$ docker container run -it --name reader \
-v shared-data:/app/data:ro \
ubuntu:20.04 /bin/bash

$ ls -l /app/data
```

## você está tentando criar um novo arquivo chamado "data.txt" no diretório "/app/data" que foi montado com a opção "ro" (somente leitura).
Isso resultará em um erro de permissão negada, pois o volume "shared-data" está montado com a opção "ro" e não pode ser modificado.
```sh
/ # echo "Try to break read/only" > /app/data/data.txt
```

## Esses comandos são usados para remover todos os contêineres e volumes Docker no sistema.
```sh
$ docker container rm -f $(docker container ls -aq)
$ docker volume rm $(docker volume ls -q)
```

## Nesses comandos, você iniciou um novo contêiner Docker usando a imagem "alpine:latest". O contêiner foi iniciado em modo iterativo e conectado ao terminal local usando a opção "-it".
Em seguida, você montou o diretório atual (representado por $(pwd)) no diretório "/app/src" dentro do contêiner usando a opção "-v". Isso significa que todos os arquivos e diretórios do diretório atual serão acessíveis dentro do contêiner no diretório "/app/src".
Dentro do contêiner, você criou um novo diretório chamado "my-web" em sua pasta home usando o comando "mkdir ~/my-web", e em seguida, mudou para o diretório "my-web" usando o comando "cd ~/my-web".
Finalmente, você criou um novo arquivo "index.html" dentro do diretório "my-web" usando o comando "echo '<h1>Personal Website</h1>' > index.html". Esse comando criou um arquivo "index.html" com o conteúdo "<h1>Personal Website</h1>" dentro do diretório "my-web".
```sh
$ docker container run --rm -it \
-v $(pwd)/src:/app/src \
alpine:latest /bin/sh

$ mkdir ~/my-web
$ cd ~/my-web

$ echo "<h1>Personal Website</h1>" > index.html
```

## O primeiro comando "vim Dockerfile" abre um editor de texto chamado "vim" para criar/editar um arquivo chamado "Dockerfile". 
Esse arquivo Dockerfile é usado para criar uma imagem Docker que usa a imagem base "nginx:alpine" e copia o conteúdo do diretório atual (representado pelo ponto '.') para o diretório '/usr/share/nginx/html' dentro da imagem.
O segundo comando "docker image build -t my-website:1.0 ." cria uma nova imagem Docker chamada "my-website:1.0" a partir do Dockerfile que acabou de ser criado. O ponto '.' indica que o contexto da imagem é o diretório atual.
O terceiro comando "docker container run -d --name my-site -p 8080:80 my-website:1.0" cria um novo contêiner Docker usando a imagem "my-website:1.0". O contêiner é iniciado em segundo plano ("-d") e recebe o nome "my-site" ("--name my-site"). A porta 8080 do host é mapeada para a porta 80 do contêiner ("-p 8080:80").
O quarto comando "vim index.html" abre o arquivo "index.html" no editor de texto "vim".
```sh
# vim Dockerfile

FROM nginx:alpine
COPY . /usr/share/nginx/html

# docker image build -t my-website:1.0 .

$ docker container run -d \
--name my-site \
-p 8080:80 \
my-website:1.0

vim index.html

<h1>Website Pessoal</h1>
<p>Isso é um teste!</p>
```

## estão removendo o contêiner "my-site" caso ele exista ("-f" força a remoção) e, em seguida, construindo uma nova imagem Docker chamada "my-website:1.0" a partir do Dockerfile no diretório atual e, finalmente, criando um novo contêiner Docker usando a imagem "my-website:1.0". O contêiner é iniciado em segundo plano ("-d") e recebe o nome "my-site" ("--name my-site"). A porta 8080 do host é mapeada para a porta 80 do contêiner ("-p 8080:80").
Assumindo que o Dockerfile não foi alterado e que o conteúdo do diretório atual não mudou, a imagem "my-website:1.0" não precisa ser construída novamente, então você pode pular a etapa de construção da imagem e simplesmente executar o comando para iniciar o contêiner.
Isso inicia o contêiner "my-site" usando a imagem "my-website:1.0". O contêiner é iniciado em segundo plano ("-d") e recebe o nome "my-site" ("--name my-site"). A porta 8080 do host é mapeada para a porta 80 do contêiner ("-p 8080:80").
```sh
# docker container rm -f my-site
# docker image build -t my-website:1.0 .
# docker container run -d \
--name my-site \
-p 8080:80 \
my-website:1.0
```

## Esses comandos estão removendo o contêiner "my-site" caso ele exista ("-f" força a remoção) e, em seguida, criando um novo contêiner Docker usando a imagem "my-website:1.0". O contêiner é iniciado em segundo plano ("-d") e recebe o nome "my-site" ("--name my-site"). A porta 8080 do host é mapeada para a porta 80 do contêiner ("-p 8080:80").
Além disso, um volume Docker é criado para mapear o diretório atual para o diretório "/usr/share/nginx/html" dentro do contêiner ("-v $(pwd):/usr/share/nginx/html"). Isso significa que qualquer alteração feita no diretório atual será refletida automaticamente dentro do contêiner.
```sh
$ docker container rm -f my-site
$ docker container run -d \
--name my-site \
-v $(pwd):/usr/share/nginx/html \
-p 8080:80 \
my-website:1.0
```

# Definindo volumes em imagens:

## O primeiro comando "docker image pull mongo" baixa a imagem Docker oficial do MongoDB para o sistema local, se já não estiver presente.
O segundo comando "docker image inspect" é usado para exibir informações sobre a imagem Docker, como suas camadas, metadados, configurações e outras propriedades. O parâmetro "--format" especifica o formato de saída do comando, que neste caso é um objeto JSON com as informações sobre os volumes configurados na imagem.
O comando "jq ." é usado para formatar a saída JSON de forma mais legível.
O terceiro comando "docker run" inicia um novo contêiner Docker com o nome "my-mongo" usando a imagem "mongo". O parâmetro "-d" inicia o contêiner em segundo plano.
O quarto comando "docker inspect" é usado para exibir informações sobre o contêiner Docker, como sua configuração, estado e outras propriedades. O parâmetro "--format" especifica o formato de saída do comando, que neste caso é um objeto JSON com as informações sobre os volumes montados no contêiner.
O comando "jq ." é usado novamente para formatar a saída JSON de forma mais legível.
```sh
docker image pull mongo
docker image inspect \
--format='{{json .ContainerConfig.Volumes}}' \
mongo | jq .
docker run --name my-mongo -d mongo
docker inspect --format '{{json .Mounts}}' my-mongo | jq .
```

