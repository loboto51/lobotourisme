+++
title = "Déployer un site web statique Hugo sur Github Pages"
date = "2021-04-22"
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

Le site que vous consultez a été construit en suivant la méthode décrite ci-dessous.

La fonctionnalité [_Github Pages_](https://pages.github.com/) de [_Github_](https://github.com/) permet d'exposer à l'adresse _MONUSER.github.io_ un site web statique dont le code (html + css + pas grand chose d'autre) est disponible sur le dépôt du même nom : _MONUSER.github.io_.

[_Hugo_](https://gohugo.io/) est un générateur de sites webs statiques : On écrit des pages en markdown, on lance Hugo, et il génère tout le html et la structure de site que _Github Pages_ exposera.


Mode opératoire :

## _Github_

1. [Créer un compte _Github_](https://github.com/) si ce n'est pas fait (Attention le nom sera dans l'adresse du site).
2. Créer un dépôt ("_New repository_") public au nom de "MONLOGINGITHUB.github.io" (_Github Pages_ est disponible gratuitement pour ce dépôt uniquement)


Créons une copie locale dans laquelle nous ferons la suite :

```sh
mkdir MONLOGINGITHUB.github.io
cd MONLOGINGITHUB.github.io
echo "# MONLOGINGITHUB.github.io" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/MONLOGINGITHUB/MONLOGINGITHUB.github.io.git
git branch -M main
git push -u origin main
```


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
cd MONLOGINGITHUB.github.io
hugo new site .
```

Choisir et télécharger un thème : [Hugo Themes](https://themes.gohugo.io/)


J'utilise [hugo-clarity](https://themes.gohugo.io/hugo-clarity/) qui est très complet et fonctionne bien sur mobile :

```sh
cd MONLOGINGITHUB.github.io/themes/
git clone https://github.com/chipzoller/hugo-clarity.git
```

Enlever les éléments inutiles du thème :

```sh
rm -Rf hugo-clarity/.git*
rm -Rf hugo-clarity/exampleSite
```

J'ai copié l'exemple de site puis 

Pour créer une page de test et vérifier que ça marche :

```sh
hugo new post/testpage.md
hugo server -D
```

Ouvrir :
[http://localhost:1313/](http://localhost:1313/)


**Note :** Le thème _hugo-clarity_ demande pas mal de conf. Plutôt que partir de zéro j'ai suivi le conseil que donne sa doc d'install et déployé le site exemple, que j'ai ensuite modifié : 

```sh
cd MONLOGINGITHUB.github.io
cp -R themes/hugo-clarity/exampleSite/* .
```


## Générer le _html_ statique directement côté _Github_

On peut se contenter de générer le _html_ statique à plat [^1] et le pousser sur _Github_, cela fonctionnera très bien.

Mais _Github_ fournit aussi la possibilité de déclencher des scripts à chaque _git push_, et on peut leur faire exécuter un _Hugo_ :
Cela permet de se contenter de versionner les fichiers sources (le _Hugo_ local sert juste aux tests), et à chaque _git push_, _Github_ génèrera le code statique tout seul.

**Note :** Cette technique et le script ci-dessous sont repris de la doc _Hugo_ : [hosting on github](https://gohugo.io/hosting-and-deployment/hosting-on-github/).

Le script génère le site statique dans une branche _gh-pages_ dédiée, qui servira uniquement à _Github Pages_. Ainsi les sources et le code généré restent bien séparés.

[^1]: En lançant la commande "hugo" seule, le _html_ statique est généré dans un répertoire _public/_, on pourrait pousser ce répertoire tel-quel à _Github_, qui saurait très bien l'exposer.


A faire :

1. Ajouter un fichier de conf _.github/workflows/gh-pages.yml_ que _Github_ exécutera à chaque push :

```sh
cd MONLOGINGITHUB.github.io
mkdir -p .github/workflows/
touch .github/workflows/gh-pages.yml
```

2. Y mettre la conf auto suivante.
Elle déclenche _Hugo_ et lui fait générer les pages statiques dans une branche dédiée _gh-pages_ :

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

## On configure _Github Pages_

Ouvrir le dépôt _github_ dans un navigateur : [https://github.com/MONLOGINGITHUB/MONLOGINGITHUB.github.io](https://github.com/MONLOGINGITHUB/MONLOGINGITHUB.github.io)

Aller dans la conf du dépot : Settings > Pages > Source et choisir :

    [Branch : gh-pages] [/root]

## On envoie tout sur _Github_ et on voit si ça marche

```sh
cd MONLOGINGITHUB.github.io
git commit -a -m "init"
git push
```

En cliquant sur l'onglet _"Action"_ du dépôt on voit le traitement se dérouler (et éventuellement planter si on a fait des bêtises).
La branche _gh-pages_ a dû être créée.


Aller sur le site _Github Pages_ résultant :
[MONLOGINGITHUB.github.io](MONLOGINGITHUB.github.io)

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





