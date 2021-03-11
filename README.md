## Current Opinions of Joel Berendzen

![DNA/tree](dna+tree.png)

**"Follow the data!"** was the dictum my advisor, 
[Hans Frauenfelder](https://en.wikipedia.org/wiki/Hans_Frauenfelder)
gave me more than 30 years ago as I was contemplating what to do after I finished my PhD.
At the time Hans said that, biology wasn't especially data-rich, but by 2021 the situation
has changed.  For example, a a premier astronomical observatory, the Webb Space Telescope, 
is expected to produce around 200 TB of data per year when it comes online in 2022.  A 
premier biological sequence observatory, the Broad Institute, has been producing that has 
been producing that much data *per month* for the last few years. 
Moreover, the number of labs with sequencers in them is a lot larger---and growing faster--than
the number of labs with telescopes.  It's not just sequencers, either; there are sizeable
data flows from protein crystallography at synchrotron beamlines worldwide, and there's
about to be huge streams coming out of microscopy-driven projects such as NIH's BRAIN
Initiative.  Biology has quietly become the most data-intensive science of the Age of 
Big Data.

Here's a short [essay on creating the Theory of Biology](theory.md)
by building bridges among  data, signatures, models, and applications.

Much of my recent work has been on genomic sequences.  Here are some 
[thoughts on bioinformatics and bioinformaticians](bioinformatics.md).

I author software to analyze data, and I try to write for the future as well as
to solve particular problems today.  If I do my part well, my efforts are to be a 
model and example, not just a means to an end.  Here are my 
[thoughts on writing scalable software](software.md).  

I have published 
[roughly 50 papers with over 12,000 citations](https://bit.ly/JoelBerendzen) that
explore the interrelations among equences, structures, gene-family trees, dynamics,
and hydration.

Here is an overview of some of my **code repositories** and other places where I've contributed:

- [azulejo](https://github.com/joelb123/azulejo) combines guilt-by-profiling (genome synteny) 
  and guilt-by-association (phylogeny) to create pangenomic collections of gene families and
  tile phylogenetic space with supertrees of proxy genes.  Uses out-of-memory external merges
  and a novel "peatmer" algorithm to achieve linear scaling with a small memory footprint to 
  accomodate large numbers of input genomes.
- [click_loguru](https://github.com/joelb123/click_loguru) Most scientific code needs a CLI and
  benefits from logging to a file.  This repository combines those two needs and is the starting
  point for most of my active codes.
- [pytest-datadir-mgr](https://github.com/joelb123/pytest-datadir-mgr) Most scientific code needs
  to test data that are too large to be kept in the repository.  The code in 
  this repository makes downloading input data and saving intermediate results an easier task.
- [pybio Gentoo Overlay](https://github.com/joelb123/pybio) Computational biologists need a
  development distro, and for years mine has been [Gentoo](https://gentoo.org) i
  because of the large number of
  biology-related packages and because its a source-code distribution.  (For production and
  container use, I like [Clear Linux](https://clearlinux.org) for its performance and update
  properties.)  This private repo contains another 100 or so packages that I find useful on top of 
  the 200 in the main tree and the 300 in the 
  [Science overlay](https://wiki.gentoo.org/wiki/Project:Science/Overlay).
- [aakbar](https://github.com/joelb123/aakbar) This Amino Acid K-mer calculator can be used to 
  calculate signature peptides by phylogenetic or other means.  Its output can be used in Sequedex
  or other signature methods.  Its input can be raw proteomes, but it's better with sets of proxy
  genes from azulejo.
- [alphabetsoup](https://github.com/joelb123/alphabetsoup) Parallel data wrangling of input sequences,
  including alphabet checking and removal of some ugly but common artifacts.
- [biofletch](https://github.com/joelb123/biofletch) Unfinished alpha code for a Apache Arrow/Parquet-
  based replacement of FASTA and GFF files. Some of the useful bits were incorporated in azulejo. 
- [Sequedex](https://sequedex.lanl.gov) is award-winning software that uses scalable signature methods
  to classify short DNA sequences as to where they come from and what they do.  Sequedex is mostly used
  in metagenomics and surveillance for emergent infectious diseases. The Sequedex open-source repository
  is [here](https://github.com/lanl/sequedex-core).  
- [SOLVE](https://solve.lanl.gov) is award-winning software that helps automate the problem of phasing
  X-ray crystal structures of proteins.  It calculates a statistic that acts very much like autofocus 
  on a camera.  SOLVE is closed-source.

