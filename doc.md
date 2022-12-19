# TP Docker

## Installation de Docker et Docker-Compose

Le mieux est d'aller directement consulter la page de docker dédié a cet effet !  [![Docker Install Ubuntu](https://docs.docker.com/engine/install/ubuntu/)]

### Voici tout de même les commandes a executer :

```
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg lsb-release`
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

### Commande pour devenir docker root
```sudo usermod -aG docker dorian```





## Executer un serveur web dans un conteneur


### Récuperer une image depuis le Docker Hub

Le serveur repository par défaut de docker est le Docker Hub, il nous suffit alors d'executer la commande de pull pour récupérer l'image.

```docker pull httpd:2.4```

Commande pour vérifier les images présente sur le serveur

```docker images```

Je crée un fichier *index.html* avec le contenu suivant : `Finally working !`


### Lancer un conteneur avec un fichier persistant


```docker run -d -p 80:80 -v /home/dorian/apps/apache/index.html:/usr/local/apache2/htdocs/index.html httpd:2.4```


**_Commandes pour supprimer des conteneurs_**
```
docker rm -f id_conteneur
docker run -d -p 80:80 -v /home/dorian/apps/apache/index.html:/usr/local/apache2/htdocs/index.html httpd:2.4
```


### Copier un fichier dans un conteneurs

Je vais ici copier l'index créé au dessus dans le repertoire apache dans le conteneur ! 

```docker cp ./index.html apache:/usr/local/apache2/htdocs/```



## Builder une image


Builder une image permet de la faconner a notre guise pour qu'elle s'adapte a ce que l'on souhaite.


Commande de build

`docker build -t . name:tag`


Builder une image depuis un autre repertoire

`docker build -f /path/dockerimage -t name:tag .`



J'ai alors créé une image avec le chemin de l'index HTML créé précédement.

En lancant un conteneur a partir de cette image je n'ai alors plus besoin d'utiliser l'argument `-d` !

`docker run -d -p 80:80 apache:0.3`



## BDD dans un conteneur
```
docker run -d --name mysql-db -e MYSQL_ROOT_PASSWORD=password mysql:5.7
docker run -d --name myadmin --link mysql-db:db -p 8080:80 phpmyadmin/phpmyadmin
```

**_Paquet permetant d'observer les procésus en cours_**
```
apt install procps
ps -ef
```
