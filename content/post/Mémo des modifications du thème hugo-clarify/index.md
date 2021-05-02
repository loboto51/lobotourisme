+++
title = "Mémo des modifications du thème hugo-clarify"
date = "2021-05-02"
categories = [
    "dev"
]
tags = [
    "hugo",
    "github pages"
]
toc = true
+++

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

enableGitInfo = true

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
dateFormat = "2006-01-02"
since = 2021

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

### Template des articles _single.html_

Dans _themes/hugo-clarity/layouts/_default/single.html_, ajout de la date de dernière mise à jour :

```html
     {{ if $.GitInfo }}
     <blockquote><p>Dernière mise à jour : <time datetime="{{ .GitInfo.AuthorDate.Format "Mon Jan 10 17:13:38 2020 -0700" }}" class="text-muted">{{ .GitInfo.AuthorDate.Format (default "Jan 2, 2006" $.Site.Params.dateFormat) }}</time></p></blockquote>
     {{ end }}
```


### Template du logo _logo.html_

Dans _themes/hugo-clarity/layouts/partials/logo.html_, ajout du titre de site en dur avec ses 2 emojis :

```html
(...)
  {{- with .logo }}
  <img src="{{ absURL . }}" class="logo" alt="{{ $t }}">
  {{- else -}}
   <h1>Lobotourisme &#x2687; &#9992;</h1>
  {{- end }}
(...)
</a>
```






