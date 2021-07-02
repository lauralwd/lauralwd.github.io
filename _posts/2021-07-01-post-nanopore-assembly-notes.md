---
title: "My notes on assembling nanopore data"
excerpt_separator: "<!--more-->"
categories:
  - Blog
  - technology
tags:
  - blog
  - technology
  - bioinformatics
  - Linux
  - Bash
toc: true
---

In this post, I adocument the _Azolla_ lab protocol for nanopore DNA metagenomics sequencing.
<!--more-->
It should be updated kind-of-regularly when applicable, and may be used by future _Azolla_ lab members or anyone else interested in starting nanopore sequencing.
# DNA extraction
For DNA extraction of _Azolla_ ferns I use a protocl from nanopore itself: "High molecular weight gDNA extracted from fever tree leaves (Cinchona pubescens)".
I don't think I can post this protocol here online, but [this talk](https://nanoporetech.com/resource-centre/fever-tree-extracting-and-preparing-dna-cinchona-pubescens) 
and [this preprint](https://www.biorxiv.org/content/10.1101/783159v1) should already be very informative.
The protocol itself can be downloaded from the nanopore community website [here](https://community.nanoporetech.com/extraction_methods#plant).
Naturally, use only the best and healthiest plant material you can find to start this protocol
The only main modification I made is grinding the plants while submerged in liquid nitrogen, rather than having a mortar in an ice bucket.
This warrants extra care though when moving the ground plant powder to the Carlson lysis buffer. 
If any liquid nitrogen remains in the Carlson buffer, this will explosivelly evaporate in the warm buffer.

Finally, I found my DNA extractions dissolve very poorly. 
Weeks at roomtemperature was not sufficient, and only when heating up to 50 or 60 degrees celcius I was able to get the full pellet into solution.
When insolution, this protocol yielded me several micrograms of DNA.
This was enough to process in a cicrulomics short read eliminator kit.
But poor elution of the DNA does warrant caution, perhaps some secondary metabolites or proteins are still contaminating this DNA extraction method for _Azolla_ species.

# Nanopore sequencing
Before starting a library prep, I always do a quick hardware check of the mininon device in the minknow software.

Then, after using the high throughput library kit, load your flowcell according to procol instructions.
We often re-use flowcells, always do a flowcell check before sequencing, and once you have placed it in the minion device.
In setting up your sequencing settings, I recommend to skip basecalling, or select fast-basecalling if you wan't that information directly.
The basecalling process is very intense, and will demand all of the servers resources. 
I prefer to have some resources available at all times and don't overload the CPUs.

## basecalling
When the sequencing run has finished, the raw signal in fast5 files needs to be converted to fastq files with nucleotide information.
This process is called basecalling, and our mpp-server is not well equiped for this process.
Frankly, a CPU is not well equiped for this process, but a GPU is.
The mpp-server does not have a GPU recent enough to support the basecalling process, so I do this at my home computer.
Just contact me and I'll happily basecall them for you.

At home, I use a recent guppy basecaller installed in my home directory, and the "super accurate basecalling model".
The basecaller runs on a GeForce RTX 2060 with 6GB ram memmory. 

```
~/bin/ont-guppy/bin/guppy_basecaller \
      -i <...input-folder-with-fast5-files...> \
      -s <...output-folder...>                 \
      --compress_fastq                         \
      -x "cuda:0"                              \
      -c ~/bin/ont-guppy/data/dna_r9.4.1_450bps_sup_prom.cfg \
      --gpu_runners_per_device 8               \
      --num_callers 8                          \
      --chunk_size 700                         \
      --chunks_per_runner 512
```

### guppy optimisation yielded hardly any improvement
For the sake of efficiency, I did a few runs to see if guppy basecalling could be optimised.
Theoreticall, parameters can be calculated with the formula `runners * chunks_per_runner * chunk_size < 100000 * [max GPU memory in GB]`
However, I found I can easily go over that without any penalty but also not big improvements.
I did a couple of test runs with a single fast5 file straight out of minknow.
When choosing parameters that were to high, I would get an error that not enough memmory could be allocated.
In all other cases, memmory use did not go over 71% of the GPU.
PICe bandwith usage was always about 80 to 84%, and GPU usage between 95 and 99%.

|Setting		|run1	|run2	|run3	|run4	|run5	|run6	|run7	|
|---			|---	|---	|---	|---	|---	|---	|---	|	
|runners_per_device	|2	|4	|4	|4	|8	|8	|2	|
|chunk_size		|1000	|1000	|1000	|1000	|500	|700	|1000	|
|chunks_per_runner	|410	|200	|300	|400	|512	|512	|256	|
|time (ms)		|129916	|131365	|136841	|130577	|133349	|127237	|129606	|
|time (min)		|2.17	|2.19	|2.28	|2.18	|2.22	|2.12	|2.16	|
|GPU mem (%)		|69	|71	|60	|61	|59	|59	|59	|
|theoretical ram (GB)	|8.2	|8	|12	|16	|20.48	|28.672	|5.12	|

All in all, using a GPU for basecalling is an enormous improvement over CPU basecalling.
But further than that, I can't seem to optimise the process furter, at least not on my consumer grade hardware.

# Assembly
When doing a first assembly, `flye` will do great. 
It's very fast, and in my limited experience does a good job.
When optimising, perhaps `canu` is also worth a try.
Both are installed in a conda environment, activate on the mpp-server like so

`conda activate /opt/miniconda/miniconda3/envs/canu`

## self correcting reads
Should you?
Flye recommends no, canu recommends yes.

## trimming and QC
comming up

## nanopore only assembly

### flye
A standard flye assembly commandline would like this

```
flye --nano-raw /stor/sequencing_files/<...yourfastq...>.fastq.gz \
      --genome-size <genomesize like 6M or 48K or 7G>              \
      --threads $(nproc)                                          \ 
      --out-dir ~/<...your output directory...>
```

I recommend using all threads available like written above (provided these are not in use by someone else) and I recommend to write to the ssd.
Your home directories are on the SSD, and there's also a `/fast` directory you could use to temporarily write stuff to.
Just make sure you move your assembly to `/stor` afterwards, so we keep the SSD clean for everyone.


### canu
This command is usually my starting point for assembling with canu.

```
canu -assemble -nanopore-raw <...nanopore_sup.fastq.gz...> \
     -d <...output_directory...>                           \
     -p <...output_prefix...>                              \
     genomeSize=<similar as for flye>                      
```

optionally, add parameters like so:

```
     ErrorRate=0.1                                         \
     -minReadLength=8000                                   \
     -minOverlapLength=4000                                \
```

## Hybrid assembly
My tool of preference is SPAdes.

# Assembly polishing
Likely quiver

# synteny
mummer -> circos

mauve

# annotatioan of prokaryotic genomes
prokka

bakta

ncbi pipeline
