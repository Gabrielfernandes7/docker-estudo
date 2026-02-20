# Boas Práticas com Docker

## Tamanho da imagem e segurança

Imagens grandes trazem problemas como: aumenta o tempo de deploy, demoram muito para ser baixadas (pull), ocupam muito espaço no disco (registry, máquina local, cluster kubernetes), aumentam a superfície de ataque (mais pacotes = mais vulnerabilidades potenciais), gastam mais banda na rede toda vez que é feito pull na imagem.

Exemplo:

```dockerfile
# Etapa 1 – BUILDER (pesada)
FROM node:20 AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# Etapa 2 – IMAGEM FINAL (leve)
FROM node:20-alpine
WORKDIR /app
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/package.json ./
EXPOSE 3000
CMD ["node", "dist/index.js"]
```


## Multi-stage builds

São construções em múltiplas etapas e são uma das boas práticas mais recomendadas na documentação oficial do Docker.

```dockerfile
# Etapa 1: BUILD (pesada, com toolchain)
FROM golang:1.25 AS builder
WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -o /myapp ./cmd/myapp

# Etapa 2: RUNTIME (mínima, segura)
FROM scratch
COPY --from=builder /myapp /myapp
CMD ["/myapp"]
```

1. Use o buildkit
    BuildKit é o motor do Docker moderno, ele é mais inteligente que o build do Docker antigo
    - Constrói só os estágios que realemente precisam (dependendo do `--target`)
    - Faz o cache melhor (mesmo em multi-stage)
    - Permite builds paralelos em etapas independentes
    - Mais seguro e mais rápido
2. Use a sintaxe recomendada no topo do Dockerfile 

    ```dockerfile
    # syntax=docker/dockerfile:1
    FROM ...
    ```

3. Nomeie todas as etapas com AS (builder, dev, prod, etc.)

    ```dockerfile
    # Exemplos Bons
    FROM node:20 AS builder          # ← nomeado
    FROM node:20-alpine AS runtime   # ← nomeado
    FROM scratch AS final            # ← nomeado

    # Evite
    FROM node:20
    FROM node:20-alpine
    # Aqui você teria que usar --from=1 (confuso!)
    ```

4. Use a flag --target para compilar só o que precisa

    - Com `--target` você diz: "pare o build nesse estágio específico"
    - Isso é perfeito para ter um Dockerfile que gera imagens diferentes 
        - Imagem de desenvolvimento (com ferramentas extras)
        - Imagem de produção (super leve e segura)
        - Imagem só para testes
    ```dockerfile
    # Etapa comum: instalação de dependências
    FROM node:20 AS deps
    WORKDIR /app
    COPY package*.json ./
    RUN npm ci

    # Etapa de build (para produção)
    FROM deps AS builder
    COPY . .
    RUN npm run build

    # Etapa de desenvolvimento (com tudo para debug e hot-reload)
    FROM deps AS dev
    COPY . .
    CMD ["npm", "run", "dev"]

    # Etapa final de produção (leve e segura)
    FROM node:20-alpine AS prod
    WORKDIR /app
    COPY --from=builder /app/dist ./dist
    COPY --from=builder /app/node_modules ./node_modules
    COPY --from=builder /app/package.json ./
    EXPOSE 3000
    CMD ["node", "dist/index.js"]
    ```