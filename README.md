•	La classe Site : Gestion d'un objet Java avec MySQL
La classe Site représente un objet simple avec un id et un nom. Elle sert à interagir avec une table site dans la base de données :
•	La classe Test
La classe Test est conçu pour interagir avec une base de données MySQL en utilisant JDBC. Elle offre deux fonctionnalités principales : l'insertion de données dans une table site via la méthode save, et la récupération des données via la méthode load. 
Cette classe utilise la bibliothèque JDBC pour gérer la connexion à la base de données, l'exécution de requêtes SQL et la manipulation des résultats.
La  méthode save(Site s) :
Cette méthode permet d'insérer un nouvel enregistrement dans la table site. 




Fonctionnalités :
Paramètre d'entrée :
Un objet de type Site qui contient le nom du site à insérer dans la base de données.
Processus :
Chargement du driver JDBC : La méthode commence par charger le driver JDBC MySQL en appelant Class.forName("com.mysql.jdbc.Driver");.
          C'est une étape nécessaire pour permettre la connexion à MySQL via JDBC.
Connexion à la base de données : La méthode établit une connexion à la base de données en utilisant DriverManager.getConnection(url, user, password);. Les informations de connexion (URL, utilisateur, mot de passe) sont définies en ligne dans la méthode.
Création d'un Statement : Après avoir établi la connexion, la méthode crée un Statement pour exécuter des requêtes SQL.
Exécution de la requête INSERT : La requête SQL pour insérer un nouveau site est construite dynamiquement à partir de l'objet Site. 
Gestion des exceptions : La méthode est entourée d'un bloc try-catch pour capturer et gérer les erreurs SQL et les erreurs liées au chargement du driver.
Libération des ressources : Enfin, la méthode ferme les ressources (connexion et statement) dans un bloc finally pour éviter les fuites de mémoire

