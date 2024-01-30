## Créer une table
```sql
CREATE TABLE nom_table (
    id INT AUTO_INCREMENT PRIMARY KEY,
    champ1 VARCHAR(255),
    champ2 DOUBLE,
    champ3 LONGTEXT
);
```
### Clés
Ajouter un champ INT avec la clés étrangère (Pour du OneToMany la clé etrangère dois être déclaré dans le One)
FOREIGN KEY (nom_clés) REFERENCES_ _table_étrangère(id);
### Inserer des données
```
INSERT INTO ma_table (colonne1, colonne2, colonne3)
VALUES
  ('valeur1_ligne1', 'valeur2_ligne1', 'valeur3_ligne1'),
  ('valeur1_ligne2', 'valeur2_ligne2', 'valeur3_ligne2'),
  ('valeur1_ligne3', 'valeur2_ligne3', 'valeur3_ligne3');
```
### Modifier une table
```
-- Supprimer une colonne
ALTER TABLE nom_table
DROP COLUMN nom_colonne
DROP FOREIGN KEY nom_clé_étrangère;
```
### Modifier des valeurs 
```sql
UPDATE nom_table
SET colonne1 = nouvelle_valeur1, colonne2 = nouvelle_valeur2, ...
WHERE condition;
```
### Récuperer les valeurs de la table
```sql
SELECT
commentary.id,
commentary.comment,
movie.title AS movie_title,
studio.label AS studio_label
FROM commentary
INNER JOIN movie ON commentary.movie_id = movie.id
INNER JOIN studio ON movie.studio_id = studio.id;
```
## Trigger
Les triggers permettent d'automatiser des actions spécifiques lorsqu'une modification de données survient, offrant ainsi une gestion efficace des événements au niveau de la base de données.
```sql
DELIMITER //

CREATE TRIGGER before_insert_on_etudiant
BEFORE INSERT ON etudiant
FOR EACH ROW
BEGIN
        IF NEW.nom = "Julien" THEN
                SIGNAL SQLSTATE "45000"
                SET MESSAGE_TEXT = "Julien n'est pas autorisé !!!";
        END IF;
END//
DELIMITER ; 
```
DELIMITER : Change le délimiteur du code dans cette exemple ';' devient '//' 
## Event
```sql
CREATE EVENT delete_toto
ON SCHEDULE EVERY 1 MINUTE 

STARTS NOW() + INTERVAL 2 WEEK + 1 HOUR
ENDS NOW() + INTERVAL 2 YEAR 

DO 
        DELETE FROM etudiant WHERE nom = "toto";
```
## Procedure
```sql
DELIMITER //
CREATE PROCEDURE insert_etudiant(
    IN v_taille INT,
    OUT v_number INT
)
BEGIN

SELECT COUNT(etudiant.id) INTO v_number FROM etudiant WHERE etudiant.taille > v_taille;

END //
DELIMITER ; 
```
```sql
DELIMITER //
CREATE PROCEDURE insert_etudiant(
    IN v_nom VARCHAR(255),
    IN v_prenom VARCHAR(255),
    IN v_taille INT,
    IN v_sexe VARCHAR(255)
)
BEGIN

INSERT INTO etudiant(nom, prenom, taille, sexe)
VALUES (v_nom, v_prenom, v_taille, v_sexe);

END //
DELIMITER ; 
```
### Appel de la procedure
```sql
CALL insert_etudiant(180, @number);
SELECT @number;
```
## Fonctions utiles
### AVG() 
Calcule la moyenne des valeurs dans une colonne numérique.
```
SELECT AVG(score) AS moyenne_scores FROM notes;
```
### SUM() 
Calcule la somme des valeurs dans une colonne numérique.
```
SELECT SUM(montant) AS total_ventes FROM ventes;
```
### COUNT()
 Compte le nombre d'enregistrements dans une table ou le nombre d'occurrences d'une valeur spécifique dans une colonne.
```
SELECT COUNT(*) AS nombre_total_clients FROM clients;
```
### MIN()
Retourne la valeur minimale d'une colonne.
```
SELECT MIN(prix) FROM produits;
```
### MAX()
Retourne la valeur maximale d'une colonne.
```
SELECT MAX(prix) FROM produits;
```
### GROUP BY 
Groupe les résultats en fonction des valeurs d'une ou plusieurs colonnes.
```sql
SELECT departement, AVG(salaire) FROM employes GROUP BY departement;
```
### DISTINCT
Retourne les valeurs uniques dans une colonne.
```
SELECT DISTINCT departement FROM employes;
```
### WHERE
Filtre les résultats en fonction d'une condition. 
```
SELECT nom, salaire FROM employes WHERE salaire > 50000;
```
### LIKE
Permet de faire des comparaisons de chaînes de caractères en utilisant des motifs.
```
SELECT nom FROM clients WHERE nom LIKE 'A%'; (Retourne les noms commençant par 'A').
```
ORDER BY
Trie les résultats en fonction d'une ou plusieurs colonnes.
```
SELECT nom, salaire FROM employes ORDER BY salaire DESC; (Trie par salaire de manière décroissante).
```
### JOIN
Combine les lignes de deux tables en fonction d'une condition de jointure.
```sql
SELECT employes.nom, departements.nom FROM employes JOIN departements ON employes.departement_id = departements.id;
```

![](https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Ftse4.mm.bing.net%2Fth%3Fid%3DOIP.J1l8cF2zIgO03iqdXrS-7wHaEK%26pid%3DApi&f=1&ipt=204b9044715c7e587389aef8ac3d9dc3e96273ac9f3a6369e52b258715016123&ipo=images)

