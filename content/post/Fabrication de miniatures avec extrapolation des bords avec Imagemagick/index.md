---
title: "Fabrication de miniatures avec extrapolation des bords avec Imagemagick"
date: 2021-05-07
categories:
- dev
tags:
- hugo
- imagemagick
---

J'ai choisi de générer des miniatures pour chacun de mes billets en réutilisant des photos issues des articles. 

J'utilise une technique qui me permet de produire des images complètement carrées :
Je redimensionne d'abord l'image pour qu'elle rentre entièrement dans un carré de 300x300 px puis je remplis les espaces vides en extrapolant à partir des zones se trouvant à proximité.

Pour ce faire j'ai légèrement adapté une série de commandes [_Imagemagick_](https://imagemagick.org/) concoctées par _fmw42_ (le fameux Fred de [Fred's ImageMagick Scripts](http://www.fmwconcepts.com/imagemagick/index.php)) dans cette page de forum  : [ImageMagick - turn image with borders into full bleed image](https://legacy.imagemagick.org/discourse-server/viewtopic.php?t=26928)

Le résultat est assez bluffant, il faut parfois regarder de près les zones extrapolées pour réaliser qu'elles sont artificielles :

![alpinestars-nucleon-racing-hr-kr_HB_niv2_face__source_revzilla_thumbnail](alpinestars-nucleon-racing-hr-kr_HB_niv2_face__source_revzilla_thumbnail.jpg)

![Course-rocker_jean_aramide_AA_thumbnail.jpg](Course-rocker_jean_aramide_AA_thumbnail.jpg)


Ce qui ne marche pas encore :

Apparemment les grands aplats de blanc ne lui conviennent pas :

![sastec-sc1-evo1-new__EA-KA-SB_niv2__source_sastec_thumbnail.jpg](sastec-sc1-evo1-new__EA-KA-SB_niv2__source_sastec_thumbnail.jpg)


Heureusement pour les fonds blancs il suffit de faire :

```sh
convert IMAGESOURCE -resize 300x300 -background white -gravity center -extent 300x300 IMAGECIBLE
```


## Le script :

J'ai créé un script shell, qui prend tous les fichiers du sous-répertoire _"a_traiter/"_ et produit les résultats dans le répertoire _"res/"_ .

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
	xcible=300
	ycible=300
	size=${xcible}x${ycible}
	convert ${fic} -resize ${size} res/tmp.png 

	xoff=`convert res/tmp.png -format "%w" info:`
	yoff=`convert res/tmp.png -format "%h" info:`
	yoff=$(((300-${yoff})/2))
	xoff=$(((300-${xoff})/2))
	vcoords="${size}-${xoff}-${yoff}"
	echo ${vcoords}
	convert res/tmp.png -transparent white +repage \( -clone 0 -alpha off -sparse-color Voronoi \
	"9,9 rgb(255,8,8)  969,9 rgb(255,255,8)  969,669 rgb(255,255,248)  9,669 rgb(255,248,248)" \) \
	+swap -compose over -composite \
	-define distort:viewport=${size}-$xoff-$yoff +distort SRT 0 +repage res/cover_${ficsource}
	rm -f res/tmp.png
done
```

Il n'y a plus qu'à mettre une image dans le répertoire _"a_traiter/"_ et lancer le script :

```sh
cd gen_thumb_sparse_voronoi/
./gen_thumb_sparse_voronoi.sh
```

Le résultat sera produit dans _"res/"_.



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





