# TP2 Docker


## Compléter le Dockerfile pour pouvoir builder l'application

npm est le gestionnaire de paquets par défaut pour l'environnement d'exécution JavaScript Node.js de Node.js. npm se compose d'un client en ligne de commande, également appelé npm, et d'une base de données en ligne de paquets publics et privés payants, appelée le registre npm. 

L'option npm permettant d'installer seulement les paquets necessaire est `--production`


Running npm install without arguments installs modules defined in the dependencies section of the package.json file.
It's important that npm install is run in the same directory as the package.json file.

La bonne pratique est d'installer le moins de packet possible pour avoir des images légère, utiliser cette option est alors la meilleure pratique concernant l'installation des packets nécessaire a notre application.

