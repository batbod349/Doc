# Initialiser un projet
Modules nécessaires :
	PHP et Composer
### Création du projet
Pour créer les fichiers du projet exécuter la commande
	'symfony new [Nom du projet]' #Ajouter —webapp apres le new pour les dépendances d'un site web
### Gestion de la base de données 
#Executer laragon
1. Créer un .env.local 
	Permet de gérer sa propre db en overwrite le .env et le mets automatique dans le gitignore
	Récuperer la ligne : _# DATABASE_URL="mysql://app:!ChangeMe!@127.0.0.1:3306/app?serverVersion=10.11.2-MariaDB&charset=utf8mb4"_
	La transformer en : DATABASE_URL__=__"__mysql://[NomUser]:[MdpUser]@127.0.0.1:3306/[NomDB]?serverVersion=10.11.2-MariaDB&charset=utf8mb4__" _#On peut normalement supprimer se qu'il y a après le [NomDB]
2. Création de la base de données
	Exécuter la commande 'symfony console doctrine:database:create'
2. Création des tables
Pour créer une table exécuter la commande 'symfony console make:entity'
Entrer le nom de la table, ne pas ajouter de broadcast entity update Symfony UX Turbo (Pourquoi??)
* Ajouter des champs
	Mettre le nom du champs
	Type du champs : string(limité à 255c), integer, text(pas de limite), relation(pour les FK)
* Modifier une Table
	Ajouter un champs d'une table déja existante : refaire la commande de création, cela fait rerentrer dans la table
	Modifier le champ d'une table : modifier directement dans Entity/[NomTable] et refaire une migration
4. Remplir la base de données avec des données randoms
	* Création des fichiers Yaml
		Exécuter la commande 'composer require doctrine-orm' puis 'composer require --dev hautelook/alice-bundle' un dossier fixtures apparait dans le projet
		Créer un fichier .Yaml pour chaques tables 
				App\Entity\Movie:
			    movie{0..50}:
			      name: "<name()>"
			      created_at: "<dateTimeBetween('-20 years', 'now')>"
			      launched_at: "<dateTimeBetween('-20 years', 'now')>"
			      note: "<numberBetween(1, 5)>"
			      image_path: "img/req1.png"
			      category: "<numberBetween(1, 3)>x @category*" #FK relié a une liste donc choisit de 1 à 3 éléments de la table catégory
			      studio: "@studio*" #FK
		Une fois les fichiers .yaml remplis, exécuter la commande 'symfony console hautelook:fixtures:load --purge-with-truncate' #Cela supprime les données déjà dans la base
5. Utilisation des fonctions de DB
	#Ajouter un constructeur
	use App\Entity\Movie;
	use App\Form\MovieType;
	use Doctrine\ORM\EntityManagerInterface;
	use App\Repository\MovieRepository;
	 public function __construct(
	        private MovieRepository $movieRepository,
	        private EntityManagerInterface $entityManager,
	    )
	    {
	    }
		* SELECT
			Pour SELECT dans la db, utiliser les fcts find() dans Repository/[NomTable] #Ne pas oublier d'ajouter 'use App\Repository\[NomTable]Repository;' dans le fichier d'utilisation de la fct
		* ADD, REMOVE..
			Pour ADD dans la db utiliser EntityManagerInterface #Ne pas oublier d'ajouter 'use Doctrine\ORM\EntityManagerInterface;' dans le fichier d'utilisation de la fct
6. Gestion des form
	* Création d'un form
		Exécuter la commande 'symfony console make:form' #La convention veut que le nom se termine par 'Type'
	* Gestion du form
		->add('imagePath', TextType::class, [
		                'label' => "Chemin de l'image"
		            ])
		            ->add('note', IntegerType::class)
		            ->add('category', EntityType::class, [
		                'class' => Category::class,
		                'choice_label' => 'label',
		                'multiple' => true,
		                'expanded' => true
		            ])
		Chaque route aura sa propre request donc on met Request $request dans les paramètres #public function [NomController]Add(Request $request)
	* Fonctions utiles
		Pour ajouter ou modifier des données  -> persist 
		Pour supprimer des données -> remove 
			Si on veut supprimer un éléments qui possède des relations on peut ajouter un paramètre dans sont fichier entity qui permet de supprimer automatiquement les données en relation avec lui, on mets ce paramètre dans la creation de la fonctions dans entity
				#[ORM\OneToMany(mappedBy: 'owner', targetEntity: Animal::class, cascade: ['remove'])]
					private Collection $animals;
		Les fonctions qu'on utilise se mettent en file d'attentes il faut donc les flush() pour envoyer la requete 
		#[Route('/movie_add', name: 'app_movie_add')]
		    public function ownerAdd(Request $request)
		    {
		        $movieEntity = new Movie();
		        $form = $this->createForm(MovieType::class, $movieEntity);
		        $form->handleRequest($request);
		        if ($form->isSubmitted() && $form->isValid()) {
		            _//persist notre owner_
		            $this->entityManager->persist($movieEntity);
		            _//flush notre owner_
		            $this->entityManager->flush();
		            _//redirection_
		            return $this->redirectToRoute('app_movie_list');
		        }
		        _//l'object request reprend toutes les super globales de PHP ($GET, $POST)_
		        return $this->render('movie/add.html.twig', [
		            'form' => $form->createView(),
		        ]);
		    } 
	### Informations utiles
	* Debug
		Utiliser dump() on a l'apparition de la cible en bas à gauche de la fenetre dans le navigateur
	* Exemple de use
		use App\Entity\Movie;
		use App\Form\MovieType;
		use Doctrine\ORM\EntityManagerInterface;
		use Symfony\Component\HttpFoundation\Request;
		use App\Repository\MovieRepository;
		use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
		use Symfony\Component\HttpFoundation\Response;
		use Symfony\Component\Routing\Annotation\Route;
	* Route
		Chaque route a une partie URL et une partie code 
			#[Route('/', name: 'app_home')]
		Appeler une route via un bouton
			<a href="{{ path('app_home') }}">Home</a>
	* Controller
		Pour envoyer une variable d'un controller à un template, créer la variable dans la fonction et l'envoyer via le return
			 public function index(): Response
			    {
			        $test = 'Bonjour !';
			        $array = ['salut', 'aurevoir', 'test'];
			        return $this->render('home/index.html.twig', [
			            'mySuperVar' => $test,
			            'monArray' => $array
			        ]);
			    }
		Pour récuperer la variable dans le template
			 {{ mySuperVar }}
			    <ul>
			    {% for value in monArray %}
			        <li> {{ value }} </li>
			    {% endfor %}
			    </ul>

