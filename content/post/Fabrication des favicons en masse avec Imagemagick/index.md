---
title: "Fabrication des favicons en masse avec Imagemagick"
date: "2021-05-15"
categories:
- dev
tags:
- hugo
- imagemagick
---

J'ai fabriqué toutes les variantes d'images de ce site avec des lignes de commande.

## Image originale

J'ai acheté une belle icône sur le très bon site [The Noun Project](https://thenounproject.com/) : [Space dessinée par Adrien Coquet](https://thenounproject.com/term/space/2217279/). 

Je l'ai modifiée dans _Inkscape_ pour ajouter un peu de couleur, puis j'ai généré une version PNG de 1024x1024 "_logo_large.png_" que j'ai mise dans _./static/logos/_.

### Génération des variantes

J'ai généré avec ImageMagick toutes les variantes recommandées par [The 2020 Guide to FavIcons for Nearly Everyone and Every Browser](https://www.emergeinteractive.com/insights/detail/the-essentials-of-favicons/) ainsi que [favicon-cheat-sheet](https://github.com/audreyfeldroy/favicon-cheat-sheet):

```sh
mkdir -p ./static/favicons/

#favicon.ico
convert ./static/logos/logo_large.png -resize 48x48 -unsharp 0x1 ./favicon.ico
#favicon-16x16.png
convert ./static/logos/logo_large.png -resize 16x16 -unsharp 0x1 ./static/favicons/favicon-16x16.png
#favicon-32x32.png
convert ./static/logos/logo_large.png -resize 32x32 -unsharp 0x1 ./static/favicons/favicon-32x32.png
#favicon-128x128.png
convert ./static/logos/logo_large.png -resize 128x128 -unsharp 0x1 ./static/favicons/favicon-128x128.png
#favicon-152x152.png
convert ./static/logos/logo_large.png -resize 152x152 -unsharp 0x1 ./static/favicons/favicon-152x152.png
#favicon-167x167.png
convert ./static/logos/logo_large.png -resize 167x167 -unsharp 0x1 ./static/favicons/favicon-167x167.png
#favicon-180x180.png
convert ./static/logos/logo_large.png -resize 180x180 -unsharp 0x1 ./static/favicons/favicon-180x180.png
#favicon-192x192.png
convert ./static/logos/logo_large.png -resize 192x192 -unsharp 0x1 ./static/favicons/favicon-192x192.png
#favicon-196x196.png
convert ./static/logos/logo_large.png -resize 196x196 -unsharp 0x1 ./static/favicons/favicon-196x196.png
#favicon-256x256.png
convert ./static/logos/logo_large.png -resize 256x256 -unsharp 0x1 ./static/favicons/favicon-256x256.png
```

Spécifiques à un navigateur :

```sh
#mstiles 70x70
convert ./static/logos/logo_large.png -resize 70x70 -unsharp 0x1 ./static/favicons/mstiles-70x70.png
#mstiles 144x144
convert ./static/logos/logo_large.png -resize 144x144 -unsharp 0x1 ./static/favicons/mstiles-144x144.png
#mstiles 310x150
convert ./static/logos/logo_large.png -resize 150x150 -unsharp 0x1  -background transparent -gravity center -extent 310x150 ./static/favicons/mstiles-310x150.png
#mstiles 310x310
convert ./static/logos/logo_large.png -resize 310x310 -unsharp 0x1 ./static/favicons/mstiles-310x310.png
```

### Fichier ieconfig.xml

Je l'ai mis à la racine.

```xml
<?xml version="1.0" encoding="utf-8"?>
    <browserconfig>
      <msapplication>
        <tile>
          <square70x70logo src="favicons/mstiles-70x70.png"/>
          <square150x150logo src="favicons/mstiles-150x150.png"/>
          <wide310x150logo src="favicons/mstiles-310x150.png"/>
          <square310x310logo src="favicons/mstiles-310x310.png"/>
          <TileColor>#FFFFFF</TileColor>
        </tile>
      </msapplication>
    </browserconfig>
```

### Fichier webmanifest

Je l'ai mis à la racine.

```json
{
    "name": "Lobotourisme",
    "short_name": "Lobotourisme",
    "icons": [
        {
            "src": "favicons/favicon-192x192.png",
            "sizes": "192x192",
            "type": "image/png"
        },
        {
            "src": "favicons/favicon-256x256.png",
            "sizes": "256x256",
            "type": "image/png"
        }
    ],
    "theme_color": "#ffffff",
    "background_color": "#ffffff",
    "display": "standalone"
}

```

### Modification du thème

Trouver le _layout_ qui liste les favicons et y ajouter :

```html
  <link rel="icon" href='{{ "favicon/favicon.png" | relURL }}'>
  <link rel="icon" type="image/png" sizes="16x16" href='{{ "favicon/favicon-16x16.png" | relURL }}'>
  <link rel="icon" type="image/png" sizes="32x32" href='{{ "favicon/favicon-32x32.png" | relURL }}'>
  <link rel="icon" type="image/png" sizes="128x128" href='{{ "favicon/favicon-128x128.png" | relURL }}'>
  <link rel="icon" type="image/png" sizes="152x152" href='{{ "favicon/favicon-152x152.png" | relURL }}'>
  <link rel="icon" type="image/png" sizes="167x167" href='{{ "favicon/favicon-167x167.png" | relURL }}'>
  <link rel="icon" type="image/png" sizes="180x180" href='{{ "favicon/favicon-180x180.png" | relURL }}'>
  <link rel="icon" type="image/png" sizes="192x192" href='{{ "favicon/favicon-192x192.png" | relURL }}'>
  <link rel="icon" type="image/png" sizes="196x196" href='{{ "favicon/favicon-196x196.png" | relURL }}'>
  <!--[if IE]><link rel="shortcut icon" href='favicon.ico'><![endif]-->
  <link rel="apple-touch-icon-precomposed" href='{{ "favicon/favicon-180x180.png" | relURL }}'>
  <link rel="shortcut icon" sizes="196x196" href='{{ "favicon/favicon-196x196.png" | relURL }}'>
  <meta name="msapplication-TileColor" content="#FFFFFF">
  <meta name="msapplication-TileImage" content='{{ "favicon/favicon-144x144.png" | relURL }}'>
  <meta name="application-name" content="Lobotourisme">
  <meta name="msapplication-tooltip" content="Tooltip">
  <meta name="msapplication-config" content='{{ "ieconfig.xml" | relURL }}'>
```



