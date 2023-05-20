# Fazendo a limpeza em seu ambiente

## docker image prune -f: Este comando é usado para remover imagens do Docker não utilizadas. A opção -f é usada para forçar a remoção.
docker container rm <container-id>: Este comando é usado para remover um contêiner Docker especificado pelo seu ID. É necessário fornecer o ID do contêiner que deseja remover.
docker container prune --force: Este comando é usado para remover todos os contêineres do Docker que não estão em execução. A opção --force é usada para forçar a remoção.
docker volume prune: Este comando é usado para remover todos os volumes do Docker que não estão em uso. Ele remove todos os volumes que não estão vinculados a nenhum contêiner em execução.
docker volume rm <volume-name>: Este comando é usado para remover um volume Docker especificado pelo seu nome. É necessário fornecer o nome do volume que deseja remover.
```sh
docker image prune -f
docker container rm <container-id>
docker container prune --force
docker volume prune
docker volume rm <volume-name>
```

# Formatação e filtragens no Docker

## docker container ps -a --format "table {{.Names}}\t{{.Image}}\t{{.Status}}": Este comando é usado para listar todos os contêineres do Docker, incluindo os que não estão em execução. A opção -a é usada para listar todos os contêineres, e --format é usada para personalizar o formato da saída.
docker images --format "{{.Repository}} has the following {{.ID}}": Este comando é usado para listar todas as imagens do Docker. A opção --format é usada para personalizar o formato da saída.
docker images --filter "<chave>=<valor>": Este comando é usado para filtrar as imagens do Docker com base em uma chave e um valor. Você pode filtrar as imagens com base em várias chaves, como "dangling", "reference" e "before".
docker images --filter "reference=node*": Este comando é usado para listar todas as imagens do Docker que tenham uma referência que comece com "node".
docker images --filter "reference=centos:latest": Este comando é usado para listar todas as imagens do Docker que tenham uma referência exata "centos:latest".
docker images --filter "reference=*:latest": Este comando é usado para listar todas as imagens do Docker que tenham uma tag "latest".
docker images --filter "dangling=true": Este comando é usado para listar todas as imagens do Docker que não estão sendo usadas por nenhum contêiner e são chamadas de "dangling images".
docker images --filter "before=<nome_imagem>": Este comando é usado para listar todas as imagens do Docker que foram criadas antes de uma imagem específica. Você deve fornecer o nome ou ID da imagem.
docker images --filter "before=centos:7": Este comando é usado para listar todas as imagens do Docker que foram criadas antes da imagem "centos:7".
docker images --filter "since=<nome_imagem>": Este comando é usado para listar todas as imagens do Docker que foram criadas desde uma imagem específica. Você deve fornecer o nome ou ID da imagem.
docker images --filter "since=centos": Este comando é usado para listar todas as imagens do Docker que foram criadas desde a imagem "centos".
```sh
docker container ps -a --format "table {{.Names}}\t{{.Image}}\t{{.Status}}"
docker images --format "{{.Repository}} has the following {{.ID}}" 
docker images --filter "<chave>=<valor>"
docker images --filter "reference=node*"
docker images --filter "reference=centos:latest"
docker images --filter "reference=*:latest"
docker images --filter "dangling=true"
docker images --filter "before=<nome_imagem>"
docker images --filter "before=centos:7"
docker images --filter "since=<nome_imagem>"
docker images --filter "since=centos"
```
  
# Limitar recursos e sistema de somente leitura

  ## Esse comando executa um contêiner Docker usando a imagem "ubuntu:latest" e executa o shell "/bin/bash" dentro do contêiner. O nome do contêiner é definido como "stress-test". O sinalizador "--rm" especifica que o contêiner será removido automaticamente quando o processo dentro do contêiner for encerrado. O sinalizador "-it" inicia o contêiner no modo interativo e alocará um pseudo-TTY para que o usuário possa interagir com o shell dentro do contêiner. O sinalizador "--memory" define o limite máximo de memória RAM que o contêiner pode usar, neste caso, definido como 512MB. Isso limitará o uso de memória pelo contêiner, evitando que ele consuma toda a memória disponível no host. 
```sh
docker container run --rm -it \
--name stress-test \
--memory 512M \
ubuntu:latest /bin/bash
```

## O primeiro comando "apt-get update && apt-get install -y stress" atualiza os repositórios do sistema operacional dentro do contêiner e, em seguida, instala o pacote de software "stress" usando o gerenciador de pacotes APT. O sinalizador "-y" especifica que o utilitário deve assumir "sim" como resposta padrão a todas as perguntas, permitindo que a instalação seja concluída sem interrupção do usuário. O segundo comando "docker stats" exibe as estatísticas de uso do sistema de todos os contêineres em execução no host do Docker. As estatísticas incluem o uso atual de CPU, memória e entrada/saída do contêiner. O terceiro comando "stress -m 4" executa o utilitário "stress" dentro do contêiner com o argumento "-m" especificando que o teste deve estressar a memória do sistema, usando quatro threads. O comando pode ser usado para testar a capacidade do sistema em lidar com altas demandas de memória.
```sh
apt-get update && apt-get install -y stress
docker stats
stress -m 4
```

## Esse comando executa um contêiner Docker usando a imagem "alpine" e executa o shell "/bin/sh" dentro do contêiner. O nome do contêiner é definido como "read-only". O sinalizador "-d" especifica que o contêiner deve ser executado em segundo plano (modo daemon), ou seja, sem interação com o usuário. O sinalizador "--tty" aloca um pseudo-TTY para que o usuário possa interagir com o shell dentro do contêiner se necessário, mesmo quando executado em segundo plano. O sinalizador "--read-only" define o sistema de arquivos do contêiner como somente leitura, o que significa que não será possível gravar nenhum arquivo no sistema de arquivos do contêiner.
```sh
docker container run --tty -d \
--name read-only \
--read-only \
alpine /bin/sh
```

## Esse comando executa o comando "echo" dentro do contêiner Docker denominado "read-only" usando o comando "docker container exec". O sinalizador "-it" especifica que a execução deve ser feita de forma interativa e alocará um pseudo-TTY para permitir a interação do usuário com o shell dentro do contêiner. O comando "echo" escreve a string "Teste de escrita!" em um arquivo chamado "sample.txt" no diretório atual. No entanto, como o contêiner foi inicializado com o sistema de arquivos somente leitura, o comando falhará com um erro indicando que o sistema de arquivos é somente leitura.
```sh
docker container exec -it read-only \
sh -c 'echo "Teste de escrita!" > ./sample.txt'
```
  
# Evitar execução como usuário não-root

## Esse comando adiciona o usuário atual (definido pela variável de ambiente ${USER}) ao grupo "docker" no sistema operacional CentOS, usando o utilitário "usermod" com o sinalizador "-aG". Ao adicionar o usuário atual ao grupo "docker", o usuário atual terá permissões para executar comandos Docker sem precisar de privilégios de root. Isso permite que o usuário execute comandos Docker de forma mais conveniente e segura, evitando a necessidade de usar "sudo" antes de cada comando.
```sh
vitor@centos $ sudo usermod -aG docker ${USER}
```

## Esse conjunto de comandos inicia uma sessão de shell como superusuário (root) no sistema operacional CentOS. O comando "sudo su" solicita ao usuário atual que insira sua senha e, em seguida, altera a identidade do usuário para superusuário (root) usando o comando "su" (superuser).
```sh
vitor@centos $ sudo su
Password: 
root@centos $
```

## Esses comandos são executados enquanto o usuário está conectado como superusuário (root) e fazem o seguinte:
O comando "echo" escreve a string "Você não pode ver isso!" em um arquivo chamado "top-secret.txt".
O comando "chmod" altera as permissões do arquivo "top-secret.txt" para que somente o usuário root possa ler e escrever o arquivo.
O comando "ls -l" lista o conteúdo do diretório atual com detalhes, incluindo as permissões de acesso e proprietário do arquivo "top-secret.txt".
O comando "exit" encerra a sessão de shell como superusuário (root) e retorna à identidade de usuário original (no caso, "vitor").
```sh
root@centos $ echo "Você não pode ver isso!" > top-secret.txt
root@centos $ chmod 600 ./top-secret.txt
root@centos $ ls -l
root@centos $ exit
vitor@centos $
```

## Esse comando tenta exibir o conteúdo do arquivo "top-secret.txt" no diretório atual usando o comando "cat". No entanto, como o arquivo foi criado com permissões que permitem apenas ao usuário root ler e escrever o arquivo, o comando falhará com uma mensagem de erro "Permission denied"
```sh
vitor@centos $ cat ./top-secret.txt
```

## Esse comando abre o editor de texto "vim" e cria/edita um arquivo denominado "Dockerfile" no diretório atual. O conteúdo do arquivo é um conjunto de instruções usadas para criar uma imagem Docker a partir do Ubuntu com o arquivo "top-secret.txt" copiado para o diretório "/secrets/" na imagem Docker. Em seguida, a imagem é configurada para executar o comando "cat /secrets/top-secret.txt" sempre que um contêiner for iniciado a partir dessa imagem.
```sh
vim Dockerfile

FROM ubuntu:latest
COPY ./top-secret.txt /secrets/
# simular o uso de arquivo restrito
CMD cat /secrets/top-secret.txt
```

## O comando "sudo su" solicita a senha do usuário root e altera a identidade do usuário atual para o superusuário (root).
O comando "docker image build -t demo-image ." cria uma nova imagem Docker com o nome "demo-image" a partir do arquivo Dockerfile no diretório atual (denotado pelo ponto "."). Esse comando pode levar algum tempo para ser concluído, pois requer a construção da imagem a partir das instruções do Dockerfile.
O comando "exit" encerra a sessão de shell como superusuário (root) e retorna à identidade de usuário original (no caso, "vitor").
O comando "docker container run demo-image" inicia um novo contêiner Docker a partir da imagem "demo-image" e executa o comando "cat /secrets/top-secret.txt" como definido no Dockerfile. Como o arquivo "top-secret.txt" foi copiado para o contêiner no diretório "/secrets/" durante a criação da imagem Docker, o comando deve exibir o conteúdo do arquivo. No entanto, como o arquivo tem permissões de acesso restritas, apenas o usuário root no contêiner poderá acessá-lo e exibir seu conteúdo.
```sh
vitor@centos $ sudo su
Password: <root password>
root@centos $ docker image build -t demo-image .
root@centos $ exit
vitor@centos $
vitor@centos $ docker container run demo-image
```

## Esse comando abre o editor de texto "vim" e edita o arquivo "Dockerfile" no diretório atual.
O conteúdo do arquivo é um conjunto de instruções usadas para criar uma imagem Docker a partir do Ubuntu, criar um novo usuário "demo-user" no grupo "demo-group" com permissões restritas, copiar o arquivo "top-secret.txt" para o diretório "/secrets/" e, em seguida, definir o usuário "demo-user" como o usuário padrão que será usado quando um contêiner for iniciado a partir dessa imagem. O comando "cat /secrets/top-secret.txt" é definido como o comando padrão a ser executado sempre que um contêiner for iniciado a partir dessa imagem
```sh
vim Dockerfile

FROM ubuntu:latest
RUN groupadd -g 4000 demo-group && useradd -r -u 5000 -g demo-group demo-user
USER demo-user
COPY ./top-secret.txt /secrets/
# simular o uso de arquivo restrito
CMD cat /secrets/top-secret.txt
```

## O comando "sudo su" solicita a senha do usuário root e altera a identidade do usuário atual para o superusuário (root).
O comando "docker image build -t demo-image ." cria uma nova imagem Docker com o nome "demo-image" a partir do arquivo Dockerfile no diretório atual. Esse comando pode levar algum tempo para ser concluído, pois requer a construção da imagem a partir das instruções do Dockerfile.
O comando "exit" encerra a sessão de shell como superusuário (root) e retorna à identidade de usuário original (no caso, "vitor").
O comando "docker container run demo-image" inicia um novo contêiner Docker a partir da imagem "demo-image" e executa o comando "cat /secrets/top-secret.txt" como definido no Dockerfile. Como o arquivo "top-secret.txt" foi copiado para o contêiner no diretório "/secrets/" durante a criação da imagem Docker e só pode ser acessado pelo usuário "demo-user" com as permissões apropriadas, o comando deve exibir o conteúdo do arquivo apenas se o usuário padrão do contêiner for "demo-user". Se o usuário padrão do contêiner for diferente, o comando não poderá acessar o arquivo.
```sh
vitor@centos $ sudo su
Password: 
root@centos $ docker image build -t demo-image .
root@centos $ exit
vitor@centos $ docker container run demo-image
```
