# Processus de déploiement de WordPress
Le but va être de déployer sur le web votre installation locale de Wordpress.

## Déploiement vers l'hébergement

Le déploiement se fait en 3 temps :
1. Uploader les fichiers sur le serveur.
2. Migrer votre base de donnée.
3. Modifier la configuration de l'installation Wordpress.

### 1. Uploader les fichiers

<small>Prérequis : Pour accéder à votre serveur vous aurez besoin d'un client FTP. Nous utiliserons [FileZilla](https://filezilla-project.org/download.php?type=client). </small>

<small>Attention si vous devez ré-installer FileZilla : lors de l'installation il vous propose d'installer également des adwares (genre : McAfee, Opera). Just Say No.</small>

1. Lancez FileZilla
2. Cliquez sur le bouton en haut à gauche pour ouvrir la fenêtre de "Site Manager".
3. Si vous n'avez aucun "site" enregistré dans la colonne de gauche passé au point 4 sinon sélectionnez le "site" désiré dans la colonne de gauche et cliquez sur le bouton "Connect" (passez au point 5) [<a href="github/screenshots/ftp_01.png" target="_blank">screenshot</a>]
4. Si vous n'avez pas de "site" enregistré, cliqué sur le bouton "New site" et insérez les informations FTP communiquées précédemment (FTP host, FTP user, FTP password). [<a href="github/screenshots/ftp_02.png" target="_blank">screenshot</a>] Puis cliquez sur le bouton "Connect".
5. Une fois connecté, on va devoir transféré TOUS les fichiers de notre site Wordpress local (colonne de gauche) vers le serveur (colonne de droite) via un Glisser/Déposer chaloupé [<a href="github/screenshots/ftp_03.png" target="_blank">screenshot</a>]. Le transfert des fichiers peut prendre un temps certain, l'opération est terminée quand il n'y a plus de fichiers dans la fenêtre "Queued files" (en bas).

### 2. Migrer votre base de donnée (DB)

La première partie de cette étape (l'export de la DB locale) se fait différement si vous êtes sur Windows ou Mac.

#### Windows

1. Vérifiez que Laragon a bien ses services démarré et cliquez sur le bouton "Base de données" (Database) pour lancer HeidiSQL [<a href="github/screenshots/win_01.png" target="_blank">screenshot</a>]

2. Sur HeidiSQL, ouvrez la session Laragon [<a href="github/screenshots/win_02.png" target="_blank">screenshot</a>]

3. Dans la colonne de gauche, faites un clic droit sur le nom de la DB que vous voulez exporter et séléctionnez "Exportez la base de donnée en SQL..." [<a href="github/screenshots/win_03.png" target="_blank">screenshot</a>]

4. Dans la fenêtre d'Export SQL, configurez celle-ci de la même manière que le <a href="github/screenshots/win_04.png" target="_blank">screenshot</a>, soit : 
    - Base(s) de donn. : décocher les deux cases
    - Table(s) : cocher les deux cases
    - Données : sélectionner "DELETE + INSERT"
    - Nom de fichier : sélectionner un dossier et un nom de fichier

5. Cliquez sur "Exporter"

#### Mac

1. Lancez "Sequel Pro"

2. Connectez-vous à votre "connection" favorie [<a href="github/screenshots/mac_01.png" target="_blank">screenshot</a>]

3. En haut à gauche, séléctionnez la DB que vous voulez exporter. [<a href="github/screenshots/mac_02.png" target="_blank">screenshot</a>]

3. Une fois séléctionné, allez dans le menu "File" (Fichier) et cliquez sur "Export..." [<a href="github/screenshots/mac_03.png" target="_blank">screenshot</a>]

5. Dans la fenêtre d'Export, vous pouvez laisser la config par défaut ou si vous n'êtes pas sûr, vous pouvez configurer celle-ci de la même manière que le <a href="github/screenshots/mac_04.png" target="_blank">screenshot</a>. Faîtes juste bien attention à où vous enregistrez le fichier .sql (Path)

6. Cliquez sur "Export"

#### La suite est commune pour les Win/Mac

1. Accédez ensuite à PHPMyAdmin via https://phpmyadmin.ovh.net/ et connectez-vous avec les accès que l'on vous a communiqués.

2. Une fois connecté, cliquez sur le nom de votre db dans la colonne de gauche (c'est le même nom que votre DB USER).

3. Si vous voyez déjà des tables dans votre DB (ce qui devrait être le cas, vu que l'on a fait l'exercice en classe), on va commencer par supprimer celles-ci : 
    - Cochez la case checkbox "Tout cochez" en dessous du tableau 
    - Dans le select box à coté de cette case, sélectionnez l'option "Supprimer".  [<a href="github/screenshots/pma_01.png" target="_blank">screenshot</a>]
    - Cliquez sur le bouton "Exécuter"
    - Confirmez la suppression en cliquant sur "Oui" à la demande "Faut-il vraiment exécuter la requête suivante ?" [<a href="github/screenshots/pma_02.png" target="_blank">screenshot</a>]
    - Si tous c'est bien passé vous devriez avoir maintenant une notification du genre : "Aucune table n'a été trouvée dans cette base de données."
    
4. Cliquez sur l'onglet "IMPORTER" puis choisissez, via le champ d'upload, le ficher .sql que vous avez exporté en local et cliquez sur le bouton "Exécuter". 

Si tout c'est bien déroulé, vous devriez voir vos nouvelles tables dans la colonne de gauche ainsi qu'une jolie notification verte du genre : "L'importation a réussi,..."

### 3. Modifier la configuration de l'installation Wordpress hébergée

Maintenant que nos fichiers ont été uploadés sur le serveur et que notre DB a été migrée, il nous reste à indiquer à notre installation Wordpress, les nouvelles informations pour accéder à cette DB (càd DB_NAME, DB_USER, DB_PASSWORD et DB_HOST)

Nous allons retourner sur FileZilla et nous connecter à nouveau au serveur. A la racine de celui-ci vous allez trouver le fichier wp-config.php (pas wp-config-sample.php). Nous allons devoir modifier celui-ci. Le plus simple est de faire un clic droit sur le fichier du coté serveur (coté droit) et de choisir "View/Edit" [<a href="github/screenshots/ftp_04.png" target="_blank">screenshot</a>] pour l'ouvrir dans votre éditeur de texte favoris. 

Ce qui nous interesse ce situe entre la ligne 22 et la ligne 32 du fichier, vous devriez avoir qqch comme ceci :


      /** The name of the database for WordPress */
      define('DB_NAME', 'wordprexmas');
      
      /** MySQL database username */
      define('DB_USER', 'root');
      
      /** MySQL database password */
      define('DB_PASSWORD', '');
      
      /** MySQL hostname */
      define('DB_HOST', 'localhost');

Le but va être de remplacer les valeurs "locales" par les informations d'accès à la DB que vous avez reçu précédement, comme ceci :

      /** The name of the database for WordPress */
      define('DB_NAME', '[DB user]');
      
      /** MySQL database username */
      define('DB_USER', '[DB user]');
      
      /** MySQL database password */
      define('DB_PASSWORD', '[DB password]');
      
      /** MySQL hostname */
      define('DB_HOST', '[DB host]:[DB port]');
      
Faites bien attention à la dernière ligne, elle contient le "host" ET le "port" séparés par le caractère ":". 

Une fois l'opération réalisée, sauvegardez le fichier et retournez sur FileZilla où vous allez devoir confirmer l'upload de ce fichier modifié (cliquez sur "Yes") [<a href="github/screenshots/ftp_05.png" target="_blank">screenshot</a>]

### Félicitation !

Si tout c'est bien déroulé, votre site Wordpress est à présent accessible sur le Web. GG. N'hésitez pas si questions...