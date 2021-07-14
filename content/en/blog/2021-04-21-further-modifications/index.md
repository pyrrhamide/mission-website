---
title: further modifications
author: [admin]
date: '2021-04-21'
slug: further-modifications
categories: [R, tutorials]
tags: [blogdown, story time]
links:
- icon: door-open
  icon_pack: fas
  name: project
  url: my-projects/creating-beautiful-content/
- icon: link
  icon_pack: fas
  name: français
  url: fr/blog/autres-modifications/
summary: 'Additional tweaks for when you cannot stop googling and dreaming of what your website could be.'
featured: no
image:
  caption: '[Photo by Sigmund on Unsplash](https://unsplash.com/photos/4BSULrfDc7w)'
  focal_point: ''
  preview_only: no
---

{{% toc %}}

I talked about how to build a basic website and about what standard customisations one could do. I scratched the surface of everything one can do with `blogdown`, Hugo and Netlify, my current Holy Trinity. However, with the way _**a talking toucan**_ looks like now, I felt I needed to properly write down all the other little things I did. I can only go so far back in my commit history, before thinking "eff it, I don't remember how I did this, whatever".

I'll make this my motto from now on: Your Mileage May Vary. This post is mainly for me, so that I can remember how I proceeded or so that I can cringe at the naïveté of it all! It's my public diary if you will :)

## Multilingual adventure

:scroll: Dear reader, I am pleased to announce that this website now comes in the languages of Shakespeare and of Molière. What a marvellous and monumental project of mine, I can demonstrate my language skills to the world, and all it took was 2 hours[^1]! Let us rewind...

The main file in this story is `config/_default/languages.yaml`, the Academic developers were nice enough to leave comments to guide us if we wanted to configure the website in a second language. I decided to leave English as the default language, and add French as the second language. First, I moved all my English content in a `content/en/` subfolder, copied it and renamed the copy `fr`. I then uncommented every line that needed some uncommenting[^2] in `languages.yaml`, added several `fr` here and there, renamed stuff. Configuring the menu was also straightforward: once `contentDir` was set, I copy-pasted the English menu. It's relatively easy!

It's relatively easy, and I still made a silly mistake when I first tried it: **I inadvertently deleted the indentation before `contentDir: content/en` when I uncommented the line**. I could not go forward with the multilingual mode, which was quite frustrating. Once I found my mistake, the process went smoothly. All that was left to do was translating the existing content.

{{% callout warning %}}
When translating content/creating new content, make sure your folders have the same name in both languages so that posts in different languages are linked. The globe icon appears on the navigation bar when done correctly.
{{% /callout %}}

Wowchemy also comes with a "dictionary" of common words that are translated in all supported languages. They can be found in the `themes/github.com/.../i18n/` folders. My only complaint is that months are missing, therefore when browsing the French-side of the site, the dates are not fully translated. I'm working on that.

Last point: English is the default language, so when I reference within-the-site English content, the path does not change[^3]. However, when dealing with the French-side, I must add `/fr/` otherwise it will link to the English-side.

**References**

* [Wowchemy - Language and translation](https://wowchemy.com/docs/guide/language/).
* [Hugo - Multilingual mode](https://gohugo.io/content-management/multilingual/).

[^1]: It should not actually take that long, just like initially creating the website should not have taken more than 15 minutes. Technical issues are a nightmare innit.
[^2]: Not a real word. I'm quite knackered these days.
[^3]: Setting `defaultContentLanguageInSubdir` to `true` in `config.yaml` changes this behaviour, and forces to add `/en/` in the path.

## Another makeover with `custom.scss`

This is an important file for the following sections. SCSS stands for _Syntactically awesome style sheets_. It's used to style the site, from the width of the navigation bar, the letter spacing, hover animations, code styling...it's great!

I still don't know much about CSS and HTML (WIP!), so I copied Alison Hill's file, read it carefully and changed things slowly while `blogdown` was serving the site. If I was stuck on something or if I really wanted to understand what I was doing, I googled.[^4] It's actually by googling and browsing GitHub that I keep getting new ideas for this site. I cannot recommend this type of curiosity enough.

Create your `custom.scss` file under the following folders: `assets/scss/`, and write down your aesthetic changes.

{{% callout look %}}
**Update**: or create `custom.scss`, write down all your general changes applicable to the whole website, then create individual `_itemname-custom.scss` files for specific elements, files that you then `@import` to `custom.scss`. See [here](/blog/further-modifications-2/#making-customscss-more-readable) for more details.
{{% /callout %}}

**References**

* [Alison Hill](https://github.com/rbind/apreshill).
* The `themes` folder.

[^4]: I will always express my appreciation for Google. Love the service, hate the company.

## The footer

There is absolutely nothing wrong with the default footer. The only culprit is myself, I went too deep in this makeover journey and I thought "why not?" change my footer.

I created `site_footer.html` off of AH's repo, under `layouts/partials/`: I deleted everything licence-related, and linked the footer to my `terms.md` file and my GitHub repo. Once this was done, I found the font too small[^5], so I changed the footer style in `custom.scss`, based on the original formatting that can be found in `themes` folder.

**References**

* [Alison Hill](https://github.com/rbind/apreshill). Again.
* The `themes` folder.

[^5]: I set the default font size for the entire site to M, that's why it was so tiny.

## Table of Contents

I am still finding my way around those. So far, I successfully implemented two ways to get the TOCs:

1. I created `table_of_contents.html` in `layouts/shortcodes/`. I then wrote `{{</* table_of_contents */>}}` at the beginning of my articles, just after the YAML header. This requires some tweaks in `custom.css`. Ultimately, I did not stick with this method, because I found...
2. The `{{%/* toc */%}}` shortcode, prepacked with the Academic theme. It's much faster, it does the job. And I like it!

More can still be done though, I am trying out a third way in a test branch:

3. Creating a floating TOC on the side. It's difficult, I'm never happy with how it looks in terms of alignment and padding.

I changed the depth of my TOCs in `config.yaml`: they are set to not show anything below or above a `h2` (`##`) header.

**References**

* [Markdown Elements for Hugo/Wowchemy](https://iphysresearch.github.io/blog/post/writting-markdown/#table-of-contents).

## Div tips and anchor links

Div tips are special blocs of content that pop out and get your attention. They are useful when emphasising something important. Wowchemy comes with two types of callouts, note and warning. To include them in your writing, use the following code:

```html
{{%/* callout note */%}}
This is a callout note.
{{%/* /callout */%}}
```
{{% callout note %}}
This is a callout note.
{{% /callout %}}

```html
{{%/* callout warning */%}}
This is a callout warning.
{{%/* /callout */%}}
```
{{% callout warning %}}
This is a callout warning.
{{% /callout %}}

In previous versions of Wowchemy/Hugo, `callout` used to be `alert`. I initially tried to call div tips by using `alert` but as it turns out, my version is too new!

You can create your own div tips, and personalise them in `custom.scss`. At the moment, I do not have plans for custom div tips, although I do like the ones created by [Alison Hill](https://alison.rbind.io/) and [Desirée De Leon](http://desiree.rbind.io/post/2019/making-tip-boxes-with-bookdown-and-rmarkdown/).

This leaves me with anchor links: those are useful if you want to share a section of your page. Here, you can find the anchor icon next to `h2` and below headers: if you click on them, you get the direct link to the selected section. Neat!

Create `render-heading.html` under `layouts/_default/_markup/` to introduce anchor links, and edit `custom.scss` to make them pretty.

**References**

* [Markdown Elements for Hugo/Wowchemy](https://iphysresearch.github.io/blog/post/writting-markdown/#callouts).
* [📸 Page Elements: Writing content with Markdown, LaTeX, and Shortcodes](https://wowchemy.com/docs/content/writing-markdown-latex/).
* as for the anchor links...Alison Hill!

## Additional little things and TIL

* Navigation bar: choosing whether to display network icons in the header. Twitter is set on `true` by default, I deactivated that. Can also show current language next to globe by setting `show_language` to `true` in `params.yaml`.
* Browsing the `themes` folder: great to understand the default layout, finding the different shortcodes...
* Escaping special characters (in this article's case, [curly braces](https://github.com/gohugoio/hugoDocs/blob/master/content/en/content-management/shortcodes.md)): writing shortcodes with quotes does not prevent the shortcode from running (_i.e._ TOCs actually appear). You have to escape the special characters by adding `/* ... */` inside the curly braces of the shortcode.
* I changed the deploy settings on Netlify, to publish my different branches as well. It's great if I want to share and get feedback from others, before pushing on my production branch.
