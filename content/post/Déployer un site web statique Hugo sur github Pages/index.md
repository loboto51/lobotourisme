---
title: "Déployer un site web statique Hugo sur Github Pages"
date: "2021-04-22"
lastmod: "2021-05-05"
description: "Réalisation d'un site web statique avec Github Pages et Hugo."
categories: 
- dev
tags: 
- hugo
- github pages
- hugo clarity
toc: true
---

Le site que vous consultez est constitué d'un site web statique _Hugo_ hébergé sur _Github_ :

[_Github_](https://github.com/) fournit la possibilité d'exposer un site _web_ statique pour chaque dépôt public du compte. Il suffit d'avoir :

- Du _html+css_ dans le dépôt ;
- La fonctionnalité [_Github Pages_](https://pages.github.com/) activée.

Le code statique de chaque dépôt sera alors accessible à l'url :
_https://MONLOGINGITHUB.github.io/MONDEPOT/_

[_Hugo_](https://gohugo.io/) quand à lui est un générateur de sites _web_ statiques : On écrit des pages en markdown, on lance Hugo, et il génère tout le html et la structure de site que _Github_ exposera.


Mode opératoire :


## _Hugo_


### Installation


Sous _Ubuntu_ la version dispo dans les dépôts est dépassée. Le plus simple est d'aller récupérer le dernier .deb dans les _releases_ _Hugo_ :

[https://github.com/gohugoio/hugo/releases](https://github.com/gohugoio/hugo/releases) 

Ensuite :

```sh
wget https://github.com/gohugoio/hugo/releases/download/v0.82.1/hugo_extended_0.82.1_Linux-64bit.deb
sudo dpkg -i hugo_extended_0.82.1_Linux-64bit.deb
```

si erreurs de dépendances :

```sh
sudo apt-get install -f
```


### Créer un socle de site


```sh
mkdir MONSITE
cd MONSITE
hugo new site .
```

Choisir et télécharger un thème : [Hugo Themes](https://themes.gohugo.io/)


J'utilise [hugo-clarity](https://themes.gohugo.io/hugo-clarity/) qui est très complet et fonctionne bien sur mobile :

```sh
cd MONSITE/themes/
git checkout https://github.com/chipzoller/hugo-clarity.git
```

Enlever le suivi git du thème :

```sh
rm -Rf hugo-clarity/.git*
```

Pour créer une page de test et vérifier que ça marche :

```sh
hugo new post/testpage.md
hugo server -D
```

Ouvrir :
[http://localhost:1313/](http://localhost:1313/)


**Note :** Le thème _hugo-clarity_ demande pas mal de conf. Plutôt que partir de zéro j'ai suivi le conseil que donne sa doc d'install et déployé le site exemple, que j'ai ensuite modifié : 

```sh
cd MONSITE
cp -R themes/hugo-clarity/exampleSite/* .
hugo server -D
```

## _Github_

### Créer le dépôt

1. [Créer un compte _Github_](https://github.com/) "_MONLOGINGITHUB_" si ce n'est pas fait ;
2. Créer un dépôt ("_New repository_") public qui contiendra nos pages.

*Astuce _Github_ :*

Par défaut les dépôts sont exposés dans https://MONLOGINGITHUB.github.io/NOMDUDEPOT/.

Mais si vous nommez votre dépôt "_MONLOGINGITHUB.github.io_", il sera accessible à la racine, dans https://MONLOGINGITHUB.github.io/.

Si on le souhaite, cela permet d'avoir plusieurs dépôts "projet" dans les sous-repertoires et une "page perso" à la racine :

```
Dépots                          Sites correspondants
------                          --------------------
dayofthetentacle.github.io      https//dayofthetentacle.github.io/
├── ifeel                       ├──  https//dayofthetentacle.github.io/ifeel/
├── likeicould                  ├──  https//dayofthetentacle.github.io/likeicould/
├── takeovertheworld            ├──  https//dayofthetentacle.github.io/takeovertheworld/
```

Si vous créez un compte _Github_ uniquement pour héberger votre site, vous pouvez donc vous contenter du dépôt _MONLOGINGITHUB.github.io_.


### Premier commit pour pouvoir activer _Github Pages_

Mettons notre site _Hugo_ sur le dépôt MONSITE :

```sh
cd MONSITE
echo "# MONSITE" >> README.md
git init
git add .
git commit -m "first commit"
git remote add origin https://github.com/MONLOGINGITHUB/MONSITE.git
git branch -M main
git push -u origin main
```

Aller dans _Settings > Pages_ et activer _Pages_ avec comme source :

    [Branch : gh-pages] [/root]


### Générer le _html_ statique directement côté _Github_

**Note :** Cette technique et le script ci-dessous sont repris de la doc _Hugo_ : [hosting on github](https://gohugo.io/hosting-and-deployment/hosting-on-github/).

On pourrait générer le _html_ statique et le pousser sur _Github_, mais on peut aussi le faire faire par _Github_ :

En ajoutant un script d'action à nos sources, à chaque _git push_ on va lui faire :

- Exécuter un _Hugo_ pour générer le html ;
- Ecrire le répertoire _public/_ généré dans une branche dédiée _gh-branche_ que viendra lire _Github Pages_.

Créer le fichier de conf :

```sh
cd MONSITE
mkdir -p .github/workflows/
touch .github/workflows/gh-pages.yml
```

2 - Y mettre la conf (j'ai activé l'option "_extended = true_" par rapport au modèle, pour les besoins du thème Clarity) :

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
          extended: true

      - name: Build
        run: hugo --minify

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
```

### On envoie tout sur _Github_ et normalement ça marche

```sh
cd MONSITE
git add .
git commit -a -m "init"
git push
```

Dans l'onglet _"Action"_ du dépôt on voit le traitement se dérouler (et éventuellement planter si on a fait des bêtises).
La branche _gh-pages_ a dû être créée.

Aller sur le site _Github Pages_ résultant :
[MONLOGINGITHUB.github.io/MONSITE](MONLOGINGITHUB.github.io/MONSITE)

En théorie tout y est.


## Annexes

### Mise en place des commentaires sur les articles

La doc _Hugo_ de référence : [https://gohugo.io/content-management/comments/](https://gohugo.io/content-management/comments/)

Supports possibles selon la doc + analyse perso  (V quand possible avec la solution _Hugo_ + _Github Pages_):

- V __Disqus__
    - [-] Stockage des messages : Chez _Disqus_
    - [+] Commentaire invité
    - [-] Pubs : oui
    - [-] Trackers : oui ( beaucoup)
    - Lectures :
        - Vie privée, etc. : [Gazoo.vrv - Replacing Disqus with Github Comments](http://donw.io/post/github-comments/)
        - Doc d'install : [https://gohugo.io/content-management/comments/](https://gohugo.io/content-management/comments/)
- V __Staticman__
    - [+] Open source
    - [+] Stockage des messages : _Github issues_
    - [-] Commentaire invité : non (compte _Github_)
    - Comparaison avec _Utterances_ : [BARTEKR - testing-utterances-comments](https://blog.bartekr.net/2021/01/17/testing-utterances-comments/)
- X ~~__Talkyard__~~
    - [!] Auto-hébergé
- X ~~__IntenseDebate__~~
    - [!] Wordpress 
- X ~~__Graph Comment__~~
    - [!] Payant
- X ~~__Muut__~~
    - [!] Payant
- X ~~__Isso__~~
    - [!] Auto-hébergé
- V __Utterances__
    - [+] Open source
    - [+] Stockage des messages : _Github issues_
    - [-] Commentaire invité : non (compte _Github_)
    - [+] Pubs : non
    - Comparaison avec _Staticman_ : [BARTEKR - testing-utterances-comments](https://blog.bartekr.net/2021/01/17/testing-utterances-comments/)
    - Doc d'install ici :
- X ~~__Remark__~~
    - [!] Auto-hébergé
- X ~~__Commento__~~
    - [!] Payant
- V __Hyvor Talk__
    - [!] Stockage des messages : Chez _Hyvor_
    - [+] Commentaire invité : Oui
    - [+] Pubs : non
    - Docs d'install : 
        - [Create a Free Blog Site Using GitHub Pages and Hugo#comments](https://youngkin.github.io/post/createafreeblogsite/#add-support-for-comments)
        - [https://talk.hyvor.com/](https://talk.hyvor.com/)

Autres pistes :

- ? __Github Issue API__
    - Décrit ici :
        - [Gazoo.vrv - Replacing Disqus with Github Comments](http://donw.io/post/github-comments/)
- ? __Twitter__
    - Décrit ici :
        - [PÄKSTECH - I'm Switching to Twitter for My Blog Comments](https://pakstech.com/blog/switching-to-twitter-comments/)
        - [How to use Twitter as comment section in your static blog and automatically create a tweet per post](https://theprogress.site/2020-06-30-how-to-use-twitter-as-comment-section-in-your-static-blog/) (Inspiré par le précédent)
    - [!] Stockage des messages : Chez _Twitter_
    - [-] Commentaire invité : Non
    - [+] Pubs : non
    - [?] Intégration pas évidente
    - Docs d'install : 


## Liens utiles

Docs sur Hugo sur github :

- [Create a Free Blog Site Using GitHub Pages and Hugo](https://youngkin.github.io/post/createafreeblogsite/)
- [doc Hugo : hosting on github](https://gohugo.io/hosting-and-deployment/hosting-on-github/)

- 3 manières possibles d'organiser ses contenus _Hugo_ : [discourse.gohugo.io - content-organization-best-practice](https://discourse.gohugo.io/t/discussion-content-organization-best-practice/6360/9) (j'ai opté pour l'organisation B : Un répertoire par publication contenant le .md et les pièces jointes. Cela évite les fausses manips, et sera plus pratique si un jour il y en a beaucoup)

Références Markdown :

- [Quick Markdown Example - page de référence bourrée d'exemple](http://www.unexpected-vortices.com/sw/rippledoc/quick-markdown-example.html)





