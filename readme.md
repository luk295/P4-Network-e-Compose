### P4 - Network e Compose
## Docker Network
# 1-Crea unha rede en Docker
Creo a rede con `docker network create A_miña_primera_Rede`.

Me devolve a NETWORK ID da rede creada. E agora co comando `docker network ls`listo as redes. E vexo que está a nova rede creada.

# 2- Crea dous contenedores unidos a esa rede
Creo os contenedores na misma rede:

```
docker run -it -name P4_contenedor1 --network A_miña_primera_Rede ubuntu bash
docker run -it -name P4_contenedor2 --network A_miña_primera_Rede ubuntu bash
```

# 3- Comprobar que os contenedores están na rede.
Para comprobalo fago `docker network inspect A_miña_Rede`.

# 4-Comprobar que os contenedores poden verse entre eles.
Para comprobar que poden verse entre eles podemos probar con `ping`.

Hacemos `ping`á dirección da outra máquina do outro contenedor. ( Para saber a propia dirección facemos `ip a`.)

>[!TIP]
>Se non temos os comandos básicos bash podemos instalalos cos comandos que veñen a continuación.

```
apt update && apt install iputils-ping
apt update && apt install iproute2
```
**Fan ping entre elas**

# 5- Listar os contenedores conectados á rede
Isto podo facelo co comando `docker network inspect A_miña_primera_Rede`.

Na última parte do arquivo, mostra un apartado que di **containers**, onde vese toda a información de cada máquina conectada a esa rede.

![Imaxe dos contenedores](https://github.com/luk295/P4-Network-e-Compose/blob/main/Contenedores.png)

# 6- Listar as propiedades da rede
Podo listar as propiedades co mesmo comando: `docker network inspect A_miña_primera_Rede`.

Móstrame o arquivo no que aparece o **name** da rede, a **id**, **cando** foi creado, a **configuración** da rede (Subrede, Rango, puerta de enlace)...

![Propiedades da rede](https://github.com/luk295/P4-Network-e-Compose/blob/main/Rede_info.png)

# 7-Crea outra rede
Creo outra rede coas mesmas caracteristicas ou outra completamente distinta. Para isto utilizo o comando `docker network create` + `as características da rede` + `o nome da nova rede`.

```
docker network create --subnet 172.29.0.0/16 --ip-range 172.29.5.0/24 --gateway 172.29.5.1 SEGUNDA_REDE

```
# 8- Lanza dous contenedores novos conectados a esa rede.
Contenedor5: `docker run -it --name Contenedor5 --network SEGUNDA_REDE ubuntu bash`

Contenedor6: `docker run -it --name Contenedor6 --network SEGUNDA_REDE ubuntu bash`

# 9- Comproba as posibles conexións entre os 4 contenedores
Probo a facer ping entre eles. E solo poden verse os contenedores que están na mesma rede.

Isto fai posible crear contenedores nunha red que vense entre sí, aillados doutros contenedores creados noutra rede.
## Docker Compose

# 10- Agora que sabes algo máis de docker-compose, crea un arquivo (ou varios arquivos) de configuración que ó ser lanzados cun docker-compose up, resulten nunha rede docker á que estean conectados 3 contenedores, explica os parámetros do .yaml usado
Creei un arquivo .yaml moi simple para lanzar 3 contenedores con `docker-compose up`. (Pero sin rede):
```
version: '3.8'

services:
 ubuntu1:
  image: ubuntu
  container_name: ubuntu_N1
  tty: true

 ubuntu2:
  image: ubuntu
  container_name: ubuntu_N2
  tty: true

 ubuntu3:
  image: ubuntu
  container_name: ubuntu_N3
  tty: true
```

>[!NOTE]
>ACTUALIZACIÓN DO EXERCICIO 10

Creei varios arquivos para lanzar os contenedores seguindo as estruccións do enunciado. Primeiro fixen 3 arquivos .yaml no que cada un creará un contenedor. Exemplo do primer contenedor:
```
services:
  ubuntu1:
    image: ubuntu
    container_name: ubuntu_N1
    tty: true
    networks:
      - MIRED

```
en `services` digo que me cree o ubuntu1, que será unha `image` (imaxe) ubuntu, co `container_name nome` (nome) ubuntu_N1, que terá terminal activa asociada `tty:true`e por último, en `networks`, que estará conectada nunha rede chamada `MIRED` (Esta rede a crearei no arquivo principal para todos os contenedores, que é o `docker-compose.yml`).

Esta arquivo chámase ubuntu1.yaml, e farei dous contenedores máis coas mesmas características, cambiando o número do nome da máquina. 

Agora con ubuntu1.yaml, ubuntu2.yaml, ubuntu3.yaml creados, creo o arquivo principal que é o `docker-compose.yml`:

```
include:
  - ubuntu1.yaml
  - ubuntu2.yaml
  - ubuntu3.yaml

networks:
  MIRED:
    driver: bridge

```
No include introduzco os arquivos .yaml, que son as máquinas. En `networks` creo a rede MIRED que estará en `bridge`.

### Agora solo queda executar o arquivo docker-compose.yml e crearase os 3 contenedores conectados nesa mesma rede:
Para executar o arquivo entro na carpeta onde teño a proxecto compose (onde creei o arquivo docker-compose). E executo: **docker compose up**

![IMAXE DO COMPOSE](https://github.com/luk295/P4-Network-e-Compose/blob/main/docker-compose.png)

# 11- Busca e proba 4 parámetros e configuracións diferentes que podes incluir no arquivo compose, explica qué fan. (por exemplo diferentes cousas que facer coa opción RUN)

