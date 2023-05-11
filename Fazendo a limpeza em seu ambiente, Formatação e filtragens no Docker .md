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
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
