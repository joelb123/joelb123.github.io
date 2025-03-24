## Current Opinions of Joel Berendzen

![DNA/tree](images/dna+tree.png)

**"Follow the data!"** was the dictum my advisor, [Hans Frauenfelder](hans.md)
gave me more than 30 years ago as I was thinking about what to do post-PhD.
At the time Hans said that, biology wasn't especially data-rich, but now
the situation has changed.  For example, the Webb Space Telescope is expected
to produce around 200 TB of data per year.  A premier biological sequence
observatory, the Broad Institute, has been producing that has been producing
that much data *per month* for the last few years. It's not just sequencers, 
either; there are sizeable flows from protein crystallography, and there's about
to be huge streams coming out ofmicroscopy-driven projects such as the Human Brain
Mapping Initiative.  Biology has quietly become the most data-intensive science
in the Age of Big Data.

# Essays

Currently (early 2025), I"m writing more code than words, but here are some
short-form essays on topic I have thought about:

- Throughout my career I've worked on [motions in proteins](dynamics.md). I've returned
  to this work lately with a new phylogeny-based perspective.

- I believe in [creating the Theory of Biology](theory.md)
  by building bridges among  data, signatures, models, and applications.

- Some of my work has been on genomic sequences.  Here are some 
  thoughts on [bioinformatics and bioinformaticians](bioinformatics.md).

- I create software to analyze data, and I try to write for the future as well as
  to solve particular problems today. Here are my thoughts on
  [writing scalable scientific software](software.md).  

# Papers

I have published 
[roughly 50 papers with over 13,000 citations](https://bit.ly/JoelBerendzen) that
explore the interrelations among sequences, structures, gene-family trees, dynamics,
and hydration.

# Code

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
  to test using data files too large to be kept in the repository.  The code in 
  this repository makes downloading input data and saving intermediate results an easier task.
- [aakbar](https://github.com/joelb123/aakbar) This Amino Acid K-mer calculator can be used to 
  calculate signature peptides by phylogenetic or other means.  Its output can be used in Sequedex
  or other signature methods.  Its input can be raw proteomes, but it's better with sets of proxy
  genes from *azulejo*.
- [alphabetsoup](https://github.com/joelb123/alphabetsoup) Parallel data wrangling of input sequences,
  including alphabet checking and removal of some ugly but common artifacts.
- [Sequedex](https://sequedex.lanl.gov) is R&D100 award-winning software that uses scalable signature methods
  to classify short DNA sequences as to where they come from and what they do.  Sequedex is mostly used
  in metagenomics and surveillance for emergent infectious diseases. The Sequedex open-source repository
  is [here](https://github.com/lanl/sequedex-core).  
- [SOLVE](https://solve.lanl.gov) is R&D100 award-winning software that helps automate the problem of phasing
  X-ray crystal structures of proteins.  It calculates a statistic that acts very much like autofocus 
  on a camera.  SOLVE is closed-source.

You can comment on this page or reach me on [BlueSky](https://bsky.app/profile/joelb123.bsky.social).
