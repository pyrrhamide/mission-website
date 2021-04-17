---
title: an instagramable site
author: KF
date: '2021-04-15'
slug: an-instagramable-site
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
  url: fr/post/decorer-le-site/
summary: 'How to make your new website more to your taste, a crash course.'
featured: no
image:
  caption: '[Photo by Joanna Kosinka on Unsplash](https://unsplash.com/photos/1_CMoFsPfso)'
  focal_point: ''
  preview_only: no
---

{{< table_of_contents >}}

[Last time](/post/l-origine-du-site/), I explained how to quickly set up a new website with `blogdown`. Now, I will talk about what **I** did in terms of customisation. Once again, I heavily relied on Alison Hill's [blogpost](https://alison.rbind.io/post/new-year-new-blogdown/) and [repo](https://github.com/rbind/apreshill), as well as Wowchemy's [documentation](https://wowchemy.com/docs/).

Wowchemy's Academic template comes with a lot of example content to get inspired from. However, you might want to make it more personalised and streamlined. I know I did! I went ahead and tried to make it "my own", although I practically copied Alison's theme and fonts (they're so nice, I really like them).

I did not follow the next steps linearly, but I wish I did. I will definitely do so next time. Without further ado, let me take you through a sculpting journey...

## Widget - `activate: false`

The homepage is made up of widgets, from a presentation widget to a word cloud one. I personally found the Academic homepage horrendous: it was quite overwhelming and there were a few widgets that I did not need.

Determine the ones you do not want. Once you are sure, go to the `content/home/` folder, click on each undesired `.md` files and write down `activate: false` in the metadata[^1]. This is the first step towards clearing things up![^2]

[^1]: I went a bit faster there and initially deleted the widgets I wanted gone. If I ever need them back, I can find them in the `themes/starter-academic/` folder.
[^2]:I kept the following widgets active: `about`, `experience`, `index`, `posts`, `projects` and `skills`. To this day, I have not published anything and have no achievement I would like to boast about, therefore I deactivated everything else!

## Your info

Up next, you want to customise everything by adding your own information details. First, let's modify the default author. Open the file `content/authors/admin/_index.md`, and edit the metadata. Under the YAML, you can write a snippet of your bio. You can also change the icon by uploading an image named `avatar.png` or `.jpg`.

Now we'll change the widgets' contents. In `content/home/`, edit `skills.md`.

This leaves us (or me really) with `experience`, `posts` and `projects`. You can modify them now if you wish (which, to be fair, is what I did), especially if you are okay with keeping them on your homepage. I was not, and this is where landing pages come in.

## Landing pages

This is where my narration gets personal because I'm not sure it was the right way to do it, hence I don't feel like bossing you around.

I wanted those widgets to have their own pages that I could access by clicking on the top bar. I take the `projects` example as a reference here, since it was the weirdest one to set up.

1. First, I created a subfolder in `content/` that I named `my-projects/`. This would be the landing page. In it, I created `_index.md`, copied the metadata from another `content/folder/_index.md` (the underscore is important here), and changed the title to "My projects".
2. I then went back to `content/project/`, added a `index.md` file and wrote this in it:


```r
---
summary: More about my work experience
title: "Resume"
type: widget_page # important!!!
---
```

3. In the same folder, I copied the `content/home/projects.md` widget file.
4. There were two subfolders `external-project` and `internal-project` left, I moved them to the landing page folder `my-projects`.
5. Almost done. I had to redirect the top bar to the landing page folder, rather than to the homepage widget (before I forget, now may be the time to deactivate the widget in `content/home/`). I went to `config/_default/menus.yaml` and changed the url to `'my-projects/'`. And then tada!

You can change the setup of your widget page by editing the `content/home/projects.md` widget file.

It was easier for `posts.md` and `experience.md`. I only needed to create a new subfolder for `experience`, copy the widget there, write an `index.md` (or `_index.md` for my posts), redirect the top bar, and that was it. I don't know what is special about the project page.

Anywho! I was done with the layout of my site[^3], and I jumped happily into my next mission: making things pretty.

[^3]:LIES! I was SO not done. I still changed stuff up, especially the widgets layouts. However I don't think I could explain what I did in great detail.

## 'I made it nice!'

Again, YMMV. This section involves some HTML/CSS/SCSS stuff that, I'm embarrassed to admit, I don't master at all. I copied Alison's files (duh), read and tested each attributes, and here we are. I understand what I did, I'm just not sure I could create this kind of file from scratch with my current knowledge. Here's to a personal goal then!

Below is a list of my biggest changes:

1. I changed the little icon that appears in the browser tab: `assets/media/icon.png`.
2. I "created" a custom theme inspired by Alison's and changed the primary and active colours to shades of pink (at the time of writing), in order to match my avatar: `data/themes/custom_theme.toml`.
3. Ditto, but for fonts: `data/fonts/custom_fonts.toml`.
4. For Hugo to apply this custom theme, I modified the `theme` and `font` items in `config/_default/params.yaml`. You can also use Academic's pre-set themes.
5. I changed the footer `layouts/partials/site_footer.html` and the general behaviour of the site `assets/scss/custom.scss`.

Commit yo changes, push yo changes, let Netlify build and deploy, and voilà!

My website was finally aesthetically pleasing! I can't wait to shake everything up in a month :smile:.

If you made it this far in the post, thank you very much. I hope it can inspire you and help you build your own website. It's been fun to get this website on its legs, I can't wait to make it further my own!

## BONUS: `*.rbind.io` subdomain name

My OG Netlify URL is [kanto-does-things.netlify.app](https://kanto-does-things.netlify.app). I quite like it, but with a name as unique as mine, I loved the idea of having a custom subdomain with only my name.

RStudio offers a free subdomain `*.rbind.io`: I took advantage of this. I [requested](https://alison.rbind.io/post/2017-06-12-up-and-running-with-blogdown/#rbindio-domain-names) one, I followed their instructions, and I can now redirect you to the one and only [kanto.rbind.io](http://kanto.rbind.io)!
