---
title: "Hugo - astuce pour faire remonter les articles modifiés à la surface"
date: "2023-11-02"
categories: 
- Dev
tags: 
- Hugo
- Github pages
---

Je modifie parfois de vieux articles pour y rajouter des infos et je cherchais un moyen simple pour faire remonter ceux-ci sur le site et le flux RSS, sans modifier mon thème :

Il suffit d'ajouter dans les meta-données des articles un attribut **lastmod** avec la date de mise à jour :

```yaml
lastmod: "2023-08-22"
```

Puis de modifier la conf frontmatter du site, dans le _.toml_ ou _.yml_ à la racine, pour prendre cet attribut **lastmod** en priorité sur l'attribut **date** :

```yaml
frontmatter:
  date:
  - lastmod
  - date
```

```toml
[frontmatter]
  date = ['lastmod', 'date']
```

Quelques sources :

- Modifier son _front matter_ : [https://gohugo.io/getting-started/configuration/#configure-front-matter](https://gohugo.io/getting-started/configuration/#configure-front-matter)
- Des articles qui vont bien plus loin dans les modifications, en utilisant la date de mise à jour git, etc. (et modifient leur thème pour y arriver) :
    - [https://makewithhugo.com/add-a-last-edited-date/](https://makewithhugo.com/add-a-last-edited-date/)
    - [https://mertbakir.gitlab.io/hugo/last-modified-date-in-hugo/](https://mertbakir.gitlab.io/hugo/last-modified-date-in-hugo/)
