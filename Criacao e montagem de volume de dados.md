# Criação e montagem de volumes de dados:

## é usado para criar e executar um novo contêiner Docker. Neste caso, está sendo criado um novo contêiner com o nome "demo" a partir da imagem "alpine". o resultado será um novo contêiner criado com o nome "demo" e um arquivo chamado "sample.txt" dentro dele contendo o texto "Isso é um teste".
```sh
docker container run --name demo \
alpine /bin/sh -c 'echo "Isso é um teste" > sample.txt'
```

## diff, create e inspect:
```sh
docker container diff demo (é usado para mostrar as alterações no sistema de arquivos de um contêiner Docker em relação à imagem base. No caso, o contêiner "demo" criado anteriormente está sendo inspecionado.)
docker volume create sample (é usado para criar um novo volume Docker chamado "sample". Volumes são usados para armazenar dados persistentes de um contêiner Docker)
docker volume inspect sample ( é usado para exibir informações detalhadas sobre um volume Docker chamado "sample", como o caminho do diretório do host em que o volume está armazenado e outras configurações relacionadas ao volume. O comando exibirá as informações em formato JSON.)
```

##  cria um contêiner com permissões elevadas, permitindo o acesso a todos os dispositivos do host e ao namespace de processos do host. Esse contêiner pode ser usado para fins de depuração ou para executar tarefas que exigem acesso de nível mais baixo ao sistema operacional. O "fundamentalsofdocker/nsenter" é uma imagem que contém a ferramenta "nsenter" que permite que você acesse o namespace de processos de outros contêineres ou do host.
```sh
docker run -it --rm --privileged --pid=host \
fundamentalsofdocker/nsenter
```

## Os comandos digitados indicam que você entrou no contêiner em execução usando o comando "docker container run" anteriormente, e agora está dentro do contêiner. O comando "cd /var/lib/docker/volumes/sample/_data" mudou o diretório atual para o diretório de dados do volume "sample" que foi criado anteriormente usando o comando "docker volume create".
Em seguida, o comando "ls -l" é usado para listar os arquivos e diretórios no diretório atual, que deve ser o diretório de dados do volume "sample".
Por fim, o comando "docker container rm test" tenta remover um contêiner com o nome "test", mas esse comando não pode ser executado dentro do contêiner em que você está agora. Para sair do contêiner, você pode digitar o comando "exit" e pressionar Enter.
```sh
/ # cd /var/lib/docker/volumes/sample/_data
/ # ls -l
docker container rm test
```

## é usado para criar e executar um novo contêiner Docker a partir da imagem "alpine". O contêiner é iniciado em modo iterativo e conectado ao terminal local usando a opção "-it". Ele é nomeado como "test" usando a opção "--name".
A opção "-v sample:/data" cria um volume chamado "sample" e monta ele no diretório "/data" dentro do contêiner. Isso permite que os dados sejam persistidos fora do contêiner.
Dentro do contêiner, o comando "cd /data" muda o diretório atual para o diretório montado do volume.
Em seguida, o comando "echo 'Algum dado' > data.txt" cria um novo arquivo chamado "data.txt" dentro do diretório montado do volume, contendo o texto "Algum dado".
O comando "echo 'Some more data' > data2.txt" cria outro arquivo chamado "data2.txt" dentro do mesmo diretório, contendo o texto "Some more data".
Por fim, o comando "exit" é usado para sair do contêiner.
```sh
docker container run --name test -it \
-v sample:/data \
alpine /bin/sh
 / #cd /data / # echo "Algum dado" > data.txt
 / #echo "Some more data" > data2.txt
 / #exit
```

## está dentro do contêiner em execução e mudou o diretório atual para o diretório de dados do volume "sample" usando o comando "cd /var/lib/docker/volumes/sample/_data". O comando "ls -l" foi usado para listar o conteúdo do diretório, mas nenhum arquivo foi encontrado.
O comando "cat data2.txt" tentou exibir o conteúdo do arquivo "data2.txt", mas não foi exibido nada, provavelmente porque o arquivo não existe.
Em seguida, o comando "echo 'Teste de criação de arquivo' > host-data.txt" foi usado para criar um novo arquivo chamado "host-data.txt" no diretório atual, que é o diretório de dados do volume "sample". Esse arquivo é criado fora do contêiner, no host, porque o contêiner está sendo executado com as opções "--privileged" e "--pid=host", que permitem que ele acesse o namespace do host.
Por fim, o comando "docker container rm test" é usado para remover o contêiner anteriormente criado com o nome "test". Esse comando deve ser executado fora do contêiner em execução. Se você executar esse comando dentro do contêiner em execução, um erro será gerado.
```sh
docker run -it --rm --privileged --pid=host \
fundamentalsofdocker/nsenter
/ # cd /var/lib/docker/volumes/sample/_data
/ # ls -l
/ # cat data2.txt
/ # echo "Teste de criação de arquivo" > host-data.txt
docker container rm test
```

## O comando "docker container run" é usado para criar e executar um novo contêiner Docker a partir da imagem "centos:7". O contêiner é iniciado em modo iterativo e conectado ao terminal local usando a opção "-it". Ele é nomeado como "test2" usando a opção "--name".
A opção "-v sample:/app/data" monta o volume "sample" no diretório "/app/data" dentro do contêiner.
Dentro do contêiner, o comando "cd /app/data" muda o diretório atual para o diretório montado do volume.
O comando "ls -l" lista o conteúdo do diretório atual.
O comando "docker volume rm sample" remove o volume "sample" do Docker.
O comando "docker container rm -f $(docker container ls -aq)" remove todos os contêineres que estão em execução no Docker.
```sh
docker container run --name test2 -it \
-v sample:/app/data \
centos:7 /bin/bash
cd /app/data
ls -l
docker volume rm sample
docker container rm -f $(docker container ls -aq)
```











































































































