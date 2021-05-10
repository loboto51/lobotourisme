---
title: "Mémo des modifications du thème hugo-clarity"
date: "2021-05-02"
lastmod : "2021-05-10"
categories:
- dev
tags:
- hugo
- hugo clarity
- github pages
- imagemagick
toc: true
---

---

Edit du 07/05/2021 : Ajout de la gestion des icônes. 

Edit du 10/05/2021 : Modification du template _single.html_.

---

_Résumé :_

Ce site est construit en combinant _Hugo_ + _Github Pages_.
J'utilise le thème [hugo-clarity](https://themes.gohugo.io/hugo-clarity/). Ce _post_ récapitule les configurations et modifications que j'y ai apporté.

Le thème _hugo-clarity_ demande pas mal de conf. Plutôt que partir de zéro j'ai suivi le conseil que donne sa doc d'install et je suis parti du _examplesite/_ fourni dans le thème, que j'ai ensuite modifié.

## documents non-utilisés

### Suppression des exemples en portugais

```sh
rm ./config/_default/menus/menu.pt.toml
rm ./content/about.pt.md
```

### Suppression des pages exemples dans _./content_

```sh
rm ./content/archives.md
rm ./content/homepage/about.md
rm ./content/homepage/work.md
rm ./content/post/* # exemples de pages
rm ./static/images/* # images non utilisées
```

## Modification des fichiers de conf

### Modification de _./config/_default/config.toml_

Modifié :

```toml
baseurl = "https://loboto51.github.io/"
title = "Lobotourisme"
author = "Loboto"
paginate = 10
theme = "hugo-clarity"

DefaultContentLanguage = "fr"

[taxonomies]
category = "categories"
tag = "tags"
series = "series"
```

### Modification de _./config/_default/languages.toml_

Modifié :

```toml

[fr]
  title = "Lobotourisme"
  LanguageName = "French"
  weight = 1
```

### Modification de _./config/_default/params.toml_

Très long fichier, principales modifs :

```toml
author = "Loboto"
introDescription = "(...)"
enforceDarkMode = true
dateFormat = "02/01/2006"
since = 2021
footerLogo = "logos/logo.png"

```

### Création d'un _menu.fr.toml_ à partir de _menu.en.toml_

```sh
mv ./config/_default/menus/menu.en.toml ./config/_default/menus/menu.fr.toml
```

Modifié pour le restreindre à 2 blocs :

```toml
[[main]]
  name = "A propos"
  url = "about/"
  weight = -107

# social menu links

[[social]]
  name = "rss"
  weight = 4
  url = "index.xml"
```


## Icones, logo, etc.

### Image originale

J'ai acheté une belle icône sur le très bon site [The Noun Project](https://thenounproject.com/) : [Space dessinée par Adrien Coquet](https://thenounproject.com/term/space/2217279/). 

Je l'ai modifiée dans Inkscape pour ajouter un peu de couleur, puis j'ai généré une version PNG de 1024x1024 "_logo_large.png_" que j'ai mise dans _./static/logos/_.

#### Génération des variantes

J'ai généré avec ImageMagick toutes les variantes présentes dans le template :

```sh
mkdir -p ./static/icons/

#logo.png
convert ./static/logos/logo_large.png -resize 75x75 -unsharp 0x1 ./static/logos/logo_footer.png

#favicon.ico
convert ./static/logos/logo_large.png -resize 48x48 -unsharp 0x1 ./static/icons/favicon.ico
#favicon-16x16.png
convert ./static/logos/logo_large.png -resize 16x16 -unsharp 0x1 ./static/icons/favicon-16x16.png
#favicon-32x32.png
convert ./static/logos/logo_large.png -resize 32x32 -unsharp 0x1 ./static/icons/favicon-32x32.png

#android-chrome-192x192.png
convert ./static/logos/logo_large.png -resize 192x192 -unsharp 0x1 ./static/icons/android-chrome-192x192.png
#android-chrome-256x256.png
convert ./static/logos/logo_large.png -resize 256x256 -unsharp 0x1 ./static/icons/android-chrome-256x256.png
#apple-touch-icon.png
convert ./static/logos/logo_large.png -resize 180x180 -unsharp 0x1 ./static/icons/apple-touch-icon.png
#apple-touch-icon.png
convert ./static/logos/logo_large.png -resize 180x180 -unsharp 0x1 ./static/icons/apple-touch-icon.png

#mstile-150x150.png
convert ./static/logos/logo_large.png -resize 150x150 -unsharp 0x1 -bordercolor transparent -border 60 ./static/icons/mstile-150x150.png
```


ainsi que celles recommandées par [The 2020 Guide to FavIcons for Nearly Everyone and Every Browser](https://www.emergeinteractive.com/insights/detail/the-essentials-of-favicons/)

```sh
#favicon-128x128.png
convert ./static/logos/logo_large.png -resize 128x128 -unsharp 0x1 ./static/icons/favicon-128x128.png

#favicon-152x152.png
convert ./static/logos/logo_large.png -resize 152x152 -unsharp 0x1 ./static/icons/favicon-152x152.png

#favicon-167x167.png
convert ./static/logos/logo_large.png -resize 167x167 -unsharp 0x1 ./static/icons/favicon-167x167.png

#favicon-180x180.png
convert ./static/logos/logo_large.png -resize 180x180 -unsharp 0x1 ./static/icons/favicon-180x180.png

#favicon-192x192.png
convert ./static/logos/logo_large.png -resize 192x192 -unsharp 0x1 ./static/icons/favicon-192x192.png

#favicon-196x196.png
convert ./static/logos/logo_large.png -resize 196x196 -unsharp 0x1 ./static/icons/favicon-196x196.png
```


### webmanifest

Gadget : Cela sert à rendre le site utilisable comme une appli.
Personne ne le fera pour celui-ci, mais bon je l'ai fait pour l'exercice !

J'ai copié l'original :

```sh
cp ./themes/hugo-clarity/static/icons/site.webmanifest ./static/icons/
```

Et je l'ai modifié pour ajouter le nom du site :

```json
    "name": "Lobotourisme",
    "short_name": "Lobotourisme",
```

## Modification des pages principales

### Modification de _./content/about.md_

Il s'agit juste d'une page normale, affichée lorsqu'on clique sur "à propos".
 

### Modification de _./content/_index.md_

```toml
+++
author = "Loboto"
description = "Lobotourisme"
+++
```

## Modification des templates

Point important :
_Hugo_ permet de passer outre n'importe quel élément du thème en mettant une version locale dans l'arborescence du site.

Exemple :
_./layouts/_default/single.html_ sera pris en priorité sur _./themes/hugo-clarity/layouts/_default/single.html_

Cela permet de modifier son site tout en ne touchant pas aux sources du thème. On peut donc toujours se référer à l'original ou revenir en arrière.


### Template des articles _single.html_

---

Edit du 10/05/2021 : Finalement je suis revenu au template initial, et je marque les éditions comme ici, pour pouvoir les contextualiser.

---

```sh
mkdir -p ./layouts/_default
cp ./themes/hugo-clarity/layouts/_default/single.html ./layouts/_default/
```

J'ai ajouté la date de dernière mise à jour si le paramètre _lastmod_ existe :

```html
    {{ if $p.lastmod }}
    <blockquote><p>Dernière mise à jour : <time datetime="{{ $p.lastmod.Format "Mon Jan 10 17:13:38 2020 -0700" }}">{{ $p.lastmod.Format (default "Jan 2, 2006" $.Site.Params.dateFormat) }}</time></p></blockquote>
    {{ end }}
```


### Template de l'entête des billets _post-meta.html_

```sh
mkdir -p ./layouts/partials
cp ./themes/hugo-clarity/layouts/partials/post-meta.html ./layouts/partials/
```

J'ai ajouté les catégories, avec un bouton plus imposant, avant les tags :

```html
(...)
    <span class="post_time"> · {{ T "reading_time" . }}&nbsp;</span>
  {{ end }}
   {{- with .Params.categories -}}
      <span>
        {{- range . }}
          {{- $category := urlize . }} ·
          <a href='{{ absLangURL (printf "categories/%s" $category) }}' title="{{ . }}" class="button button_translucent">
            {{- . }}
          </a>
        {{- end }}
      </span>
   {{- end }}
  {{- with .Params.tags -}}
(...)
```

### Modification de _excerpt.html_ pour afficher les miniatures prises dans les ressources de l'article : En priorité l'image nommée cover.*, puis en second choix la première image du doc 

```sh
mkdir -p ./layouts/partials
cp ./themes/hugo-clarity/layouts/partials/excerpt.html ./layouts/partials/
```

J'ai remplacé toute la section thumbnails par l'algorithme suivant : 

- Si il existe des ressources "image" du billet :
    - On prend la première image nommée cover.* et on l'affiche telle-quelle ;
    - Sinon on prend la première image trouvée, on la redimensionne en 300x300 et on l'affiche ;
- Sinon on n'affiche rien.

```html
(...)
    {{- with .Resources.ByType "image" }}
    <div class="excerpt_footer partition">
        <div class="excerpt_thumbnail">
        {{ with .GetMatch "{cover.*}" }}
                <img src="{{ .Permalink }}" alt="{{ $.Title }}"/>
        {{ else}}
            {{ with .GetMatch "{cover.*,*.jpg,*.png,*.jpeg}" }}
                {{ $cover := .Fit "300x300" }}
                {{ with $cover }}
            <img src="{{ .Permalink }}" alt="{{ $.Title }}"/>
                {{ end }}
            {{ end }}
        {{ end }}
        </div>
    {{ else }}
    <div class="excerpt_footer">
    {{- end }}
(...)
```

