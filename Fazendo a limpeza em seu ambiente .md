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

