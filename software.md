# Building Scalable Software

I was lucky enough to get to interact for a brief time with **Max Perutz**, for my money
one of the greatest scientists of all time, back when I made the first crystallographic movies of "his" 
protein myoglobin in action.  Back in 1951, when Perutz had his grad student John Kendrew learn 
how to program Cambridge's pioneering EDSAC computer in order to compute the structure of myoglogin,
scientists probably constituted more than half of the global community of computer programmers.
The myoglobin calculations did the Fourier transform by direct synthesis, running on
a signficant fraction of the global programming fraternity.  In 1965, Cooley and Tukey published
their paper on the discrete Fourier Transform, giving timings on the IBM 7094, a state-of-the-art
scientific computer, that was being used extensively orbital trajectory calculations that employed 
thousands of programmers in NASA, the military, and IBM itself.  By my calculations, the 7094 ran 
Fourier transforms at roughly 8000 times the speed of  EDSAC.  The speedup of the DFT algorithm 
over the algorithm that Perutz's team used, on the other hand, was given in the Cooley-Tukey paper
as a factor of 800,000! 

My points in telling this story are twofold.  First, over the course of a
single career **algorithmic improvements tend to beat hardware improvements**
by a large factor, in this case two orders of magnitude.  Increasing the speed of calculations
by throwing an ever-larger pile of silicon and network cabling at a problem can be viewed
as an exercise in making the same old mistakes faster.  Second, **the code that you 
can write is tiny**,  a diminishing fraction of the sum total of human creativity being expended
on software that might be useful as part of solving the problem.  The saying that "a year in the
lab will save you an afternoon in the library" is nowhere more true than in writing software.
So a good consumer of scientific software is a person who watches streams of news for
interesing codes.  Some streams I personally find useful are the customizable ones
such as Twitter and Google Scholar, incubator sites such as the Apache Foundation, 
and software streams like the science section of my favorite Linux distribution.

###  Files, Legacies, and Carbon
Someday you will move on. The databases you built will be spun down, the containers you used
will be terminated, and the funding that supported you will cease.  Your team will disperse and
become parts of other teams, and the field will shift to the Next Big Thing.  But your 
data---and the publications that describe what you could figure out from them---will 
remain.  

At the 25th anniversary of the Human Genome Project, many of its founders openly wondered if 
the genome would ever be practically useful.  It may take a while before your data gets connected
with another idea, but if you have faith in the scientific process, you believe it 
eventually will.  When it does, they're going to want to do a survey back through it to test 
ideas out, before
they get funding to collect it again, perhaps on a vast scale.  When I'm architecting data
flow, I like to think about those future scientists.  I can't really predict
what conditions they will work under, but carbon neutrality seems inescapable to me, and
Addressing this concern starts with picking or inventing efficient algorithms.
Nowhere more so than in how one reads files into memory.
There is no point in saying bad things about legacy file formats or legacy software; they
are the broken bit of road that brought us here, built in a hurry with shabby tools while
Huns were attacking. 

My approach lately has been to ask the question **"How long will it take someone to load a
this data into memory from disk?"**, treating everything else as secondary.
Scientific data is mostly write-once-read-many, so write times don't especially matter.
But time spent parsing files is a kind of tax, a bit like the costs drivers pay in
having to commute over a bad highway.  In any given minute of the day,
there's perhaps a hundred biologists waiting on output for programs that read FASTA and 
GFF files, so collectively over the course of a year, **every second of wait time adds up to
20% of a career**.  People are increasingly paying
attention to the carbon costs of their actions, and waiting on those file parsers to
finish not only costs the power used in cycles but also the carbon capital sunk into
the finite lifetime of the computer and of the scientist running the analysis.
Internal compression---really common for columnar storage schemese these days---should be
chosen to optimize speed from a fast NVME M.2 disk because that's the best guess as to
what the future will look like. 

Other secondary considerations do matter, for instance **loss of metatadata**, which happens
far too often in practice.  A downstream user shouldn't have to look up data types in an
external dictionary just to be able to parse a file properly, nor should one have to obey 
a complex scheme to hide metadata in the names of features.  **Human readability matters
only occasionally**, mostly at the beginning when it's 
handy to be able to use tools like [tsv-utils](https://github.com/eBay/tsv-utils) to filter,
select and groupby on columns.  So, in designing data organization, it's good to commit
to building tools to interconvert betwen your chosen file formats and TSV.

The direction that I find most promising for in-memory organization of data is 
[Apache Arrow](https://arrow.apache.org) with storage to 
[Feather](https://arrow.apache.org/docs/python/feather.html), because this path is
blazingly fast, easily interconverts to TSV or legacy formats, and seems likely to 
become standard in data science.  Read speeds of Feather files into pandas
are more than 10x faster than TSV, and even more compared with FASTA and GFF.

### Memory vs. Database vs. Filesystem
If your field is hot and your code was built to last, somebody is going to scale
it up until it runs out of memory.  The usual solutions are either to find a bigger
machine or to distribute memory over many instances.  But you pay a price for memory
that can be expressed in terms of fanout, energy, dollars, and time to spin up.
Depending on how your code scales, that can get prohibitively expensive fast,
especially for someone years later who may only want to look at a small subset
of data.  **Memory management is all too ofetn a black box on the back burner**,
and I've gotten burned on that back burner.

Databases can be wonderful, especially for distributed writes where the data
to be written isn't large. Some, like Redis, are joys to work with.  Some
are designed to share and scale well.  But it's good to ask when architecting
**"How robust will this database be with 1000X more data?"**.  This is
especially an issue if you are relying on the database to do joins, because
this put significant science logic outside your code and in database-land.

My approach lately has been to **put large joins into code that can be run either
on-disk or in-memory**  based on demand and available resources.  For production,
those merges can be put into networked filesystems with an external
merge server that can be high-memory if need be.  But development still fits
onto a GPU-equipped consumer desktop with good price/performance ratio and energy
efficiency, which is still the best bet for being future-proof.

### Managing technical debt
Some projects are a Death March to results.  Others are more leisurely.  There's
always a trade-off between an easy solution now instead of a better approach
that would take longer.  You have to decide at the beginning of a project
whether you have the luxury of learning new technology and doing the numerical
experiments that will let you write scalable, testable, maintainable code.  In
this view, it's better to **manage, not minimize technical debt**, 
letting it build up sometimes and going after it aggressively at others.

It helps to have a good set of templates, like say those in 
[Hypermodern Python](https://github.com/cjolowicz/cookiecutter-hypermodern-python)
as a familiar base on which to build
the next project.  That way, technical debt gets limited in the areas in which
it appears.  There are some areas where no good solution exists and you have
to keep an eye out for new solutions as they arise.  To the best of my knowlege,
there isn't any great solution for Data Containerization as yet, and that's
something that scientific data wrangling sorely needs.  I use Python as my
primary development language, and despite many things that I love about the
language, Python packaging leaves a lot to be desired in comparison to, say,
npm.  
