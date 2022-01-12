---
title: Écrire son mémoire avec RMarkdown
author: Kanto Fiaferana
date: '2022-01-11'
slug: ecrire-son-memoire-rmarkdown
categories: [R, long récit]
tags: [travail perso]
summary: 'J''ai écrit mon mémoire de M2 avec RMarkdown, Word n''étant plus mon ami. Je partage ici tout ce que j''ai fait pour obtenir un beau (et long) fichier pdf.

L''article est long. Avoir une petite tasse de thé à côté ne serait pas de trop.'
featured: no
image:
  caption: '[Photo de Ken Suarez sur Unsplash](https://unsplash.com/photos/4IxPVkFGJGI)'
  focal_point: ''
  preview_only: no
projects: []
math: true
---

{{% callout look %}}
Coucou.

Tout d'abord, bonne année. J'espère qu'elle sera meilleure que la précédente.

Alors, il faut savoir que cet article est resté un brouillon pendant plusieurs mois. J'étais déterminée à le publier après la soutenance de mon mémoire, mais _life happened_. J'ai repris la rédaction en 2022 parce que faut pas abuser non plus. Toutefois, ma narration est imparfaite. Il n'y a pas de concordance des temps, certaines phrases sont restées des notes (on dirait mon mémoire). Mes professeurs de français s'arracheraient les cheveux s'ils lisaient cet article. 

Mais ils ne le verront jamais et j'ai eu 10 au bac de français, donc j'écris comme je veux ! C'est auprès de toi, chère lectrice, que je m'excuse.

Sur ce, bonne lecture et bonne chance pour ton mémoire si ce cas s'applique à toi.
{{% /callout %}}

Il y a deux ans, j'ai écrit mon mémoire de licence avec Word, ne connaissant pas mieux et devant utiliser Stata qui ne génère pas de rapports. Ce n'était pas facile au niveau de la mise en forme des tableaux, du référencement, et c'était un long document. En gros, ce n'était pas une expérience agréable. 

Pendant mon master, j'ai appris à utiliser R et RMarkdown qui est pratique pour incorporer directement mes données/codes/graphiques/tableaux dans du texte. J'ai donc décidé d'écrire mon mémoire de M2 avec RMarkdown, pour finir avec un beau fichier pdf.

C'était un grand projet. Je partage ici les grandes lignes de ce procédé.

## Les ingrédients

Plusieurs éléments sont essentiels pour que tout fonctionne bien:

* R et RStudio;
* $\LaTeX$: MiKTeX, ou [TinyTeX](https://yihui.org/tinytex/) avec R;
* Visual Studio Code (facultatif/préférence personnelle);
* De la patience et de la persévérance.

## Le dossier

Au sein du grand dossier de mon mémoire (codes, données, bibliographie, graphiques, tableaux, etc.) qui est aussi un projet R, je crée un dossier "texte" contenant tous mes fichiers `.Rmd`. Ces fichiers sont mes chapitres.

{{% callout warning %}}
[Le répertoire de travail d'un fichier `.Rmd` est le dossier dans lequel ce fichier est sauvegardé](https://bookdown.org/yihui/rmarkdown-cookbook/working-directory.html), et non pas le dossier du projet R. Pour moi, c'était donc `/texte/` et non `Memoire/`. J'ai découvert ça quand j'ai essayé de compiler les chapitres `.Rmd` dans lesquels j'appelais mes données ou mes images: j'utilisais le chemin relatif `"sous-dossier/fichier.ext"`, par exemple `"code/fonctions.R"`, sans succès. 

Il fallait mettre le chemin entier depuis le dossier parent, soit `"~/Memoire/sous-dossier/fichier.ext"`, ou plus simplement `"../sous-dossier/fichier.ext"`.
{{% /callout %}}

Le dossier `/texte/` est organisé comme suit:
```plaintext
texte /
├── biblio.bib
├── asrlf.csl
├── 00_page_de_garde.Rmd
├── 01_intro.Rmd
├── ...
├── main.Rmd
└── preamble.tex
```
Les pdf compilés, beaux et propres, sont immédiatement sauvegardés dans ce dossier.

`main.Rmd` est le fichier qui regroupe tous les chapitres, je n'écris pas de contenu d'analyse dedans. Comme ça, je peux écrire les chapitres séparément au lieu de tout faire d'un coup dans un long Rmd. C'est plus propre et, personnellement, ça m'aide pour clairement organiser mes arguments.

## Les packages LaTeX utilisés {#pkg-latex}

Puisque le fichier final est un fichier pdf, je peux utiliser des packages $\LaTeX$ pour des modifications supplémentaires que RMarkdown seul ne peut pas apporter. Dans un fichier `preamble.tex`, j'écris tous les packages LaTeX utilisés. On retrouve ensuite ce fichier dans l'en-tête YAML de `main.Rmd`: pendant la compilation, les packages seront écrits dans le fichier `.tex` intermédiaire.

{{% callout note %}}
Ca n'empêche pas de mettre les packages dans les chapitres intermédiaires, pour avoir une idée de quel visage ça aura à la fin.
{{% /callout %}}

J'utilise les packages suivants:
```latex
\usepackage{ragged2e} % alignement
\usepackage{setspace} % espace entre les lignes

% pour les diagrammes
\usepackage{tikz}
\usetikzlibrary{positioning, trees}

% pour les titres de tables et figures, format et langue
\usepackage{caption}
\usepackage{subcaption}
\captionsetup{format=hang,font=small}
\captionsetup[table]{name=Tableau}
\captionsetup[figure]{name=Figure}

% pour...les mots?
\usepackage[utf8]{inputenc}
\usepackage[T1]{fontenc}
\usepackage[french]{babel}

% pour numérotation des figures et tables selon section, et non continu
\usepackage{chngcntr}
\counterwithin{figure}{section}
\counterwithin{table}{section}

% pour afficher "Chapitre" avant chaque titre de section
\usepackage{titlesec}
\titleformat{\section}{\Large\bfseries}{Chapitre \thesection.}{1em}{}

% pour les annexes
\usepackage[toc,title,page]{appendix}

% quelques métadonnées 
\usepackage{hyperref}
\hypersetup{
    pdftitle = {Nom du fichier qui apparaîtra sur le lecteur pdf},
    pdfauthor = {Mon Nom}
}
```

J'ai tout ce qu'il faut pour commencer la rédaction du mémoire.

## Assembler le document

Il s'agit maintenant d'écrire mes chapitres individuellement pour ensuite les rassembler dans un document final qui montrera les résultats de mes efforts!

### L'en-tête YAML {#en-tete-yaml}

Dans l'en-tête du fichier `main.Rmd` qui regroupera tous les chapitres rédigés séparément, je précise:

* Le type de document (pdf), les packages $\LaTeX$ à charger, et l'application de la numérotation des sections;
* La taille des marges, la police et sa taille, l'interligne[^1];
* La taille de la feuille[^2];
* Le fichier `.bib` avec mes références et le fichier `.csl` du langage de référencement.

[^1]: L'interligne est utile pour la lisibilité, et pour que les profs puissent écrire leurs notes confortablement entre les lignes. C'est pas juste pour augmenter artificiellement le nombre de pages :)
[^2]: J'ai remarqué assez tard que la taille par défaut est celle du format lettre US 216 x 279mm.

Voilà à quoi le début de `main.Rmd` ressemble:

```yaml
---
output: 
  pdf_document:
    includes:
      in_header: preamble.tex
    number_sections: true
geometry: margin=2.5cm
fontsize: 12pt
fontfamily: fourier
linestretch: 1.5
indent: true
papersize: a4
bibliography: biblio.bib
csl: asrlf.csl
---
```

J'ai l'option de recopier la même en-tête dans les fichiers des chapitres, mais ce n'est pas obligatoire. J'ai choisi de faire ainsi parce que je voulais visualiser chaque chapitre individuellement en pdf.

{{% callout warning %}}
Les en-têtes des chapitres ne seront pas pris en compte dans la compilation finale de `main.Rmd`.
{{% /callout %}}

### La page de garde

J'ai construit la page de garde propre à mon université. J'en ai profité pour aller à fond dans du $\LaTeX$ pour expérimenter.

La page de garde est sauvegardée dans un fichier Rmd, `00_page_de_garde.Rmd` mais elle n'est pas construite avec le langage R/RMarkdown. _Instead_[^3], j'ai utilisé LaTeX. Le format Rmd supporte les commandes LaTeX si le fichier généré est un fichier pdf (donc ça ne fonctionne pas si `output: html_document` pour être limpide), et c'est ce que j'ai fait parce que c'était plus "facile" (gros guillemets bien épais, c'est dur LaTeX) pour construire ma page comme je le souhaitais. 

[^3]: Cet article est coincé dans les limbes depuis septembre 2021, j'ai décidé que mettre un peu d'anglais me motiverait. _Sorry_.

Alternativement, j'aurais pu faire un document pur LaTeX pour la page de garde. Je l'aurais déclarée dans [l'en-tête de `main.Rmd`](#en-tete-yaml), avec `before_body: "page_de_garde.tex"` après `in_header: "preamble.tex"`.

Voici le code de ma page de garde:

```latex
\begin{titlepage}

\par
\raisebox{-.5\height}{\includegraphics[width=4cm]{image1.png}}
\hfill
\raisebox{-.5\height}{\includegraphics[width=5cm]{image2.png}}
\par

\vspace{1cm}
\begin{center}

\textit{Master Sciences Sociales - Parcours Quantifier en Sciences Sociales}

2020-2021

\vspace{5mm}

\textsc{Mémoire de recherche}

\vfill 

{\LARGE\bfseries Titre du mémoire\par}

{\Large\bfseries Sous-titre\par}

\vfill

{\large\itshape Soutenu par}

{\large Moi}

\vspace{5mm}
\textit{Session}

Septembre 2021

\vspace{5mm}
\textit{Directrice}

Ma directrice

\vspace{5mm}
\textit{Rapporteure}

La rapporteure

\end{center}
\end{titlepage}

\newpage
\pagenumbering{roman}

\phantomsection
\addcontentsline{toc}{section}{Remerciements}
\section*{Remerciements}
```

J'ai tout de suite ajouté une page de remerciements et une page de dédication (pas montrée ici, très personnelle, quelques larmes ont été versées), numérotées avec des chiffres romains pour les distinguer du corps du mémoire.

Problème avec les sections non-numérotées: n'apparaît pas dans la table des matières + mauvaise section liée (i.e. une section non-numérotée était liée à la section numérotée précédente). "Régler" en ajoutant `\phantomsection` pour dire que c'est bien un chapitre, `addcontentsline{toc}{section}{Titre}` pour donner le titre qui apparaîtra dans la table des matières. Manip nécessaire si on déclare une section avec `\section*{Titre}`, plus courte si le titre est sous forme markdown.

Après l'en-tête et avant le titre de niveau 1 du chapitre:

```md
\phantomsection

# Titre du chapitre non-numéroté {-}
```

### Le corps du texte

On en vient enfin au contenu sur lequel j'étais notée. J'ai créé un fichier `.Rmd` par chapitre, j'ai bien mis un titre de niveau 1 au début. J'ai ajouté la liste des packages LaTeX utilisés pour chaque chapitre dans l'en-tête YAML sous `header-includes:` (les déclarer directement `preamble.tex` comme vu plus haut fonctionne aussi), les options de marge, de police, etc. 

J'ai préféré répéter l'en-tête dans chaque chapitre parce que je les générais en pdf individuellement pour m'assurer que tout fonctionnait bien. C'était aussi utile si je voulais faire des changements esthétiques et que je voulais les visualiser directement sur un morceau du mémoire. J'avais juste à changer l'en-tête d'un chapitre sans que ça impacte le reste, au lieu de modifier `preamble.tex` à chaque fois que je voulais changer de police.

De toute façon, l'en-tête de `main.Rmd` est le seul pris en compte à la fin. 

### Bibliographie

Il faut [voir ici](https://stateofther.github.io/finistR2020/r_citr.html) pour obtenir une meilleure explication. Je raconte quand même grossièrement ce que j'ai fait.

J'ai eu besoin des ingrédients suivants: Zotero, l'extension Better BibTeX pour Zotero, et le package `citr` pour R.

Pour Better BibTeX, j'ai sélectionné l'option "garder à jour" quand j'ai exporté mes références pour qu'elles soient automatiquement mises à jour si je change quelque chose dans Zotero.

Le langage de référencement par défaut pour les `.Rmd` est Chicago, il ne me convenait pas car il était en anglais, il ne montrait pas certains éléments ou en montrait trop, et parce que j'ai l'habitude de travailler avec le système Harvard (rpz Warwick). Du coup, j'ai [surfé sur le web](https://www.zotero.org/styles) et j'ai choisi le modèle de [l'Association de Science Régionale de Langue Française](https://editor.citationstyles.org/styleInfo/?styleId=http%3A%2F%2Fwww.zotero.org%2Fstyles%2Fassociation-de-science-regionale-de-langue-francaise) parce qu'il me plaisait et correspondait le plus au système avec lequel j'étais à l'aise. Je l'ai légèrement personnalisé pour que "et" apparaisse au lieu de "&" s'il y avait deux auteurs, en modifiant le fichier CSL (pour _Citation Style Language_) avec [ce site](https://editor.citationstyles.org/about/).

J'ai dit à R que tout ça existait, dans l'en-tête, avec les lignes suivantes:

```yaml
bibliography: biblio.bib
csl: asrlf.csl
```

Encore une fois, j'avais le choix entre les écrire dans chaque chapitre ou directement dans `main.Rmd`. J'ai fait dans les deux parce que YOLO.

Dans le texte:

* `@ref` génère Auteur (an), 
* `[@ref]` génère (Auteur, an), 
* `[@ref1; @ref2]` pour plusieurs références à la fois entre parenthèses, 
* ou `[-@ref]` pour n'avoir que l'année si l'auteur est mentionné au préalable. 

Dans RStudio, j'utilise l'addin de `citr` pour ajouter les citations, à partir d'une interface interactive. J'ai besoin de préciser le fichier `.bib` dans l'en-tête pour que le package trouve les références. 

Je tricote le fichier, et les références se trouvent automatiquement à la fin du document. Ce n'est pas problématique s'il n'y a pas d'annexe, c'est un problème dans le cas contraire. Pour imprimer les références à l'endroit qu'on veut, il suffit d'écrire `<div id="refs"></div>` dans `main.Rmd`.

```latex
\newpage
\phantomsection
\addcontentsline{toc}{section}{Références}
\section*{Références}

<div id="refs"></div>
```

### Appendices

Ca m'a cassé la tête. J'ai eu besoin du package `appendix` de $\LaTeX$ et de l'environnement `appendices` pour signaler le début des annexes. 

Dans `preamble.tex`, `\usepackage[toc,title,page]{appendix}` dit qu'on génère un titre "Appendices" au début de l'environnement. Ce titre apparaît dans la table des matières et au début de mes annexes. Cependant, j'ai dû explicitement définir le mot qui apparaissait au début de chaque section de ces appendices ("Annexe ") car le mot "Chapitre" était généré à la place.

```latex
% dans le preamble
\usepackage[toc,title,page]{appendix}

% dans le fichier main.Rmd (ou 99_annexe.Rmd)
\begin{appendices}

\titleformat{\section}{\Large\bfseries}{Annexe \thesection.}{1em}{}

\section{Chapitre A dont l'annexe dépend}
\subsection{Annexe A1}
\subsection{Annexe A2}

\section{Chapitre B dont l'annexe dépend}
\subsection{Annexe B1}

\end{appendices}
```

## Tout compiler

C'est la dernière ligne droite. Sous [l'en-tête](#len-tête-yaml) de `main.Rmd`, on pointe du doigt tous les fichiers `.Rmd` pour tous les chapitres dans des chunks `child`. 

**Important, je l'ai déjà dit et je le redis**: les en-têtes YAML propres à chaque fichier `.Rmd` ne sont pas prises en compte dans la construction du document final. Seul celui du fichier principal compte.

````r
```{r child="00_page_de_garde.Rmd"}
```

\newpage
\begin{spacing}{1}

\phantomsection
\addcontentsline{toc}{section}{Table des matières}
\tableofcontents
\listoftables
\listoffigures

\end{spacing}

\newpage
\pagenumbering{arabic}
\setlength{\parskip}{1em}

```{r child="01_intro.Rmd"}
```

\newpage
```{r child="02_lit_review.Rmd"}
```

\newpage
```{r child="03_data_methodo.Rmd"}
```

# tous les autres chapitres


\newpage
\singlespacing
\setlength{\parskip}{0.5em}


\phantomsection
\addcontentsline{toc}{section}{Références}
\section*{Références}

<div id="refs"></div>

\newpage
```{r child="99_annexe.Rmd"}
```
````

Pour tricoter le fichier sans utiliser la souris, `Ctrl + Maj + K` !

Et voilà! Le mémoire est tout beau tout propre. Il ne manque plus qu'à relire, réviser, et préparer sa soutenance :smile:

## Pour résumer - TL;DR {#tldr}

L'article est long, un résumé s'impose si tu n'as pas tout lu. Le gist qui suit montre un squelette de `main.Rmd` et de `preamble.tex`.

Les points principaux:
* `main.Rmd` est le fichier "parent" dans lequel on déclare tous les chapitres. 
* Ces chapitres sont rédigés individuellement dans des fichiers "enfants" `chapitre.Rmd`. Si un chapitre n'est pas numéroté, il faut ajouter la commande `\phantomsection` avant le titre du chapitre. 
* On déclare les packages LaTeX dont on a besoin dans `preamble.tex`.

{{< gist pyrrhamide c716d95b34ce9871678d35801fd6b16e  >}}

J'ai créé un dépôt où j'ai mis [le code et le texte de mon mémoire](https://github.com/pyrrhamide/memoire-ambassades), pour que tu puisses comprendre ce que toutes ces étapes donnent en pratique. Il manque les données dans leurs formes nettoyées parce qu'elles étaient trop larges pour GitHub (donc ce n'est pas directement reproductible, pas très _open data_ de ma part), mais le fichier Excel original est bien présent.

## Mes notes

* Packages R qui n'ont pas fonctionné, par exemple `DiagrammeR` qui fonctionne qu'avec des fichiers HTML. M'a poussée à faire mes diagrammes en LaTeX pur, dans le corps de texte Rmd. Une aventure.
* Ce qui m'emmène au référencement des éléments du document: 
  * quand il s'agit de qqch que j'ai écrit en LaTeX, important de mettre `\label{lab}` après le titre. Référencement dans le doc se fait `\ref{lab}`. Easy peasy lemon squeezy.
  * quand il s'agit de qqch généré par RMarkdown (du style kable, ggplot, etc.), important de nommer les code chunks. Référencement dans le doc se fait `\ref{type:label-code-chunk}`, avec type `tab`, `fig` ou `eqn`.
* Mélange des langages dans les documents: franchement sympa, simplifie la vie quand on connaît la commande dans un langage mais pas dans l'autre. Cependant, devient rapidement très brouillon. 
* C'est du bricolage pur et dur.


## Références pour cet article

* Etienne Bacher (2020), [Writing a Master’s thesis with R Markdown and Bookdown](https://www.etiennebacher.com/posts/2020-07-16-tips-and-tricks-r-markdown/).
* Anna (idk), [Write your dissertation in RMarkdown - Using RMarkdown to create complex pdf documents](https://ourcodingclub.github.io/tutorials/rmarkdown-dissertation/).
* Tyson Barrett (2018), [Writing Your Dissertation (or Thesis) in RMarkdown](https://tysonbarrett.com/jekyll/update/2018/02/11/r_dissertation/).
* Yihui Xie, Christophe Dervieux, Emily Riederer (2020), [R Markdown Cookbook](https://bookdown.org/yihui/rmarkdown-cookbook/).
