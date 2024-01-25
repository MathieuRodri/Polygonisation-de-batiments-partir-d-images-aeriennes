# 🌐 Projet de Modélisation en Vision

<br><br><br>

* [📌 Introduction](#---introduction)
  
* [🗂 Dataset et Contexte](#---dataset-et-contexte)
  
* [🛠 Méthodologie](#---m-thodologie)
  
  + [🔍 Segmentation Sémantique](#---segmentation-s-mantique)
    
  + [📐 Polygonisation](#---polygonisation)
    
* [📊 Observations et Évaluations](#---observations-et--valuations)
  
* [🔚 Conclusion](#---conclusion)

<br><br><br>
  
## 📌 Introduction

Dans le cadre de mon Master en Informatique avec une spécialisation en Vision à l'Université Paris Cité, j'ai travaillé sur un projet fascinant de modélisation en vision. Ce projet, réalisé en collaboration avec Hajar Goddi et supervisé par Sylvain Lobry, a porté sur la polygonisation des bâtiments à partir d'images aériennes.



## 🗂 Dataset et Contexte

![Dataset](https://github.com/MathieuRodri/Polygonisation-de-b-timents-partir-d-images-a-riennes/blob/main/Images/1.png)

Le cœur de ce projet reposait sur l'utilisation du dataset du Mapping Challenge d'AIcrowd. Ce dataset comprenait des images satellites en RGB annotées pour identifier les bâtiments, offrant un contexte humanitaire significatif. Le challenge s'est déroulé en deux tours, avec une annonce des résultats le 20 août 2018. Pour mon projet, j'ai utilisé les ensembles de données `train.tar.gz` et `val.tar.gz`, en répartissant les données en 70% pour l'entraînement et 30% pour la validation (`train.tar.gz`) et 100% des données de `val.tar.gz` pour le test.



## 🛠 Méthodologie

![Méthodologie](https://github.com/MathieuRodri/Polygonisation-de-b-timents-partir-d-images-a-riennes/blob/main/Images/2.png)

### 🔍 Segmentation Sémantique

La première étape a été la segmentation sémantique. J'ai initialement mis en place un réseau UNet, mais j'ai vite réalisé qu'il était trop simple et inefficace pour apprendre correctement à partir de mes données. 

Confronté à ce défi, j'ai opté pour une architecture hybride plus avancée. Cette architecture combinait un ResNet en tant qu'encodeur et un U-Net comme décodeur. 

Le ResNet, connu pour sa capacité à bien capter les caractéristiques à partir des images, a servi d'encodeur robuste. Tandis que U-Net, efficace pour la localisation précise des objets dans les images, a fonctionné comme décodeur. 

Cette combinaison a permis une meilleure distinction et localisation des bâtiments dans les images satellites.

![Test](https://github.com/MathieuRodri/Polygonisation-de-b-timents-partir-d-images-a-riennes/blob/main/Images/3.png)
![Test](https://github.com/MathieuRodri/Polygonisation-de-b-timents-partir-d-images-a-riennes/blob/main/Images/4.png)

En évaluant qualitativement les résultats de la segmentation semantic, j'ai constaté des résultats globalement satisfaisants. Cependant, il y avait des aspects nécessitant une attention particulière.

Premièrement, la présence de bruit dans certaines prédictions a été remarquée. Cela se manifestait par des irrégularités ou des artefacts dans les zones prédites, ce qui pouvait entraîner des imprécisions dans l'identification des bâtiments. 

Deuxièmement, il y avait des cas où des trous étaient présents au milieu de certains bâtiments prédits. Cela signifiait que le modèle ne parvenait pas toujours à capturer l'intégralité de la structure d'un bâtiment, laissant des vides au sein des contours prédits. 

![Segmentation Semantic](https://github.com/MathieuRodri/Polygonisation-de-b-timents-partir-d-images-a-riennes/blob/main/Images/5.png)

### 📐 Polygonisation

La seconde étape cruciale était la polygonisation. Après la segmentation, les résultats étaient encore bruts et nécessitaient un affinement pour améliorer leur précision. La polygonisation a joué un rôle clé dans ce processus d'affinement. J'ai utilisé les fonctions `cv2.findContours` et `cv2.approxPolyDP` de la bibliothèque OpenCV. `cv2.findContours` a aidé à détecter les contours des bâtiments dans les images segmentées. Ensuite, j'ai appliqué `cv2.approxPolyDP` pour simplifier ces contours. Cette fonction utilise l'algorithme de Douglas-Peucker, un processus qui réduit le nombre de points dans un contour tout en préservant sa forme générale. Il élimine les points qui sont trop proches d'une ligne droite formée entre deux points consécutifs, créant ainsi des formes polygonales simplifiées et exploitables pour des applications pratiques comme la cartographie et la surveillance des zones urbaines.

![Polygonisation](https://github.com/MathieuRodri/Polygonisation-de-b-timents-partir-d-images-a-riennes/blob/main/Images/6.png)

## 📊 Observations et Évaluations


Dans cette phase du projet, j'ai procédé à une évaluation des résultats obtenus après la segmentation et la polygonisation. Les observations ont révélé des améliorations significatives dans la structure et l'uniformité des prédictions. La qualité des résultats de segmentation s'est nettement améliorée après leur conversion en formes polygonales, ce qui a été essentiel pour assurer des applications pratiques précises.

Pour évaluer quantitativement ces améliorations, j'ai utilisé deux métriques clés : le score Intersection sur Union (IoU) et le score F1. Le score IoU, une mesure courante dans les tâches de segmentation, compare la zone d'intersection sur l'union entre la prédiction et la vérité terrain. Un score IoU de 0.81 indique une bonne concordance entre les zones prédites et les zones réelles. Le score F1, qui évalue la précision et le rappel, a atteint 0.94.

![IoU](https://github.com/MathieuRodri/Polygonisation-de-b-timents-partir-d-images-a-riennes/blob/main/Images/7.png)
![Evaluation](https://github.com/MathieuRodri/Polygonisation-de-b-timents-partir-d-images-a-riennes/blob/main/Images/8.png)
![Comparatif](https://github.com/MathieuRodri/Polygonisation-de-b-timents-partir-d-images-a-riennes/blob/main/Images/9.png)

## 🔚 Conclusion

Ce projet de polygonisation des bâtiments à partir d'images aériennes, dans le cadre de mon Master en Informatique spécialisé en Vision, a été une expérience enrichissante et un pas significatif dans mon parcours académique et professionnel. Il a non seulement mis en évidence le potentiel du traitement d'images et de la vision par ordinateur, mais a également souligné les défis et opportunités d'amélioration dans ces domaines. Les résultats, bien qu'imparfaits, ont offert des connaissances précieuses sur les méthodes de segmentation et de polygonisation, ouvrant la voie à des recherches futures et à des applications pratiques dans des contextes variés, notamment humanitaires. Ce projet a donc été une étape clé dans ma compréhension et ma maîtrise des technologies de vision par ordinateur.
