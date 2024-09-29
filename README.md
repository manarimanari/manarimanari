//travail a rendre 
package developpement;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;

public class Scripts {
static final String user ="root";
static final String password ="";; 
static final String url = "jdbc:mysql://localhost/DataBase";
  
public static void main (String []args){
Statement st = null;
Connection cn = null;

try {
//Etape 1 : Chargement du driver
Class.forName("com.mysql.jdbc.Driver");
//Etape 2 : Récupération de la connexion
cn = DriverManager.getConnection(url, user, password);
//Etape 3 : Création d'un statement
st = cn.createStatement();
String req1 = "CREATE TABLE DevData (\n" +
"Developpeurs VARCHAR (32),\n" +
"Jour CHAR (11),\n" +
"NbScripts INTEGER) ";
st.executeUpdate(req1);
System .out.println(" la table est cree ");
// Insertion des données avec les corrections
String req2 = "INSERT INTO `DevData` (`Developpeurs`, `Jour`, `NbScripts`) VALUES ('ALAMI', 'Lundi', 1)";
String req3 = "INSERT INTO `DevData` (`Developpeurs`, `Jour`, `NbScripts`) VALUES ('WAFI', 'Lundi', 2)";
String req4 = "INSERT INTO `DevData` (`Developpeurs`, `Jour`, `NbScripts`) VALUES ('SLAMI', 'Mardi', 9)";

st.executeUpdate(req2);
st.executeUpdate(req3);
st.executeUpdate(req4);

System .out.println(" les valeurs sont inseres ");
} catch (SQLException e) {
System.out.println("SQL erreur" + e.getMessage());
} catch (ClassNotFoundException ex) {
System.out.println("Impossible de charger le driver");
}finally{
   try {
//Etape 4 : Libérer les ressources de la mémoire
    if(st!=null)
st.close();
    if(cn!=null)   
cn.close();
} catch (SQLException ex) {
System.out.println("Impossible de libérer les ressources");
}
} 
}
    
}
package developpement;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;
import java.sql.ResultSet;
import java.util.Scanner;
import java.sql.ResultSetMetaData;

public class ExioJDBC {
   static final String user ="root";
static final String password ="";; 
static final String url = "jdbc:mysql://localhost/DataBase";    
public static void main (String []args){
Statement st = null;
Connection cn = null;
ResultSet rs = null ;
ResultSetMetaData metaData=null;

try {
//Etape 1 : Chargement du driver
Class.forName("com.mysql.jdbc.Driver");
//Etape 2 : Récupération de la connexion
cn = DriverManager.getConnection(url, user, password);
//Etape 3 : Création d'un statement
st = cn.createStatement();
 // Exécution de la première requête
           String req1 = "SELECT Developpeurs, Jour, max(NBScripts) as MaxScripts FROM devdata GROUP BY Jour";
            rs = st.executeQuery(req1);

            System.out.println("Résultats de la première requête :");
            while (rs.next()) {
                System.out.println("Développeur: " + rs.getString("Developpeurs") + ", Jour: " + rs.getString("Jour") + ", MaxScripts: " + rs.getInt("MaxScripts"));
            }

            // Exécution de la deuxième requête
            String req2 = "SELECT Developpeurs, SUM(NBScripts) as TotalScripts FROM devdata GROUP BY Developpeurs ORDER BY TotalScripts DESC";
            rs = st.executeQuery(req2);

            System.out.println("\nRésultats de la deuxième requête :");
            while (rs.next()) {
                System.out.println("Développeur: " + rs.getString("Developpeurs") + ", TotalScripts: " + rs.getInt("TotalScripts"));
            }

// Calcul du nombre total de scripts réalisés en une semaine
            String queryTotalScripts = "SELECT SUM(NbScripts) AS totalScripts FROM devdata";
            rs = st.executeQuery(queryTotalScripts);
            if (rs.next()) {
                int totalScripts = rs.getInt("totalScripts");
                System.out.println("Nombre total de scripts réalisés en une semaine : " + totalScripts);
            }

            // Saisie du programmeur donné
            Scanner scanner = new Scanner(System.in);
            System.out.print("Entrez le nom du programmeur : ");
            String nomProgrammeur = scanner.nextLine();

            // Calcul du nombre total de scripts réalisés par un programmeur donné
            String queryScriptsByDev = "SELECT SUM(NbScripts) AS totalScripts FROM devdata WHERE Developpeurs = '" + nomProgrammeur + "'";
            rs = st.executeQuery(queryScriptsByDev);
            if (rs.next()) {
                int totalScriptsByDev = rs.getInt("totalScripts");
                System.out.println("Nombre total de scripts réalisés par " + nomProgrammeur + " : " + totalScriptsByDev);
            }
// Saisie de la requête libre par l'utilisateur
            
            System.out.print(" larequête SQL : ");
            String reqlibre = scanner.nextLine();

            // Exécution de la requête
            boolean hasResultSet = st.execute(reqlibre);

            if (hasResultSet) {
                // La requête a produit un ResultSet
                rs = st.getResultSet();

                // Récupérer les métadonnées
                metaData = rs.getMetaData();
                int columnCount = metaData.getColumnCount();

                // Afficher le nombre de colonnes
                System.out.println("Nombre de colonnes : " + columnCount);

                // Afficher les noms et types des colonnes
                for (int i = 1; i <= columnCount; i++) {
                    String columnName = metaData.getColumnName(i);
                    String columnType = metaData.getColumnTypeName(i);
                    System.out.println("Colonne " + i + " : " + columnName + " (" + columnType + ")");
                }

                // Afficher le contenu de chaque ligne
                System.out.println("\nContenu des résultats :");
                while (rs.next()) {
                    for (int i = 1; i <= columnCount; i++) {
                        System.out.print(rs.getString(i) + "\t");
                    }
                    System.out.println();
                }
            } else {
                // La requête ne retourne pas de ResultSet (par exemple une requête INSERT, UPDATE ou DELETE)
                int rowsAffected = st.getUpdateCount();
                System.out.println("Nombre de lignes affectées : " + rowsAffected);
            }
} catch (SQLException e) {
System.out.println("SQL erreur" + e.getMessage());
} catch (ClassNotFoundException ex) {
System.out.println("Impossible de charger le driver");
}finally{
   try {
//Etape 4 : Libérer les ressources de la mémoire
    if(st!=null)
st.close();
    if(cn!=null)   
cn.close();
    if(rs!=null)   
rs.close();
    
} catch (SQLException ex) {
System.out.println("Impossible de libérer les ressources");
}
} 
}
    
} 
//TP1
package beans;

/**
 *
 * @author s
 */
public class Site {
private int id;
private String nom;
public Site(){
    
}
public Site (String nom) {
this.nom = nom;}
public int getId () {
return id;}
public void setId (int id) {
this. id = id;}
public String getNom () {
return nom;}
public void setNom (String nom) {
this. nom = nom;}    
}
package beans;


import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;
import java.sql. ResultSet;



public class Test {
    public static void save (Site s) {
// Information d'accès å la base de donnies
String user ="root";
String password =""; 
String url = "jdbc:mysql://localhost/db";
Connection cn = null;
Statement st = null;
try {
//Etape 1: Chargement du driver
Class.forName ("com.mysql.jdbc.Driver");
//Etape 2: Récupération de la connexion
cn = DriverManager.getConnection (url, user, password);
//Etape 3: Création d' un statenent
st = cn. createStatement ();
String req = "insert into site values (null , '" + s.getNom() + "' )";
//Etape 4: Exécution de la requête
st.executeUpdate(req);
} catch (SQLException e) {
System.out.println ("Erreur 3QL"+ e.getMessage());
}catch(ClassNotFoundException ex){ 
System. out .println (" Impossible de charger le driver " + ex.getMessage() ) ;
} finally {
try {
//Etape 5 : Libérer les ressources de la mênoire
  
st. close();
cn. close();
} catch (SQLException ex) {
System.out .println (" Inpossible de libérer les ressources");
    
}
}
}
  public static void load() {
//Information d'accès à la base de données
String user = "root";
String password = "";
String url = "jdbc:mysql://localhost/db";
Connection cn = null;
Statement st = null;
ResultSet rs = null;
try {
//Etape 1 : Chargement du driver
Class.forName("com.mysql.jdbc.Driver");
//Etape 2 : Récupération de la connexion
cn = DriverManager.getConnection(url, user, password);
//Etape 3 : Création d'un statement
st = cn.createStatement();
String req = "select * from site";
//Etape 4 : Exécution de la requête
rs = st.executeQuery(req);
//Etape 5 : Parcours de ResultSet
while (rs.next()) {
System.out.println(rs.getInt(1) + " " + rs.getString(2));
}
} catch (SQLException e) {
System.out.println(e.getMessage());
} catch (ClassNotFoundException ex) {
System.out.println("Impossible de charger le driver");
} finally {
try {
//Etape 6 : Libérer les ressources de la mémoire
st.close();
cn.close();
} catch (SQLException ex) {
System.out.println("Impossible de libérer les ressources");
}
}
}
 //Dans la méthode main tester la fonction save(site).
public static void main(String[] args) {
//insertion des données
save(new Site("SAFI"));
save(new Site("MARRAKECH"));
save(new Site ("EL JADIDA"));
load();}
       }
