# Comandos Exec e Attaching em um contêiner em execução:

## é usado para iniciar um shell interativo dentro de um contêiner Docker chamado "trivia":
```sh
docker container exec -i -t trivia /bin/sh
   /app # ps (ele exibirá apenas os processos em execução dentro desse contêiner)
```

## é usado para exibir informações sobre os processos em execução dentro de um contêiner Docker chamado "trivia":
```sh
docker container exec trivia ps
```

## é usado para iniciar um shell interativo dentro do contêiner Docker chamado "trivia", enquanto define a variável de ambiente "MY_VAR" com o valor "Hello World":
```sh
docker container exec -it \
 -e MY_VAR="Hello World" \
 trivia /bin/sh
 /app echo $MY_VAR
```

## é usado para anexar-se ao console de um contêiner em execução chamado "trivia":
```sh
docker container attach trivia
```

## é usado para iniciar um contêiner Docker a partir da imagem "nginx:alpine", mapeando a porta 80 do contêiner para a porta 8080 do sistema host e nomeando o contêiner como "nginx":
```sh
docker run -d --name nginx -p 8080:80 nginx:alpine
```

##  é usado para fazer uma solicitação HTTP GET para o servidor web Nginx em execução no contêiner Docker que mapeia a porta 80 do contêiner para a porta 8080 do sistema host:
```sh
curl -4 localhost:8080
```

## é usado para anexar-se ao console do contêiner Docker chamado "nginx":
```sh
docker container attach nginx
```

## O comando "for n in {1..10}; do curl -4 localhost:8080; done" é usado para fazer 10 solicitações HTTP GET para o servidor web Nginx em execução no contêiner Docker que mapeia a porta 80 do contêiner para a porta 8080 do sistema host.
O comando usa um loop "for" para executar o comando "curl -4 localhost:8080" 10 vezes em sequência. Isso simula o comportamento de um usuário que acessa repetidamente o servidor web:
```sh
for n in {1..10}; do curl -4 localhost:8080; done
```

## é usado para remover o contêiner Docker chamado "nginx":
```sh
docker container rm nginx
```

# Lidando com logs no docker:

## Ao executar este comando, você verá a saída do log do contêiner, que pode incluir informações como:
Saída de aplicativos em execução dentro do contêiner
Mensagens de erro
Informações de inicialização do contêiner
Comandos executados dentro do contêiner
Os logs do contêiner podem ser úteis para entender o comportamento do aplicativo em execução dentro dele e solucionar problemas:
```sh
docker container logs trivia
```

## é usado para exibir as últimas 5 linhas dos logs do contêiner Docker chamado "trivia": 
```sh
docker container logs --tail 5 trivia
```

## é usado para exibir as últimas 5 linhas dos logs do contêiner Docker chamado "trivia" e, em seguida, acompanhar as novas mensagens de log que são adicionadas enquanto o comando é executado:
```sh
docker container logs --tail 5 --follow trivia
```

## Aqui está o que cada parte do comando significa:
"docker container run" é usado para criar e executar um novo contêiner Docker.
"--name test" é usado para dar um nome ao novo contêiner Docker, que será "test" neste caso.
"-it" é usado para iniciar o contêiner em modo interativo, permitindo que você entre nele e interaja com o shell.
"--log-driver none" é usado para desativar o registro de logs do contêiner. Isso significa que nenhum log será gravado em nenhum lugar, seja no host ou em um driver de log externo.
"busybox" é a imagem que será usada para criar o contêiner.
"sh -c 'for N in 1 2 3; do echo "Hello $N"; done'" é o comando que será executado dentro do contêiner. Ele inicia um shell e executa um loop "for" que imprime "Hello" seguido do valor de "N" (que varia de 1 a 3) no console:
```sh
docker container run --name test -it \
 --log-driver none \
 busybox sh -c 'for N in 1 2 3; do echo "Hello $N"; done'
```

##  é usado para exibir os logs do contêiner Docker chamado "test":
```sh
docker container logs test
```

## é usado para remover o contêiner Docker chamado "test":
```sh
docker container rm test
```

