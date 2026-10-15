# Priorisation de la maintenance d'un parc immobilier

Application exploratoire permettant d'évaluer la maturité d'équipements techniques à partir de données historiques de durée de vie et d'identifier les actifs nécessitant une attention prioritaire.

Le projet transforme des historiques de durée de vie en un indicateur simple d'aide à la décision, puis restitue les résultats dans une interface Streamlit avec visualisation cartographique.

## Objectif

Dans un parc immobilier comprenant de nombreux équipements techniques, tous les actifs ne nécessitent pas le même niveau d'attention au même moment.

L'objectif de ce projet est de proposer une méthode simple et explicable permettant de :

- situer les équipements par rapport aux durées de vie observées historiquement ;
- identifier les actifs approchant ou dépassant leur durée de vie de référence ;
- hiérarchiser les équipements selon plusieurs niveaux de priorité ;
- restituer ces informations dans un outil interactif d'aide à la décision.

## Méthodologie

### 1. Calcul de la durée de vie historique

Pour les équipements disposant d'une date d'installation et d'une date de retrait, la durée de vie observée est calculée en mois.

Les durées incohérentes sont exclues de l'analyse.

### 2. Calcul de l'âge des équipements en service

Pour les équipements ne disposant pas de date de retrait, l'âge est calculé à partir de leur date d'installation et de la dernière période observée dans le dataset.

Le jeu de données étant historique, la date de référence utilisée dans le prototype est décembre 2008.

### 3. Construction d'une durée de vie de référence

Une durée de vie de référence est calculée à partir de la médiane des durées historiques observées.

L'estimation est réalisée en priorité au niveau du **type d'équipement**, uniquement lorsqu'au moins 10 observations historiques sont disponibles.

Lorsque ce niveau de détail n'est pas suffisamment documenté, une estimation au niveau du **système** est utilisée selon le même seuil minimal.

Cette approche permet de limiter l'utilisation de références reposant sur un nombre trop faible d'observations.

### 4. Estimation de la durée restante

Pour chaque équipement encore en service :

`Durée restante estimée = Durée de vie de référence - Âge de l'équipement`

Cette valeur ne représente pas une date prévisionnelle de panne. Elle permet uniquement de positionner l'équipement par rapport aux durées historiquement observées.

### 5. Niveau de priorité

Les équipements sont répartis en cinq catégories :

| Niveau | Règle |
|---|---|
| Dépassement | Durée restante estimée < 0 mois |
| Prioritaire | Durée restante estimée < 24 mois |
| Surveillance | Durée restante estimée comprise entre 24 et 59 mois |
| Faible | Durée restante estimée ≥ 60 mois |
| Non évalué | Historique insuffisant pour produire une estimation |

Le statut **Non évalué** permet de distinguer un équipement pour lequel le niveau de priorité semble faible d'un équipement pour lequel les données disponibles ne permettent simplement pas d'établir de référence.

## Application

L'application Streamlit permet :

- de filtrer le parc par type d'équipement ;
- de filtrer par système ;
- de filtrer par niveau de priorité ;
- de suivre le nombre de bâtiments et d'équipements concernés ;
- d'identifier les équipements prioritaires ou ayant dépassé leur durée de vie de référence ;
- de visualiser le parc sur une carte interactive.

La carte affiche notamment le bâtiment, le système, le type d'équipement, son âge, sa durée restante estimée et son niveau de priorité.

## Visualisation géographique

Les coordonnées géographiques utilisées dans l'application sont **simulées**.

Chaque bâtiment est associé à une ville française uniquement afin d'illustrer la visualisation d'un parc immobilier distribué géographiquement.

Ces localisations ne correspondent pas aux emplacements réels des bâtiments présents dans la base source.

## Données

Les données utilisées proviennent de la **ASHRAE Service Life and Maintenance Cost Database**.

Source officielle : https://costdatabase.ashrae.org/service_life.asp

Les données présentes dans ce dépôt sont utilisées uniquement pour permettre la démonstration du projet. Pour toute réutilisation des données, il convient de se référer directement à la source officielle et à ses conditions d'utilisation.

## Technologies utilisées

- Python
- Pandas
- NumPy
- Streamlit
- Pydeck

## Limites

Ce projet constitue un outil exploratoire de **priorisation de la maintenance** et non un modèle industriel de maintenance prédictive.

L'indicateur repose principalement sur l'âge des équipements et sur les durées de vie historiquement observées. Il ne prend notamment pas en compte des variables qui pourraient être nécessaires à une véritable modélisation du risque de défaillance, telles que l'état réel de l'équipement, son intensité d'utilisation, les interventions de maintenance réalisées ou des données issues de capteurs.

Les résultats doivent donc être interprétés comme des **ordres de grandeur permettant de faciliter la priorisation**, et non comme des probabilités de panne ou des dates prévisionnelles de remplacement.

## Perspectives

Une évolution vers une approche réellement prédictive nécessiterait des données supplémentaires sur l'état et l'utilisation des équipements.

Le prototype pourrait notamment être enrichi avec :

- l'historique des interventions de maintenance ;
- les coûts de maintenance et de remplacement ;
- les données d'utilisation des équipements ;
- des mesures issues de capteurs ;
- des variables décrivant l'environnement et les conditions d'exploitation ;
- une modélisation du risque de défaillance ou du temps avant panne.

Ces informations permettraient de passer d'une logique de maturité fondée sur des références historiques à une approche prédictive plus complète.
