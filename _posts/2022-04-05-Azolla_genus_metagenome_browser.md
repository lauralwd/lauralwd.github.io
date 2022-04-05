---
title: "Browsing the contigs of the _Azolla_ genus-wide metagenome"
excerpt_separator: "<!--more-->"
categories:
  - Blog
tags:
  - blog
  - Azolla
  - metagenome
  - data analysis
  - bioinformatics
  - genomics
classes: "wide"
---
For my project on the _Azolla_ genus-wide metagenome, I wanted to simultaneously visualise many metagenome assemblies.
"<!--more-->"
The visualisation should allow assessing (dis)similarities amongst the species' metagenomes.
The resulting figure is an interactive R shiny app.
This app takes multiple metagenome assemblies classified by [CAT]() and plots the contigs by length, depth and taxonomy.
The colours represent the approximate taxonomy of anyone contig.
I included some filters to remove any noise from the taxonomies assessed, as well as tools to zoom in on specific regions.
Have fun exploring the _Azolla_ genus metagenome:

<iframe height="8000" width="100%" frameborder="no" src="https://utrecht-university.shinyapps.io/Azolla_genus-wide_metagenome_taxonomy/"> </iframe>

