# swarm-traefik-app

### Requisitos

* Docker instalado nos nodes com o cluster Swarm ativo, preferencialmente utilizando os 03 nodes como master.
* HAProxy instalado e configurado para balancear carga entre os nodes Master do cluster Swarm.

### Topologia

Topologia simples baseada em um cluster  Docker Swarm composto por 03 nodes, 02 nodes para utilização do HAProxy balanceando carga para os nodes Swarm (instalação e configuração do HAProxy não fará parte deste repositório).

![Topologia Swarm Traefik App](https://github.com/eduhermann/swarm-traefik-app/blob/main/diagram.png)

### Recursos utilizados

Aqui estão alguns dos recursos utilizados para realizar o deploy do ambiente.

Lembrando que é possível adaptar o ambiente caso necessário.

* 05 VMs
    * S.O. Ubuntu 20.04 LTS
    * 02 GB de Memória RAM
    * 02 core CPU
    * 01 network card
        * 3 VMs para os nodes Masters
        * 2 VM para o HAProxy em redundância

É possível remover a camada "High Availability Zone" composta por duas VMs para o HAProxy, deixando somente os 03 nodes Docker Swarm.

### Executando a aplicação

1 - Efetue o clone do repositório:
```
$ git clone https://github.com/eduhermann/swarm-traefik-app.git
```

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

### Enjoy!