---
title: "Post: My notes on assembling nanopore data"
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
---

In this post, I aim to document the _Azolla_ lab protocol for nanopore DNA metagenomics sequencing.
It should be updated kind-of-regularly when applicable, and may be used by future _Azolla_ lab members or anyone else interested in starting nanopore sequencing.
Since the protocols in the lab are well done and provided by oxford nanopore, I choose to focus here on the analysis part of the workflow.

# Nanopore sequencing
Before starting a library prep, I always do a quick hardware check of the mininon device in the minknow software.

Then, after using the high throughput library kit, load your flowcell according to procol instructions.
We often re-use flowcells, always do a flowcell check before sequencing, and once you have placed it in the minion device.
In setting up your sequencing settings, I recommend to skip basecalling, or select fast-basecalling if you wan't that information directly.
The basecalling process is very intense, and will demand all of the servers resources. 
I prefer to have some resources available at all times and don't overload the CPUs.

## basecalling
Ideally, after sequencing, you send the fast5 files (that's the files with the raw signal) to me.
I'll do the basecalling at home on a GPU, this will be more accurate, and way faster as well.
We don't have a decent enough GPU in the mpp-server as of now, but we may get one in the future.

If you send the fast5 files to me, I'll run these at home.
I use a recent guppy basecaller installed in my home directory, and the "super accurate basecalling model".
The basecaller runs on a GeForce RTX 2060 with 6GB ram memmory. 
The chunksize and runner settings can be optimised further, but the speed increase is already great as it is.

```
~/bin/ont-guppy/bin/guppy_basecaller \
      -i <...input-folder-with-fast5-files...> \
      -s <...output-folder...>                 \
      --compress_fastq                         \
      -x "cuda:0"                              \
      -c ~/bin/ont-guppy/data/dna_r9.4.1_450bps_sup_prom.cfg \
      --gpu_runners_per_device 2               \
      --chunk_size 500                         \
      --chunks_per_runner 256                  \
      --runners_per_device 4
```
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
