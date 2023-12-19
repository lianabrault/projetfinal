**Date** : Le 24 novembre 2023<br>
**Cote et nom du cours** : CMN 4500<br>
**Prénom et nom de l'étudiant(e)** : Liana Brault<br>
**Présenté à Jean-Sébastien Marier**<br>

# Projet 2 : Analyse et de visualisation de données simple

# Demandes mensuelles de service en 2023

## 1. Introduction

Ce travail a été effectué dans le cadre du cours CMN 4500, le deuxième projet de la session. Je vais analyser un jeu de données de la Ville d'Ottawa portant sur l'enjeu des demandes de service pour le mois de janvier 2023. Les données ont été amassées par la ville d'Ottawa, plus précismément Brody Johnston de la Direction générale des services novateurs pour la clientèle, ServiceOttawa. Voici deux liens pertinents; 

[Jeu de données original sur Ottawa ouverte.](https://ouverte.ottawa.ca/datasets/demandes-mensuelles-de-service-en-2023/explore)
[Version CSV sur mon portail GitHub.](https://raw.githubusercontent.com/jsmarier/course-datasets/main/ottawa-311-service-requests-january-2023.csv)

Le travail sera divisé dans les sections suivantes; Obtention des données, Compréhension des données, Publication des données ainsi qu'une conclusion.

## 2. Obtenir les données

### 2.1. Comment obtenir les données sur Google Sheets

J'ai sauvegardé le fichier CSV à mon Google Drive. Ensuite, en utilisant la fonction "Fichier" et "Importer", j'ai sélectionner le fichier CSV que j'avais téléchargé à mon Drive. Les données ont ensuite apparû sur l'onglet de Google Sheet sur lequel j'étais.

![feuillegooglesheets.png](<Feuille Google Sheets.png>)<br>
Figure 1 : <em>Capture d'écran du jeu de données de la ville d'Ottawa portant sur les demandes de service.</em>

[Lien public vers le document Google Sheets.](https://docs.google.com/spreadsheets/d/1mqTH-qpFlpiasbBbU5FuCegAJ8jed3jC_aN8HqFxLO0/edit?usp=sharing)


### 2.2. Observations

#### 2.2.1. Observations générales
Il y a 7 colonnes dans le jeu de données et 26 846 rangées. La première rangée comprends les titres (Subject, Reason, Type, Date, Channel, Ward et Objet), ce qui fait en sorte qu'il y a réellement 26 845 rangées comprenant des données. En effet, la 7e colonne (Objet) comprends le vrai numéro de chaque jeu de donnée, afin de ne pas induire en erreur le lecteur. C'est-à-dire, il y a eu 26 845 demandes de service en janvier 2023. De plus, les colonnes A et B sont bilingues, tandis que les colonnes C, E et F (les seules autres colonnes avec des mots et non des chiffres) sont uniquement en anglais.

Les données semblent généralement propres, mais il y a place à amélioration. On sait que les données dans ce jeu sont toutes pertinentes au mois de janvier 2023, alors à moins qu'on cherche à savoir exactement combien de demandes ont été placées hebdomadairement on aurait pas besoin de la date. De plus, dans cette même colonne on retrouve les chiffres "00:00:00+00", qui pourraient être effacées (ils se retrouvent dans chaque jeu de données). On pourrait également enlever la colonne 7 si le numéro de demande n'est pas pertinent à notre analyse, et qu'on se souvient de soustraire 1 au numéro de rangée si le cherche. Certains des jeux de données sont bilingues et d'autres ne le sont pas- on pourrait donc choisir la langue qu'on veut et changer les données en conséquent.

#### 2.2.2. Observations spécifiques:

<ul>
<li>La colonne C comporte des variables nominales avec les types de demandes de service à l’étude. 
<li>La colonne D inclut les dates des demandes en tant que variables discrètes.
<li>La colonne F inclut les quartiers de la ville où il y a eu des demandes en tant que variables nominales.
</ul>

Quelque chose que j'ai remarqué, tel que discuté dans la section 2.2.1., est le fait que la série "00:00:00+00" apparaît dans chaque case de la colonne D, suite à la date de la demande. Également, une autre observation que j'ai faite est que dans les rangées 9, 13, 19, 25, 32, 38 et 85 on voit que les donns dans les colonnes A et B sont les mêmes, soit "Bylaw Services | Service des Règlements Municipaux". Dans la colonne C, il n'y a pas plus de spécifications, car on voit "Other | Autre".

Cette dernière observation est particulièrement intéressante, car pour les 7 rangées où on voit ce doublement d'information suivi par la notion 'Autre', le lecteur ne peut pas savoir quel était le type de demande en tant que tel. Toutefois, on sait quel jour la demande a été placée, dans quel quartier ainsi que le canal. Cet enjeu a seulement lieu dans les jeux concernant les Services de Règlement Municipaux. On se demande donc où est allée cette information, et pourquoi elle ne se retrouve pas dans le jeu de donnée.

## 3. Comprendre les données

### 3.1. Nettoyage des données

#### 3.1.1. Analyse VIMA

L'analyse suivante a été effectuée basée sur l'information communiqué la vidéo de Statistique Canada A; Exactitude et validation des données : méthodes pour assurer la qualité des données (2021).
 
<u>Colonne B</u>

Dans cette colonne, on pourrait dire que les rangées où on retrouve la valeur "Bylaw Services | Service des Règlements Municipaux" c'est une valeur invalide, car elle ne réponds pas à la question de la colonne qui est la raison de la demande. Cette valeur pourrait être remplacée par "Unknown" afin de la rendre valide.

<u>Colonne C</u>

Similairement, dans cette colonne on voit que la valeur "Other | Autre" est également invalide. Elle ne spécifie pas quel était le type de demande, en plus d'avoir des informations invalides dans la colonne B. Ces valeurs devraient aussi être remplacées par la valeur "Unknown". 

<u>Colonne F</u>

Nous retrouvons dans cette colonne plusieurs valeurs manquantes. On ne sait donc pas dan quel quartier les demandes ont eu lieu. Elles sont faciles à repérer avant même de passer à l'étape du nettoyage, car les cases vides sautent aux yeux. Les données de cette colonne ne peuvent pas nécessairement être qualifiées en tant que valides, car il y a des valeurs manquantes.


#### 3.1.2. Méthodes de nettoyage de données

<u>Nettoyage de base (sans fonctions avancées)</u>

J'ai décider de faire deux petits changements à mon jeu de données avant de procéder aux fonctions de nettoyage plus avancées. 
<ul>
<li>J'ai supprimé la colonne G, car je ne croit pas qu'elle sera pertinente à mon analyse. 
<li> J'ai également renommé la colonne D à "Date" à la place de "Date_Raised", car c'est plus court, direct et moins encombrant.
</ul>

<u>Rechercher et remplacer</u>

Avec cette fonction, j'ai décidé de supprimer la donnée "00:00:00+00" qui se retrouvait dans les cellules D2 à D26846, à côté de la date de la demande. Ces chiffres n'apportaient aucune information pertinente au jeu de donnée autre que de prendre de la place, alors j'ai opté de l'enlever.

Dans la fonction "Rechercher et remplacer", j'ai écris "00:00:00+00" dans la section "Rechercher", et j'ai ensuite mis un espace dans la case "Remplacer par". J'ai ensuite cliqué sur "Tout remplacer", et les chiffres sont disparus. Ceci a rendu ma colonne beaucoup moins encombrante.

![rechercheretremplacer.png](<Rechercher et remplacer.png>)<br>
Figure 2 : <em>Capture d'écran de la colonne D après avoir supprimé la donnée 0:00:00+00.</em>

J'ai utilisé la fonction Rechercher et remplacer une deuxième et une troisième fois, dans les colonnes B et C. Tel que discuté dans la section 3.1.1., quelques valeurs dans ces colonnes sont invalides. Les valeurs "Bylaw Services | Service des Règlements Municipaux" ainsi que "Other | Autre" devaient être corrigées à "Unknown" afin d'être valide. En suivant les mêmes étapes que dans la colonne D, c'est ce que j'ai fait grâce à la fonction de Rechercher et remplacer. 

![rechercheretremplacer2.png](<Rechercher et remplacer 2.png>)<br>
Figure 3 : <em>Capture d'écran de les colonnes B et C après avoir remplacé les valeurs invalids par "Unknown".</em>

<u>Figer une ligne</u>

J'ai figé la rangée 1 pour que les sous-titres soient visibles en déroulant le jeu de donnée. J'ai également mis les mots en gras.

En sélectionnant la rangée entière, j'ai été dans la section "Affichage", et j'ai ensuite cliqué sur "Figer", et ensuite sur "1 ligne". 

![figeruneligne.png](<Figer une ligne.png>)<br>
Figure 4 : <em>Capture d'écran du jeu de donnée après avoir figé la rangée 1 et l'avoir mis en gras.</em>

<u>`SPLIT`</u>

J'ai remarqué que la colonne A était complètement bilingue, avec le marqueur "|" séparant les deux langues. Afin de sélectionner uniquement la version anglophone de la valeur (la majorité des autres données dans le jeu sont en anglais, alors j'ai opté de garder la version anglophone du terme), j'ai utilisé la fonction `SPLIT`.

J'ai d'abord sélectionné la colonne A, et j'ai inséré deux colonnes de plus à sa droite. Ensuite, j'ai écris la fonction `SPLIT= (A2; "|")` dans la cellule B2, ce qui a fait en sorte que les versions anglaises et française de la donnée sont apparu dans les nouvelles colonnes B et C.

J'ai descendu la formule au reste de la colonne, et une fois remplie j'ai copié les colonnes B et C et collé uniquement les valeurs. Par la suite, j'ai pu supprimé mes colonnes A et C, pour garder uniquement la version anglaise. 

J'ai opté de faire cette fonction uniquement sur la colonne A, car les autres colonnes bilingues n'avaient pas tous le même démarquant entre les mots français et anglais, ce qui aurait été beaucoup plus compliqué.

![split.png](<SPLIT.png>)<br>
Figure 5 : <em>Capture d'écran du jeu de donnée après avoir effectué la fonction `SPLIT` sur la colonne A.</em>

![donnéesnettoyées.png](<Données nettoyées.png>)<br>
Figure 6 : <em>Capture d'écran final du jeu de donnée après l'avoir nettoyé.</em>

### 3.2. Analyse exploratoire des données

#### 3.2.1 Tableau croisé dynamique

En créant divers tableaux croisés dynamiques, j'ai remarqué deux autres problèmes, cette fois-ci avec la colonne F ainsi que D. 
<ul>
<li>Certains quartiers sont là deux fois, car ils ont parfois le numéro du quartier devant le nom en tant que tel, résultant dans un double des données. Puisque les données ne sont pas valides, je ne vais pas les utiliser pour l'instant.
<li>J'ai aussi remarqué en comptant le nombre de demandes de service par date, qu'il y a une valeur abérrante: une demande de service avec une date du 5 février 2023.
</ul>

Voici un tableau croisé dynamique que j'ai créé pour compter le nombre de demandes de service par jour du mois.

![tableaucroisédynamique2.png](<Tableaucroisédynamique2.png>)<br>
Figure 7 : <em>Capture d'écran du tableau croisé dynamique.</em>

#### 3.2.2. Graphique exploratoire

J'ai décider de créé un graphique en style de Nuage de points car comme on l'a vu dans la documentations de Statistique Canada sur la Visualisation des données C (2021), ce type de graphique est utile afin de montrer si des valeurs extrêmes sont présentes dans l’ensemble de données. Dans ce cas, on peut bien voir dans la figure 8 qu'il y a une valeur extrême, identifiée en rouge. C'est la valeur abérrante de la demande de service effectuée le 5 février 2023.

![nuagedepoints.png](<Nuage de points.png>)<br>
Figure 8 : <em>Capture d'écran du nuage de points.</em>

#### 3.2.3. Processus de réflexion

Pour la prochaine section, je vais créer un graphique Datawrapper qui visualise les données dans le tableau croisé dynamique de la Figure 9. On y verra combien de demandes de service de chaque département (SUBJECT) il y a eu à tous les jours du mois.

![tableaucroisédynamique4.png](<TableauCroiséDynamique4.png>)<br>
Figure 9 : <em>Capture d'écran du tableau croisé dynamique avec le nombre de demandes il y a eu dans chaque département.</em>

## 4. Publier les données

### 4.1 Graphique Datawrapper

Tel que discuté dans la section 3.2.3, j'ai d'abord préparé les données dans Google Sheets grâce à un tableau croisé dynamique (capturé dans la Figure 9). 

J'ai copié ces données et je les ai collé directement dans Datawrapper. J'ai choisi le type de graphique pour pouvoir bien voir les grandes différences entre les données, ce qu'on voit dans la Figure 10. Comme on l'a vu dans nos lectures obligatoires de Statistique Canada B (2021), le graphique circulaire devrait être créé avec des pourcentages et non les données brutes afin de mieux représenter le tout. C'est également un graphique utilisé pour résumer un ensemble de données nominales, ou de présenter les différentes valeurs d'une variable donnée. Les différents canaux (CHANNEL dans le jeu de donnée initial) sont en effet des données nominales, ce qui justifie l'utilisation d'un graphique circulaire.

J'ai choisi ces couleurs afin de bien démontrer le contrastes entre les différentes données, sans avoir des couleurs qui se différencient trop.

![](Graphique_circulaire.png)<br>
*Figure 10 : La carte créée avec Datawrapper.*
[Version interactive ici](https://www.datawrapper.de/_/imD20/)

## 5. Conclusion

En somme, dans la Figure 10 on peut particulièrement bien voir la grande différence entre l'utilisation des canaux de communication disponibles pour placer des demandes de service. Il est très évident que le Voice In est la méthode la plus populaire de placer une demande, suivie par WEB et WAP avec 11% et 12% respectivement. Les deux autres données, soit en personne ainsi qu'une valeur inconnues, sont tellement petites comparées aux autres moyens qu'ils ne paraissent même pas sur le graphique. 

Le Voice In semble être le plus populaire, mais est-il le plus efficace? Pourquoi le WEB et WAP sont tellement moins utilisés comparés au Voice In? Lesquels sont les plus "user-friendly"? Ceci sont tous des questions dont on pourrait se demander suite à l'analyse de ces données. Une prochaine étape pour l'analyse pourrait être de comparer quels moyens sont plus populaires par quartier, mais les données devront être nettoyées davantage avant de passer à cette étape.

Quelque chose d'autre à noter suite à cette analyse est le fait que le plus que les données étaient observées et importées dans divers graphiques et tableaux, le plus d'erreurs j'ai pu identifier. Le jeu de données, même s'il semblait propre à première vue, ne l'était réellement pas le plus que l'analyse a continué. Cela soulève une autre question, celle de la légitimité des données dans d'autres jeux de données publiés ouvertement par la ville. Évidemment, presqu'aucun jeu ne pourra être parfait. Toutefois, dans des situations comme celle-ci où certaines valeurs sont doublées peuvent porter confusion et mener à des erreurs.

## 6. Références

Statistique Canada A (15 novembre 2021). Exactitude et validation des données : méthodes pour assurer la qualité des données. Statistique Canada. [https://www.statcan.gc.ca/fr/afc/litteratie-donnees/catalogue/892000062020008](https://www.statcan.gc.ca/fr/afc/litteratie-donnees/catalogue/892000062020008) 

Statistique Canada B (9 février 2021). Visualisation des données: Graphique circulaire. Statistique Canada. [https://www150.statcan.gc.ca/n1/edu/power-pouvoir/ch9/pie-secteurs/5214826-fra.html] (https://www150.statcan.gc.ca/n1/edu/power-pouvoir/ch9/pie-secteurs/5214826-fra.htm) 

Statistique Canada C (9 février 2021). Visualisation des données: Nuage de points. Statistique Canada. [https://www150.statcan.gc.ca/n1/edu/power-pouvoir/ch9/scatter-nuages/5214827-fra.html] (https://www150.statcan.gc.ca/n1/edu/power-pouvoir/ch9/scatter-nuages/5214827-fra.html)