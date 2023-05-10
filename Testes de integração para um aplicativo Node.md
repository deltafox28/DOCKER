# Testes de integração para um aplicativo Node

##
```sh
mkdir ~/fod/integration-test-node && \
cd ~/fod/integration-test-node

mkdir tests api database
cd database
```


##
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


##
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

##
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


##
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


##
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


##
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


##
```sh
cd ~/fod/integration-test-node/api
npm start

docker container rm -f postgres
```

# Usando o dcoker:

##
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


##
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

##
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

##
```sh
docker container rm -f postgres api
docker network rm test-net
docker volume rm pg-data
```
