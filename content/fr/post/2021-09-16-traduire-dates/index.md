---
title: Blogdown - traduire les dates pour un site multilingue
author: KF
date: '2021-09-16'
slug: traduire-dates-site-multilingue
categories: [R, tutoriel]
tags: [blogdown]
summary: 'Traduire les dates pour un site multilingue. Horriblement facile. J''ai quand même perdu 5 heures de ma vie pour faire cette manipulation.'
featured: no
image:
  caption: '[Photo d''Estée Janssens sur Unsplash](https://unsplash.com/photos/zni0zgb3bkQ)'
  focal_point: ''
  preview_only: no
toc: false
---

Quand je me suis lancée dans la création de ce blog, je pensais que j'allais tout le temps écrire en anglais. Puis j'ai découvert qu'Hugo et le thème Academic offraient la possibilité de faire un blog multilingue: je me suis dit "les ressources sur R en anglais ne manquent pas, pourquoi pas contribuer à ce monde en français ?". C'est ce que j'ai fait un beau matin d'avril, quoique je me suis heurtée à un obstacle [qui était très simple à contourner](/fr/blog/autres-modifications/#site-bilingue).

Depuis, j'écris mes articles en anglais et en français, ces articles sont bien liés entre eux car les dossiers sont nommés identiquement, tout roule ! Enfin presque. Un petit défaut me titillait: les mois n'étaient pas traduits en français ce qui m'irritait au plus au point. J'ai essayé plusieurs fois de régler ça: j'ai créé un fichier `mois.yaml` dans `data`, puis je l'ai référencé dans une copie du template `page_metadata.html` de `layouts/partials` de la manière suivante:

```html
<span class="article-date">
  {{ $date := $page.Date.Format site.Params.date_format }}

  {{ if eq .language.Lang "en" }}
  {{ $date }}
  {{ else }}
  {{ .page.Date.Day }} {{ index site.Data.mois (printf "%d" .page.Date.Month) }} {{ .page.Date.Year }}
  {{ end }}
</span>
```

...et j'ai re-essayé, re-décliné la condition, j'ai googlé à outrance...ça ne marchait pas.

Puis je suis tombée sur [cet article](https://www.enricotips.com/post/multilingual-dates-in-hugo/). Qui ajoutait simplement la traduction des mois dans le fichier `i18n/fr.yaml`...

```yaml
- id: January
  translation: Janvier
- id: February
  translation: Février
- id: March
  translation: Mars
- id: April
  translation: Avril
- id: May
  translation: Mai
- id: June
  translation: Juin
- id: July
  translation: Juillet
- id: August
  translation: Août
- id: September
  translation: Septembre
- id: October
  translation: Octobre
- id: November
  translation: Novembre
- id: December
  translation: Décembre
```

...les ajoutait dans `i18n/en.yaml` pour que les mois ne soient pas absents du côté anglais...

```yaml
- id: January
  translation: January
- id: February
  translation: February
- id: March
  translation: March
- id: April
  translation: April
- id: May
  translation: May
- id: June
  translation: June
- id: July
  translation: July
- id: August
  translation: August
- id: September
  translation: September
- id: October
  translation: October
- id: November
  translation: November
- id: December
  translation: December
```

...pour ensuite changer le format de la date dans `page_metadata.html`...
```html
<span class="article-date">
  {{ $date := $page.Date.Format site.Params.date_format }}
  {{.page.Date.Day}} {{i18n .page.Date.Month}} {{.page.Date.Year}}
</span>
```

Et hop. Les mois sont traduits. Hah. Plus simple, tu meurs.

![](https://media.giphy.com/media/jNKSOKMhFcK9a/giphy.gif?cid=ecf05e47scf7eropyybezlaldbs904jvha8gdcbcwpf7ytos&rid=giphy.gif&ct=g)
