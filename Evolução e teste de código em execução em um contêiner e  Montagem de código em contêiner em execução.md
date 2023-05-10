# Evolução e teste de código em execução em um contêiner:

## O comando "mkdir -p ~/fod/node && cd ~/fod/node" cria um diretório chamado "node" dentro da pasta "fod" no diretório principal do usuário atual e depois entra nesse diretório.
O comando "npm init" inicializa um novo projeto Node.js no diretório atual e cria um arquivo "package.json" com informações sobre o projeto, como nome, versão, descrição e dependências.
o comando "npm install express --save" instala o framework web "Express" e adiciona a sua dependência ao arquivo "package.json" com a opção "--save". O Express é um framework popular para Node.js usado para criar aplicativos web e APIs.
```sh
mkdir -p ~/fod/node && cd ~/fod/node
apt-get npm install
npm init
npm install express --save
```

## criando um servidor web simples usando o framework Express no Node.js.
A primeira linha requer o módulo "express" e o armazena em uma constante chamada "express".
Na segunda linha, o objeto "app" é inicializado como uma instância do Express.
A terceira linha inicia o servidor na porta 3300 e no endereço IP 0.0.0.0 usando o método "listen". O callback é executado assim que o servidor é iniciado e exibe uma mensagem no console.
A partir da quarta linha, a aplicação define uma rota para a URL raiz '/' usando o método "get" do objeto "app". Quando a URL é acessada no navegador, a aplicação envia a mensagem "Sample Application: Hello World!" usando o método "send" do objeto "res".
Por fim, o comando "node index.js" é executado para iniciar a aplicação no terminal. O servidor web deve estar acessível em "http://0.0.0.0:3300/".
```sh
vim index.js
// index.js
const express = require('express');
const app = express();
app.listen(3300, '0.0.0.0', ()=>{
	console.log('Application listening at 0.0.0.0:3300');
})

app.get('/', (req,res)=>{
	res.send('Sample Application: Hello World!');
})

node index.js
```

## A primeira linha especifica a imagem base para o contêiner como a versão mais recente do Node.js.
A segunda linha define o diretório de trabalho no contêiner como "/app".
A terceira linha copia o arquivo "package.json" para o diretório de trabalho no contêiner.
A quarta linha executa o comando "npm install" para instalar as dependências do projeto listadas no arquivo "package.json".
A quinta linha copia todos os arquivos do diretório atual (incluindo o código da aplicação) para o diretório de trabalho no contêiner.
A sexta linha define o comando padrão para iniciar a aplicação como "node index.js".
```sh
vim Dockerfile

FROM node:latest
WORKDIR /app
COPY package.json ./
RUN npm install
COPY . .
CMD node index.js
```

## Os comandos acima são usados para construir e executar um contêiner Docker a partir da imagem "sample-app".
O primeiro comando "docker image build -t sample-app ." cria uma imagem Docker com o nome "sample-app" a partir do Dockerfile no diretório atual ".". O argumento "-t" especifica o nome e a tag da imagem.
O segundo comando "docker container run --rm -it --name my-sample-app -p 3300:3300 sample-app" executa um novo contêiner Docker a partir da imagem "sample-app". O argumento "--rm" remove o contêiner quando ele é encerrado. O argumento "-it" mantém o terminal do contêiner aberto para que possa ser interativo. O argumento "--name" define o nome do contêiner como "my-sample-app". O argumento "-p" mapeia a porta 3300 do contêiner para a porta 3300 do host para que a aplicação seja acessível a partir do navegador. O último argumento "sample-app" é o nome da imagem que deve ser usada para iniciar o contêiner.
Após a execução do segundo comando, o contêiner deve estar em execução e a aplicação deve estar acessível em "http://localhost:3300/" ou no endereço IP do host. O comando "Ctrl + C" pode ser usado para sair do terminal do contêiner e encerrar o contêiner.
```sh
docker image build -t sample-app .

docker container run --rm -it \
--name my-sample-app \
-p 3300:3300 \
sample-app
```

## O código acima adiciona um novo endpoint "/hobbies" à aplicação Express que retorna uma lista de hobbies em formato JSON.
```sh
vim index.js
...
const hobbies = [
	'Swimming', 'Diving', 'Jogging', 'Cooking', 'Singing'
];

app.get('/hobbies', (req,res)=>{
	res.send(hobbies);
})
```

## Após as alterações feitas no código, é necessário reconstruir a imagem Docker e reiniciar o contêiner para que as mudanças sejam refletidas na aplicação.
```sh
docker image build -t sample-app .
docker container run --rm -it --name my-sample-app -p 3300:3300 sample-app
```

# Montagem de código em contêiner em execução

## O comando cd fod/node/ é usado para mudar o diretório atual para fod/node/ dentro do container que foi criado a partir da imagem Alpine.
Explicando cada parte do comando:
cd: é o comando utilizado para mudar o diretório atual para o diretório especificado.
fod/node/: é o caminho relativo do diretório que será acessado a partir do diretório atual.
Lembrando que o comando em questão precisa ser executado dentro do container que foi criado a partir da imagem Alpine, utilizando o comando docker container run com os parâmetros --rm, -it, --volume e a imagem alpine.
```sh
docker container run --rm -it \
--volume /projects/sample-app:/app \
alpine /bin/sh

cd fod/node/
```

## O comando docker container run --rm -it --volume $(pwd):/app -p 3300:3300 sample-app é usado para criar e executar um container a partir da imagem sample-app, mapeando a porta 3300 do host para a porta 3300 do container, e montando o diretório atual do host como o diretório /app do container.
Explicando cada parte do comando:
docker container run: é o comando utilizado para criar e executar um container.
--rm: é o parâmetro que indica que o container deve ser removido automaticamente após a sua finalização.
-it: é o parâmetro que permite a interação com o terminal do container.
--volume $(pwd):/app: é o parâmetro que mapeia o diretório atual do host (obtido pelo comando $(pwd)) para o diretório /app do container.
-p 3300:3300: é o parâmetro que mapeia a porta 3300 do host para a porta 3300 do container.
sample-app: é o nome da imagem a partir da qual o container será criado e executado.
Dessa forma, ao executar esse comando, será criado um container a partir da imagem sample-app, que estará mapeando a porta 3300 do host para a porta 3300 do container e terá o diretório atual do host montado como o diretório /app do container.
```sh
docker container run --rm -it \
--volume $(pwd):/app \
-p 3300:3300 \
sample-app
```

## O comando vim index.js é usado para editar o arquivo index.js utilizando o editor de texto vim.
cria uma rota em uma aplicação web utilizando o framework ExpressJS, onde quando o usuário acessar a URL http://localhost:3300/status (supondo que a aplicação esteja sendo executada na porta 3300), o servidor irá responder com o status OK.
Este trecho de código utiliza a função app.get()
```sh
vim index.js
...

app.get('/status', (req,res)=>{
	res.send('OK');
})
```

## O comando docker container run --rm -it --name my-sample-app --volume $(pwd):/app -p 3300:3300 sample-app é usado para criar e executar um container a partir da imagem sample-app, mapeando a porta 3300 do host para a porta 3300 do container, e montando o diretório atual do host como o diretório /app do container, além de definir o nome do container como my-sample-app.
Explicando cada parte do comando:
docker container run: é o comando utilizado para criar e executar um container.
--rm: é o parâmetro que indica que o container deve ser removido automaticamente após a sua finalização.
-it: é o parâmetro que permite a interação com o terminal do container.
--name my-sample-app: é o parâmetro que define o nome do container como my-sample-app.
--volume $(pwd):/app: é o parâmetro que mapeia o diretório atual do host (obtido pelo comando $(pwd)) para o diretório /app do container.
-p 3300:3300: é o parâmetro que mapeia a porta 3300 do host para a porta 3300 do container.
sample-app: é o nome da imagem a partir da qual o container será criado e executado.
Dessa forma, ao executar esse comando, será criado um container a partir da imagem sample-app, que estará mapeando a porta 3300 do host para a porta 3300 do container, terá o diretório atual do host montado como o diretório /app do container e terá o nome definido como my-sample-app.
```sh
docker container run --rm -it \
--name my-sample-app \
--volume $(pwd):/app \
-p 3300:3300 \
sample-app
```

## Os comandos curl localhost:3300/status e docker container exec my-sample-app cat index.js são usados para testar e verificar o conteúdo do arquivo index.js do container my-sample-app.
O comando curl localhost:3300/status é usado para realizar uma requisição HTTP GET para a URL http://localhost:3300/status (supondo que a aplicação esteja sendo executada na porta 3300), que deverá retornar uma resposta com o status OK, conforme definido na rota criada no arquivo index.js.
Já o comando docker container exec my-sample-app cat index.js é usado para executar o comando cat index.js dentro do container my-sample-app, exibindo o conteúdo do arquivo index.js no terminal do host. Isso permite que seja verificado o conteúdo do arquivo index.js que está sendo executado pela aplicação no container.
Por fim, após verificar o conteúdo do arquivo index.js, o comando curl localhost:3300/status pode ser executado novamente para testar a rota criada na aplicação e verificar se ela está respondendo corretamente com o status OK.
```sh
curl localhost:3300/status
docker container exec my-sample-app cat index.js
curl localhost:3300/status
```
