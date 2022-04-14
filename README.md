#  Données transcriptomiques  
## Analyse Exploiratoire des données :
*  Défintion
*  Détectio des valeurs  aberrant
*  Suppréssions et normalisation
 # Définition : 
  
Les  * données transcriptomiques ,ou données d'expressions des gènes, sont des  données issues de technologies variées  telles que la technologie de la biopuces ou puces  à ADN.

Nos données sont des données non -supervisées constituées  de  58717 variables  et 27  observations.Les données  servent de mesure  decrivant l'etat de haute dimension de chaque expression de gènes.Notre objectif est d'extraire  ces différents  états à l'aide  de modèles de grandes  capacités capables d'identifier l'expression des gènes.

# Détection des valeurs aberrantes:

Les valeurs aberrantes peuvent  être  detectées en utilisant la visualisation, en mettant des formules mathématiques sur l'ensemble de données ou en utilsant l'approche  statistique.

## Visualisation:

1.Utilisation de  la boxplot :


![boxplot des valeur abbérrentes](https://github.com/lapha99/Projet-stage/blob/main/figure/boxplot_ab%C3%A9rrantes.png)


L'approche IQR(Inter Quartile Range) pour trouver les valeurs aberrantes est l'approche la plus courrament utilisé.

IQR = Quarile3 -Quartile1

```Python
#  IQR

Q1 = np.percentile(data["feature"],25)

Q3 = np.percentile(data["feature"],75)

IQR= Q3 -Q1
```
Aprés avoir détecter les valeurs aberrantes à l'aide de l'IQR  on va éliminer les valeurs qui ne sont pas informatives pour l'analyse autrement dit on va supprimer les valeurs aberrantes.

## Supprimer les valeurs aberrantes

### Exemple de code :
``` Python
def drop_outliers(data,features):
  Q1 = np.percentile(data["feature"],25)
  Q3 = np.percentile(data["feature"],75)
  limit=1.5*(Q3-Q1)
  data.drop(data[data["feature"]> Q3 + limit].index,inplace=True)
  data.drop(data[data["feature"]< limit - Q1].index,inplace=True)
```  
  
  

Vous trouverez ci-dessous  une  visualisation de boxplot aprés suppressions des valeurs aberrantes

3.Visualiisation

![boxplot suppression ](https://github.com/lapha99/Projet-stage/blob/main/figure/box.png)

Remarque:
Aprés suppréssions des valeurs aberrantes la dimension de nos données est passée de (58717,27) à (16030,27).

# Normalisation des données:
Les données d'expression brutes sont normalisées selon la méthode CPM. Des unités d'expression normalisées sont nécessaires pour éliminer les biais techniques dans les données sequencées telles que la profondeur du sequençage et la longueure des gènes et rendre les expressions des gènesdirectement comparables au sein et entre les échantillons.La normalisation CPM (Nombcre par million de lectures mappées) ne tient pas en compte de la longueur de la transcription,convient  aux protocoles de sequençage ou les lectures sont générés quelques soit la longueur du gène.

Note: Normalisation CPM est installer à l'aide du package python *bioinfokit*

# Analyses des  composantes principales : 

Nous  présentons un ACP sur les données transcriptomique.Un ACP  est une méthode de reduction de  dimension  qui va permettre de transformer des variables trés corrélées en nouvelles variables  décorrélées les unes des autres. Le principe est simple  il s'agit enfaite  de resumer l'information  qui est contenu dans une large base de données  en un certain nombre de variables synthétiques appelés : composantes principales.

![ACP ]( https://github.com/lapha99/Projet-stage/blob/main/figure/ACP__Suppr.png)

# Méthodes TSNE:
t-Distributed Stochastic Neighbor Embedding (t-SNE) est une technique non supervisée et non linéaire principalement utilisée pour l'exploration de données et de visualisation de données de grandes dimension.Il a été developpé par Laurens van der Maatens et Geoffrey en 2008.

## Comparaison t-SNE et ACP:

l'ACP a été développé en 1933 tandis que t-SNE a été développé en 2008.Beaucoup de choses ont changé dans le monde de la science des données depuis 1933.
Deuxiément, L'ACP est une technique de réduction de dimension lineaire qui cherche à maximiser la variance la variance et à préserver de grandes distances par paires.En d'autre termes,les choses qui sont différentes finissent par être trés éloignées l'une de l'autre.Cela peut conduire à une mauvaise visualisation, en particulier lorsqu'il s'agit de variétés non linéaires. par exemple : cylindre,boule etc...t-SNE différe de ACP  en ne préservant que de petites distances par paires,tandis que l'ACP se préoccupe de préserver de grandes distances.
Maintenant que nous savons pourquoi nous pourrions utiliser t-sne sur PCA.
Expliquons comment fonctionne t-SNE, l'algorithme t-SNE calcule une mesure de similarité entre des paires d'instances dans l'espace de grandes dimension et dans l'espace de faible dimension.Il tente ensuite d'optimiser ces deux mesures à l'aide d'une fonction de coût.

Passons à la visualisation de nos données par t-SNE  et ici pour mieux expliquer ou avoir une ressemblance de structure à notre ACP on va jouer sur la perplexity.

Exemple de code:
```Python
from sklearn.manifold import TSNE
tsne=TSNE(n_components=2,perplexity=25,init="pca",learningRate=100;random_state=0,n_jobs=-1)
X_tsne=tsne.fit_transform(Xnorm)
```

## Visualisation t-SNE:

![Visualisation t-SNE](https://github.com/lapha99/Projet-stage/blob/main/figure/Tsne-final.png)

# Model Autoencoder:

Les auto encodeurs sont des reseaux de neurones qui possédent exactement le *même nombre de neurones sur leur couche d'entrée et leur couche de sortie*.
Le but pour un auto encodeur est d'avoir une sortie la plus proche de l'entrée!
L'apprentissage est donc << auto-supervisé >> car la perte (loss) à minimiser est le coût de reconstruction entre la sortie et l'entrée.Ce qui fait de ce modèle un modèle non supervisé.

## Architecture de notre modèle:

Nous selectionnons les 8000 gènes les plus exprimés de maniére variable par ecart absolue médiane (MAD).Nous compréssons ce vecteurs d'expression génique 8000(pour tous les échantillons) en ajoutants des couches de la sorte:
## 8000--->4000--->2000--->1000--->500--->250-->100.
Nous utilisons une activation "RELU",et pour la formation de notre modèle nous utilisons  la bibliothéque Keras.

![Architecture](https://github.com/lapha99/Projet-stage/blob/main/figure/auto_encoder.png)

# Hyperparamétrisation
Dans l'ensembe, nous sélectionnons Batch-size=50,learningRate=0.001 et pour les époques  grâce à Callbacks on a importé Earlystopping qui nous permet d'arrêter l'entraînement avant de passer à l'overfeating et ainsi il nous donne la bonne epoque qui est 100.
La perte d'entraînement et de validation sur 100 époques d'entraînement pour le modèle optimal est illustrées ci-dessous.

![Autoencoder_vis](https://github.com/lapha99/Projet-stage/blob/main/figure/AE_visualisation.png?raw=true)

En tant que modèle génératif(création de nouvelles entrées), les entités à dimension reduite peuvent être simuler pour échantilloner des données.
Vous trouverez ci-dessous une visualisation de t-SNE des caracteristiques codées de l'encodeurs.

![Visualisation t-SNE output](https://github.com/lapha99/Projet-stage/blob/main/figure/tsne-out.png)



