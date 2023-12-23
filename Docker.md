### Instalation de docker
https://docs.docker.com/get-docker/
Activer la virtualisation dans le bios et l'hyperviseur dans les fonctionnalités windows 
Télécharger Docker Desktop

### Télécharger une image
Télécharge une image Docker depuis un registre, 'docker pull [OPTIONS] NAME[:TAG|@DIGEST]' 
Affiche la liste des images Docker disponibles localement, 'docker images [OPTIONS]'

### Gestion d'une image
Lance un nouveau conteneur à partir d'une image spécifiée, 'docker run [OPTIONS] nomImage'
	Options :
		* Exécute le conteneur en arrièr-plan : -d
		* Associe un port du conteneur au port de l'hôte : -p
		* Permet de relier un volume entre le conteneur et l'hôte : -v
		* Ajoute un nom au conteneur : —name
	Exemple : 'docker run --name test-wordpress -p 3000:80 wordpress' Lance le conteneur de nom test-wordpress sur le port local 3000

Arrête un containeur, 'docker stop nomContainer'
Redémarre un containeur, 'docker start nomContainer' 
Supprimer un conteneur, 'docker rm nomContainer '
	Options :
		Force la suppression d’un container en cours d'exécution : -f

Affiche la liste des conteneurs lancés, 'docker ps [OPTIONS] nomImage'
	Options :
		* Affiche tout les containers même ceux arreté : -a

### Commandes et logs
Exécute une commande dans un conteneur Docker en cours d'exécution, 'docker exec [OPTIONS] nomContainer queryCommand'
	Options :
		* Exécute la commande en arrière-plan : -d
		* Garde STDIN ouvert même si non connecté : -i
		* Alloue un pseudo-TTY : -t

Récupérer les logs d’un containeur, 'docker logs [OPTIONS] '
	Options :
		* Suivre les logs : -f
		* Nombre de lignes a récupérer (en partant de la fin) : -n

### Construire l'image d'un projet
#https://github.com/maximechbt/docker-examples
Construit une image Docker à partir d’un Dockerfile, 'docker build [OPTIONS] PATH'
	Options :
		* Associe un tag à l'image construite : -t
	Exemple : 'docker build -t test-docker .' A éxécuter dans le dossier du projet ou se situe le DockerFile, comme la fonction va chercher le DockerFile et qu'il se situe a la racine du projet je mets juste un ' . ' pour le chemin

### Dockerfile
Commandes relatives au Dockerfile afin de créer un container pour son projet
	FROM - Spécifie l’image de base pour la nouvelle construction. #Récuperer les image sur DockerHub
		Exemple : FROM php:7.2-apache
	RUN - Exécute des commandes pendant la construction de l’image. 
		Exemple : RUN npm install
	COPY - Copie des fichiers du système local dans le conteneur. #Récupère les fichiers du projet pour les insérer dans le conteneur
		Exemple : COPY app.js /usr/src/app/
	ADD - Copie des fichiers (système local ou URL) et peut également extraire des archives.
		Exemple : ADD http://example.com/archive.tar.gz /usr/src/app/
	CMD - Définit la commande par défaut lors du démarrage du conteneur.
		Exemple : CMD [“node”, “app.js”]
	ENTRYPOINT - Configure une commande pour être la commande principale au démarrage.
		Exemple : ENTRYPOINT [“node”, “app.js”]
	ENV - Définit une variable d’environnement dans le conteneur.
		Exemple : ENV NODE_ENV=production
	ARG - Définit une variable existant uniquement lors de la création de l’image.
		Exemple : ARG VERSION=latest

### Créer un volume
Ajouter un volume qui permet au container d’être synchroniser avec les modifications que vous faites sur le code.

### Docker compose
https://docs.docker.com/compose/
**Déploiement multi-conteneurs **: Coordonner et déployer plusieurs services simultanément.
**Configuration déclarative** : Définir l'architecture des services dans un fichier YAML.
**Interconnexion de services **: Faciliter la communication entre les conteneurs et établir des dépendances
**Environnements de développement** : Uniformiser les environnements pour les équipes de développement.
**Gestion de cycle de vie **: Démarrage, arrêt, mise à jour, et suppression des services de manière cohérente.

### Syntaxe du fichier de configuration
docker-compose.yml
Indique la version de la syntaxe Docker Compose utilisée dans le fichier : 'version'
Définit les différents services à exécuter, chaque service représentant un conteneur Docker : 'services'
Spécifie l'image Docker à utiliser pour le service : 'image'
Indique le chemin vers le Dockerfile pour la construction d'une image personnalisée plutôt que d'utiliser une image existante : 'build'
Mappe les ports du conteneur sur les ports de l'hôte pour permettre l'accès aux services : 'ports'
Montre les volumes à utiliser pour persister les données entre les exécutions des conteneurs : 'volumes'
Permet de définir des variables d'environnement pour le service : 'environment'
Définit les dépendances entre les services, indiquant l'ordre dans lequel ils doivent démarrer : 'depends_on'
Définit les réseaux sur lesquels les services seront connectés, facilitant la communication entre les conteneurs : 'networks'
Spécifie la commande à exécuter lorsque le conteneur démarre : 'command'
Ajoute des métadonnées au service, facilitant l'organisation et la gestion des services  : 'labels'

### Commandes docker-compose
https://docs.docker.com/compose/reference/
Build les services qui en ont besoin : 'docker-compose build [OPTIONS] [SERVICES…]'
Build, recrée et démarre les services : 'docker-compose up [OPTIONS] [SERVICES…]'
	Options :
		* Exécute les services en arrière-plan : -d 
Démarre les services existant : 'docker-compose start [OPTIONS] [SERVICES…]'
	Options :
		* Exécute les services en arrière-plan : -d 
Supprime les services qui ne sont pas en route : 'docker-compose rm [OPTIONS] [SERVICES…]'
	Options :
		* Force la suppression des services en route : -f

