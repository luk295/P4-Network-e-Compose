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