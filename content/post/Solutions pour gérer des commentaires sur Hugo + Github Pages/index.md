---
title: "Solutions pour gérer des commentaires sur Hugo + Github Pages"
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

Supports possibles selon la doc + analyse perso (en gras ceux qui me semblent intéressants) :

- Disqus
    - [-]Hébergement :
        - [-] Messages stockés chez _Disqus_
        - [-] Gratuit : 
            - [-] **beaucoup de pubs et de trackers** (voir plus bas)
        - [-] Payant :
            - [-] Encore pas mal de trackers apparemment (voir plus bas)
    - [+] Commentaire invité : Oui
    - Décrit ici :
        - Vie privée, etc. : [Gazoo.vrv - Replacing Disqus with Github Comments](http://donw.io/post/github-comments/)
        - Doc d'install : [https://gohugo.io/content-management/comments/](https://gohugo.io/content-management/comments/)
- __Staticman__
    - [+] Hébergement :
        - [+] Gratuit : Messages stockés chez _Github_ (_Github issues_)
    - [+] Open source
    - [-] Commentaire invité : Non (compte _Github_)
    - Décrit ici :
        - Comparaison avec _Utterances_ : [BARTEKR - testing-utterances-comments](https://blog.bartekr.net/2021/01/17/testing-utterances-comments/)
- __Talkyard__
    - [/]Hébergement :
        - [/] Gratuit : Auto-hébergé
        - [/] Payant (2 euros par mois) : Messages stockés chez _Talkyard_
    - [+] Commentaire invité : Oui
    - [+] Open source
    - Décrit ici :
        - [Talkyard (site officiel)](https://www.talkyard.io/)
- IntenseDebate
    - [-] Manque d'infos, interface un peu vieillotte
    - Décrit ici :
        - [IntenseDebate (site officiel)](https://www.intensedebate.com/home)
- Graph Comment
    - [/]Hébergement :
        - [-] Messages stockés chez _Graph Comment_
        - [+] Gratuit jusqu'à 1 million de vues/mois
    - [+] Commentaire invité : Oui
    - Décrit ici :
        - [GraphComment (site officiel)](https://graphcomment.com/)
- Muut
    - [-]Hébergement :
        - [-] Messages stockés chez _Muut_ 
        - [-] Payant et tarif élevé : 16$/mois
    - Décrit ici :
        - [Muut (site officiel)](https://muut.com/)
- __Isso__
    - [/]Hébergement :
        - [/] Gratuit : Auto-hébergé (Python + SQLite)
    - [/] Commentaire invité : Oui (mais pas de double authent à priori)
    - [+] Multilingue
    - Décrit ici :
        - [Isso (site officiel)](https://posativ.org/isso/)
- __Utterances__
    - [+] Hébergement :
        - [+] Gratuit : Messages stockés chez _Github_ (_Github issues_)
    - [-] Commentaire invité : Non (compte _Github_)
    - [+] Open source
    - [-] Seulement en anglais
    - Décrit ici :
        - [Utterances (site officiel)](https://utteranc.es/)
        - [BARTEKR - testing-utterances-comments](https://blog.bartekr.net/2021/01/17/testing-utterances-comments/)
- __Remark__
    - [/]Hébergement :
        - [/] Gratuit : Auto-hébergé
    - [/] Commentaire invité : Non (mais beaucoup de méthodes : _Google_, _Twitter_, _Facebook_, _Microsoft_, _GitHub_ ou _Yandex_)
    - [+] Open source
    - [-] Multilingue, mais pas de français
    - Décrit ici :
        - [Github - remark42 (site officiel)](https://github.com/umputun/remark42)
- Commento
    - [-]Hébergement :
        - [-] Payant (**10$/mois**) : Messages stockés chez __Commento_
    - Décrit ici :
        - [Commento (site officiel)](https://commento.io/)
- Hyvor Talk
    - [-]Hébergement :
        - [-] Payant (**5$/mois**) : Messages stockés chez _Hyvor_
    - [+] Commentaire invité : Oui
    - Décrit ici :
        - [Create a Free Blog Site Using GitHub Pages and Hugo#comments](https://youngkin.github.io/post/createafreeblogsite/#add-support-for-comments)
        - [https://talk.hyvor.com/](https://talk.hyvor.com/)

Autres pistes :

- Twitter
    - [/] Hébergement :
        - [/] Gratuit : Messages stockés chez _Twitter_
    - [-] Commentaire invité : non (compte _Twitter_)
    - Décrit ici :
        - [PÄKSTECH - I'm Switching to Twitter for My Blog Comments](https://pakstech.com/blog/switching-to-twitter-comments/)
        - [How to use Twitter as comment section in your static blog and automatically create a tweet per post](https://theprogress.site/2020-06-30-how-to-use-twitter-as-comment-section-in-your-static-blog/) (Inspiré par le précédent)
- __Mastodon__
    - [+] Hébergement :
        - [+] Gratuit : Messages stockés chez _Mastodon_
    - [-] Commentaire invité : Non (compte _Mastodon_)
    - [+] Open source
    - Décrit ici :
        - [Using Mastodon for comments on a static blog](https://lottalinuxlinks.com/using-mastodon-for-comments-on-a-static-blog/)
        - [Comtodon - A minimal commenting system for static blogs using external Mastodon or API compatible server](https://git.wadza.fr/me/comtodon)
        - [mastodon-api-comments - Shortcode for Hugo to display the replies to a Fediverse-thread as blog comments.](https://schlomp.space/tastytea/hugo-mastodon-api-comments)
- (Autres) Github Issue API
    - [+] Hébergement :
        - [+] Gratuit : Messages stockés chez _Github_ (_Github issues_)
    - [-] Commentaire invité : non (compte _Github_)
    - Décrit ici :
        - [Gazoo.vrv - Replacing Disqus with Github Comments](http://donw.io/post/github-comments/)
- Discourse
    - [/]Hébergement :
        - [/] Gratuit : Auto-hébergé
        - [/] Payant (**100$/mois**) : Messages stockés chez _Discourse_
    - [-] Commentaire invité : Non
    - [+] Open source
- __Schnack__
    - [/]Hébergement :
        - [/] Gratuit : Auto-hébergé (NodeJS + SQLite)
    - [-] Commentaire invité : Non (compte _Github_ ou _Twitter_)
    - [+] Open source
    - [+] très léger
    - Décrit ici :
        - [Say hello to Schnack.js: A new Disqus-like commenting drop-in for static websites](https://www.vis4.net/blog/2017/10/hello-schnack/)





