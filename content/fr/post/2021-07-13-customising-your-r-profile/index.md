---
title: Changer son .Rprofile
author: KF
date: '2021-07-13'
slug: changer-son-r-profile
categories: [R, tutoriel]
tags: [loisirs créatifs]
summary: 'Changer la manière dont R dit bonjour, en utilisant le fichier `.Rprofile`. Plus sympa et met du baume au coeur.'
featured: no
image:
  caption: 'Coucou - généré par [carbon](https://carbon.now.sh/)'
  focal_point: ''
  preview_only: no
projects: []
toc: false
---

Je naviguais sur Twitter, puis je suis tombée sur [ce tweet](https://twitter.com/djnavarro/status/1407971934021713920?s=20) de Danielle Navarro.

<center>
<blockquote class="twitter-tweet"><p lang="en" dir="ltr">I edited my .Rprofile to show me messages that I actually like reading on startup <a href="https://t.co/3d74PDuKe1">pic.twitter.com/3d74PDuKe1</a></p>&mdash; Danielle Navarro (@djnavarro) <a href="https://twitter.com/djnavarro/status/1407971934021713920?ref_src=twsrc%5Etfw">June 24, 2021</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script> 
</center>

J'étais immédiatement captivée et partante pour avoir la même chose. 
 
Les messages de base de la console dans R à chaque nouvelle sessions ne sont pas palpitants du tout. Je pensais que c'était la vie, jusqu'à ce que ce tweet apparaisse sous mes yeux. Danielle a partagé son fichier [`.Rprofile`](https://gist.github.com/djnavarro/0fa53868439f8db604fcd23bbef01288) dont je me suis inspirée. La commande `usethis::edit_r_profile()` ouvre le fichier `.Rprofile` dans un nouvel onglet (RStudio) ou dans un éditeur de texte (quand je lance la commande sur VS Code).

Je suis restée dans la simplicité: j'ai gardé l'affichage de la version actuelle de R, et j'ai ajouté une citation qui reste bloquée dans ma tête, la date du jour et le signe astrologique du moment :star:

J'ai laissé la commande de Danielle qui montre le nom de la branche utilisée quand je travaille sur un projet à version contrôlée.

```r
if(interactive()) {
  
  cat("\014") # clear screen
  cli::cli_text("")
  cli::cli_text(paste0(R.version$version.string,
                       " - ",
                       R.version$nickname))
  cli::cli_text("")
  
  cli::cli_text(paste0("Come on and ",
                       cli::col_cyan("SLAM"),
                       ", and welcome to the ",
                       cli::col_yellow("JAM"),
                       "!"))
  cli::cli_text("")
  
  cli::cli_text(
    paste0(
      stringr::str_to_title(weekdays(Sys.Date()))," ",
      format(Sys.Date(), format="%d")," ",
      months(Sys.Date())," ",
      format(Sys.Date(), format="%Y"),
      " ~ it's ",
      DescTools::Zodiac(Sys.Date()),
      " season!"
    )
  )
  cli::cli_text("")
  
  # customise the prompt
  prompt::set_prompt(function(...){
    branch <- (purrr::safely(gert::git_branch))()
    if(is.null(branch$result)) return("> ")
    return(paste0("[", branch$result, "] > "))
  })
}
```

J'ai fait mes changements, les ai sauvegardés et ai redémarré R (`Ctrl + Shift + F10`). J'ai remarqué que les emojis n'apparaissaient pas sur ma console, c'est pas top. 

Voici comment R me dit bonjour:

![](rprofile.png)

## AJA

* Pas d'apostrophe toute seule dans les en-têtes YAML, okay?
* Faut la doubler pour que ça boude pas, dak?
