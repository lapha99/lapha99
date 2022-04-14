#  Données transcriptomiques  
## Analyse Exploiratoire des données :
*  Défintion
*  Détectio des valeurs  abbérente
*  Visualisation  des données

1.  ###  Définition :

Les  * données transcriptomiques ,ou données d'expressions des gènes, sont des  données issues de technologies variées  telles que la technologie de la biopuces ou puces  à ADN.

Nos données sont des données non -supervisées constituées  de  58717 variables  et 27  observations.Les données  servent de mesure  decrivant l'etat de haute dimension de chaque expression de gènes.Notre objectif est d'extraire  ces différents  états à l'aide  de modèles de grandes  capacités capables d'identifier l'expression des gènes.

2. ###  Détectio des valeurs abbérentes:

![boxplot des valeur abbérrentes](https://github.com/lapha99/Projet-stage/blob/main/figure/boxplot_ab%C3%A9rrantes.png)

On voit qu'on a beaucoup de valeurs abbérentes  ainsi on  éssaye de supprimer le maximum de valeurs abbérente,      
Vous trouverez ci-dessous  une  visualisation de boxplot aprés suppressions des valeurs abbérentes  

3.Visualiisation

![boxplot suppression ](https://github.com/lapha99/Projet-stage/blob/main/figure/box.png)

# Analyses des  composantes principales : 

Nous  présentons un ACP sur les données transcriptomique.Un ACP  est une méthode de reduction de  dimension  qui va permettre de transformer des variables trés corrélées en nouvelles variables  décorrélées les unes des autres. Le principe est simple  il s'agit enfaite  de resumer l'information  qui est contenu dans une large base de données  en un certain nombre de variables synthétiques appelés : composantes principales.

![ACP ]( https://github.com/lapha99/Projet-stage/blob/main/figure/ACP__Suppr.png)
