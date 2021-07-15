---
title: écrire son mémoire avec RMarkdown
draft: true
author: KF
date: '2021-07-20'
slug: ecrire-son-memoire-rmarkdown
categories: []
tags: []
summary: 'J''ai écrit mon mémoire de M2 avec RMarkdown. Je partage maintenant ce que j''ai fait, parce que c''est du travail qu''il ne faudrait pas oublier tellement il était intense.'
featured: no
image:
  caption: '[Photo by Ken Suarez on Unsplash](https://unsplash.com/photos/4IxPVkFGJGI)'
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

Cette idée de faire mon mémoire sur RMarkdown m'a honnêtement mordue dans le cul. J'ai appris (assez rapidement) que [le répertoire de travail d'un fichier rmd est le dossier dans lequel ce fichier est sauvegardé](https://bookdown.org/yihui/rmarkdown-cookbook/working-directory.html), et non pas le dossier de mon projet R. Pour moi, c'était donc `/text/` et non `Memoire_QESS/`. Je l'ai découvert quand j'essayais de compiler mes documents: pour appeler mes données/images et autre, je faisais `"sous-dossier/fichier.ext"` sauf que ça fonctionnait pô. Il fallait mettre le chemin entier, depuis le big dossier, soit `"~/Memoire_QESS/sous-dossier/fichier.ext"`.

Du coup, ce fameux dossier `/text/` était organisé de cette manière:
```plaintext
text /
├── 00_page_de_garde.Rmd
├── 01_intro.Rmd
├── ...
├── main.Rmd
└── preamble.tex
```
Les pdf compilés, beaux et propres, sont immédiatement sauvegardés dans ce dossier.

## Les packages LaTeX utilisés

Dans le fichier `preamble.tex`, mettre tous les packages LaTeX utilisés. Permet de tout concentrer ici, plus cool pour les yeux. On retrouvera ce fichier dans l'en-tête YAML de `main.Rmd`: pendant la compilation, les packages seront écrits dans le fichier .tex intermédiaire.

{{% callout note %}}
Ca n'empêche pas de mettre les
{{% /callout %}}

```latex
\usepackage{ragged2e}
\usepackage{setspace}
\usepackage{tikz}
\usetikzlibrary{positioning, trees}

\usepackage{caption}
\captionsetup[table]{name=Tableau}
\captionsetup[figure]{name=Figure}
```

## Assembler le document final 

### L'en-tête YAML

Dans le fichier `main.Rmd` qui regroupera tous les chapitres rédigés séparément.

```yaml
---
output: 
  pdf_document:
    includes:
      in_header: preamble.tex
    number_sections: true
geometry: margin=2.5cm
fontsize: 12pt
fontfamily: libertine
linestretch: 1.5
indent: true
---
```

Dans les fichiers des chapitres:

```yaml
---
title: "Revue de la littérature"
output: pdf_document
geometry: margin=2.5cm
fontsize: 12pt
fontfamily: libertine
linestretch: 1.5
indent: true
header-includes:
  - \usepackage{tikz}
  - \usetikzlibrary{positioning}
---
```

{{% callout warning %}}
Ceux-ci ne seront pas pris en compte dans la compilation finale de `main.Rmd`.
{{% /callout %}}

### La page de garde

Page de garde sauvegardée dans un fichier Rmd, `00_page_de_garde.Rmd` mais pas écrite avec langage R/Markdown. Format Rmd supporte les commandes LaTeX si le fichier généré est un fichier pdf, et c'est ce que j'ai fait parce que c'était plus "facile" (gros guillemets bien épais, c'est dur LaTeX) de construire ma page comme je le souhaitais. Ces commandes ne fonctionnent pas si `output: html_document` par exemple.

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

\uppercase{Mémoire de recherche}

\vfill 

\section*{Titre}
\subsection*{Sous-Titre}
\vfill
\textit{Soutenu par}

Moi

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

## Remerciements {-}
```

J'ai tout de suite ajouté une page de remerciements et une page de dédication (pas montrée ici), numérotées avec des chiffres romains pour les distinguer du corps du mémoire.

### Le corps du texte

Un fichier `.Rmd` par chapitre (ne pas oublier de mettre titre de niveau 1 au début!). Ajout des packages LaTeX utilisés pour chaque chapitre dans l'en-tête YAML sous `header-includes:` ou bien directement `preamble.tex` comme vu plus haut + options de marge, de police, etc. J'écris et je construis le fichier pdf pour m'assurer que ça fonctionne bien.


### Tout compiler

Sous [l'en-tête](#len-tête-yaml), tout ça. 

**Important**: les en-têtes YAML propres à chaque fichier `.Rmd` ne sont pas pris en compte. Seul celui du fichier principal compte.

````r
```{r child="00_page_de_garde.Rmd"}
```

\newpage
\tableofcontents

\newpage

\pagenumbering{arabic}

```{r child="01_intro.Rmd"}
```

\newpage
```{r child="02_lit_review.Rmd"}
```

\newpage
\singlespacing
\appendix
```{r child="99_annexe.Rmd"}
```
````

`Ctrl + Maj + K` 

## Bibliographie

## Mes notes

* Packages R qui n'ont pas fonctionné, par exemple `DiagrammeR` qui fonctionne qu'avec des fichiers HTML. M'a poussée à faire mes diagrammes en LaTeX pur, dans le corps de texte Rmd. Une aventure.
* Ce qui m'emmène au référencement des éléments du document: 
  * quand il s'agit de qqch que j'ai écrit en LaTeX, important de mettre `\label{lab}` après le titre. Référencement dans le doc se fait `\ref{lab}`. Easy peasy lemon squeezy.
  * quand il s'agit de qqch généré par RMarkdown (du style kable, ggplot, etc.), important de nommer les code chunks. Référencement dans le doc se fait `\ref{type:label-code-chunk}`, avec type `tab`, `fig` ou `eqn`.
* Mélange des langages dans les documents: franchement sympa, simplifie la vie quand on connaît la commande dans un langage mais pas dans l'autre. Cependant, devient rapidement très brouillon, e.g. page de garde. 


## Références pour cet article

* Etienne Bacher (2020), [Writing a Master’s thesis with R Markdown and Bookdown](https://www.etiennebacher.com/posts/2020-07-16-tips-and-tricks-r-markdown/).
* Anna (idk), [Write your dissertation in RMarkdown - Using RMarkdown to create complex pdf documents](https://ourcodingclub.github.io/tutorials/rmarkdown-dissertation/).
* Tyson Barrett (2018), [Writing Your Dissertation (or Thesis) in RMarkdown](https://tysonbarrett.com/jekyll/update/2018/02/11/r_dissertation/).
* Yihui Xie, Christophe Dervieux, Emily Riederer (2020), [R Markdown Cookbook](https://bookdown.org/yihui/rmarkdown-cookbook/).
