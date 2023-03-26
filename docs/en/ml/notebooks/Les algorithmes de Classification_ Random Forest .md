

# Les algorithmes de Classification: Random Forest 


1. Introduction 
	2. Rappel sur le principe de classification 
	3. Rappel sur le principe d'arbre de décision  
2. Les Classification And Regression Tree (CART) en théorie
	3. La notation mathématique du problème : critère de division, critère d’arrêt, elagage de l’arbre et construction de la suite emboîtée d’arbres
	4. Le feature engineering et l'encodage des varibales qualitatives
	5. L'analyse et la validation du modèle : test train split et  accuracy score 

3. L'importance des hyperparametres avec la librairie scikit-learn

--> https://towardsdatascience.com/hyperparameter-tuning-the-random-forest-in-python-using-scikit-learn-28d2aa77dd74 
______________________________________________ 

# Contexte 

Le but de ce cours est de vous faire comprendre les principes théoriques des arbres de décision, qui permettent d’établir des règles successives permettant de classer des observations ou de faire des régressions. 


... 





Les random forest (en français, forêts d’arbres aléatoires) s’inscrivent dans le champ des méthodes de partitionnement récursif, qu’on connait plutôt sous l’acronyme CART (Classification And Regression Tree). On parle de classification lorsque la variable cible est qualitative (catégorielle), cas que nous traiterons plus tard. 

Dans un premier temps nous nous intéresserons au Regression Trees qui interviennent dans le cas d’une variable cible quantitative. Leur avantage réside dans leur représentation graphique aisément lisible. L’arbre est composé de trois types d’éléments :



*   La racine, où réside l’ensemble des données d’apprentissage.
*   Les noeuds/branches, qui représente les points à partir de la racine où les données sont séparées en deux groupes selon un critère lié aux variables explicatives.
*   Les feuilles, qui sont les noeuds terminaux de l’arbre et auxquels sont associés un valeur dans le cas où ***Y*** est quantitative et une classe lorsque ***Y*** est qualitative.

Ainsi à partir de la racine on défini un noeud qui divise l’ensemble des données selon un critère lié à une variable explicative, pour chacune des deux branches ainsi créées on répète le même procédé, et ainsi de suite jusqu’à ce qu’aucune division ne satisfasse le critère de construction d’un noeud et on définit alors un noeud terminal ou feuille.
