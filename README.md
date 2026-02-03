# Statistique en Grande Dimension et Apprentissage
## TP – Projet-Master 2 Data Science-Université d’Angers - Département de Mathématiques
### 1. Introduction
Ce projet s’inscrit dans le cadre du cours Statistique en Grande Dimension et Apprentissage du Master 2 Data Science de l’Université d’Angers. L’objectif est de mettre en œuvre les méthodes
d’apprentissage statistique vues au cours du semestre, aussi bien sur des données synthétiques que sur des données réelles.
Le travail est composé de deux parties. La première concerne un problème de régression supervisée en grande dimension, pour lequel l’objectif est de construire le meilleur prédicteur
possible au sens de l’erreur quadratique moyenne. La seconde partie repose sur un data challenge issu de la plateforme ChallengeData, portant sur la prédiction de la charge sinistre d’un contrat d’assurance multirisque agricole.
### 2. Exercice 1– Régression en grande dimension
#### 2.1 Description des données
Les données fournies sont issues d’un générateur synthétique. Le problème étudié est une tâche de régression supervisée comprenant p = 100 variables explicatives et une variable réponse
réelle. L’échantillon d’apprentissage est constitué de n = 1000 observations.Certaines variables contiennent des valeurs manquantes. Un échantillon test est également fourni, sans la variable réponse, et sert à produire les prédictions finales.
#### 2.2 Analyse exploratoire et valeurs manquantes
Une analyse exploratoire des données a été menée afin de mieux comprendre leur structure.En particulier, le pourcentage total de valeurs manquantes dans l’échantillon d’apprentissage a été calculé. On observe une proportion de 0.99% de données manquantes, ce qui est négligeable.
Une étude plus détaillée par variable a permis d’identifier les variables les plus affectées par les valeurs manquantes. Par ailleurs, une analyse de corrélation entre les variables explicatives
et la variable cible a été réalisée afin d’obtenir une première intuition sur les relations linéaires potentielles.

#### 2.3 Prétraitement des données
Afin de pouvoir entraîner l’ensemble des modèles considérés, une stratégie d’imputation simple a été mise en place. Les valeurs manquantes ont été remplacées par la médiane des variables correspondantes, ce choix permettant de limiter l’influence de valeurs extrêmes.
Dans un second temps, les variables explicatives ont été standardisées. Cette étape est particulièrement importante pour les modèles linéaires pénalisés, dont les performances dépendent de l’échelle des variables.
L’ensemble de ces étapes a été intégré dans un pipeline afin de garantir la cohérence du prétraitement lors de la validation croisée.
#### 2.4 Modèles considérés
Plusieurs modèles de régression ont été testés afin de comparer leurs performances dans un contexte de grande dimension :
 — la régression Ridge;
 — la régression Elastic Net;
 — le Gradient Boosting Regressor;
 — la forêt aléatoire;
 — le modèle XGBoost.
Ces modèles ont été choisis afin de couvrir à la fois des approches linéaires pénalisées et des méthodes non linéaires basées sur des ensembles d’arbres de décision.
#### 2.5 Validation croisée et sélection des hyperparamètres
Les hyperparamètres de chaque modèle ont été ajustés à l’aide d’une validation croisée à cinq folds. La métrique utilisée pour la sélection des modèles est l’erreur quadratique moyenne(MSE).
Pour les modèles Ridge et Elastic Net, différents paramètres de pénalisation ont été testés.Le modèle offrant la plus faible MSE en validation croisée a été retenu. Les modèles de type Gradient Boosting, forêt aléatoire et XGBoost ont également fait l’objet d’une recherche sur une
grille d’hyperparamètres.
#### 2.6 Comparaison des performances
Les performances des différents modèles ont été comparées sur la base de leur MSE obtenue en validation croisée. Les résultats montrent que le Gradient Boosting et XGBoost, offrent de meilleures performances que les modèles linéaires pénalisés dans ce contexte.
Ces résultats s’expliquent par la capacité de ces modèles à capturer des relations non linéaires entre les variables explicatives et la variable cible.
#### 2.7 Prédictions finales
Le modèle offrant les meilleures performances a été entraîné sur l’ensemble des données d’apprentissage. Il a ensuite été utilisé pour produire les prédictions sur l’échantillon test. Les prédictions finales ont été sauvegardées dans un fichier ypred.csv, conformément aux consignes
du projet.
### 3 Exercice 2– Data Challenge
#### 3.1 Présentation du problème
Le second exercice repose sur un data challenge portant sur la prédiction de la charge sinistre d’un contrat d’assurance multirisque agricole. La variable CHARGE est modélisée comme le produit de trois composantes : la fréquence des sinistres (FREQ), le coût moyen (CM) et l’année d’assurance.
L’objectif est de modéliser séparément ces composantes afin d’obtenir une prédiction précise de la charge globale.
#### 3.2 Prétraitement des données
Les données contiennent à la fois des variables numériques et des variables catégorielles, ainsi que des valeurs manquantes. Les variables numériques ont été imputées par la valeur zéro, tandis que les variables catégorielles ont été complétées par une modalité spécifique Missing.
Certaines variables jugées non pertinentes pour la modélisation, telles que l’identifiant du contrat et l’année d’assurance, ont été retirées des variables explicatives. Les variables catégo
rielles ont ensuite été encodées à l’aide d’un Count Encoding, permettant de transformer les modalités en variables numériques.
#### 3.3 Modélisation de la fréquence
La fréquence des sinistres (FREQ) a été modélisée à l’aide d’un modèle XGBoost Regressor. Ce choix repose sur la capacité de ce modèle à capturer des relations non linéaires complexes tout en limitant le surapprentissage grâce à des mécanismes de régularisation et de sous-échantillonnage.
Les hyperparamètres ont été fixés de manière à privilégier des arbres peu profonds et un taux d’apprentissage modéré.
#### 3.4 Modélisation du coût moyen
Le coût moyen (CM) a été modélisé à l’aide d’un Gradient Boosting Regressor. Ce modèle permet de combiner plusieurs arbres de décision entraînés séquentiellement afin d’améliorer progressivement la qualité des prédictions, tout en contrôlant la variance.
#### 3.5 Prédiction de la charge
La charge finale a été obtenue en combinant les prédictions de la fréquence et du coût moyen,selon la relation :CHARGE = FREQ×CM×ANNEE_ASSURANCE'.
Les performances ont été évaluées sur l’échantillon d’entraînement à l’aide de la racine de l’erreur quadratique moyenne (RMSE). Les prédictions finales sur l’échantillon test ont été exportées dans un fichier submission_test_2.csv.

#### 4.Conclusion
Ce projet a permis de mettre en pratique les méthodes d’apprentissage statistique adaptées aux problèmes de grande dimension. La première partie a mis en évidence l’intérêt des méthodes dites ensemblistes pour des données synthétiques de forte dimension, tandis que la seconde partie
a illustré la complexité des données réelles.
Dans l’ensemble, ce travail souligne l’importance du prétraitement des données, du choix des modèles et de la validation rigoureuse des performances dans des problèmes de régression appliquée.
