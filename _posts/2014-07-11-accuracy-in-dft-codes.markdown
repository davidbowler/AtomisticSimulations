---
layout: post
title: Characterising Accuracy in DFT Codes
category: Accuracy
tags: DFT testing parameters Techniques
year: 2014
month: 07
day: 11
summary: How can we characterise the accuracy of solid-state DFT codes ?
published: true
---
The question of how to characterise the accuracy of a computer code is a difficult one, and I have touched on these issues before ([here](/blog/what-if-dft-is-wrong), [here](/blog/increasing-accuracy) and [here](/blog/dft-weak-interactions) for instance).  However, given the large number of codes available, it should be possible to compare them to each other and to experiment (or higher level calculations) to test them.  This is a well-established process in the quantum chemistry community, where there are various test sets for different properties, including enthalpies of formation(G97/2)[[1]](#R1), weak bonding (S22)[[2]](#R2) (and a vast database of different properties[[3]](#R3)).

A recent paper[[4]](#R4) and associated web site[[5]](#R5) offers a first approach for solid state codes, with the comparison based on the differences between all-electron and pseudopotential calculations.  The presumption here is that all-electron calculations are the touchstone (though the website notes that the all-electron results have been refined to use extremely accurate tolerances and small muffin-tin radii, so there is clearly room for improvement in any method).

The idea of the comparison is to calculate binding energy curves for most elements in the periodic table, and from these curves to derive a single number which characterises the deviation from all-electron results.  The deviation per element can be viewed, as can the deviation from experiment for the all-electron calculations.  The deviation, delta, is defined by an integral over all calculated values of volume, and thus includes implicitly both the lattice constant and the bulk modulus, though not the cohesive energy.  Most results shown on the website are for plane-wave codes, which generally perform rather well (the old norm-conserving FHI pseudopotentials are less accurate, though, and should be treated with care).

The approach is a good one, though a little heavy on the tester, and scripts to perform the necessary calculations are made freely available.  However, the choice of the elemental form makes the tests rather restricted: there is no way to examine different types of bonding or different oxidation state, for instance.  It is quite easy to imagine developing a set of these test suites for different purposes in solid state codes, just as there are different test sets in quantum chemistry.

The accuracy tests seem to be most illuminating for the pseudopotentials, rather than the codes themselves, and I think that it would be of immense value to the community if the pseudopotential generation details were made available.  This should not just include the core radii and the reference configurations, but also a clear description of the pseudopotential algorithm (or an appropriate reference along with details).

There is something of a danger of using codes and supplied pseudopotential libraries as black boxes: there is a need to test parameters, though it’s rare (and I am happy to acknowledge that I don’t do it as much as I should).  This paper and associated developments should go some way to standardising plane wave codes, and giving quantitative information on their reliability.

Note As I was writing this, a new paper in Science has just been published which takes a different view, and compares results from different functionals; it’s worth a read[[6]](#R6)

#<a name="R1"></a>
[1] J. Chem. Phys. 106, 1063 (1997) DOI:[10.1063/1.473182](http://dx.doi.org/10.1063/1.473182)

#<a name="R2"></a>
[2] Phys. Chem. Chem. Phys. 8, 1985 (2006) DOI:[10.1039/B600027D](http://dx.doi.org/10.1039/B600027D)

#<a name="R3"></a>
[3] [http://t1.chem.umn.edu/db/](http://t1.chem.umn.edu/db/) and see arXiv [1212.0944](http://arxiv.org/abs/1212.0944)

#<a name="R4"></a>
[4] Crit. Rev. Sol. Stat. Mat. Sci. 39, 1–24. DOI:[10.1080/10408436.2013.772503](http://dx.doi.org/10.1080/10408436.2013.772503)

#<a name="R5"></a>
[5] [http://molmod.ugent.be/deltacodesdft](http://molmod.ugent.be/deltacodesdft)

#<a name="R6"></a>
[6] Science 345, 197 (2014) DOI:[10.1126/science.1253486](http://dx.doi.org/10.1126/science.1253486)
