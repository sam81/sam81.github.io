--- 
title: "A R Guide"
subtitle: "0.3.10"
author: "Samuele Carcagno"
date: "14 aprile, 2020"
site: bookdown::bookdown_site
output: bookdown::gitbook
documentclass: memoir
classoption: twoside
fontsize: 12pt
bibliography: [R.bib]
link-citations: yes
github-repo: sam81/rguide
geometry: margin=3.4cm
always_allow_html: yes
description: "A guide for the R programming language."
---
\pagestyle{bodypagestyle}


```{=latex}
\setlength{\parindent}{0pt}
\rule{\textwidth}{0.5mm}
```

[A R Guide](http://samcarcagno.altervista.org/soft/r.html) by Samuele Carcagno <sam.carcagno@gmail.com> is licensed under a [Creative Commons Attribution-ShareAlike 4.0 International License](http://creativecommons.org/licenses/by-sa/4.0/).

![Creative Commons License](images/by-sa_88x31.png)

Based on a work at https://github.com/sam81/rguide.

The latest html version of this guide is available at https://sam81.github.io/r_guide_bookdown/preface.html

A pdf version of this guide can be downloaded at https://sam81.github.io/r_guide_bookdown/rguide.pdf

Associated datasets available at https://sam81.github.io/r_guide_bookdown/rguide_datasets.zip

```{=latex}
\rule{\textwidth}{0.5mm}
%\vspace{1cm}
\newpage
% Trigger ToC creation in LaTeX
\begin{KeepFromToc}
  \tableofcontents
\end{KeepFromToc}
```

# Preface {-}

I started writing this guide in 2006 while I was learning R. It was always a work in progress, with incomplete bits and parts, and it wasn't updated for several years. Much as changed since then both in the R ecosystem, and in the way I use R for data analysis. I'm now in the process of revising it to reflect these changes. Because this guide was initially written when I still had very little knowledge not only of R but of programming in general, it tends to explain things with a very simple, beginner-friendly approach. I hope to maintain this aspect of the guide as I revise it. This remains very much a work in progress and  comes with NO WARRANTY whatsoever of being correct in any of its parts.

For me, this is like a R desk reference. Once I figure out some new useful function, how to solve a particular problem in R, or how to generate a certain graphic, I write it down in this guide. Next time I have to solve the same problem I quickly know where to find the answer, rather than wading through internet forums, stack overflow, or other websites hunting down the answer.
