# Ajout de Tailwind
Modules nécessaires :
	Node.js et npm (Tailwind CLI)
1. Installation de Webpack Encore
	Exécuter la commande 'composer require symfony/webpack-encore-bundle'
2. Installation de Tailwind CSS
	Exécuter les commandes 'npm install -D tailwindcss postcss postcss-loader autoprefixer' et 'npx tailwindcss init -p'
3. Activer le support PostCSS
	Dans webpack.config.js ajouter la ligne : '.enablePostCssLoader()' en dessous de 'Encore'
4. Configuration des chemins de templates
	Dans tailwind.config.js ajouter
		```javascript
		content: [
		    "./assets/**/*.js",
		    "./templates/**/*.html.twig",
		  ],
		```5. Ajout des directives Tailwind
	Dans assets/styles/app.css ajouter
		@tailwind _base_;
		@tailwind _components_;
		@tailwind _utilities_;
6. Activation de Tailwind
	Mettre en commentaire l'entièreté du fichier assets/bootstrap.js afin qu'il n'y est pas de conflit entre bootstrap et tailwind
	Afin que la gestion des classes Tailwind se fasse en temps réel il faut exécuter la commance 'npm run watch'

