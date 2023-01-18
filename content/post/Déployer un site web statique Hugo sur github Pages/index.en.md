---
title: "Deploying a static Hugo website on Github Pages"
date: "2021-04-22"
categories: 
- Dev
tags: 
- Hugo
- Github pages
- Hugo clarity
---

_(This post was automatically translated with [www.DeepL.com/Translator](http://www.DeepL.com/Translator))_

The site you are viewing consists of a static _Hugo_ website hosted on _Github_ :

[_Github_](https://github.com/) provides the ability to expose a static _web_ site for each public repository in the account. All that is required is:

- _html+css_ in the repository;
- The [_Github Pages_](https://pages.github.com/) feature enabled.

The static code of each repository will then be accessible at the url:
_https://MONLOGINGITHUB.github.io/MONDEPOT/_

[_Hugo_](https://gohugo.io/) is a generator of static _web_ sites : You write pages in markdown, you launch Hugo, and it generates all the html and the structure of site that _Github_ will expose.

<!--more-->

> _Edit of 19/02/2022 : I switched to the [Anubis](https://github.com/mitrichius/hugo-theme-anubis) theme which is lighter and cleaner._  
> _Edit of 06/02/2022 : Updated the action script with the latest version available on [Hugo - hosting on github](https://gohugo.io/hosting-and-deployment/hosting-on-github/)._


How it works:


## _Hugo_


### Installation


Under _Ubuntu_ the version available in the repositories is outdated. The easiest way is to get the last .deb in the _releases_ _Hugo_ :

[https://github.com/gohugoio/hugo/releases](https://github.com/gohugoio/hugo/releases) 

Then :

```sh
wget https://github.com/gohugoio/hugo/releases/download/v0.82.1/hugo_extended_0.82.1_Linux-64bit.deb
sudo dpkg -i hugo_extended_0.82.1_Linux-64bit.deb
```

if dependency errors :

```sh
sudo apt-get install -f
```


### Create a site base


```sh
mkdir MYSITE
cd MYSITE
hugo new site .
```

Choose and download a theme : [Hugo Themes](https://themes.gohugo.io/)


I use [hugo-clarity](https://themes.gohugo.io/hugo-clarity/) which is very complete and works well on mobile:

```sh
cd MONSITE/
git submodule add https://github.com/chipzoller/hugo-clarity.git themes/hugo-clarity
```

To create a test page and check that it works:

```sh
hugo new post/testpage.md
hugo server -D
```

Open:
[http://localhost:1313/](http://localhost:1313/)


**Note:** The _hugo-clarity_ theme requires a lot of configuration. Rather than starting from scratch I followed the advice given in its install doc and deployed the sample site, which I then modified: 

```sh
cd MYSITE
cp -R themes/hugo-clarity/exampleSite/* .
hugo server -D
```

## _Github_

### Create the repository

1. [Create a _Github_ account](https://github.com/) "_MONLOGINGITHUB_" if not already done;
2. Create a public repository ("_New repository_") that will contain our pages.

*_Github_ Tip:*

By default repositories are exposed in https://MONLOGINGITHUB.github.io/NOMDUDEPOT/.

But if you name your repository "_MONLOGINGITHUB.github.io_", it will be accessible at the root, in https://MONLOGINGITHUB.github.io/.

If you want, this allows you to have several "project" repositories in the subdirectories and a "personal page" at the root:



```
Repositories                    Corresponding sites
------------                    -------------------
dayofthetentacle.github.io      https//dayofthetentacle.github.io/
├── ifeel                       ├──  https//dayofthetentacle.github.io/ifeel/
├── likeicould                  ├──  https//dayofthetentacle.github.io/likeicould/
├── takeovertheworld            ├──  https//dayofthetentacle.github.io/takeovertheworld/
```

If you are creating a _Github_ account just to host your site, then you can just use the _MONLOGINGITHUB.github.io_ repository.


### First commit to activate _Github Pages_

Let's put our site _Hugo_ on the MONSITE repository:

```sh
cd MONSITE
echo "# MONSITE" >> README.md
git init
git add .
git commit -m "first commit
git remote add origin https://github.com/MONLOGINGITHUB/MONSITE.git
git branch -M main
git push -u origin main
```

Go to _Settings > Pages_ and activate _Pages_ with the following source:

    [Branch: gh-pages] [/root]


### Generate the static _html_ directly on the _Github_ side

**Note:** This technique and the script below are taken from the _Hugo_ doc: [hosting on github](https://gohugo.io/hosting-and-deployment/hosting-on-github/).

We could generate the static _html_ and push it to _Github_, but we can also have it done by _Github_ :

By adding an action script to our sources, at each _git push_ we will make it:

- Run a _Hugo_ to generate the html ;
- Write the generated _public/_ directory in a dedicated _gh-branch_ that _Github Pages_ will read.

Create the conf file :

```sh
cd MONSITE
mkdir -p .github/workflows/
touch .github/workflows/gh-pages.yml
```

2 - Put in the conf (I have enabled the "_extended = true_" option with respect to the template, for the purposes of the Clarity theme):

```yml
name: github pages

on:
  push:
    branches:
      - main # Set a branch to deploy
  pull_request:

jobs:
  deploy:
    runs-on: ubuntu-20.04
    steps:
      - uses: actions/checkout@v2
        with:
          submodules: true # Fetch Hugo themes (true OR recursive)
          fetch-depth: 0 # Fetch all history for .GitInfo and .Lastmod

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: 'latest'
          extended: true

      - name: Build
        run: hugo --minify

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        if: github.ref == 'refs/heads/main'
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
```

### We send everything to _Github_ and normally it works

```sh
cd MONSITE
git add .
git commit -a -m "init
git push
```

In the _"Action"_ tab of the repository we can see the processing going on (and possibly crashing if we did something wrong).
The _gh-pages_ branch must have been created.

Go to the resulting _Github Pages_ site:
[MONLOGINGITHUB.github.io/MONSITE](MONLOGINGITHUB.github.io/MONSITE)

In theory everything is there.




## Useful links

Docs about Hugo on github :

- Create a Free Blog Site Using GitHub Pages and Hugo](https://youngkin.github.io/post/createafreeblogsite/)
- Hugo doc : hosting on github](https://gohugo.io/hosting-and-deployment/hosting-on-github/)

- 3 possible ways to organize your content _Hugo_ : [discourse.gohugo.io - content-organization-best-practice](https://discourse.gohugo.io/t/discussion-content-organization-best-practice/6360/9) (I chose organization B : One directory per publication containing the .md and the attachments. This avoids false manipulations, and will be more convenient if one day there are many)

Markdown references:

- [Quick Markdown Example - reference page stuffed with examples](http://www.unexpected-vortices.com/sw/rippledoc/quick-markdown-example.html)

