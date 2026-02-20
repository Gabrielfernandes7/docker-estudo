# Cache efiente de camadas

Cache eficiente de camadas
Ao construir imagens de container, o Docker armazena em cache as camadas criadas. Essas camadas podem ser reutilizadas posteriormente ao construir outras imagens, reduzindo o tempo de build e o uso de banda. Para aproveitar bem esse mecanismo, é importante entender como ele funciona.

O Docker cria uma nova camada para cada instrução no Dockerfile (como RUN, COPY, ADD, etc.). Se a instrução não mudou desde o último build, o Docker reutiliza a camada existente.

Como funciona o cache de build
Quando você constrói a mesma imagem várias vezes, entender o cache permite builds mais rápidos.

```dockerfile
FROM ubuntu:latest

RUN apt-get update && apt-get install -y build-essentials
COPY main.c Makefile /src/
WORKDIR /src/
RUN make build
```

Exemplo real com o node

Dockerfile ruim:

```dockerfile
# ruim lento
FROM node:20

COPY . .
RUN npm install
RUN npm build
```

```dockerfile
# bom (rápido)
FROM node:20

COPY package.json package-lock.json .
RUN npm install

COPY . .
RUN npm build
```

Se mudar apenas o código → npm install fica em cache
Build fica muito mais rápido