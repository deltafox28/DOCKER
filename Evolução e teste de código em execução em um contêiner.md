# Evolução e teste de código em execução em um contêiner

##
```sh
mkdir -p ~/fod/node && cd ~/fod/node
npm init
npm install express --save
```

##
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

##
```sh
vim Dockerfile

FROM node:latest
WORKDIR /app
COPY package.json ./
RUN npm install
COPY . .
CMD node index.js
```

##
```sh
docker image build -t sample-app .

docker container run --rm -it \
--name my-sample-app \
-p 3300:3300 \
sample-app
```

##
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

##
```sh
docker image build -t sample-app .
docker container run --rm -it --name my-sample-app -p 3300:3300 sample-app
```
