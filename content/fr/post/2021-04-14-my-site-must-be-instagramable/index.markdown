---
title: Blogdown - décorer le site
author: Kanto Fiaferana
date: '2021-04-15'
slug: decorer-le-site
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
  url: blog/an-instagramable-site/
summary: 'Le modèle Academic inchangé est très moche. J''ai fait des changements simples et rapides sur le contenu affiché et quelques ajustements esthétiques.'
featured: no
image:
  caption: '[Photo de Joanna Kosinka sur Unsplash](https://unsplash.com/photos/1_CMoFsPfso)'
  focal_point: ''
  preview_only: no
---

<!--
{{< table_of_contents >}}
-->

{{%toc%}}

[La dernière fois](/fr/post/l-origine-du-site/), j'ai expliqué comment rapidement mettre un nouveau site en ligne à l'aide du package R `blogdown`. Cette fois-ci, je vais parler de mes modifications esthétiques personnelles. Je me suis de nouveau fortement inspirée de l'[article](https://alison.rbind.io/post/new-year-new-blogdown/) d'Alison Hill, ainsi que le [repo](https://github.com/rbind/apreshill) de son site. Je décerne également une mention honorable à la [documentation](https://wowchemy.com/docs/) de Wowchemy.

Le modèle Academic vient avec beaucoup d'exemples pour aider à se lancer dans la construction de son site. Toutefois, on peut avoir envie d'alléger le site et de le faire sur mesure. Dans mon cas, j'ai répondu à l'affirmative et suis partie dans une croisade tortueuse pour qu'Academic soit à mon goût. Après, je ne connais pas grand chose en HTML/CSS et je ne suis pas très créative, par conséquent je me suis _beaucoup_[^1] inspirée de l'esthétique (et donc du code) du site d'Alison. [Jetez y un coup d'oeil](https://alison.rbind.io), il est très joli.

[^1]:Grosse emphase sur "beaucoup".

Je n'ai pas suivi les prochaines étapes dans le même ordre de rédaction. Un coup j'ai changé la police, puis j'ai crée une nouvelle page, puis j'ai re-changé la police...bref. C'est important d'être organisée. Ceci est mon petit guide pour la prochaine fois! Avant de se jeter dans le bain, je souhaite prévenir du fait que c'est un récit condensé de tout ce que j'ai fait, il existe d'autres petites améliorations que je laisse l'audience découvrir d'elle-même.

Sans plus tarder, parlons de _widgets_...

## Widget - `activate: false`

La page d'accueil est remplie de _widgets_, qui s'occupe de la présentation personnelle à la présentation d'un nuage de mot. Je trouvais cette page hideuse et très encombrée de _widgets_ dont je n'avais pas besoin.

On détermine donc d'abord ceux qu'on ne veut pas garder. Une fois que notre choix est certain, on navigue vers le dossier `content/home/`, on ouvre les fichiers `.md` des widgets qu'on veut faire disparaître et on ajoute la ligne `activate: false` dans les metadata[^2]. Ca devrait bien épurer la page[^3].

[^2]:Je suis allée trop vite au début et ai supprimé les _widgets_ indésirables. Si jamais je veux les réintégrer, je peux les retrouver dans le dossier du modèle Academic `themes/starter-academic/`. **Attention à ne rien modifier dans ce fichier**.
[^3]:J'ai gardé les _widgets_ suivants: `about`, `experience`, `index`, `posts`, `projects` et `skills`.

## Mes informations personnelles

La deuxième étape était d'ajouter mes propres informations, et de dire au revoir à Nelson Bighetti. Tout d'abord, on modifie l'auteur.e par défaut, dans le fichier `content/authors/admin/_index.md`. Après l'en-tête YAML, on peut écrire un petit paragraphe biographique. Dans ce même dossier `admin`, on peut changer son avatar en chargeant une image intitulée `avatar.png` (ou `.jpg`, peu importe).

J'ai également changé le contenu de quelques _widgets_, notamment `skills.md`.

Il me restait `experience`, `posts` et `projects`. On peut les modifier tout de suite (c'est ce que j'ai fait), surtout si on compte les garder sur la page d'accueil. Je voulais en faire des pages à part, ce qui m'a mené aux _landing pages_ (ou page de renvoi).

## Les _landing pages_

Je voulais que ces _widgets_ aient leur propre page auxquelles je pouvais accéder à partir de la barre de navigation. Ici je vais raconter l'histoire de ma page "Projets" parce que c'était la plus alambiquée à construire.

1. J'ai créé un dossier dans `content/` que j'ai nommé `my-projects/`. C'est le dossier de la landing page. Dedans, j'ai créé `_index.md`, ai copié les metadata d'un autre fichier de ce type `content/folder/_index.md` (le tiret bas est important ici) puis, toujours dans `_index.md`, j'ai changé le titre à "Mes projects".
2. Ensuite, j'ai ouvert le fichier `content/project/`, ai ajouté un `index.md` et ai écrit ceci:


```r
---
summary: More about my work experience
title: "Resume"
type: widget_page # important!!!
---
```

3. Toujours dans `project/`, j'ai copié le fichier _widget_ de `content/home/projects.md`.
4. Il restait deux sous-dossiers `external-project` et `internal-project`, je les ai déplacés dans le dossier `my-projects` de la landing page.
5. Il ne manquait plus qu'à rediriger le bouton "Projets" de la barre de navigation vers le dossier `my-projects`, au lieu de vers le widget de la page principale (avant que j'oublie d'en parler, on peut maintenant désactiver le widget `content/home/projects.md`). J'ai ouvert `config/_default/menus.yaml` et j'ai changé l'url en `'my-projects/'`. On a créé une nouvelle landing page!

On peut changer la mise en page de la widget page en modifiant `content/home/projects.md`.

Changer `posts.md` et `experience.md` était beaucoup plus simple. Il suffisait de créer un nouveau dossier pour `experience`, y copier le widget, ajouter un `index.md` (ou `_index.md` pour mes articles) et rediriger la barre de navigation. Je n'ai aucune idée de ce qui rend la page projet aussi spéciale.

Bref! J'avais enfin la structure de base de mon site[^4], il fut donc temps d'accepter ma prochaine mission: rendre le site joli.

[^4]:J'aurais aimé annoncer que c'était la structure finale, _que nenni_. J'ai modifié la présentation des widgets par exemple. Toutefois, je ne pense pas pouvoir correctement expliquer ce que j'ai fait.

## Nouveau look pour une nouvelle vie

Je suis légèrement gênée d'avouer que je ne sais pas exactement de quoi je parle ici, puisque cette section demande des connaissances en HTML/CSS/SCSS que je ne possède pas. J'essaie d'y remédier! Sinon j'ai copié les fichiers d'Alison Hill, les ai lus et relus attentivement, ai modifié des petits détails à gauche à droite pour comprendre à quoi l'item x se rapportait...et vous pouvez voir le résultat :relaxed:

Voici quelques pistes de personnalisation:

1. J'ai changé l'image qui apparaît dans l'onglet du navigateur: `assets/media/icon.png`.
2. J'ai créé mon thème à partir de celui d'Alison. La différence la plus visible est l'utilisation du orange au lieu du vert: `data/themes/custom_theme.toml`.
3. Idem, mais pour les polices: `data/fonts/custom_fonts.toml`.
4. Pour que ces deux points soient pris en compte dans la construction du site, j'ai modifié les éléments `theme` et `font` dans `config/_default/params.yaml`. On peut également utiliser les thèmes qui accompagnent le modèle Academic (voir la documentation de Wowchemy).
5. J'ai modifié le pied de page `layouts/partials/site_footer.html` et le comportement général du site `assets/scss/custom.scss`. Je vous laisse regarder mon code, si jamais vous êtes curieux.se.

On _commit_ les modifications, on _push_ les modifications, on laisse Netlify construire et déployer, et voilà!

Mon site est en ligne, structuré et agréable à regarder. J'ai hâte de faire une refonte complète le mois prochain :sparkles:

Nous sommes arrivés au bout. Merci beaucoup d'avoir lu tout ça. J'espère vous avoir inspiré, et peut-être vous avoir aidé à créer un site web à votre image. Pour moi, ce fut trois jours frustrants comme récréatifs et je suis très fière de ce que j'ai construit jusqu'ici. Je suis impatiente à l'idée de contribuer davantage à ce site!

## BONUS: le sous-domaine `*.rbind.io`

Mon adresse URL Netlify est [kanto-does-things.netlify.app](https://kanto-does-things.netlify.app). Je l'aime beaucoup, mais avec un prénom aussi unique que le mien, j'aurais adoré avoir un sous-domaine personnalisé avec seulement mon prénom.

Grâce à RStudio, mon rêve est devenu réalité. RStudio offre un sous-domaine gratuitement, `*.rbind.io`: j'ai saisi cette opportunité. J'ai [fait la demande](https://alison.rbind.io/post/2017-06-12-up-and-running-with-blogdown/#rbindio-domain-names) sur le _repo_ `rbind/support`, j'ai suivi leurs instructions, et je peux maintenant vous rediriger vers l'unique [kanto.rbind.io](http://kanto.rbind.io)!

Toutefois, il y a quelques problèmes au niveau du protocole HTTPS. Quand on ajoute un sous-domaine personnalisé sur Netlify (**et donc qu'on change le baseurl de `config.yaml`!**), le site utilise HTTP au lieu de HTTPS. On peut forcer la redirection vers HTTPS avec un fichier `static/_redirects`:


```r
http://kanto.rbind.io/*    https://kanto.rbind.io/:splat  301!
```

C'est ce que j'ai fait, mais il arrive souvent que mon site soit inaccessible en raison d'un certificat SSL invalide. Ca m'irrite beaucoup, au point où je considère rester en HTTP, ce qui n'est pas un défaut en soi.

