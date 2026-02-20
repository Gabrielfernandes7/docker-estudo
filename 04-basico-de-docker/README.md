# Básico de Docker

Docker é uma plataforma que simplifica building, enpacotamento e deploy de aplicações de forma leve, em containers portáveis.

A chave dos componentes incluem Dockerfiles (contruir as instruções), imagens (snapshots) e containers (instâncias em execução). Os comandos principais são para baixar as imagens, executar containers com mapeamento de portas e gerenciar tanto containers quanto imagens.

## Docker componentes

Existem três componentes chave no ecossistema Docker

Dockerfile: Um arquivo de texto que possui as instruções para contruir uma imagem.

Docker Image: Um snapshot do container, criado com o Dockerfile. Imagens são armazenadas no registry, como Dockerhub e podem ser requisitadas ou enviadas para o registry.

Docker Container: a instância em execução de uma imagem Docker.

## Docker comandos

* `docker pull <image>`: Baixa uma imagem do registry (Dockerhub)

* `docker build -t <image_name> <path>`: Contrói uma imagem a partir do Dockerfile, onde `<path>` é o diretório que contém o Dockerfile.

* `docker image ls`: lista todas imagens disponíveis em sua máquina local.

* `docker run -d -p <host_port>:<container_port> --name <container_name> <image>`: executa um container a partir de uma imagem, mapeando portas do host para portas do container.

* `docker container ls`: Lista todos os containers **em execução**.

* `docker container stop <container>`: Para um container em execução.

* `docker container rm <container>`: Remove um container parado.

* `docker image rm <image>`: Remove uma imagem da sua máquina local.

Exemplo de imagem Docker:

```Dockerfile
FROM python:3.10-slim

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY . .

CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```
