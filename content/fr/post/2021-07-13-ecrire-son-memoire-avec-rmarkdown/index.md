---
title: Écrire son mémoire avec RMarkdown
draft: true
author: Kanto Fiaferana
date: '2021-10-01'
slug: ecrire-son-memoire-rmarkdown
categories: []
tags: []
summary: 'J''ai écrit mon mémoire de M2 avec RMarkdown, Word n''étant plus mon ami. Je partage ici tout ce que j''ai fait pour obtenir un beau (et long) fichier pdf.'
featured: no
image:
  caption: '[Photo de Ken Suarez sur Unsplash](https://unsplash.com/photos/4IxPVkFGJGI)'
  focal_point: ''
  preview_only: no
projects: []
math: true
---

{{% toc %}}

Mémoire de M2, analyse de données et de réseaux. J'avais écrit mon mémoire de licence avec Word (aïe), ce qui n'était franchement pas facile au niveau des tableaux, du référencement, etc. Utiliser R et RMarkdown pour incorporer directement mes données/codes/graphiques/tableaux, mise en page pas complètement gérer par moi. Générer un beau fichier pdf avec $\LaTeX$.

## Les ingrédients

* R et RStudio;
* $\LaTeX$: MiKTeX, ou TinyTeX avec RStudio;
* Visual Studio Code (facultatif mais j'aime bien);
* de la patience et de la persévérance.

## Le dossier

Organisation de mon dossier contenant tous mes fichiers `.Rmd`, au sein d'un plus grand dossier/projet pour l'entièreté de mon mémoire (codes, données, bibliographie, graphiques, tableaux, etc.). 

Cette idée de faire mon mémoire sur RMarkdown m'a honnêtement mordue dans le cul. J'ai appris (assez rapidement) que [le répertoire de travail d'un fichier rmd est le dossier dans lequel ce fichier est sauvegardé](https://bookdown.org/yihui/rmarkdown-cookbook/working-directory.html), et non pas le dossier de mon projet R. Pour moi, c'était donc `/text/` et non `Memoire_QESS/`. Je l'ai découvert quand j'essayais de compiler mes documents: pour appeler mes données/images et autre, je faisais `"sous-dossier/fichier.ext"` sauf que ça fonctionnait pô. Il fallait mettre le chemin entier, depuis le big dossier, soit `"~/Memoire_QESS/sous-dossier/fichier.ext"`, ou plus simplement `"../sous-dossier/fichier.ext"`.

Du coup, ce fameux dossier `/text/` était organisé de cette manière:
```plaintext
text /
├── biblio.bib
├── asrlf.csl
├── 00_page_de_garde.Rmd
├── 01_intro.Rmd
├── ...
├── main.Rmd
└── preamble.tex
```
Les pdf compilés, beaux et propres, sont immédiatement sauvegardés dans ce dossier.

`main.Rmd` est le fichier qui regroupera tous les chapitres, je n'ai pas écrit de contenu d'analyse dedans. Comme ça on peut écrire les chapitres séparément au lieu de tout dans un long Rmd, c'est plus propre et, personnellement, ça m'aidait à séparer qu'est-ce qui allait dans quoi.

## Les packages LaTeX utilisés {#pkg-latex}

Dans le fichier `preamble.tex`, mettre tous les packages LaTeX utilisés. Permet de tout concentrer ici, plus cool pour les yeux. On retrouvera ce fichier dans l'en-tête YAML de `main.Rmd`: pendant la compilation, les packages seront écrits dans le fichier .tex intermédiaire.

{{% callout note %}}
Ca n'empêche pas de mettre les packages dans les chapitres intermédiaires, pour avoir une idée de quel visage ça aura à la fin.
{{% /callout %}}

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

% pour je sais pas quoi
\usepackage{hyperref}
\hypersetup{
    pdftitle = {Nom qui apparaîtra sur le lecteur pdf},
    pdfauthor = {Mon Nom}
}
```

## Assembler le document

Voir le résultat de ses efforts!

### L'en-tête YAML

Dans l'en-tête du fichier `main.Rmd` qui regroupera tous les chapitres rédigés séparément, j'ai précisé:

* le type de document (pdf), les packages $\LaTeX$ à charger, et l'application de la numérotation des sections;
* la taille des marges, la police et sa taille, l'interligne;
* la taille de la feuille (j'ai remarqué assez tard que la taille par défaut est celle du format lettre);
* le fichier `.bib` avec mes références et le fichier `.csl` du langage de référencement.

Voilà à quoi ça ressemble:

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

Dans les fichiers des chapitres (quoiqu'on peut aussi répliquer celui du fichier principal, notamment pour les packages latex):

```yaml
---
title: "Revue de la littérature"
output: pdf_document
geometry: margin=2.5cm
fontsize: 12pt
fontfamily: fourier
linestretch: 1.5
indent: true
papersize: a4
header-includes:
  - \usepackage{tikz}
  - \usetikzlibrary{positioning}
---
```

{{% callout warning %}}
Ceux-ci ne seront pas pris en compte dans la compilation finale de `main.Rmd`.
{{% /callout %}}

### La page de garde

Propre à mon université.

Page de garde sauvegardée dans un fichier Rmd, `00_page_de_garde.Rmd` mais pas écrite avec langage R/Markdown. Format Rmd supporte les commandes LaTeX si le fichier généré est un fichier pdf, et c'est ce que j'ai fait parce que c'était plus "facile" (gros guillemets bien épais, c'est dur LaTeX) de construire ma page comme je le souhaitais. Ces commandes fonctionnent que si doc final est un pdf, ne fonctionnent pas si `output: html_document` par exemple.

Alternativement, faire un document pur latex et ajouter dans l'en-tête `before_body: "page_de_garde.tex"` après `in_header: "preamble.tex"`.

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

{\LARGE\bfseries Pigeons love doves\par}

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

J'ai tout de suite ajouté une page de remerciements et une page de dédication (pas montrée ici), numérotées avec des chiffres romains pour les distinguer du corps du mémoire.

Problème avec les sections non-numérotées: n'apparaît pas dans la table des matières + mauvaise section liée [mieux expliquer]. "Régler" par:

```latex
\phantomsection
\addcontentsline{toc}{section}{La section}
\section*{La section}
```

### Le corps du texte

Un fichier `.Rmd` par chapitre (ne pas oublier de mettre titre de niveau 1 au début!). Ajout des packages LaTeX utilisés pour chaque chapitre dans l'en-tête YAML sous `header-includes:` ou bien directement `preamble.tex` comme vu plus haut + options de marge, de police, etc. J'écris et je construis le fichier pdf pour m'assurer que ça fonctionne bien.

### Bibliographie

[Voir ici](https://stateofther.github.io/finistR2020/r_citr.html) pour meilleure explication.

Ingrédients: Zotero, Better BibTeX, package `citr`.

Better BibTeX: sélectionner l'option "garder à jour" quand j'exporte mes références pour que ces dernières soient automatiquement mises à jour si je change quelque chose.

Langage de référencement par défaut Chicago, ne me convenait pas car était en anglais, ne montrait pas certains éléments ou en montrait trop, et parce que j'ai l'habitude du système Harvard. Du coup, j'ai choisi le modèle de [l'Association de Science Régionale de Langue Française](https://editor.citationstyles.org/styleInfo/?styleId=http%3A%2F%2Fwww.zotero.org%2Fstyles%2Fassociation-de-science-regionale-de-langue-francaise) parce qu'il me plaisait. Personnaliser légèrement pour que "et" apparaisse au lieu de "&" si deux auteurs en modifiant le fichier CSL (pour _Citation Style Language_) avec [ce site](https://editor.citationstyles.org/about/). Voir [ce site](https://www.zotero.org/styles) pour d'autres styles.

```yaml
bibliography: biblio.bib
csl: asrlf.csl
```

Dans le texte, `@ref` pour Auteur (an) ou `[@ref]` pour (Auteur, an), `[@ref1; @ref2]` pour plusieurs références à la fois entre parenthèses, ou `[-@ref]` pour avoir que l'année si auteur mentionné au préalable. Dans RStudio, j'utilise l'addin de `citr` pour ajouter les citations, à partir d'une interface interactive. Besoin de préciser le fichier `.bib` dans l'en-tête pour que le package trouve les références. 

Tricoter le fichier, références se trouvent automatiquement à la fin du document. Pas un problème si pas d'annexes ajoutées, problème dans le cas contraire. Pour imprimer les références à l'endroit qu'on veut, ajouter `<div id="refs"></div>` dans `main.Rmd`.

```latex
\newpage
\phantomsection
\addcontentsline{toc}{section}{Références}
\section*{Références}

<div id="refs"></div>
```

### Appendices

Ca m'a cassé la tête. Besoin du package `appendix` de $\LaTeX$ et de l'environnement `appendices` pour signaler le début des annexes. 

Dans le preamble, `\usepackage[toc,title,page]{appendix}` dit qu'on génère un titre "Appendices" au début de l'environnement, que ce titre apparaisse dans la table des matières et que x [page?]. J'ai dû redéfinir le mot qui apparaît au début de chaque section car clash avec ce que j'avais défini [dans le preamble](#pkg-latex).

```latex
% dans le preamble
\usepackage[toc,title,page]{appendix}

% dans le fichier main.Rmd (ou 99_annexe.Rmd)
\begin{appendices}

\titleformat{\section}{\Large\bfseries}{Annexe \thesection.}{1em}{}

\section{Chapitre}
\subsection{Annexe A1}
\subsection{Annexe A2}

\end{appendices}
```

## Tout compiler

Sous [l'en-tête](#len-tête-yaml) de `main.Rmd`, tout ça. 

**Important**: les en-têtes YAML propres à chaque fichier `.Rmd` ne sont pas pris en compte dans la construction du document final. Seul celui du fichier principal compte.

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

```{r child="01_intro.Rmd"}
```

\newpage
```{r child="02_lit_review.Rmd"}
```

# tous les autres chapitres

\newpage
\phantomsection
\addcontentsline{toc}{section}{Références}
\section*{Références}

<div id="refs"></div>

\newpage
\singlespacing
```{r child="99_annexe.Rmd"}
```
````

Pour tricoter le fichier sans utiliser la souris, `Ctrl + Maj + K` !

## Pour résumer

Le fichier `main.Rmd`:

{{< gist pyrrhamide c716d95b34ce9871678d35801fd6b16e  >}}

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
