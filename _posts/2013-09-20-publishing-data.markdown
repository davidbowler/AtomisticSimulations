---
layout: post
title: Publishing data
year: 2013
month: 09
day: 20
published: true
category: techniques
tags: software testing
summary: Should computational scientists make their files publicly available ?
---
For experimental science, the data collected forms an enormously important resource. Of course it is of little use without details of how the experiments were performed, and this allows for careful testing of the assumptions that went into the design of the experiment. If the data sets and experimental protocols are published, other scientists can verify the results, and problems such as selection bias can be found. (Though we won’t discuss it here, it’s important to understand that you can introduce selection bias as well as other erroneous assumptions into simulations as well.) Moreover, there is an increasing drive to publish negative results, which are often ignored at present.

Within computational science, publishing data is much less common. I’ve [written before about software testing](http://www.atomisticsimulations.org/software-testing/) and giving details of the code version, models etc, but we should do better. Really, every atomistic simulation paper should give access to the coordinates of the atoms (at least the starting structures, and relaxed structures where that’s what’s being done). For molecular dynamics runs, details of the system evolution could easily be treated as a data set to be provided. The wavefunction coefficients from electronic structure calculations are rather too large to be included, but provided the full details of the calculation conditions are given (basis set, grids, pseudopotentials where used etc) these can be reproduced.

Many journals allow supplementary information to be included with a paper; in practice, this tends to be only a PDF giving the more detailed descriptions of what was done and how. In terms of making data available, the website [figshare](http://figshare.com/) seems very interesting: it promises archiving and unlimited space for publicly-available data, and some private data space. It’s underwritten by Digital Science, which is part of Macmillan (who publish Nature among other journals). The data is licensed under a creative commons licence, so can be re-used by anyone, with appropriate attribution. This is a good thing: by increasing repeatability and checks, we will make our science more rigorous. Moreover, each data set is given a DOI, and so can be cited and tracked easily.

It’s possible to add papers to figshare along with data, though a preprint server with an established community like [arXiv](http://arxiv.org/) might be a better route. These approaches are all part of the move towards open access to scientific research (now mandatory for UK publicly funded research). Atomistic simulators should join in, and make their data available as part of their publications.
