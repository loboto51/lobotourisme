---
title: "3 méthodes Imagemagick pour remplir les zones vides d'une image recadrée"
date: "2021-05-07"
categories:
- Dev
tags:
- Imagemagick
---

Je décris dans cet article 4 méthodes permettant de combler les zones vides d'une image lorsqu'on l'agrandit, une technique basique, et 3 s'appuyant sur l'image initiale, pour un remplissage plus naturel.
A la fin de l'article, je donne un script shell permettant de générer les 4 versions pour une image donnée.

<!--more-->

> _Edit du 22/04/2023 : Ajout du remplissage avec la couleur de fond initiale._  

## Méthode 1 : Remplir des zones vides par un fond uni

### Simple : Couleur de fond fixée (blanc)

```sh
convert IMAGESOURCE  -trim -fuzz 5% +repage -auto-orient -resize XxY -background white -gravity center -extent XxY IMAGECIBLE
```

_NB :_ Remplacer XxY par les dimensions souhaitées et "_white_" par la couleur souhaitée.

Exemple :

![Mode 11](bottes_stylmartin_rocket_picto.jpg.mode11.jpg)
![Mode 11](EN13594-1_-_DXR_-_SANCHEZ_PUNCHER.jpg.mode11.jpg)
![Mode 11](EN13594-1KP_-_FIVE_-_TFX2_WOMAN_WP.jpg.mode11.jpg)

### Avancée : Couleur de fond prise dans le premier pixel en haut à gauche de l'image originale

```sh
convert IMAGESOURCE -gravity northwest -crop 1x1+0+0 -format "%[pixel:u]" info:- | xargs -I {} convert IMAGESOURCE -resize XxY -background {} -gravity center -extent XxY IMAGECIBLE
```

Exemple :

![Mode 12](bottes_stylmartin_rocket_picto.jpg.mode12.jpg)
![Mode 12](EN13594-1_-_DXR_-_SANCHEZ_PUNCHER.jpg.mode12.jpg)
![Mode 12](EN13594-1KP_-_FIVE_-_TFX2_WOMAN_WP.jpg.mode12.jpg)


## Méthode 2 : Remplir les zones vides en surimposant l'image sur sa version floue et déformée

Inspiration : 

> _Fred/fmw42_ dans cette page de forum : [ImageMagick - Keep Aspect Ratio on Resize and Fill with Blur Background](https://legacy.imagemagick.org/discourse-server/viewtopic.php?t=28035).

```sh
convert IMAGESOURCE \( -clone 0 -blur 0x9 -resize XxY\! \) \( -clone 0 -resize XxY \) -delete 0 -gravity center -compose over -composite  IMAGECIBLE
```

_NB :_ Remplacer XxY par les dimensions souhaitées.


Exemple :

![Mode 2](bottes_stylmartin_rocket_picto.jpg.mode2.jpg)    
![Mode 2](EN13594-1_-_DXR_-_SANCHEZ_PUNCHER.jpg.mode2.jpg)                                        
![Mode 2](EN13594-1KP_-_FIVE_-_TFX2_WOMAN_WP.jpg.mode2.jpg)

## Méthode 3 : Remplissage des zones vides par extrapolation

Inspiration : 

> _Fred/fmw42_ dans cette page de forum : [ImageMagick - turn image with borders into full bleed image](https://legacy.imagemagick.org/discourse-server/viewtopic.php?t=26928).

Le code est un peu long, je ne le met pas ici, il se trouve dans le script _shell_ plus bas.

Le résultat est assez bluffant sur les photos, en particulier si les dimensions ne sont pas trop éloignées de l'original :
Il faut parfois regarder de près les zones extrapolées pour réaliser qu'elles sont artificielles (ici en 300x300).

En revanche les grands aplats de blanc ne lui conviennent pas et ressortent en rouge.

Exemple :

![Mode 3](bottes_stylmartin_rocket_picto.jpg.mode3.jpg)                                            
![Mode 3](EN13594-1_-_DXR_-_SANCHEZ_PUNCHER.jpg.mode3.jpg)
![Mode 3](EN13594-1KP_-_FIVE_-_TFX2_WOMAN_WP.jpg.mode3.jpg)

## Script shell

Ce script shell prend tous les fichiers du sous-répertoire _"a_traiter/"_ et produit les résultats selon les 3 modes dans le répertoire _"res/"_ .

- Mode 11 : Aplat de blanc ;
- Mode 12 : Aplat de la couleur de fond ;
- Mode 2 : Extension ;
- Mode 4 : Voronoi.

Mise en place :

```sh
mkdir gen_thumbs
cd gen_thumbs
mkdir a_traiter res
touch gen_thumbs.sh
chmod +x gen_thumbs.sh
```

Editer le fichier _"gen_thumbs.sh"_ et y mettre :


```sh
#!/bin/bash

for fic in a_traiter/*
do
	echo ${fic}
	ficsource=$(basename "${fic}")
	xcible=300
	ycible=200
	size=${xcible}x${ycible}
	convert "${fic}" -resize ${size} res/tmp.png 

	xoff=`convert res/tmp.png -format "%w" info:`
	yoff=`convert res/tmp.png -format "%h" info:`
	yoff=$(((${ycible}-${yoff})/2))
	xoff=$(((${xcible}-${xoff})/2))
	vcoords="${size}-${xoff}-${yoff}"
	echo ${vcoords}

    # Mode 11 - fond blanc
	convert "${fic}" -trim -fuzz 5% +repage -auto-orient -resize ${xcible}x${ycible} -background white -gravity center -extent ${xcible}x${ycible} res/"${ficsource}".mode11.jpg

    # Mode 12 - fond repris de l'original
	convert "${fic}" -gravity northwest -crop 1x1+0+0 -format "%[pixel:u]" info:- | xargs -I {} convert "${fic}" -trim -fuzz 5% +repage -auto-orient -resize ${xcible}x${ycible} -background {} -gravity center -extent ${xcible}x${ycible} res/"${ficsource}".mode12.jpg

    # Mode 2 - superposition
	convert "${fic}" \( -clone 0 -blur 0x9 -resize ${xcible}x${ycible}\! \) \( -clone 0 -resize ${xcible}x${ycible} \) -delete 0 -gravity center -compose over -composite res/"${ficsource}".mode2.jpg

	# Mode 3 - superposition
	convert res/tmp.png -transparent white +repage \( -clone 0 -alpha off -sparse-color Voronoi \
	"9,9 rgb(255,8,8)  969,9 rgb(255,255,8)  969,669 rgb(255,255,248)  9,669 rgb(255,248,248)" \) \
	+swap -compose over -composite \
	-define distort:viewport=${size}-${xoff}-${yoff} +distort SRT 0 +repage res/"${ficsource}".mode3.jpg
	rm -f res/tmp.png
done
```

Il n'y a plus qu'à mettre une image dans le répertoire _"a_traiter/"_ et lancer le script :

```sh
cd gen_thumbs/
./gen_thumbs.sh
```

Le résultat sera produit dans _"res/"_.

> Note : Pour modifier les dimensions cible, il suffit de changer les valeurs de xcible=xxx et ycible=yyy.



