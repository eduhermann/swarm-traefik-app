# swarm-traefik-app

## Requisitos

* Docker instalado nos nodes com o cluster Swarm ativo
* HAProxy instalado e configurado para balancear carga entre os nodes Master do cluster Swarm.

## Executando a aplicação

1 - Efetue o clone do repositório:

2 - Efetuar a alteração das variáveis necessárias de acordo com o seu ambiente:
```
Arquivo: deploy.yml
    - "traefik.http.routers.webserver.rule=Host(`SUA_URL_AQUI`)"
```

2 - Efetuar o build da imagem da aplicação:
```
$ cd ./app-build/

$ docker image build -t web-app:1.0 .
```

    * A imagem deverá ser enviada a um registry ou então deverá ser efetuado o build em todos os nodes do cluster.

3 - Criação de nova network
```
$ docker network create -d overlay traefik-public
```

4 - Efetuar a criação da nova stack no cluster Swarm:

```
$ docker stack deploy -c deploy.yml
```