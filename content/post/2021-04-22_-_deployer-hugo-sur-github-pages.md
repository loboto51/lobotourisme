+++
title = "Déployer un site web statique Hugo sur github Pages"
date = "2021-04-22"
categories = [
    "dev"
]
tags = [
    "hugo",
    "github pages"
]
+++


Préparer dépôt github :
=======================

Pour être disponible via Github Pages le repository github doit être public et créé avec le nom :

    {user github}.github.io


Créer une copie locale :

```sh
mkdir loboto51.github.io
cd loboto51.github.io
echo "# loboto51.github.io" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/loboto51/loboto51.github.io.git
git branch -M main
git push -u origin main
```

Hugo :
======

Package :
---------

Récupérer dernière version (ici ubuntu) :

```sh
wget https://github.com/gohugoio/hugo/releases/download/v0.82.1/hugo_extended_0.82.1_Linux-64bit.deb
sudo dpkg -i hugo_extended_0.82.1_Linux-64bit.deb
```

si erreurs de dépendances :

```sh
sudo apt-get install -f
```

Créer un socle de site :
-----------------------

```sh
cd loboto51.github.io
hugo new site .
```

Installer un thème (ici [hugo-clarity](https://themes.gohugo.io/hugo-clarity/)):

```sh
cd loboto51.github.io/themes
git clone https://github.com/chipzoller/hugo-clarity.git
```

Enlever les éléments inutiles du theme :

```sh
rm -Rf hugo-clarity/.git*
rm -Rf hugo-clarity/exampleSite
```

Pour créer une page de test (sinon n'importe que .md dans le rep post/ fait l'affaire) :

```sh
hugo new post/testpage.md
```


Vérification que ça marche :
----------------------------

Lancer serveur Hugo en local pour tester :

```sh
hugo server -D
```

ouvrir :
[http://localhost:1313/](http://localhost:1313/)

Si tout se passe bien on peut poursuivre.


Génération automatique des pages sur le dépôt :
===============================================

Ajouter un fichier de conf au dépôt pour générer automatiquement les pages Hugo à chaque push ([doc Hugo : hosting on github](https://gohugo.io/hosting-and-deployment/hosting-on-github/)) :

```sh
cd loboto51.github.io
mkdir -p .github/workflows/
touch .github/workflows/gh-pages.yml
```

Y mettre la conf auto suivante.
Elle génère les pages statiques Hugo dans une branche gh-pages :

```yml
name: github pages

on:
  push:
    branches:
      - main  # Set a branch to deploy

jobs:
  deploy:
    runs-on: ubuntu-18.04
    steps:
      - uses: actions/checkout@v2
        with:
          submodules: true  # Fetch Hugo themes (true OR recursive)
          fetch-depth: 0    # Fetch all history for .GitInfo and .Lastmod

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: 'latest'
          # extended: true

      - name: Build
        run: hugo --minify

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
```

Push et dernières confs sur github :
====================================

```sh
cd loboto51.github.io
git commit -a -m "init"
git push
```

Se connecter au dépôt git, dans "Action" on voit le traitement se dérouler (et éventuellement planter).
Aller dans la conf du dépot : Pages > Source
Choisir :

    [Branch : gh-pages] [/root]

Aller sur le site :
[loboto51.github.io](loboto51.github.io)
En théorie tout marche.


Liens utiles :
==============

Hugo sur github :
- [Create a Free Blog Site Using GitHub Pages and Hugo](https://youngkin.github.io/post/createafreeblogsite/)
- [doc Hugo : hosting on github](https://gohugo.io/hosting-and-deployment/hosting-on-github/)

Markdown :
- [Quick Markdown Example - page de référence bourrée d'exemple](http://www.unexpected-vortices.com/sw/rippledoc/quick-markdown-example.html)





