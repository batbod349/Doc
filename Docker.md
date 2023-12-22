# Instalation de docker
#https://docs.docker.com/get-docker/
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
Construit une image Docker à partir d’un Dockerfile, 'docker build [OPTIONS] PATH'
	Options :
		* Associe un tag à l'image construite : -t
	Exemple : 'docker build -t test-docker .' A éxécuter dans le dossier du projet ou se situe le DockerFile, comme la fonction va chercher le node_modules et qu'il se situe a la racine du projet ou mets juste un ' . ' pour le chemin

