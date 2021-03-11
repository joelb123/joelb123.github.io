## Bioinformatics and Bioinformaticians

Data analysis, on a good day, is a creative process, especially in the questions
one poses of the data.  A good hypothesis starts from consideration
of what a plausible mechanism might be, and the most essential discriminatory element
is not what numerical technique one uses but rather the **tingle one gets at the top of
one's spine** when one gets to see clearly into a new view of nature.
Among quantitative types, bioinformaticians frequently have a reputation for intellectual
shabbiness, a reputation that is only partly deserved.  One reason is that it's a long
reach to connect sequence to physical or chemical insight. Another
reason is that sequencing has historically suffered from some pernicious correlated errors
buried fairly deep in the technologies, and the error modes of a given generation of
sequencing technology only get fully appreciated once the next generation of sequencing
is out.  But the biggest reason that bioinformatics gets disparaged
is the tendency of bioinformaticians to view a small number of software packages as the
only tools worth using.  If your toolbox only contains hammers, then every problem looks
like a nail.

### BLAST and Assembly are Usually the Wrong Approach
Let me admit that BLAST is software that implements a wide variety of algorithms, and that
it can be configured to implement a lot of different analysis strategies.  But the way
that most bioinformaticians use BLAST is to calculate sequence similarities.  As
a structural biologist, my view is that **sequence similarity is a bankrupt concept**
(as contrasted with sequence *identity*, which is the basis of phylogeny).  While the 
similarity matrices did describe the substitutions seen once in a set of protein sequences,
that was back in the day when
protein sequences were published semi-annually on dead trees, and those sequences were
not representative of anything other than what was easy to purify and align at the time.
If a part of the sequence is part of a catalytic site, the quantum-mechanical demands of
crossing a barrier are an accuracy of something like 0.01 nm or less, and either the
the correct residue to carry out the reaction is there where it needs to be or the
reaction doesn't proceed.  To put it another way, there is an enormous range of
functional pressures and mutation acceptance rates over a given sequence, and in 
some places it's "touch this and you die", while in others it's "if it still folds, it works".
Those pressures aren't even entirely in peptide space, because sequences have
to be packaged and expressed, creating functional pressures lurking in nucleic-acid space.
RNA-world structures live in an entirely different physical space with a different
grammar that characterizes stem/loop likelihoods.  Our models of RNA world aren't very
good just yet, and even if they were, they aren't incorporated into BLAST.  Yet
many bioinformaticians create a kind of religion around E-values, when the significance
of a given E value depend heavily on the functional pressures and phylogenetic distances
of the pair of sequences being aligned.  

Another nasty issue that codes for alignment and assembly have to deal with is that **some 
regions of sequence are low-complexity**.  In protein space, these may appear as
near-repeats like the piratical-sounding `...ARARARRAARRARAR...`.  Some repeats and 
near-repeats are unusually stable as nucleic acids due to unusual hydrogen-bonding
properties, and may signal special RNA-world handling.  Non-coding regions are also
noticably lower-complexity than coding regions, yet a given protein sequence may have
have evolved by incorporating adjacent non-coding regions into the coding sequence,
leaving behind islands of low-complexity sequence.  The fraction of peptide sequence
that is clearly low-complexity is small, less than one part per thousand, but these
**slighly-rare low-complexity regions can dominate matches**, especially at medium 
to large phylogenetic distances.
This may not seem like a problem for studies involving a single organism such as humans,
but every eukaryote contains homologous sequences at large phylogenetic distances,
either in the organelles, in nuclear sequences transferred from the organelles, or
sequence horizontally transferred by mechanisms such as retrotransposons.  Local-alignment
and assembly codes therefore include code to recognize these low-complexity
regions and exclude them from matching.  But these complexity-masking codes, such as
SEG and DUST, aren't perfect.  They are entropy-based (meaning they only consider
character frequency, not order), they contain a number of tunable parameters that were
tuned for a problem different than yours, and most users rarely think to adjust them.
Complexity masking is an empirical algorithm that was tested and tuned on a tiny
fraction of the universe of sequences.  **There are no trivial changes to empirical 
algorithms.**

Finally, a lot of researchers think that the optimal route for analyzing sequence
runs through genome assembly.  That point of view is hard to argue with if one starts
with a clonal collection of nucleic acids and if the final product is a genomic
assembly.  In those circumstances, assembly does a lot of error-correction and
data-reduction steps that are valuable for many uses.  But when the desired output
is fundamentally *counts* of signatures and the input is not necessarily clonal,
then **assembly is terrible for counting because it is non-linear**.  The chances of 
finding two pieces of sequence
that overlap isn't linear, so the count data that you are seeking gets put through
a non-linear filter that depends on distributions of other biota, their genome sizes,
their distribution of complexity-masked regions, as well as the usual non-linearity
of any amplification steps.

### Dimensionality, Machine Learning, and Underspecification
Every base in a biological sequence is another lever that can be potentially tweaked 
as part of adaptation.  Even if we restrict our view to only peptide sequences, 
**evolution occurs in a million-dimensional space**. A high fraction of proteins
interact other proteins in three-dimensional assemblies in the cellular milieu, and
[epistasis](https://en.wikipedia.org/wiki/Epistasis) is a sizeable effect, which
means that those million degrees of freedom interact with each other in non-trivial
ways across genes.  The number of interacting residues goes into the exponent and
quickly contributes to a combinatorial explosion.  No matter what bioinformatic
problem you're working on, **dimensionality reduction is an imperative**.

Machine learning is doing some remarkable things, including making big progress on 
solving the 50-year-old Grand Challenge of the [Protein Folding
Problem](https://deepmind.com/blog/article/AlphaFold-Using-AI-for-scientific-discovery).
But **no software can overcome the mathematical limitations** that underpin
a problem.  Recent work by the Google Team has illuminated
the issue of [*underspecification*](https://arxiv.org/pdf/2011.03395.pdf), which
seems to impact a wide variety of ML applications, including genomic ones.
Briefly put, most ML models have a large number of underlying parameters, and
if there are multiple ways that different combinations of parameter values can
achieve a good fit to the training data, which combination ends up in the ML
classifier is a determined by unimportant things like selection bias and noise.
ML models that do an excellent job of classifying the test data can fail
miserably when real-world data is put into them.

What underspecification means for practitioners of ML-based bioinformatics is
still being worked out, but two principles have emerged already.  The first
principle is that **ML models need a connection to causal models** if they are 
to be trustworthy.  Sensitivity analysis is one route to mining ML models for
the inputs to which they are most sensitive.  The other principle is that
**spanning the range of possible inputs on the training and test data is key**
to getting credible and reliable results.  For RNA-Seq data, for instance, a spanning set
might by obtained by pushing the system by varying the metabolic, signalling,
and regulatory-gene possibilies and ensuring that the reduced-dimension
basis vectors are stable with respect to partitioning between training and test
data. There is no substitute for having enough **independent data sets that span the input space.**

### Resist Joining a Priest Caste
I have noticed that in some laboratories, bioinformaticians play a role akin to that
of goalies in hockey, a member of the team that isn't really in the team.  The worst
case is in groups where the bioinformatician is considered a rare find because she is capable
of doing work that nobody else can do.  So as a very junior member of 
the team, while still forming her own skills, she gets called in to participate in
every other project that depends heavily on analytical skills to complete.  In some
cases, the PI may insist on a particular trusted method (like assembly) gets employed
even if it isn't a good match.  Sometimes the bioinformatician pulls out an analysis
and looks like a hero just by dint of mere competence. In a few cases, the experimental
design was flawed from the outset, and the best that your in-group Nerd can do is to 
administer last rites.

Stopping this pattern of bad behaviors all around requires a bit of commitment from
everyone.  If you're a PI, you should recognize that it's way better to **pull in a
collaborator with statistical savvy** at the experimental design stage than to hire
a grad student to do salvage analysis at the end.  That collaborator will have the
guts to insist that you need to do the right thing and pay for three replicates once
you find an effect rather than spending the effort on surveying three times as much
new terrain.  If you're a student with a desire to do biology as a career, you need 
to realize that  **programming is as essential as basic literacy** to anyone who has
to analyze data.  You don't have to become a software author as a career, but everyone needs to
be able to write a simple script for the same reasons that everyone needs to
be able to write an abstract paragraph for publication.  If you pride yourself as
a career data analyst, as I do, then you need to **mentor other team members with kindness
and generosity**, inquiring about their needs and listening to their complaints.  Something
is wrong with a person if they don't have the desire to throw every computer in their
lives out the window now and then.  Science consists of many many hours of
grunt work that enable a few moments of insight, moments too rewarding for just
one member of the team to hog to himself.
