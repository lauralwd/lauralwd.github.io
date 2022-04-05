---
permalink: /science/
layout: single
title: "Science"
toc: true
toc_label: "On this page"
toc_icon: "cog"
toc_sticky: "true"
---

As an academic in training, I aim to contribute to future food security by working on a highly productive plant and novel crop: _Azolla_.
In our _Azolla_ research group, we benefit from each others' specialities; 
I support my colleagues with a [computational infrastructure](/blog/post-mpp-server), technical issues and intricacies of data analysis for example. 
Additionally, I take joy in initiating a lot of our lab's outreach activities. 
{: .text-justify}


# The Azolla metagenome
In my PhD project, I work on the metagenome (genomes of bacteria associated with) the amazing fern _Azolla_. 
_Azolla_ is an aquatic floating fern, known for its high growth rate and protein content.
Both these propperties, are a direct consequence of the special symbiosis _Azolla_ has with a nitrogen fixing cyanobacterium inside its leaves.
When domesticated propperly, the _Azolla_ symbiosis can be a sustainable protein source grown in temperate regions,
thereby reducing the need for imported protein grown in tropical regions.
{: .text-justify}

## Foul play in the pocket
In the Foul play in the pocket paper, my colleagues and I stumbled upon not one, but multiple bacteria residing inside the leaves of _Azolla_.
Whilst the known symbiont is by far the most abundant, multiple other microbes seem to be present systematically too. 
Similar bacteria were found to be present in other species of the _Azolla_ genus.
These groups of bacteria are known to also fix nitrogen in some symbiosis, but genomic and nitrogen isotope experiments pointed out that this is not the case here in _Azolla_.
[Is there foul play in the leaf pocket? The metagenome of floating fern Azolla reveals endophytes that do not fix N2 but may denitrify](https://doi.org/10.1111/nph.14843)
{: .text-justify}

![lowly abundant microbes in _Azolla_ species](https://nph.onlinelibrary.wiley.com/cms/asset/a5839007-9b9b-4a24-9145-6407ee51562f/nph14843-fig-0002-m.jpg)
_See our publication linked above for more details_
{: .text-justify}

## Partners & Passengers
The partners & passengers manuscript focusses on systematically acquiring microbial genomes associated with the _Azolla_ genus as a whole.
More details will follow later once the manuscript is released as a preprint.
Until then, the methods used towards this end are documented openly on [GitHub here](https://github.com/lauralwd/azolla_genus_metagenome).
Additionally, one of the interactive main figures is available online too: [Azolla genus-wide metagenome browser](/blog/AGMB). 
{: .text-justify}

![The _Azolla_ genus-wide metagenome](/assets/images/Azolla_genus_metagenome-order.png)
Overview of the _Azolla_ genus-wide metagenome. 
Contig of SPAdes metagenome assemblies were assigned taxonomy with CAT and visualised side by side by their length, abundance and taxonomy.

## Phylogeny workflow
My projects primary interest is the microbial community associated with _Azolla_, yet I also work with colleages to understand more about the physiology of the fern itself.
One way I help, is infering the evolutionary history of certain proteins of our interest.
While learning to reliably infer such a phylogeny, I decided to document my workflow for others, and published this on [Github](https://github.com/lauralwd/lauras_phylogeny_wf).
With this workflow, students, colleagues, or anyone else, can use my methods to infer their phylogeny.
This work was achieved only with generous help and advice from the Berend Snel group.
Some examples of how I used this workflow myself, are also on GitHub. 
These are examples of how I learn to practice Open Science, documenting all my actions, analyses, log files, notes and mistakes using jupyter notebooks and version control software Git.
{: .text-justify}

 - LAR phylogeny [GitHub](https://github.com/lauralwd/LAR_phylogeny_gungor-et-al-2020/tree/v1.00.00) [Zenodo](https://doi.org/10.5281/zenodo.3959057)
 - MIKC phylogeny [GitHub](https://github.com/lauralwd/MIKC_tree) [Zenodo](https://doi.org/10.5281/zenodo.4564374)
 - R2R3 MYB phylogeny [GitHub](https://github.com/lauralwd/azolla_MYBs) [Zenodo](https://doi.org/10.5281/zenodo.4564441)

The LAR phylogeny was published in [Güngör _et al._ (2021)](https://doi.org/10.1111/nph.16896)
![LAR phylogeny](https://media.githubusercontent.com/media/lauralwd/LAR_phylogeny_gungor-et-al-2020/main/figures/main_text/LAR_orthogroup_selectionv1_guide_v5_17cm_8pts_shalrt_circular_v4.2_brackets_600.svg.png)

## Far-red light induces the _Azolla filiculoides_ symbiosis sexual reproduction
This packed paper summarises the work of many graduate students I have had the pleasure to supervise over my years as a PhD candidate.
Simultaneously, it documents how my colleagues in the _Azolla_ lab found their way in doing bioinformatics analyses themselves.
The narrative centers around an RNA-seq experiment profiling both the host fern and the cyanobacterial symbiont in transition to sexual reproduction.
My contributions were the MIKC and MYB phylogenies linked above, some photography and the supervision of (under)graduate students who contributed data underlying most tables and figures.
Additionally, I designed and maintained the computational infrastructure on which the _Azolla_ lab members learned and performed their bioinformatics experiments.
{: .text-justify}

<figure style="width: 200px">
  <img src="https://www.frontiersin.org/files/Articles/693039/fpls-12-693039-HTML-r2/image_m/fpls-12-693039-g001.jpg" alt="">
  <figcaption>_Azolla_ shoot apical meristem and cyanobacterial colonies.</figcaption>
</figure>
My favourite figure from this paper is not one of my own, but a confocal image made by Valerie Buijs.
These images show the _Azolla_ shoot apical meristem with colonies of _Nostoc azollae_ (a).
Not how leaf primordia encapsulate part of this meristem colony thereby innoculating the leaf pockets / leaf cavities (a; lc).
We assume that specialised trichomes are essential in maintaining the symbiosis (b; t).
Young fern spores -spore initials- are also innoculated in this early stage, taking _N. azollae_ from the apical colony (c)
{: .text-justify}
