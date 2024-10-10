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
apt update $$ apt install iputils-ping
apt update $$ apt install iproute2
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

