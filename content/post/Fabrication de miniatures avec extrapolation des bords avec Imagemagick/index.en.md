---
title: "3 Imagemagick methods to fill the empty areas of a cropped image"
date: "2021-05-07"
categories:
- Dev
tags:
- Imagemagick
---

_(This post was automatically translated with [www.DeepL.com/Translator](http://www.DeepL.com/Translator))_

I describe in this article 3 methods to fill the empty areas of an image when enlarging it, a basic technique, and 2 based on the initial image, for a more natural filling.
At the end of the article, I give a shell script to generate the 3 versions for a given image.

<!--more-->

## Method 1: Fill empty areas with a plain background.


```sh
convert IMAGESOURCE -resize XxY -background white -gravity center -extent XxY IMAGECIBLE
```

Replace XxY by the desired dimensions and "_white_" by the desired color.

Example:

![mode 2](sastec-sc1-06evo-new__EB-KB_niv2__vs__esquad_hilon_EA-KA-SA_niv1.jpg.mode2.jpg)


## Method 2: Fill in the blanks by superimposing the image over its blurred and distorted version

Inspiration: 

> _Fred/fmw42_ in this forum page: [ImageMagick - Keep Aspect Ratio on Resize and Fill with Blur Background](https://legacy.imagemagick.org/discourse-server/viewtopic.php?t=28035).

```sh
convert IMAGESOURCE \( -clone 0 -blur 0x9 -resize XxY! \) \( -clone 0 -resize XxY \) -delete 0 -gravity center -compose over -composite IMAGECIBLE
```

NB :_ Replace XxY by the desired dimensions.


Example:

![filling with blurred and distorted version](sastec-sc1-06evo-new__EB-KB_niv2__vs__esquad_hilon_EA-KA-SA_niv1.jpg.mode3.jpg)


## Method 3: Fill in the empty areas by extrapolation

Inspiration: 

> _Fred/fmw42_ in this forum page: [ImageMagick - turn image with borders into full bleed image](https://legacy.imagemagick.org/discourse-server/viewtopic.php?t=26928).

The code is a bit long, I don't put it here, it is in the _shell_ script below.

Example :

![mode 1](sastec-sc1-06evo-new__EB-KB_niv2__vs__esquad_hilon_EA-KA-SA_niv1.jpg.mode1.jpg)



The result is quite amazing, if the dimensions are not too far from the original, you sometimes have to look closely at the extrapolated areas to realize that they are artificial (here in 300x300) :

![alpinestars-nucleon-racing-hr-kr_HB_niv2_face__source_revzilla_thumbnail](alpinestars-nucleon-racing-hr-kr_HB_niv2_face__source_revzilla_thumbnail.jpg)

![Course-rocker_jean_aramide_AA_thumbnail.jpg](Course-rocker_jean_aramide_AA_thumbnail.jpg)


On the other hand, the large white areas do not suit him:

![sastec-sc1-evo1-new__EA-KA-SB_niv2__source_sastec_thumbnail.jpg](sastec-sc1-evo1-new__EA-KA-SB_niv2__source_sastec_thumbnail.jpg)


## Shell script

This shell script takes all the files in the _"a_process/"_ subdirectory and produces the results according to the 3 modes in the _"res/"_ directory.

- Mode 1 : Voronoi ;
- Mode 2 : Extension ;
- Mode 3: Flat white.

Set up:

```sh
mkdir gen_thumb_sparse_voronoi
cd gen_thumb_sparse_voronoi
mkdir a_process res
touch gen_thumb_sparse_voronoi.sh
```



Edit the file _"gen_thumb_sparse_voronoi.sh"_ and put :


```sh
#!/bin/bash

for fic in a_traiter/*
do
	echo ${fic}
	ficsource=$(basename ${fic})
	xcible=800
	ytarget=300
	size=${xcible}x${ycible}
	convert ${fic} -resize ${size} res/tmp.png 

	xoff=`convert res/tmp.png -format "%w" info:`
	yoff=`convert res/tmp.png -format "%h" info:`
	yoff=$(((${ycible}-${yoff})/2))
	xoff=$(((${xcible}-${xoff})/2))
	vcoords="${size}-${xoff}-${yoff}"
	echo ${vcoords}
	convert res/tmp.png -transparent white +repage( -clone 0 -alpha off -sparse-color Voronoi
	"9,9 rgb(255,8,8) 969,9 rgb(255,255,8) 969,669 rgb(255,255,248) 9,669 rgb(255,248,248)" \) \
	+swap -compose over -composite \
	-define distort:viewport=${size}-${xoff}-${yoff} +distort SRT 0 +repage res/${ficsource}.mode1.jpg
	rm -f res/tmp.png
	
	convert ${fic} -resize ${xcible}x${ycible} -background white -gravity center -extent ${xcible}x${ycible} res/${ficsource}.mode2.jpg
	
	convert ${fic} \( -clone 0 -blur 0x9 -resize ${xcible}x${ycible}! \) -delete 0 -gravity center -compose over -composite res/${ficsource}.mode3.jpg
done
```

All you have to do is put an image in the _"a_traiter/"_ directory and run the script:

```sh
cd gen_thumb_sparse_voronoi/
./gen_thumb_sparse_voronoi.sh
```

The result will be produced in _"res/"_.

> Note: To change the target dimensions, just change the xcible=800 and ycible=300.



