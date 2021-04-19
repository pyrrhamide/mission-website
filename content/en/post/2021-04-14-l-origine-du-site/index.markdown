---
title: l'origine du site
author: [admin]
#authors: # if I want to add author under title. might want to change to Kanto Fiaferana tho
#  - admin
date: '2021-04-14'
slug: l-origine-du-site
categories: [R, tutorials]
tags: [R, blogdown, very informal]
links:
- icon: door-open
  icon_pack: fas
  name: project
  url: my-projects/creating-beautiful-content/
- icon: link
  icon_pack: fas
  name: français
  url: fr/post/l-origine-du-site/
summary: 'How to build a website with blogdown, a crash course.'
featured: no
image:
  caption: '[Photo by Danielle MacInnes on Unsplash](https://unsplash.com/photos/IuLgi9PWETU)'
  focal_point: ''
  preview_only: no
---

<!-- old way for TOC
{{< table_of_contents >}}
-->

<!-- new and improved way
{{% toc %}}
-->

Howdy y'all!

Today, as my first official post, I will talk about how I built this website. If I ever feel like going through another round of `blogdown` insanity, I will have this post to help me be efficient quickly, and also to compare if I find better ways to go about things !

I will focus first on getting the site up and running. I will talk about customisation and all that jazz another time.

Alison Hill's blogpost ["Up & running with blogdown in 2021"](https://alison.rbind.io/post/new-year-new-blogdown/) as well as her website [repo](https://github.com/rbind/apreshill) were my reference, for getting started and for customising this site. I copied her fonts file because it was super pretty. The remainder of this post is based (or copied, really) on her words. I also periodically checked [*blogdown: Creating Websites with R Markdown*](https://bookdown.org/yihui/blogdown/) by Yihui Xie, Amber Thomas and Alison Hill, for further details.

<!--- I am awfully distracted by kids running upstairs. They're so noisy. Damn. --->

## What you need

* R and RStudio,
* The `blogdown` package,
* GitHub,
* Netlify.

## Setting up shop

1. Create a new public repo, with a README.md file, but no .gitignore;
2. Clone the repo locally[^1], and create a new Rproject in the repo;
3. Do stuff with `blogdown`;
4. Commit and push the changes to GitHub;
5. Deploy the site on Netlify.
6. FIN.

Not really. Let's go deeper into blogdown.

## `blogdown`

I chose the [Academic](https://academic-demo.netlify.app/) theme as my framework. You can check [Wowchemy](https://wowchemy.com/) for other themes and [Hugo](https://gohugo.io/) too I believe. The Academic theme is simple and super nice to customise (says me AFTER building the dreaded thing. ANYWHO). Once my R project was good and ready to go, I followed Alison's tutorial scrupulously. The following lines of code resume the essential commands to get started:


```r
install.packages("blogdown")
blogdown::new_site(theme = "wowchemy/starter-academic")
blogdown::serve_site()
blogdown::new_post(title = "Hi Hugo",
                     ext = '.Rmarkdown',
                     subdir = "post")
```

`blogdown::serve_site()` launches a live preview of the site in the Viewer pane[^2]. No need to hit refresh, it does it on its own when you save and/or knit. `blogdown::new_post` creates...a new post.

After this command, I changed the default author (`blogdown.author`), default file extension type (`blogdown.ext`) and default new file subdirectory (`blogdown.subdir`) in the `.Rprofile`. Whenever you change your .Rprofile, do not forget to save then `Ctrl+Shift+F10` restart the session!!

Have a look at `config.yaml` and `config/_default/params.yaml` for further identification changes purposes, such as the title or the theme of your website.

Write your post and `Ctrl+Shift+K` knit the Rmarkdown to get the Markdown file[^3]. This is the type of file Hugo (or Netlify, I'm not sure) uses to build your content. Once you are happy with your changes, check the following stuff up[^4].


```r
blogdown::check_config()
blogdown::check_gitignore()
blogdown::check_content()
blogdown::check_netlify()
blogdown::check_hugo()
# You get some [TODO] items. Do them.
```

You can also use the all-in-one command, `blogdown::check_site()`. Once everything is clear, you can go on with publication!

## Publishing the site

1. Netlify;
2. Sign in with GitHub;
3. Choose the repo you want to deploy;
4. Change the subdomain name.

<!--- I'd really like to make an alert note out of this sentence. TO LOOK UP --->
{{% callout warning %}}
Everytime you change the domain name on Netlify, you **must** change the baseurl in the `config.yaml` file.
{{% /callout %}}

You are all set! All in all, I believe it took me a good hour to get the site ready. Netlify will take care of building and deploying all the changes pushed on GitHub. But surely, you do not want to keep the cluttered template and you want to customise it a bit? (me: yeah!) Well, I will cover this topic in another post. Watch this space!

<!-- https://makingwebsiteswithr.rbind.io/tutorial/ -->

## BONUS: new commands and keyboard shortcuts I learned

* `file.create()` to create a file in the current working directory. If I want to create a file in a subfolder, I specify the path, until the name of the new file and its extension. The folders must already exist! If they don't:
* `dir.create()` to create a folder. If I specify the path, I can create a new folder within another folder. It's like Inception!
* `rstudioapi::navigateToFile(path.to.file)` mint.
* `Ctrl+W` to close one tab, `Ctrl+Shift+W` to close all tabs. I haven't found the use for "close all except current" yet.

[^1]:I cloned it with GitHub Desktop after trying to clone it directly through RStudio. I could not commit my changes, an index.lock file kept blocking me, idk why.
[^2]:I can also use my browser by clicking 'Show in new window'.
[^3]:I chose not to set `blogdown.knit.on.save` to `TRUE` because it confuses me. I don't like autosave.
[^4]:TIL you have to type footnotes separately with `blogdown`, as opposed to `bookdown` where I could write my footnotes in the text, and it would numerate them automatically. Huh.
