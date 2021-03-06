---
title: Blogdown - l'origine du site
author: Kanto Fiaferana
date: '2021-04-14'
slug: l-origine-du-site
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
  url: blog/l-origine-du-site/
summary: 'Comment construire un site avec le package R `blogdown`, une introduction rapide.'
featured: no
image:
  caption: '[Photo de Danielle MacInnes sur Unsplash](https://unsplash.com/photos/IuLgi9PWETU)'
  focal_point: ''
  preview_only: no
---

<!--
{{< table_of_contents >}}
-->

{{%toc%}}

Bonjour!

Aujourd'hui, pour mon premier article, je vais parler du processus de construction de mon site. Si jamais je décide d'en faire un autre un jour, et donc de lutter avec `blogdown`, j'aurai cet article ici au chaud, pour avoir un site opérationnel en moins de 15 minutes. Ou bien ce sera un moment de gêne où je me demanderai "mais pourquoi j'ai fait ça comme ça ? Ohlala la bourde...".

Je parle ici de la mise en marche du site. Je me concentrerai sur la mise en forme dans le prochain article.

Mes sources principales (pour la construction et personnalisation) sont l'article d'Alison Hill ["Up & running with blogdown in 2021"](https://alison.rbind.io/post/new-year-new-blogdown/), ainsi que le [répertoire](https://github.com/rbind/apreshill) de son site. Les mots ci-dessous sont presque entièrement les siens, la différence majeure est qu'ils sont en français !

J'ai également consulté [*blogdown: Creating Websites with R Markdown*](https://bookdown.org/yihui/blogdown/) de Yihui Xie, Amber Thomas et Alison Hill, pour d'autres petits détails.

<!--- I am awfully distracted by kids running upstairs. They're so noisy. Damn. --->

## Les ingrédients

* R et RStudio,
* Le package `blogdown`,
* GitHub,
* Netlify.

## Les étapes, en gros

1. Créer un répertoire (j'utilise le mot *repo* à partir d'ici) public sur GitHub, avec un fichier README.md, mais pas .gitignore;
2. Cloner le repo localement[^1], et créer un nouveau Rproject dans le repo;
3. Utiliser son huile de coude sur `blogdown`;
4. Commit et push (je ne connais pas les termes en français) les changements effectués sur GitHub;
5. Déployer le site sur Netlify.
6. FIN.

Facile, n'est-ce pas? Allons-y. Mettons nos mains dans le cambouis.

## `blogdown`

J'ai choisi le modèle [Academic](https://academic-demo.netlify.app/) comme armature de mon site. Academic est relativement simple à manier et sympa à personnaliser. D'autres thèmes de base sont disponibles sur [Wowchemy](https://wowchemy.com/) et également sur [Hugo](https://gohugo.io/).

Une fois mon projet R prêt, j'ai suivi le tutoriel d'Alison scrupuleusement. Ci-dessous, les lignes de code essentielles pour construire le site de base:


```r
install.packages("blogdown")
blogdown::new_site(theme = "wowchemy/starter-academic")
blogdown::serve_site()
blogdown::new_post(title = "Hi Hugo",
                     ext = '.Rmarkdown',
                     subdir = "post")
```

`blogdown::serve_site()` lance un aperçu simultané du site dans le volet Viewer[^2]. Il n'y a pas besoin de rafraîchir la page à chaque changement, `blogdown` s'en charge quand on sauvegarde et/ou qu'on "tricote" nos documents. `blogdown::new_post` crée un nouvel article.

Après cette commande, j'ai changé plusieurs paramètres par défaut dans le fichier `.Rprofile`: l'auteure (`blogdown.author`), l'extension de fichier (`blogdown.ext`) et le sous-répertoire de base pour les nouveaux articles (`blogdown.subdir`). Après chaque modification de .Rprofile, il ne faut pas oublier de sauvegarder puis de `Ctrl+Shift+F10` redémarrer R!

On peut également modifier `config.yaml` et `config/_default/params.yaml` pour changer le titre, le thème du site, et bien d'autres choses.

On écrit tranquillement un article dans un fichier Rmarkdown si on veut inclure du code R, qu'on `Ctrl+Shift+K` tricote par la suite pour obtenir le fichier Markdown[^3]. C'est le type de fichier que Hugo (ou Netlify, je ne suis pas sûre) utilise pour publier du contenu. Si on veut écrire du texte seul, on peut directement écrire en Markdown, sans avoir à tricoter quoique ce soit.

Une fois satisfaite avec mes modifications, j'ai effectué de dernières vérifications avec les commandes suivantes[^4].


```r
blogdown::check_config()
blogdown::check_gitignore()
blogdown::check_content()
blogdown::check_netlify()
blogdown::check_hugo()
# Il peut y avoir des tâches [TODO]
```

On peut également utiliser la commande tout-en-1 `blogdown::check_site()`. S'il n'y a plus de tâches à faire, on peut enfin publier le site!

## La publication

1. Netlify;
2. Se connecter avec GitHub;
3. Choisir le repo à déployer;
4. Changer le nom de domaine.

<!--- I'd really like to make an alert note out of this sentence. TO LOOK UP --->
{{% callout warning %}}
À chaque fois qu'on change le nom de domaine sur Netlify, **il faut changer le baseurl** dans le fichier `config.yaml`.
{{% /callout %}}

Et hop! Mon site est en ligne! Je pense avoir pris une heure pour en arriver là. Netlify prend soin de la construction et du déploiement à chaque modification qu'on pousse sur GitHub. Attention à bien tricoter les fichiers `.RMarkdown`!

Bon, cool, on a un site, mais il est un petit peu encombré. Et si on le personnalisait? Rendez-vous dans mon prochain article pour répondre à cette question!

## BONUS: quelques commandes et raccourcis clavier

* `file.create()` pour créer un fichier dans le répertoire de travail en cours. Si je veux créer un fichier dans un dossier, je précise le chemin, jusqu'au nom souhaité du fichier et son extension. Le(s) dossier(s) doit déjà exister! Si ce n'est pas le cas:
* `dir.create()` pour créer un dossier. Si je précise le chemin, je peux créer un dossier dans un dossier.
* `rstudioapi::navigateToFile(path.to.file)` pour ouvrir un fichier existant sans cliquer :)
* `Ctrl+W` pour fermer un onglet, `Ctrl+Shift+W` pour fermer tous les onglets, `Ctrl+Shift+Alt+W` pour fermer tous les onglets sauf celui ouvert;

[^1]:J'ai cloné mon repo avec GitHub Desktop après avoir essayé de le cloner directement avec RStudio. Malheureusement je ne pouvais rien commit, un fichier index.lock bloquait tout, jsp pourquoi.
[^2]:Je peux également utiliser mon navigateur principal en cliquant sur 'Show in new window'.
[^3]:J'ai laissé `blogdown.knit.on.save` sur `FALSE` parce que je n'aime pas la sauvegarde automatique. On a tous des défauts.
[^4]:AJA qu'il faut écrire les notes de bas de page séparément avec `blogdown`, contrairement à `bookdown` avec lequel je pouvais écrire mes notes directement dans le texte, notes qui étaient automatiquement numérotées.
