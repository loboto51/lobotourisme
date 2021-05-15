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

Edit du 10/05/2021 : Modification du template _single.html_.

Edit du 14/05/2021 : Depuis la rédaction de ce document, je suis passé au thème [hugo-future-imperfect](https://github.com/statnmap/hugo-future-imperfect) qui présente quelques fonctions supplémentaires et que je trouve plus élégant. 

Edit du 15/05/2021 : Déplacement de la gestion des icônes dans une page dédiée : [Fabrication des favicons en masse avec Imagemagick]({{< ref "/post/Fabrication des favicons en masse avec Imagemagick/index.md">}}). 

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

