---
title: "Commentaires sur Hugo + Github Pages"
date: "2021-05-10"
categories: 
- dev
tags: 
- hugo
- github pages
- hugo clarity
---

Veille sur les systèmes de commentaire intégrables à _Hugo_ + _Github Pages_.

La doc _Hugo_ de référence : [https://gohugo.io/content-management/comments/](https://gohugo.io/content-management/comments/)

Discussion sur le forum _Hugo_ : [lternative-to-disqus-needed-more-than-ever](https://discourse.gohugo.io/t/alternative-to-disqus-needed-more-than-ever/5516)

Supports possibles selon la doc + analyse perso (V quand possible avec la solution _Hugo_ + _Github Pages_ + solution gratuite):

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
    - [!] Wordpress 
- X ~~__Graph Comment__~~
    - [!] Payant
- X ~~__Muut__~~
    - [!] Payant
- X ~~__Isso__~~
    - [!] Auto-hébergé (Python)
    - [+] Commentaire invité : Oui
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
    - [!] Payant ou auto-hébergé (Go)
    - [+] Commentaire invité : ?
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
- ? ~~__Twitter__~~
    - Décrit ici :
        - [PÄKSTECH - I'm Switching to Twitter for My Blog Comments](https://pakstech.com/blog/switching-to-twitter-comments/)
        - [How to use Twitter as comment section in your static blog and automatically create a tweet per post](https://theprogress.site/2020-06-30-how-to-use-twitter-as-comment-section-in-your-static-blog/) (Inspiré par le précédent)
    - [!] Stockage des messages : Chez _Twitter_
    - [-] Commentaire invité : Non
    - [+] Pubs : non
    - [?] Intégration ?
- X ~~__Discourse__~~
    - [+] Open source
    - [!] Payant ou auto-hébergé
    - [+] Commentaire invité : Non
- X ~~__Schnack__~~
    - [+] Open source
    - [!] Auto-hébergé (NodeJS + SQLite)
    - [+] très léger
    - [+] Commentaire invité : Non (compte Github ou Twitter)
    - Site : [Say hello to Schnack.js: A new Disqus-like commenting drop-in for static websites](https://www.vis4.net/blog/2017/10/hello-schnack/)
- ? __Reddit__ 
