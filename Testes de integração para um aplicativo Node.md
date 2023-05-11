# Testes de integração para um aplicativo Node

## Após criar o diretório integration-test-node e os subdiretórios tests, api e database, você mudou o diretório atual para database usando o comando cd.
```sh
mkdir ~/fod/integration-test-node && \
cd ~/fod/integration-test-node

mkdir tests api database
cd database
```

## O comando vim init-script.sql abre um editor de texto chamado Vim para criar/editar um arquivo chamado init-script.sql. Dentro do arquivo init-script.sql, você criou uma tabela chamada hobbies com duas colunas: hobby_id (tipo serial e chave primária) e hobby (tipo VARCHAR com restrições UNIQUE e NOT NULL). Em seguida, você inseriu cinco linhas de dados na tabela hobbies usando a instrução INSERT INTO.
```sh
vim init-script.sql

CREATE TABLE hobbies(
	hobby_id serial PRIMARY KEY,
	hobby VARCHAR (255) UNIQUE NOT NULL
);

insert into hobbies(hobby) values('swimming');
insert into hobbies(hobby) values('diving');
insert into hobbies(hobby) values('jogging');
insert into hobbies(hobby) values('dancing');
insert into hobbies(hobby) values('cooking');
```

## 
O comando docker volume create pg-data cria um volume Docker chamado pg-data. Esse volume será usado para armazenar os dados do banco de dados PostgreSQL.
Em seguida, você mudou para o diretório ~/fod/integration-test-node usando o comando cd.
O comando docker container run cria e inicia um contêiner Docker usando a imagem do PostgreSQL postgres:11.5-alpine. Este contêiner é chamado de postgres.
Os argumentos fornecidos para o comando docker container run configuram o contêiner da seguinte maneira:
-d: Executa o contêiner em segundo plano (modo "detached").
--name postgres: Define o nome do contêiner como postgres.
-p 5432:5432: Mapeia a porta 5432 do contêiner para a porta 5432 do host.
-v $(pwd)/database:/docker-entrypoint-initdb.d: Monta o diretório database (que foi criado anteriormente) como um volume no contêiner, no caminho /docker-entrypoint-initdb.d. Isso permite que o script SQL init-script.sql seja executado automaticamente quando o contêiner for iniciado.
-v pg-data:/var/lib/postgresql/data: Monta o volume pg-data criado anteriormente no caminho /var/lib/postgresql/data dentro do contêiner. Isso permitirá que os dados do PostgreSQL sejam armazenados persistentemente mesmo após o contêiner ser interrompido.
-e POSTGRES_USER=dbuser: Define o usuário do banco de dados como dbuser.
-e POSTGRES_DB=sample-db: Cria um banco de dados chamado sample-db.
postgres:11.5-alpine: Especifica a imagem Docker a ser usada para criar o contêiner. Neste caso, é a imagem do PostgreSQL postgres:11.5-alpine. Finalmente, o comando docker container logs postgres exibe os logs do contêiner postgres, permitindo verificar se ele foi iniciado com sucesso e se o script SQL init-script.sql foi executado corretamente.
```sh
docker volume create pg-data

cd ~/fod/integration-test-node

docker container run -d \
--name postgres \
-p 5432:5432 \
-v $(pwd)/database:/docker-entrypoint-initdb.d \
-v pg-data:/var/lib/postgresql/data \
-e POSTGRES_USER=dbuser \
-e POSTGRES_DB=sample-db \
postgres:11.5-alpine

docker container logs postgres
```


## O comando cd ~/fod/integration-test-node/api muda para o diretório api usando o comando cd. Em seguida, o comando npm init é executado para inicializar um projeto Node.js. Isso criará um arquivo package.json que irá armazenar as informações do projeto, incluindo suas dependências. Depois disso, você abriu o arquivo package.json no editor de texto Vim usando o comando vim package.json. O arquivo package.json é um arquivo importante em projetos Node.js, pois ele especifica as informações do projeto e as dependências. Dentro do arquivo package.json, você adicionou um novo script chamado "start" ao objeto "scripts". Este script executará o arquivo index.js quando o comando npm start for executado. Por fim, você instalou a dependência do Express usando o comando npm install express --save. O parâmetro --save é usado para adicionar a dependência ao arquivo package.json.
```sh
cd ~/fod/integration-test-node/api
npm init

vim package.json
...

"scripts": {
    "start": "node index.js",
    ...
...

npm install express --save
```

## O comando vim index.js abre o arquivo index.js no editor de texto Vim. Este código define uma aplicação Express simples que responde à rota / com a mensagem "Sample API". A aplicação está ouvindo a porta 3000, que foi definida na variável port.Quando a aplicação é iniciada usando o comando npm start (ou node index.js), ela exibe a mensagem "Example app listening at http://localhost:3000" no console. Isso indica que a aplicação está em execução e pronta para receber solicitações na porta 3000.
```sh
vim index.js

// index.js

const express = require('express')
const app = express()
const port = 3000

app.get('/', (req, res) => {
  res.send('Sample API')
})

app.listen(port, () => {
  console.log(`Example app listening at http://localhost:${port}`)
})
```

## O comando npm start inicia a aplicação Express que foi definida no arquivo index.js. Em seguida, o comando curl localhost:3000 é executado para fazer uma solicitação HTTP GET à aplicação em execução. A resposta deve ser a mensagem "Sample API".Depois, o comando cd ~/fod/integration-test-node/tests && npm init é executado para criar um novo projeto Node.js no diretório tests. Será criado um arquivo package.json no diretório tests. Em seguida, o comando npm install --save-dev jasmine é executado para instalar o framework de teste Jasmine como uma dependência de desenvolvimento. O parâmetro --save-dev é usado para adicionar a dependência ao arquivo package.json. Depois, o comando node node_modules/jasmine/bin/jasmine init é executado para inicializar o projeto Jasmine no diretório tests. Isso criará a estrutura básica de diretórios e arquivos necessários para executar testes com o Jasmine. Por fim, o comando cd spec/support/ -- ls é executado para mudar para o diretório spec/support/ e listar o conteúdo desse diretório usando o comando ls.
```sh
npm start
curl localhost:3000

cd ~/fod/integration-test-node/tests && \
npm init

https://www.npmjs.com/package/jasmine
npm install --save-dev jasmine
node node_modules/jasmine/bin/jasmine init
cd spec/support/ -- ls
```


## O comando vim jasmine.json abre o arquivo jasmine.json no editor de texto Vim. Esse arquivo é a configuração do Jasmine, que especifica onde estão os arquivos de teste (spec_dir), quais arquivos de teste devem ser executados (spec_files) e algumas outras opções de configuração.Nesse caso, os arquivos de teste estão no diretório spec e têm o padrão **/*[sS]pec.js. Isso significa que o Jasmine procurará por arquivos que terminam com "spec.js" ou "Spec.js" em qualquer diretório abaixo de spec. A opção stopSpecOnExpectationFailure é definida como false, o que significa que o Jasmine não interromperá a execução do teste se uma expectativa falhar. A opção random é definida como false, o que significa que os testes serão executados na mesma ordem sempre que forem executados.
```sh
vim jasmine.json

{
	"spec_dir": "spec",
	"spec_files": [
	 "**/*[sS]pec.js"
	],
	"stopSpecOnExpectationFailure": false,
	"random": false
}
```

##
```sh
cd ~/fod/integration-test-node/tests

vim package.json

"scripts": {
 "test": "jasmine"
},

npm install request --save-dev
```


## O comando vim api-spec.js abre o arquivo api-spec.js no editor de texto Vim. Esse é um arquivo de teste Jasmine que testa a API criada anteriormente. O teste verifica se a rota raiz (/) retorna um código de status 200 e se o corpo da resposta é igual a "Sample API". O describe é usado para agrupar vários testes em uma suíte de teste. Dentro da suíte de teste, o it é usado para definir um único teste. O teste usa a biblioteca request para fazer uma solicitação HTTP GET para a rota raiz. Em seguida, verifica se o código de status da resposta é 200 e se o corpo da resposta é igual a "Sample API". O valor da variável base_url é definido como process.env.BASE_URL || 'http://localhost:3000'. Isso permite que o valor da variável seja definido como uma variável de ambiente ou o valor padrão é http://localhost:3000.
```sh
cd spec/

vim api-spec.js

// api-spec.js

var request = require("request");

const base_url = process.env.BASE_URL || 'http://localhost:3000'

describe("API test suite", () => {
    describe("GET /", () => {
        it("returns status code 200", function(done) {
            request.get(base_url, (error, response, body) => {
                expect(response.statusCode).toBe(200);
                done();
            });
        });
        it("returns description", () => {
            request.get(base_url, (error, response, body) => {
                expect(body).toBe("Sample API");
            });
        });
    });
});

cd .. 
```


## Antes de parar o contêiner do Postgres, é importante verificar se ele está em execução. Isso irá parar e remover o contêiner chamado "postgres". O contêiner será removido permanentemente e todos os dados do contêiner serão perdidos.
```sh
cd ~/fod/integration-test-node/api
npm start

docker container rm -f postgres
```

# Usando o dcoker:

## O comando FROM node:alpine especifica que estamos construindo a imagem a partir de uma imagem base do Node.js Alpine. Essa imagem base é uma imagem Alpine Linux que inclui o Node.js instalado.
O comando WORKDIR /usr/src/app define o diretório de trabalho da imagem.
O comando COPY package.json ./ copia o arquivo package.json para o diretório de trabalho da imagem.
O comando RUN npm install executa o comando npm install para instalar as dependências do aplicativo.
O comando COPY . . copia todo o conteúdo do diretório atual (que contém o código-fonte do aplicativo) para o diretório de trabalho da imagem.
O comando EXPOSE 3000 expõe a porta 3000 para o contêiner.
O comando CMD npm start especifica o comando que será executado quando um contêiner baseado nesta imagem for iniciado.
```sh
cd ~/fod/integration-test-node/api

vim Dockerfile

FROM node:alpine
WORKDIR /usr/src/app
COPY package.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD npm start
```


## O comando FROM node:alpine especifica que estamos construindo a imagem a partir de uma imagem base do Node.js Alpine.
O comando WORKDIR /usr/src/app define o diretório de trabalho da imagem.
O comando COPY package.json ./ copia o arquivo package.json para o diretório de trabalho da imagem.
O comando RUN npm install executa o comando npm install para instalar as dependências do aplicativo.
O comando COPY . . copia todo o conteúdo do diretório atual (que contém os testes) para o diretório de trabalho da imagem.
O comando CMD npm test especifica o comando que será executado quando um contêiner baseado nesta imagem for iniciado.
```sh 
cd ~/fod/integration-test-node/tests

vim Dockerfile

FROM node:alpine
WORKDIR /usr/src/app
COPY package.json ./
RUN npm install
COPY . .
CMD npm test
```

## O script test.sh que você editou é um script em shell que contém uma série de comandos para construir e executar imagens Docker, criar uma rede Docker e executar contêineres Docker para um ambiente de teste. Esses comandos são executados em uma linha de comando. 
```sh
cd /root/fod/integration-test-node

vim test.sh

 docker image build -t api-node api
 docker image build -t tests-node tests

 docker network create test-net
 docker container run --rm -d \
    --name postgres \
    --net test-net \
    -v $(pwd)/database:/docker-entrypoint-initdb.d \
    -v pg-data:/var/lib/postgresql/data \
    -e POSTGRES_USER=dbuser \
    -e POSTGRES_DB=sample-db \
    postgres:11.5-alpine

 docker container run --rm -d \
    --name api \
    --net test-net \
    -p 3000:3000 \
    api-node

 echo "Sleeping for 5 sec..."
 sleep 5

 docker container run --rm -it \
    --name tests \
    --net test-net \
    -e BASE_URL="http://api:3000" \
    tests-node

bash test.sh
```

## O comando docker container rm -f postgres api remove os contêineres chamados "postgres" e "api". O parâmetro -f força a remoção dos contêineres, mesmo que estejam em execução. O comando docker network rm test-net remove a rede chamada "test-net".Já o comando docker volume rm pg-data remove o volume chamado "pg-data".
```sh
docker container rm -f postgres api
docker network rm test-net
docker volume rm pg-data
```
