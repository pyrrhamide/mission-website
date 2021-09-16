---
title: Blogdown - d'autres modifications
author: [admin]
date: '2021-04-21'
slug: autres-modifications
categories: [R, tutoriel]
tags: [blogdown, créer le site]
links:
- icon: door-open
  icon_pack: fas
  name: projet
  url: fr/my-projects/creer-contenu-r/
- icon: link
  icon_pack: fas
  name: english
  url: blog/further-modifications/
summary: 'Des modifications supplémentaires, causées par un googlage incessant et par une vision idéale du site. Le site est bilingue et a encore changé de tête.'
featured: no
image:
  caption: '[Photo de Sigmund sur Unsplash](https://unsplash.com/photos/4BSULrfDc7w)'
  focal_point: ''
  preview_only: no
---

{{% toc %}}

J'ai construit le site et j'ai fait des petites modifications esthétiques de base. J'ai parlé superficiellement de ma nouvelle Sainte Trinité, `blogdown`, Hugo et Netlify. Entre temps, _**un toucan qui parle**_ a subi de multiples transformations que j'oublie petit à petit. J'en arrive au point où mon historique GitHub ne m'aide plus à me remémorer ma démarche, puisqu'il devient de moins en moins lisible. Il est donc temps que je couche sur le papier virtuel ce que j'ai fait, pour garder une trace de ma manière de procéder ~ à l'époque ~.

## Site bilingue

:scroll: Chèr.e lecteur.rice, j'ai l'honneur de vous annoncer que ce site vous parvient désormais dans les langues de Shakespeare et de Molière. C'est l'opportunité pour votre rédactrice de démontrer ses talents linguistiques, après avoir passé deux heures sur ce chantier[^1]! Laissez moi vous racontez une histoire...

Le fichier essentiel ici est `config/_default/languages.yaml`, les développeurs du modèle Academic ont laissé des commentaires pour nous aider à configurer le site dans une ou plusieurs langues. J'ai choisi de laisser l'anglais comme langue principale, et d'ajouter le français comme deuxième langue. Tout d'abord, j'ai déplacé le contenu anglais dans un dossier `content/en/` que j'ai ensuite copié et renommé `fr`. J'ai enlevé tous les croisillons[^2] dans `languages.yaml`, j'ai ajouté `fr` là où il fallait, j'ai renommé des objets, etc. Paramétrer le menu était simple: une fois que `contentDir` était défini, j'ai copié-collé mon menu anglais. Qu'est-ce que c'est facile!

C'est facile, et j'ai quand même trouvé le moyen de faire une erreur lors de mon premier essai: **par mégarde, j'ai effacé l'alinéa avant `contentDir: content/en` quand j'ai supprimé le croisillon**. C'est tout bête et frustrant, et ça a complètement bloqué la mise en place du mode bilingue. Une fois que j'ai compris d'où venait le problème, tout s'est bien déroulé. Il ne restait plus qu'à traduire le contenu existant.

{{% callout warning %}}
Par rapport à la traduction: il faut faire attention à ce que les fichiers aient les mêmes noms dans les deux langues, pour que les articles soient liés entre eux. Si c'est bien fait, un globe apparaît dans la barre de navigation.
{{% /callout %}}

Wowchemy fournit un "dictionnaire" de mots clés déjà traduits dans plusieurs langues. On les trouve dans `themes/github.com/.../i18n/`. Ma seule critique est que les mois ne sont pas inclus, par conséquent les dates ne sont pas entièrement traduites quand on est sur le côté français du site.

Un dernier point: l'anglais étant la langue par défaut, le chemin pour mentionner du contenu intra-site ne change pas par rapport à avant[^3]. Par contre, je dois ajouter `/fr/` dans le cas français, autrement ça renvoie le lien vers la page en anglais.

**Réferences**

* [Wowchemy - Language and translation](https://wowchemy.com/docs/guide/language/).
* [Hugo - Multilingual mode](https://gohugo.io/content-management/multilingual/).

[^1]: Ce n'est pas censé prendre aussi longtemps que ça, tout comme initialiser le site ne devrait pas prendre plus de 15 minutes. Les difficultés techniques sont chronophages.
[^2]: Plus connu sous le nom "dièse", mais Wikipédia dit que ce n'est pas le bon terme. Le "dièse" appartient au solfège. _Plus on en sait_...
[^3]: Régler `defaultContentLanguageInSubdir` sur `true` dans `config.yaml` change ce comportement, et force l'ajout de `/en/` dans le chemin.

## Chirurgie esthétique avec `custom.scss`

C'est un fichier important pour les sections suivantes. Sass est l'acronyme pour _Syntactically awesome style sheets_, c'est le langage de script. Scss, pour _Sassy CSS_, est la syntaxe plus récente dérivée de Sass. J'utilise Scss pour modifier le visuel du site, comme l'épaisseur de la barre de navigation, l'espace entre les lettres, la mise en forme du code...

Je ne connais toujours pas grand chose sur CSS et HTML (cqfd: je ne me vois pas mettre que j'ai des notions dans mon CV), donc j'ai copié le fichier d'Alison Hill, je l'ai lu minutieusement et j'ai changé des morceaux petit à petit en visualisant les modifications en direct avec `blogdown::serve_site()`. Si j'étais bloquée ou si je voulais comprendre au lieu de simplement reproduire, j'utilisais Google.[^4] C'est en googlant et en parcourant GitHub que ma liste d'idées pour le site s'élargit. La curiosité n'est pas un vilain défaut ici.

On crée le fichier `custom.scss` dans le dossier suivant: `assets/scss/`.

**Réferences**

* [Alison Hill](https://github.com/rbind/apreshill).
* Le dossier `themes`.

[^4]: C'est l'outil le plus utile de mon quotidien. J'adore le service, je déteste l'entreprise.

## Pied de page

Je n'avais aucun souci avec le pied de page de base. J'étais juste partie si loin dans ma grande mission de personnalisation que j'ai décidé de ne pas épargner le pied de page.

J'ai crée le fichier `site_footer.html` à partir du repo d'AH, dans `layouts/partials/`: j'ai supprimé les lignes qui se rapportaient à la licence et j'ai relié le pdp à `terms.md` ainsi qu'à mon repo GitHub. Je trouvais la taille de la police trop petite[^5], donc j'ai changé le style du pdp dans `custom.scss`, en me basant sur la mise en forme originale qu'on peut trouver dans le fichier `themes`.

**Réferences**

* [Alison Hill](https://github.com/rbind/apreshill). Comme toujours.
* Le dossier `themes`.

[^5]: J'avais réglé la taille de la police sur M pour le site entier. Le pdp est petit de base, c'était miniscule après.

## Table des matières

C'est un travail en cours. Je procède par tâtonnement pour ça, la présentation actuelle n'est peut-être pas la présentation finale. Jusqu'à maintenant, j'ai établi avec succès deux manières de montrer les tdm:

1. J'ai créé `table_of_contents.html` dans `layouts/shortcodes/`. Ensuite, j'écrivais `{{</* table_of_contents */>}}` au début des articles, après l'en-tête YAML. Ca demande des ajustements supplémentaires dans `custom.css`. Ce n'est plus la méthode que j'utilise, car j'ai découvert...
2. Le shortcode `{{%/* toc */%}}`, inclus avec le modèle Academic. C'est rapide et efficace.

Il existe une troisième manière, avec laquelle j'expérimente dans une branche d'essai:

3. Créer une tdm flottante, à côté du corps de l'article. J'ai du mal à l'implémenter parce que je ne suis pas satisfaite de l'alignement, de la hauteur de la tdm, et je n'arrive pas à changer une chose sans que ça déforme le reste du site.

J'ai modifié la "profondeur" des tdm dans `config.yaml`: elles ne montrent pas les titres au-dessus ou en-dessous du niveau `h2` (`##` sur (R)Markdown).

**Réferences**

* [Markdown Elements for Hugo/Wowchemy](https://iphysresearch.github.io/blog/post/writting-markdown/#table-of-contents).

## Blocs spéciaux et _anchor links_

Les blocs spéciaux (ou _div tips_ dans la documentation anglaise) sont des blocs de texte qui détonnent par rapport au texte normal et attirent l'oeil vers du contenu important. Deux types de blocs viennent avec Wowchemy: un de note, et un d'avertissement. Pour les utiliser dans un texte, on écrit ce qui suit:

```html
{{%/* callout note */%}}
Ceci est une note générale.
{{%/* /callout */%}}
```
{{% callout note %}}
Ceci est une note générale.
{{% /callout %}}

```html
{{%/* callout warning */%}}
Ceci est un avertissement.
{{%/* /callout */%}}
```
{{% callout warning %}}
Ceci est un avertissement.
{{% /callout %}}

On peut créer ses propres blocs et les personnaliser dans `custom.scss`. Pour le moment, ce n'est pas dans mes plans, quoique j'aime beaucoup ceux créés par [Alison Hill](https://alison.rbind.io/) et par [Desirée De Leon](http://desiree.rbind.io/post/2019/making-tip-boxes-with-bookdown-and-rmarkdown/).

Il reste les _anchor links_: ces derniers sont utiles si on veut partager directement une section d'une page au lieu de la page entière. Ici, on trouve le symbole de deux maillons à côté des titres de sections: en cliquant dessus, on obtient le lien de la section.

J'ai créé `render-heading.html` dans `layouts/_default/_markup/` pour introduire les _anchor links_, et j'ai modifié `custom.scss` pour les rendre jolis.

**Réferences**

* [Markdown Elements for Hugo/Wowchemy](https://iphysresearch.github.io/blog/post/writting-markdown/#callouts).
* [📸 Page Elements: Writing content with Markdown, LaTeX, and Shortcodes](https://wowchemy.com/docs/content/writing-markdown-latex/).
* pour les _anchor links_...Alison Hill!

## Petits détails et AJA

* Barre de navigation: on peut choisir d'y afficher ou non les logos de réseaux sociaux. Par défaut, Twitter est réglé sur `true`, je l'ai désactivé. On peut aussi afficher le nom de la langue en vigueur à côté du globe, en réglant `show_language` sur `true` dans `params.yaml`.
* Explorer le dossier `themes`: pas mal pour comprendre la structure du site, pour trouver les différents shortcodes...
* Caractères d'échappement (dans le cadre de cet article, [les accolades](https://github.com/gohugoio/hugoDocs/blob/master/content/en/content-management/shortcodes.md)): écrire les shortcodes entre guillemets n'empêche pas le shortcode de s'exécuter (_i.e._ les tdm apparaissaient au milieu du texte). Pour traiter les accolades comme des caractères ordinaires, il faut ajouter `/* ... */` dans le shortcode.
* J'ai changé les paramètres de publication en ligne sur Netlify, afin de publier les différentes branches en plus de la principale. C'est pratique si je veux partager mon travail en cours et recevoir des retours, avant de pousser les modifications sur la branche de prod.
