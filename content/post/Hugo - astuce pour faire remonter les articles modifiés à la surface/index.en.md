---
title: "Hugo - tip for bringing modified articles to the surface"
date: "2023-11-02"
categories: 
- Dev
tags: 
- Hugo
- Github pages
---

_(This post was automatically translated with [www.DeepL.com/Translator](http://www.DeepL.com/Translator))_

I sometimes modify old articles to add new information and I was looking for a simple way to make them appear on the site and the RSS feed, without modifying my theme:

All you have to do is add a **lastmod** attribute with the update date to the article metadata:

``yaml
lastmod: "2023-08-22
```

Then modify the site's frontmatter conf, in the _.toml_ or _.yml_ at the root, to give this **lastmod** attribute priority over the **date** attribute:

yaml
frontmatter:
  date:
  - lastmod
  - date
```

```toml
[frontmatter]
  date = ['lastmod', 'date']
```

Some sources:

- Modifying your _front matter_: [https://gohugo.io/getting-started/configuration/#configure-front-matter](https://gohugo.io/getting-started/configuration/#configure-front-matter)
- Articles that go much further in their modifications, using the git update date, etc. (and modify their theme to do so):
    - [https://makewithhugo.com/add-a-last-edited-date/](https://makewithhugo.com/add-a-last-edited-date/)
    - [https://mertbakir.gitlab.io/hugo/last-modified-date-in-hugo/](https://mertbakir.gitlab.io/hugo/last-modified-date-in-hugo/)