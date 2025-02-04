# projetPythonGroupe3

1. Liste des membres du groupe
   a. ATAKOUN Fulgence
   b. ATCHAMOU Isnelle
   c. GOZO Jaurès
   d. NOUDAYI Joël

2. Rapport

INTRODUCTION
L’analyse des données météorologiques est une étape essentielle dans la compréhension et la prédiction des phénomènes climatiques. Avec la quantité croissante de données disponibles, il est désormais possible d’exploiter ces informations pour modéliser les conditions météorologiques futures avec une précision qui n’était pas envisageable auparavant. Ainsi, les prévisions climatiques fiables peuvent anticiper des événements comme des vagues de chaleur, des tempêtes ou des périodes de sécheresse, et par conséquent permettre de prendre des décisions éclairées.
Ce rapport relate l'analyse de données météorologiques tout en examinant les différentes étapes du processus, depuis la collecte des données jusqu'aux prédictions. 
Dans la suite du document, nous présenterons les méthodes utilisées pour transformer ces données brutes en informations exploitables, permettant ainsi de réaliser des prévisions météorologiques fiables et d’explorer des tendances climatiques intéressantes.





1.	COLLECTE DE DONNÉES
Pour cette analyse, nous avons utilisé le jeu de données issu de cette source (https://donneespubliques.meteofrance.fr/ ).
•	Brève description du jeu de données 
Ce jeu de données comprend plusieurs variables clé telles que : la température, l’humidité relative (indicateur de la quantité de vapeur d'eau dans l'air qui influence directement la sensation de chaleur ou de froid), la pression atmosphérique (pression exercée par l’atmosphère en un point), la vitesse du vent, les précipitations (indicateur de la quantité d’eau tombée sous forme de pluie, neige ou grêle), etc.
Ces paramètres sont essentiels pour établir une compréhension complète des fluctuations climatiques et pour la réalisation de prévisions fiables sur le moyen et long terme. Ils fournissent des informations sur les phénomènes météorologiques récurrents, ainsi que sur les changements saisonniers ou inhabituels.
•	Importation des bibliothèques nécessaires 
Pour ce projet, nous avons importé et utilisé plusieurs bibliothèques à savoir :
-	pandas :  pour la manipulation des données ;
-	numpy : pour les calculs numériques ;
-	scikit-learn : pour les algorithmes de machine learning ;
-	matplotlib.pyplot : pour les visualisations ;
-	seaborn : pour les visualisations statistiques.

•	Lecture et affichage des premières et dernières lignes 
Nous avons eu recours à la commande df = pd.read_csv pour charger le fichier et aux commandes df.head() et df.tail() pour visualiser les premières et lignes du fichier.
Voici un aperçu de notre fichier 

![image](https://github.com/user-attachments/assets/d5e990cc-b650-494b-b85a-8fbfaaea6d22)

Aperçu des premières lignes 

![image](https://github.com/user-attachments/assets/44a559c1-ba81-4356-9793-5c0564105ff0)
 
Aperçu des dernières lignes

Le tableau ci-dessus présente une série de données météorologiques collectées sur plusieurs jours. Chaque ligne correspond à un enregistrement unique, tandis que les colonnes offrent des informations détaillées sur différents paramètres mesurés à des moments précis.

2.	PRÉTRAITEMENT DES DONNÉES
Avant de procéder à l’analyse proprement dite des données, il est essentiel de les nettoyer et de les préparer. Le but de cette phase est d'assurer que les données utilisées sont fiables, complètes et cohérentes. Le prétraitement des données permet de garantir que les analyses effectuées fourniront des résultats précis et exploitables. Cette étape de préparation se compose de plusieurs autres étapes qui se déclinent comme suit :
•	Suppression des données manquantes 
Les données météorologiques peuvent parfois comporter des valeurs manquantes pour diverses raisons. Afin de gérer ces vides et éviter que les données biaisent les résultats d’analyse, il existe plusieurs méthodes qui peuvent être appliquées comme :
-	La suppression des données incomplètes : elle consiste à supprimer totalement l’entrée lorsque la proportion de données manquante est trop élevée ;
-	Le remplacement des valeurs manquantes : (il s’agit de remplacer les valeurs manquantes lorsqu’elles sont peu nombreuses par des valeurs statistiques comme la moyenne ou la médiane de la colonne concernée) ;
-	 L’interpolation : elle consiste à substituer les valeurs manquantes par les valeurs interpolées en se basant sur les données avant et après l'intervalle concerné ;
-	 L’imputation par algorithmes : des algorithmes d’imputation basés sur des modèles statistiques, comme les k plus proches voisins (KNN) ou la régression linéaire, peuvent être employés pour prédire et remplir les valeurs manquantes en fonction des relations entre les variables.
Dans notre cas, nous avons utilisé l’imputation par algorithme avec l’algorithme de KNN pour estimer les valeurs basées sur des données similaires.
Nous l’avons appliqué sur la colonne de "Variation de pression en 3 heures".

•	Suppression des doublons
Il arrive que des entrées dupliquées apparaissent lors de la fusion de différentes sources de données ou en raison d’erreurs lors de la collecte des informations. Pour éviter cela, les étapes suivantes sont suivies :
-	Suppression automatique des doublons exacts : Les enregistrements complètement identiques sont supprimés directement.
-	Vérification des quasi-doublons : Une vérification approfondie est effectuée pour identifier des enregistrements quasi-identiques (par exemple, des données ayant des valeurs très proches mais pas exactement les mêmes) afin de ne pas fausser l’analyse. Ces quasi-doublons peuvent être corrigés par une agrégation ou un ajustement des valeurs.
En ce qui concerne les doublons de notre fichier, nous avons appliqué la fonction df.drop_duplicates(). 

•	Traitement des valeurs aberrantes 
Les valeurs aberrantes (ou outliers) sont des observations qui s'écartent de manière significative des autres données. Ces valeurs peuvent être dues à des erreurs de mesure ou à des événements climatiques exceptionnels. Voici les méthodes utilisées pour les gérer :
-	Utilisation de boîtes à moustaches (boxplots) : Les boîtes à moustaches sont une méthode graphique qui permet de détecter les valeurs qui se situent en dehors des intervalles interquartiles (au-delà de 1,5 fois l'écart interquartile). Ces valeurs sont souvent considérées comme des outliers potentiels.
-	Suppression ou remplacement : En fonction de l'impact des valeurs aberrantes sur la distribution des données, elles peuvent être supprimées ou remplacées. Si les valeurs extrêmes sont dûes à des erreurs de saisie, elles seront supprimées, tandis que si elles sont justifiées (par exemple, une tempête extrême), elles peuvent être remplacées par une estimation plus réaliste.
-	Analyse statistique : Une analyse statistique supplémentaire peut être réalisée pour déterminer si les valeurs aberrantes sont dues à des erreurs humaines ou à des événements climatiques extrêmes.
Dans notre cas nous avons utilisé les commandes df.info() pour lister les types de variables présentes et repérer les valeurs manquantes et df.isnull().sum() pour compter le nombre de valeurs manquantes par colonne.
En voici les aperçus

 ![image](https://github.com/user-attachments/assets/d9b436c0-4ec1-4f07-b0d7-2b4788f42117)

Aperçu des variables présentes dans chaque colonne

 ![image](https://github.com/user-attachments/assets/e7e11ac3-bf89-40d5-af9c-1e83a7575709)

Aperçu des valeurs manquantes par colonnes 

Dans le point suivant, nous aborderons plus en détail l’analyse des statistiques .

3.	ANALYSE EXPLORATOIRE  
A ce niveau, nous avons procédé à une visualisation des statistiques descriptives et des distributions.
Les statistiques descriptives (moyenne, médiane, écarts-types, etc) sont importantes car elles permettent de connaître la variabilité des mesures. Pour ce faire, nous avons utilisé les fonctions df.describe().T et df.describe(include='object').
En voici l’aperçu
 

 ![image](https://github.com/user-attachments/assets/8410020e-e19e-4afa-8655-b612a64eba57)

![image](https://github.com/user-attachments/assets/c1d105cc-1dc0-4569-a688-3d46658b7c27)

•	Explication du tableau 
La commande df.describe().T en Pandas calcule les statistiques descriptives de chaque colonne d'un DataFrame (df) et transpose les résultats. Cette transposition permet de présenter les statistiques sous forme de colonnes pour chaque variable, avec les différentes mesures statistiques affichées en lignes, offrant ainsi une meilleure lisibilité.
Par ailleurs, il faut rappeler que les statistiques descriptives résument rapidement les principales caractéristiques de chaque variable numérique de notre jeu de données. Voici les mesures qui sont calculées pour chaque colonnes :
-	count : Le nombre d'observations non-nulles (c’est-à-dire sans valeurs manquantes) dans chaque colonne ;
-	mean : La moyenne arithmétique des valeurs de la colonne ;
-	std : L'écart-type, qui mesure la dispersion des valeurs autour de la moyenne ;
-	min : La valeur minimale observée dans la colonne ;
-	25% : Le premier quartile, c’est-à-dire la valeur en dessous de laquelle se trouvent 25% des observations ;
-	50% : La médiane (deuxième quartile), qui représente la valeur en dessous de laquelle se trouvent 50% des observations. C’est également la moyenne si la distribution est symétrique ;
-	75% : Le troisième quartile, la valeur en dessous de laquelle se trouvent 75% des observations ;
-	max : La valeur maximale observée dans la colonne.

Par exemple, l’observation des statistiques de la colonne humidité nous permet de déduire que :
-	mean : 66.33 ; L’humidité relative moyenne est de 66,33%
-	std : 19.00 ; L’écart type de l’humidité est de 19,00% , indiquant une variabilité significative 
-	min : 11.00 ; L’humidité minimale enregistrée est de 11%
-	max : 100.00 ; L’humidité maximale enregistrée est de 100%

Les statistiques descriptives offrent plusieurs avantages pour l'analyse des données, notamment l’aperçu rapide des données, l’identification des valeurs aberrantes, la comparaison des variables et la prise de décision pour le nettoyage des données.

Nous avons aussi visualisé les distributions pour chaque colonne numérique à l’aide d’histogrammes montrant la répartition des valeurs et des boxplot permettant de repérer les valeurs aberrantes.
Voici une image montrant la distribution de la température :


![kelvin](https://github.com/user-attachments/assets/012ee51c-e016-43d2-b21e-a74a8cb42f03)

•	Analyse du graphique 

-	Température Moyenne
La température moyenne semble se situer autour du pic de la distribution, c’est-à-dire entre 287 K et 288 K. Cette estimation visuelle est en accord avec les statistiques descriptives de la température, qui indiquaient une moyenne proche de 288.95 K.
-	Variabilité de la Température
L'écart-type, qui mesure la dispersion des données autour de la moyenne, peut être estimé visuellement grâce à la largeur de la courbe. Plus la courbe est étendue (large), plus l'écart-type est élevé, indiquant une plus grande variabilité des températures. Le graphique montre que les températures s’écartent assez largement de la moyenne, ce qui suggère une variabilité modérée.
-	Valeurs Aberrantes
Sur ce graphique, aucune valeur aberrante évidente (des points très éloignés du reste de la distribution) n’est visible. Cela indique que les données sont relativement homogènes et qu’il n’y a pas de températures extrêmes ou anormales qui pourraient perturber l’analyse.

•	Utilisations et intérêts du graphique 

-	Comprendre les Données
Ce graphique offre une manière intuitive de visualiser la répartition des températures et d'obtenir un aperçu immédiat des principales caractéristiques des données, comme la température moyenne et la variabilité. Cela permet de mieux comprendre les tendances globales des données.
-	Identifier les Tendances
Le graphique permet de détecter les tendances suivantes : la température moyenne qui se trouve autour de la valeur centrale, la plage de températures typiques (les valeurs autour du pic) qui est clairement visible et la variabilité des températures (écart-type) qui peut être estimée visuellement, ce qui aide à comprendre à quel point les températures peuvent varier au cours du temps.
-	Comparaison des Données
Si nous disposions de données de température pour différentes périodes ou différents lieux, ce graphique serait très utile pour comparer les distributions de températures. Par exemple, en comparant les courbes de densité de température de différentes années ou régions, on pourrait identifier des différences significatives ou des tendances climatiques spécifiques.

•	Analyse des corrélations
L’examen des relations entre les variables permet de comprendre les interdépendances pouvant influencer les prévisions météorologiques :

 ![image](https://github.com/user-attachments/assets/abd3c70a-bc12-400b-9426-ba8e6d8ab5ad)

Les nuages de points permettent d’explorer les tendances linéaires et non linéaires entre deux variables spécifiques. Prenons donc ce nuage de points et analysons-le.
•	Analyse du nuage de points

-	Corrélation entre la pression au niveau mer et la température 
Le nuage de points suggère une corrélation positive entre la pression atmosphérique au niveau de la mer et la température. En d'autres termes, plus la pression est élevée, plus la température tend à être élevée. Cette observation est logique et correspond à des principes météorologiques généraux :
Une pression élevée est souvent associée à un anticyclone, une zone de haute pression qui est généralement synonyme de temps stable et ensoleillé, favorisant une température plus chaude.
Cependant, bien que cette tendance générale soit évidente, la relation n'est pas parfaite. Il existe une certaine dispersion des points, indiquant qu'il y a des exceptions où une pression élevée ne mène pas nécessairement à une température élevée, ou inversement.

-	Forme de la relation
La forme de la relation entre la pression et la température n'est pas strictement linéaire. Bien qu'une tendance générale semble se dégager, il pourrait exister une légère courbure dans la relation, suggérant une forme curviligne.

-	Groupes ou Clusters
En observant le nuage de points, on remarque qu'il n'y a pas de groupes distincts ou de clusters visibles. Les points semblent distribués de manière relativement continue, sans formation évidente de groupes distincts de données. Cela pourrait indiquer qu'il n'y a pas de sous-populations distinctes dans l'ensemble des observations, du moins en ce qui concerne la relation entre la pression et la température.

-	Points Aberrants ou Outliers 
Il n'y a pas de points aberrants évidents qui se détachent clairement du reste du nuage de points. Les données semblent relativement homogènes, sans valeurs extrêmes ou des observations qui pourraient être considérées comme des erreurs de mesure ou des phénomènes météorologiques inhabituels.

-	Dispersion des Points 
La dispersion des points autour de la tendance générale suggère que, bien que la pression atmosphérique ait une certaine influence sur la température, d'autres facteurs jouent également un rôle important dans la détermination de la température. Parmi ces facteurs, on peut citer l’humidité, la vitesse et la direction du vent, la latitude, l’heure de la journée, la saison, etc.

•	Interprétations et Observations

-	Relation positive et Logique 
La relation positive entre pression et température est conforme aux principes météorologiques habituels. En général, une pression élevée est associée à des conditions météorologiques plus clémentes, favorisant des températures plus élevées.
-	Influence d’autres facteurs 
La dispersion des points montre qu’il existe de nombreux autres facteurs qui influencent la température. En effet, des éléments comme l'humidité, la vitesse du vent, et la latitude peuvent jouer un rôle tout aussi important que la pression atmosphérique. Cela rend la relation entre pression et température complexe, et suggère qu’une analyse plus approfondie est nécessaire pour isoler les effets de chaque facteur.

-	Nécessité d’analyses complémentaires 
Pour mieux comprendre la force de la relation entre pression et température, et pour déterminer si elle est statistiquement significative, il serait pertinent de réaliser des analyses statistiques supplémentaires. Par exemple, le calcul du coefficient de corrélation de Pearson permettrait de mesurer la force de la relation linéaire. Une régression linéaire ou non linéaire pourrait également aider à mieux modéliser cette relation et à évaluer son caractère précis.

•	Utilisations et Intérêts

-	Visualisation des Relations 
Le nuage de points permet de visualiser la relation entre deux variables de manière claire et intuitive. Cela aide à repérer les tendances générales et à formuler des hypothèses.
-	Identification de Tendances
En identifiant des tendances comme la relation positive entre pression et température, le graphique oriente l’analyse vers des hypothèses sur les phénomènes climatiques.
-	Exploration Initiale des Données
Ce type de graphique est un excellent outil d’exploration des données. Il permet de détecter des relations simples mais cruciales qui peuvent être explorées en profondeur à l’aide de techniques d’analyse plus avancées.


-	Affinement des Modèles Prédictifs 
Une fois que des hypothèses ont été formulées grâce à des observations comme celle-ci, elles peuvent être utilisées pour affiner des modèles d’apprentissage automatique. Les relations entre pression, température, et autres facteurs peuvent être intégrées dans des modèles pour prévoir des conditions climatiques futures avec plus de précision.
 
4.	PREDICTIONS
A ce niveau, nous avons séparé les données en entrainement (80 %) et test (20 %) ; la variable cible étant la Température (°C).
Pour prédire les futures données, nous avons utilisé deux modèles de traitements : la régression linéaire et la forêt aléatoire.

•	Régression Linéaire 

 ![image](https://github.com/user-attachments/assets/1736e0b3-8bb8-48e8-b7a2-0ee7229657cc)

La régression linéaire prévoit une relation linéaire simple entre la température et d'autres variables. Les évaluations mentionnent : l’Erreur Quadratique Moyenne (MSE) qui mesure la précision des prédictions et le score R² qui indique le pourcentage de variation expliqué par le modèle.
Voici les performances du modèle
 {'MSE' : 2.7477733982466577e-06, 'R^2 Score' : 0.9999999475566491}








•	Forêt aléatoire
![image](https://github.com/user-attachments/assets/8831404a-be42-4e32-9f2c-67a2a97e70f0)

 
La forêt aléatoire prédit mieux en capturant les relations complexes en combinant plusieurs arbres décisions.  Les mêmes métriques d’évaluation sont utilisées.
Voilà les performances du modèle
{'MSE' : 0.00010265904173205461, 'R^2 Score' : 0.999998040673897}

•	Comparaison des modèles 

 ![image](https://github.com/user-attachments/assets/d8ea85ec-3ded-4b54-8b16-0332b5995b96)

En comparant ces deux différents modèles, on peut conclure que la forêt aléatoire dépasse la régression linéaire et est donc plus précise.
 

5.	VISUALISATION
Après avoir traité les données, voici des graphes montrant les prédictions :
  ![image](https://github.com/user-attachments/assets/f809653c-f08a-480b-b39c-ea6d65206945)

•	Interprétation du graphique (image) :

-	Corrélation : Le graphique montre une forte corrélation positive entre les valeurs prédites et les valeurs réelles. Les points sont globalement alignés le long d'une droite diagonale, ce qui est un bon signe. Cela indique que le modèle de régression linéaire est capable de capturer la relation entre les variables de manière assez précise
-	Qualité du modèle : Plus les points sont proches de la droite diagonale (y = x), meilleure est la précision des prédictions. On peut constater que la plupart des points sont relativement proches de cette droite, ce qui suggère que le modèle a de bonnes performances.
-	Erreurs de prédiction : Les points qui s'écartent de la droite diagonale représentent les erreurs de prédiction du modèle. Plus un point est éloigné de la droite, plus l'erreur de prédiction est importante. Cependant, dans ce cas-ci, la majorité des erreurs semblent relativement faibles.	

 

![image](https://github.com/user-attachments/assets/f2821b59-6454-46ce-8125-a55757e9d8c5)

•	Interprétation et observations :

-	Précision des prédictions : Le modèle de Forêt Aléatoire semble être capable de prédire les valeurs avec une grande précision, ce qui est un excellent résultat.
-	Données d'entraînement ou de test : Il est important de noter que ce type de graphique est généralement tracé sur des données de test (des données que le modèle n'a pas vues pendant l'entraînement). Si le graphique était tracé sur les données d'entraînement, il pourrait y avoir un surapprentissage (overfitting), et les résultats seraient trop optimistes
-	Généralisation : Pour s'assurer que le modèle généralise bien à de nouvelles données, il est essentiel de le tester sur un ensemble de données de test représentatif et suffisamment grand.
	

CONCLUSION
Cette analyse a permis d’identifier des tendances climatiques et de mettre en place des modèles de prévision météorologique. Les résultats montrent que les modèles plus complexes, comme la forêt aléatoire, offrent une meilleure précision que la régression linéaire. Les techniques explorées peuvent inspirer d’autres applications de prévision climatique ou d’optimisation environnementale.
Pour aller plus loin, l’intégration de sources de données en temps réel (satellites, stations météo connectées) et l'optimisation des hyperparamètres des modèles pourraient améliorer les performances prédictives et permettre une anticipation plus fine des conditions météorologiques à venir.

