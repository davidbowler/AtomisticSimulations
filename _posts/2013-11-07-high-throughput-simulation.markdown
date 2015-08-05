---
layout: post
year: 2013
month: 11
day: 07
title: High-throughput simulation
category: techniques
tags: DFT parameters software
published: true
---
There is an increasing trend in atomistic simulation, mirroring the wider world of science, to use the techniques sometimes known as Big Data to search for structures: large numbers of simulations with little intervention from the simulator (we discussed some of these approaches in Section 5.2.5 of the book). In this post, I want to discuss some of these approaches and their uses, and the implications they have for our field.

Ab initio random structure searching (AIRSS)[[1]](#R1) takes a remarkably simple approach: create random unit cells (cell size and angles as well as atomic positions within the cell) and optimise using density functional theory (DFT). The received wisdom in the field was that this was unlikely to work, as the energy landscape was assumed to be highly corrugated. The results, however, are startling: the approach is very successful, finding the lowest energy states for many materials; it has been, perhaps, most valuable in high pressure physics.

The Materials Project ([http://www.materialsproject.org/](http://www.materialsproject.org/)) has the grand aim of computing properties of all known materials, as well as suggesting new materials for specific areas[[2]](#R2). This approach is subtly different: it aims to use DFT to calculate properties for known materials which can then be searched via data mining approaches. It is part of the US Materials Genome Initiative, and it is encouraging to see atomistic simulation taking such an important place in science. A similar though smaller scale approach has been taken in Denmark[[3]](#R3).

Finally (at least as far as this blog post goes: I’m sure that you can find many other approaches) the Harvard Clean Energy Project[[4]](#R4) aims to use ideas from drug discovery: systematic screening of millions of candidate molecules for organic photo-voltaics. Significantly, the project brings together these approaches along with data-mining, and mixes DFT with quantum chemistry techniques which are more accurate for the excited states required for these applications (see Chapter 7 of the book). They have recently reported the first results ([here](http://dx.doi.org/10.1039/c3ee42756k)) following 150 million calculations on 2.3 million motifs – quite possibly a record.

All these approaches emphasise the importance of re-using existing results, and being able to use massive computational resources efficiently. A paper from Google researchers about four years ago entitled “On the unreasonable effectiveness of data” ([here](http://doi.ieeecomputersociety.org/10.1109/MIS.2009.36)) described their finding that a very simple model combined with vast amounts of data was more effective in machine translation than complex models with less data. (The title is a reference to Wigner’s famous essay “The unreasonable effectiveness of mathematics in the natural sciences”.) But how many scientists have the necessary skills to work with big data, and the motivation to stay in academia when these skills are in demand in industry[[5]](#R5) ? And how much data is there available, sitting on hard drives, which could be mined, but is not shared or open (see [this previous post]({{ site.baseurl}}/blog/publishing-data) on publishing data) ? Atomistic simulators may need to consider a significant shift in data sharing and career development to stay competitive.

<a name="R1">[1]</a> J. Phys.:Condens. Matter **23**, 053201 [DOI:10.1088/0953-8984/23/5/053201](http://dx.doi.org/10.1088/0953-8984/23/5/053201)

<a name="R3">[2]</a> APL Materials **1**, 011002 (2013) [DOI:10.1063/1.4812323](http://dx.doi.org/10.1063/1.4812323)

<a name="R3">[3]</a> Comput. Sci. Discov. **2** 015006 (2009) [DOI:10.1088/1749-4699/2/1/015006](http://dx.doi.org/10.1088/1749-4699/2/1/015006)

<a name="R4">[4]</a> J. Phys. Chem. Lett. **2** 2241 (2011) [DOI:10.1021/jz200866s](http://dx.doi.org/10.1021/jz200866s)

<a name="R5">[5]</a> [The big data brain drain, J. Vanderplas](http://jakevdp.github.io/blog/2013/10/26/big-data-brain-drain/)
