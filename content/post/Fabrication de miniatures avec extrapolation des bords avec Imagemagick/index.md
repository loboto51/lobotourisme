---
title: "Fabrication de miniatures avec extrapolation des bords avec Imagemagick"
date: 2021-05-07
categories:
- dev
tags:
- hugo
- imagemagick
---

---

Edit du 15/05/2021 : J'ai intégré 3 modes de fabrication de miniature dans le script, pour pouvoir ensuite choisir facilement le meilleur résultat.
Entre temps j'ai changé de thème Hugo ([hugo-future-imperfect](https://github.com/statnmap/hugo-future-imperfect)) et les miniatures de 800x300 s'y prêtent mieux.

---

J'ai choisi de générer des miniatures pour chacun de mes billets en réutilisant des photos issues des articles. 

J'utilise 3 techniques pour produire des images aux dimensions qui me conviennent :

## Technique 1 : Remplissage des zones vides par extrapolation

J'ai légèrement adapté une série de commandes [_Imagemagick_](https://imagemagick.org/) concoctées par _fmw42_ (le fameux Fred de [Fred's ImageMagick Scripts](http://www.fmwconcepts.com/imagemagick/index.php)) dans cette page de forum  : [ImageMagick - turn image with borders into full bleed image](https://legacy.imagemagick.org/discourse-server/viewtopic.php?t=26928).

Le code est un peu long, je mets à la fin de l'article un script _shell_ comportant l'ensemble.

Le résultat est assez bluffant, il faut parfois regarder de près les zones extrapolées pour réaliser qu'elles sont artificielles (ici en 300x300) :

![alpinestars-nucleon-racing-hr-kr_HB_niv2_face__source_revzilla_thumbnail](alpinestars-nucleon-racing-hr-kr_HB_niv2_face__source_revzilla_thumbnail.jpg)

![Course-rocker_jean_aramide_AA_thumbnail.jpg](Course-rocker_jean_aramide_AA_thumbnail.jpg)


En revanche les grands aplats de blanc ne lui conviennent pas :

![sastec-sc1-evo1-new__EA-KA-SB_niv2__source_sastec_thumbnail.jpg](sastec-sc1-evo1-new__EA-KA-SB_niv2__source_sastec_thumbnail.jpg)


## Technique 2 : Remplissage des zones vides par un fond blanc

Comme le titre l'indique.

La commande est simple :

```sh
convert IMAGESOURCE -resize XxY -background white -gravity center -extent XxY IMAGECIBLE
```

## Technique 3 : Remplissage des zones vides par une version floue et déformée de l'image

J'ai repris une commande aussi concoctée par _Fred/fmw42_ dans cette page de forum : [ImageMagick - Keep Aspect Ratio on Resize and Fill with Blur Background](https://legacy.imagemagick.org/discourse-server/viewtopic.php?t=28035).

```sh
convert IMAGESOURCE \( -clone 0 -blur 0x9 -resize XxY\! \) \( -clone 0 -resize XxY \) -delete 0 -gravity center -compose over -composite  IMAGECIBLE
```



## Le script :

J'ai créé un script shell, qui prend tous les fichiers du sous-répertoire _"a_traiter/"_ et produit les résultats selon les 3 modes dans le répertoire _"res/"_ .

Mise en place :

```sh
mkdir gen_thumb_sparse_voronoi
cd gen_thumb_sparse_voronoi
mkdir a_traiter res
touch gen_thumb_sparse_voronoi.sh
```

Editer le fichier _"gen_thumb_sparse_voronoi.sh"_ et y mettre :


```sh
#!/bin/bash

for fic in a_traiter/*
do
	echo ${fic}
	ficsource=$(basename ${fic})
	xcible=800
	ycible=300
	size=${xcible}x${ycible}
	convert ${fic} -resize ${size} res/tmp.png 

	xoff=`convert res/tmp.png -format "%w" info:`
	yoff=`convert res/tmp.png -format "%h" info:`
	yoff=$(((${ycible}-${yoff})/2))
	xoff=$(((${xcible}-${xoff})/2))
	vcoords="${size}-${xoff}-${yoff}"
	echo ${vcoords}
	convert res/tmp.png -transparent white +repage \( -clone 0 -alpha off -sparse-color Voronoi \
	"9,9 rgb(255,8,8)  969,9 rgb(255,255,8)  969,669 rgb(255,255,248)  9,669 rgb(255,248,248)" \) \
	+swap -compose over -composite \
	-define distort:viewport=${size}-${xoff}-${yoff} +distort SRT 0 +repage res/${ficsource}.mode1.jpg
	rm -f res/tmp.png
	
	convert ${fic} -resize ${xcible}x${ycible} -background white -gravity center -extent ${xcible}x${ycible} res/${ficsource}.mode2.jpg
	
	convert ${fic} \( -clone 0 -blur 0x9 -resize ${xcible}x${ycible}\! \) \( -clone 0 -resize ${xcible}x${ycible} \) -delete 0 -gravity center -compose over -composite res/${ficsource}.mode3.jpg
done
```

Il n'y a plus qu'à mettre une image dans le répertoire _"a_traiter/"_ et lancer le script :

```sh
cd gen_thumb_sparse_voronoi/
./gen_thumb_sparse_voronoi.sh
```

Le résultat sera produit dans _"res/"_.

Exemple :

![mode 1](sastec-sc1-06evo-new__EB-KB_niv2__vs__esquad_hilon_EA-KA-SA_niv1.jpg.mode1.jpg)

![mode 2](sastec-sc1-06evo-new__EB-KB_niv2__vs__esquad_hilon_EA-KA-SA_niv1.jpg.mode2.jpg)

![mode 3](sastec-sc1-06evo-new__EB-KB_niv2__vs__esquad_hilon_EA-KA-SA_niv1.jpg.mode3.jpg)



## Question : Pourquoi ne pas utiliser simplement les fonctions Hugo ?

Effectivement, Hugo fournit pas mal de fonctions pour manipuler les images : Redimensionnement/découpage/etc. ([gohugo - image processing](https://gohugo.io/content-management/image-processing/)), et il suffit de bricoler ses fichiers _layout_ pour avoir rapidement quelque chose de présentable (ex : [discourse-gohugo - Display cover image on post list](https://discourse.gohugo.io/t/solved-display-cover-image-on-post-list/13141/2)).

Les seules de ces techniques qui permettent d'avoir des miniatures de mêmes dimensions (ex. : 300 px x 300 px) sont _.Resize_ qui va déformer l'image et _.Fill_ qui va la tronquer, mais si ces limitations ne vous gênent pas, cette méthode est nettement plus pratique que celle au dessus.


L'ensemble des transformations disponibles dans _Hugo_ :

- _.Resize_ : Permet de redimensionner l'image selon 3 modes :
    - Hauteur fixée -> Les proportions sont maintenues ;
    - Largeur fixée -> Les proportions sont maintenues ;
    - Hauteur et largeur fixées -> Les proportions ne sont pas maintenues ;
- _.Fit_ : On indique une hauteur et une largeur **maximum** et Hugo redimensionne l'image pour qu'elle tienne dedans (c'est un _.Resize_ qui s'arrête dès qu'il ne peut plus poursuivre sans modifier les proportions) ;
- _.Fill_ : On indique une hauteur et une largeur **minimum** et Hugo tente d'y insérer le maximum de l'image sans modifier les proportions. Par défaut il choisit seul mais on peut lui donner des directives : _smart_ (défaut), _left_, _right_...

Il y a énormément d'exemples ici : [gohugo - image processing](https://gohugo.io/content-management/image-processing/)





