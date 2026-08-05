# Docker-Desktop: Create Volumes

#### Install
> nodemon package install korle bar bar terminal e project run korte hoina update dekher jonno. kono kichu update korle ta project ekbar run korlei dekha jai.
```bash
npm i nodemon
```
---

#### package.json e add koro
```bash
"dev": "nodemon index.js"
```
```bash
{
  "name": "node-app",
  "version": "1.0.0",
  "description": "Docker Course",
  "license": "ISC",
  "author": "",
  "type": "commonjs",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "nodemon index.js"
  },
  "dependencies": {
    "express": "^5.2.1",
    "nodemon": "^3.1.14"
  }
}
```
---

#### terminal e command daw
```bash
npm run dev
```
> ekhon kono kichu update hole browser e update hoye jabe.
---

#### globally run korar jonno 
```bash
"dev": "nodemon --legacy-watch index.js"
```
```bash
{
  "name": "node-app",
  "version": "1.0.0",
  "description": "Docker Course",
  "license": "ISC",
  "author": "",
  "type": "commonjs",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "nodemon --legacy-watch index.js"
  },
  "dependencies": {
    "express": "^5.2.1",
    "nodemon": "^3.1.14"
  }
}
```
---

#### dockerfile update koro
> RUN npm install -g nodemon ei line nodemon ke globally install korbe. WORKDIR /app ei line diye work directory bole dibe. CMD ["npm", "run", "dev"] ei line e cmd command er jonno
```bash
FROM node:latest
RUN npm install -g nodemon
WORKDIR /app
COPY . .
RUN npm install
EXPOSE 3000
CMD ["npm", "run", "dev"]
```
---

#### docker image create koro
```bash
docker build -t my-node-app .
```
---

#### create containers with volumes
> containers create hobe terminal e project run hobe project e kichu update hole browser er output update hoyejabe.
```bash
docker run --name my-containers -p 3000:3000 --rm -v "C:/Users/User/Desktop/Node App:/app" my-node-app
```
---
