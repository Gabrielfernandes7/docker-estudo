# Persistência no Docker

No Docker, o sistema de arquivos interno do container faz parte da camada temporária criada no momento da execução. Quando o container é removido, essa camada também é descartada. Isso é útil para ambientes imutáveis e reproduzíveis, mas inadequado para aplicações que precisam armazenar dados permanentes, como bancos de dados ou uploads de usuários. Para resolver isso, o Docker oferece mecanismos como volumes e bind mounts, que permitem armazenar dados fora do ciclo de vida do container, garantindo persistência mesmo após reinicializações ou recriações.

Para contornar esse problema, a persistência dos dados é feita fora do ciclo de vida do container;

A aplicação roda dentro do container e os dados vivem fora do container. Os dois mecanismos para que isso ocorra são: "bind mount" e "volume mount".

## Bind Mount - Pasta do host conectada ao container

Você conecta uma pasta real da sua máquina (host) a uma pasta dentro do container. Exemplo:

```shell
docker run -v /home/usuario/dados:/app/data nginx
```

O container usa o /app/data, mas o dados estão na sua máquina, se o container for removido, os arquivos continuam no host

## Volume Mount - Volume gerenciado pelo Docker

Docker cria e gerencia uma área de armazenamente independente do container.

### Criando o volume

```shell
docker volume create meu_volume
```

### Usando o volume

```shell
docker run -v meu_volume:/app/data nginx
```

```shell
# listar volumes
docker volume ls

# remover volumes
docker volume rm meu_volume

# ver informações detalhadas dos volumes
docker volume inspect meu_volume
```

## Conteinerizando Banco de Dados

```shell
docker run --name my-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -e MYSQL_DATABASE=mydb -d mysql:latest
```
