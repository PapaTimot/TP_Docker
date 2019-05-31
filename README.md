# TP Docker

## Description
 * Ce projet contient les différentes expérimentations liés au TP sur docker 
effectué dans le cadre de notre formation en quatrième année à l'ESIR.
Le sujet est disponible [ici](https://github.com/barais/ESIRTPDockerSampleApp).
L'objectif du TP était de construire entièrement une première image docker à partir d'un Dockerfile pour 
  * mettre en place l'environnement nécessaire à l'exécution du programme java
  * cloner à partir de git et build les bibliothèques d'open CV 
  * cloner et build le projet java lui même
Il en résultait une image forcément un peu lourde (presque 7GB sur mon disque), 
le second objectif était donc d'arriver à construire l'image docker (pouvant exécuter le programme correctement)
la plus petite possible, par tous les moyens.

## Mise en oeuvre
* Tout d'abord nous avons commencé spontannément par essayer de build opencv et de faire fontionner le projet 
sans passer par docker (cf les dossiers opencv et ESIRTPDockerSampleApp dans le répertoire Other_docker_tests). 
C'était déjà relativement long du fait de la taille des bibliothèques utilisées et du temps de build, 
mais celà nous a permis de bien comprendre toutes les étapes de build. 

* Ensuite nous avons réalisé quelques exercices avec docker afin de commencer à apréhender les concepts et 
prendre en mains son utilition (cf les dossiers Tests et TP_Part2_Nginx dans Other_docker_tests).

* A partir de là nous avons pu attaquer la construction de l'image docker à partir du Dockerfile.
  * Comme nous avions fait nos tests de build en local sur nos machines, 
  nous sommes spontannément partie sur la construction d'images se basant sur Ubuntu 
  (voir FinalWebAppBuild_Ubuntu et TP_Part3_WebApp_Ubuntu dans Other_docker_tests).
  * Mais nous nous sommes rendus compte, en lisant de la documentation et en cherchant des informations 
  sur l'optimisation de la taille des images Docker, que Ubuntu n'était pas la meilleure solution pour ce problème.
  Avant même de finir les Dockerfile commencé nous avons donc décidé de reprendre le build en utilisant cette fois Alpine :
  un OS ultra-léger ne totalisant que 5Mo d'espace disque !
    * Nous avons alors build toutes l'image avec le Dockerfile (commenté) dans le dossier Build_Alpine.
    * Ensuite pour construire la seconde image la plus légère possible nous sommes passé par trois étapes :
    1. Partir à nouveau d'une image parent contenant Alpine et un JRE, 
    puis copier uniquement les fichiers indispensables à l'exécution depuis la grosse image de build. 
    (voir dans le dossier Light_Imgs_Alpine puis Light_Img).
    2. Ensuite nous avons eut l'idée de partir d'une image `from scratch` pour éliminer le poids des fichiers de Alpine
    non nécessaires à l'exécution du programme java.
    3. Finalement nous avons même supprimé à la main, à l'intérieur d'un conteneur docker, certaines librairies inutiles de opencv 
    ainsi que certaines librairies présentes dans les dossiers /usr/* et /lib/. 
    Ensuite nous avons créé une image à partir de ce conteneur (un snapshot), et nous avons finalement construit notre dernière
    image (la plus légère) en copiant les fichiers à partir de ce snapshot.
    
## Résultats
Grâce à ces multiples expérimentations, nous avons réussi à obtenir 
une image docker particulièrment légère mais restant fonctionnelle pour exécuter le programme de serveur web en java utilisant opencv.
Les images sont disponibles sur le docker hub, dans le répertoire timmesir/mditpdocker.
La plus petite image est celle ayant le tag suivant : timmesir/mditpdocker:lightestimg.

## Auteurs
 * Timothée Schneider-Maunoury
 * Yoann Breton


