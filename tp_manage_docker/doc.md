# TP Docker

## Installation de Docker et Docker-Compose

Le mieux est d'aller directement consulter la page de docker dédié a cet effet !  [![Docker Install Ubuntu](https://docs.docker.com/engine/install/ubuntu/)]

#### Voici tout de même les commandes a executer :

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

#### Commande pour devenir docker root
```sudo usermod -aG docker dorian```

/!\ Commande nécessitant un changement de session.

## Executer un serveur web dans un conteneur

#### Récuperer une image depuis le Docker Hub

Le serveur repository par défaut de docker est le Docker Hub, il nous suffit alors d'executer la commande de pull pour récupérer l'image.

```docker pull httpd:2.4```

Commande pour vérifier les images présente sur le serveur

```docker images```


#### Lancer un conteneur avec un fichier local 

Je crée un fichier *index.html* avec le contenu suivant : `Finally working !`


**_Commande permettant de bind le repertoire `/home/dorian/apps/apache/` qui contient le fichier *index.html* dans le conteneur_**
```
docker run -d -p 80:80 -v /home/dorian/apps/apache/:/usr/local/apache2/htdocs/ httpd:2.4
```

**_Commande suppression de conteneurs_**
```
docker rm -f id_conteneur
```

**_Permet de supprimer tous les services et images non utilisé_**
```
docker systeme purge
```


#### Copier un fichier dans un conteneurs

Je vais ici copier l'index ajouté précédement dans le repertoire apache dans le conteneur ! 

```docker cp ./index.html apache:/usr/local/apache2/htdocs/```


## Builder une image


Builder une image permet de la faconner a notre guise pour qu'elle s'adapte a nos besoins.

**_Commande de build_**
```
docker build -t  name:tag .
```

**_Builder une image depuis un autre repertoire_**
```
docker build -f /path/dockerimage -t name:tag .
```

[WARNING] Faire attention au path des arguments COPY depuis le repertoire où on lance le build !

### Lancer un conteneur a partir de l'image crée précedement

En lancant un conteneur a partir de cette image je n'ai alors plus besoin d'utiliser l'argument `-v` !
```
docker run -d -p 80:80 apache:0.3
```


Utilisé des images builder permet :

- d'éviter les commandes à rallonge
- de partager une image avec tout son contenu (fichier html...)


## BDD dans un conteneur
```
docker run -d --name mysql-db -e MYSQL_ROOT_PASSWORD=password mysql:5.7
docker run -d --name myadmin --link mysql-db:db -p 8080:80 phpmyadmin/phpmyadmin
```

## Docker Compose

Voici a quoi ressemberait la même conf avec un fichier docker-compose.yml 


```
Version: '3.5'

services:

  db:
    image: mysql:5.7
    restart: always
    environment:
      MYSQL_USER: user
      MYSQL_PASSWORD: upassword
      MYSQL_DATABASE: lab
      MYSQL_ROOT_PASSWORD: password

  admin:
    image: phpmyadmin/phpmyadmin
    restart: always
    ports:
      - 8080:80
    depends_on:
      - db
```

Le fichier docker-compose permet une meilleure visibilité de la configuration complete dans les conteneurs.

C'est la variable `environment` qui permet de configurer les services.


## Observation des réseaux dans Docker

Création de 3 services (web, app et db) et 2 réseaux (frontend et backend) avec l’image praqma/network-multitool.

Voici de docker-compose

```
version: '3.7'

networks:
  front-end:
    driver: bridge
  back-end:

services:

  web:
    image: wbitt/network-multitool
    networks:
      - front-end

  app:
    image: wbitt/network-multitool
    networks:
      - front-end
      - back-end
      
  db:
    image: wbitt/network-multitool
    networks:
      - back-end

```





**_Paquet permetant d'observer les processus en cours_**
```
apt install procps
ps -ef
```


