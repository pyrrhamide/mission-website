---
title: l'origine du site
author: KF
date: '2021-04-14'
slug: l-origine-du-site
categories: [R, tutorials]
tags: [R, blogdown, very informal]
summary: 'How to build a website with blogdown, a crash course.'
lastmod: '2021-04-14T17:55:32+02:00'
featured: no
image:
  caption: ''
  focal_point: ''
  preview_only: no
---

Howdy y'all!

Today (14th of April 2021), as my first official post, I will talk about how I built this website. If I ever feel like going through another round of `blogdown` insanity, I will have this post to help me (and maybe you the reader, if there is any) be efficient quickly, and also to compare if I find better ways to go about things !

I will focus first on getting the site up and running. I will talk about customisation and all that jazz another time.

Allison Hill's blogpost ["Up & running with blogdown in 2021"](https://alison.rbind.io/post/new-year-new-blogdown/) as well as her website [repo](https://github.com/rbind/apreshill) were my reference. I copied her fonts file because it was super pretty. The remainder of this post is her. That's all.

<!--- I am awfully distracted by kids running upstairs. They're so noisy. Damn. --->

## What you need

* R and RStudio,
* the `blogdown` package,
* GitHub,
* Netlify.

## Setting up shop

1. Create a new public repo, with a README.md file, but no .gitignore;
2. Clone the repo on my laptop[^1], and create a new Rproject in the repo;
3. Do stuff with `blogdown`;
4. Commit and push the changes to GH;
5. Deploy the site on Netlify.
6. FIN.

But not really. Let's go deeper into the soup.

## `blogdown`

I chose to go with the [Academic](https://academic-demo.netlify.app/) theme. It's simple and super nice to customise (says me only AFTER building the dreaded thing. ANYWHO). Once my R project was good and ready to go, I followed Alison's tutorial scrupulously. It can be resumed with the following lines of code:


```r
install.packages("blogdown")
blogdown::new_site(theme = "wowchemy/starter-academic")
blogdown::serve_site() 
blogdown::new_post(title = "Hi Hugo", 
                     ext = '.Rmarkdown', 
                     subdir = "post")
```

`blogdown::serve_site()` launches a live preview of the site in the Viewer pane[^2]. No need to hit refresh, it does it on its own. `blogdown::new_post` creates...a new post. After this command, I changed the author (`blogdown.author`), default file extension type (`blogdown.ext`) and default new file subdirectory (`blogdown.subdir`) in the `.Rprofile`. Save then `Ctrl+Shift+F10` restart the session!! 

Write your post and `Ctrl+Shift+K` knit the thing to get the html file[^3]. Once you're happy with your changes, check stuff up[^4].


```r
blogdown::check_gitignore()
blogdown::check_content()
blogdown::check_netlify()
blogdown::check_hugo()
# You get some [TODO] items. Do them.
```

## Publishing the site

* Netlify;
* Sign in with GitHub;
* Choose the repo you want to deploy;
* Change the subdomain name.

<!--- I'd really like to make an alert note out of this sentence. TO LOOK UP --->
Everytime you change the domain name on Netlify, you **must** change the baseurl in the `config.yaml` file.

You're all set! All in all, I believe it took me 3 hours to be operational. Netlify will take care of building and deploying all the changes you'll push on GitHub. But surely, you don't want to keep the clutered template and you want to customise it a bit? (me: yeah!) Well, I'll cover this topic in another post. *Aufwiedersehen*.

## BONUS: new commands and keyboard shortchuts I learned

* `file.create()` to create a file in the current working directory. If I want to create a file in a subfolder, I specify the path, until the name of the new file and its extension. The folders must already exist! If they don't:
* `dir.create()` to create a folder. If I specify the path, I can create a new folder in another folder. It's like Inception!
* `rstudioapi::navigateToFile(path.to.file)` mint.
* `Ctrl+W` to close one tab, `Ctrl+Shift+W` to close all tabs. I haven't found the use for "close all except current" yet.

[^1]:I cloned it with GitHub Desktop after trying to clone it directly through RStudio. I could not commit my changes, an index.lock file kept blocking me, idk.
[^2]:I can also use my browser by clicking 'Show in new window'.
[^3]:I chose not to set `blogdown.knit.on.save` to `TRUE` because it confuses me. I don't like autosave. That's all.
[^4]:TIL you have to type footnotes separately with `blogdown`, as opposed to `bookdown` where I could write my footnotes in the text, and it would numerate them automatically. Huh.
