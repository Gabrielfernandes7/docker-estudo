# Dockerhub (Registro de containers)

Dockerhub é o maior repositório público e nuvem de imagens Docker

- **Repositórios públicos e privados**  
  - Públicos: ilimitados (gratuito) → qualquer um pode puxar sua imagem  
  - Privados: limitados no plano gratuito (geralmente 1 privado), mais no pago  

- **Imagens oficiais**  
  São imagens mantidas pelos próprios criadores das tecnologias (ex: `nginx`, `node`, `python`, `postgres`, `redis`).  
  Sempre confiáveis, atualizadas e com boas práticas de segurança.

- **Builds automatizados**  
  Conecte seu GitHub/GitLab/Bitbucket → toda vez que você fizer push no repositório, o Docker Hub compila automaticamente uma nova imagem Docker.  
  Perfeito para CI/CD simples sem precisar de GitHub Actions ou GitLab CI só para build de imagem.

- **Webhooks**  
  Após um build ou push, o Docker Hub pode chamar uma URL (ex: disparar deploy no Kubernetes, notificar Slack, etc.).

- **Integração com Docker CLI**  
  Comandos simples que você já usa:
  ```bash
  docker pull node:20-alpine          # baixa do Docker Hub
  docker push usuario/minha-app:1.0   # envia sua imagem para o Hub
  docker tag minha-app usuario/minha-app:1.0
  docker login