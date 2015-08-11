---
layout: post
title: Water interactions with carbon nanotubes
year: 2013
month: 07
day: 04
summary: Looking at comparison between experiment and modelling in the context of water interaction with carbon nanotubes
category: Techniques
tags: comparison experiment water nanotubes testing
published: true
---
We have discussed the importance of comparison between simulations and
experiments in [a previous blog post]({{ site.baseurl/blog/large-defects-in-silicon }}) as well as in Chapter 18,
Comparison to Experiment. But we do have to be extremely careful to ensure that
the simulations are high quality, directly comparable to the experiment, and do
not introduce effects because of the simulation protocols. I want to discuss
some of these questions while looking at papers which model the interaction
between water and carbon nanotubes. (I should make it clear that I’m using the
papers as a springboard, rather than criticizing these papers; nevertheless, it
is an important part of science to read papers critically, and ask what might be
wrong.)

In a recent Phys. Rev. Lett. [[1]](#R1), Homma et al. measured the
photoluminescence (PL) of carbon nanotubes in vacuum and in water vapour, and
found a strong shift of the PL emission and excitation; they also used Raman
spectroscopy to measure the frequency of the radial breathing mode (RBM) of the
nanotube. The phenomena of changing PL and RBM on exposure to the atmosphere are
known, though the detailed mechanism is not. To help understand the changes,
they also performed MD simulations. Similar simulations have been performed
before [[2]](#R2) and the present paper refers to this work. The simulations
show a similar shift in the RBM to the experiments, and show strong structure in
the water molecules around the nanotube.

### Methodological Questions

When I was reading the simulation part of the paper by Homma[[1]](#R1), I was
left with various questions. The implication is that they used the same
simulation protocols as Longhurst and Quirke[[2]](#R2), but this is not made
clear. The water model used (SPC/E) is not the most reliable, and the effect of
the water-nanotube interaction on the results is not tested. In fact, there is
very little testing of the simulation parameters. How does the density of vapour
used compare to the experimental vapour pressure ? Do the results depend
strongly on unit cell size ? These questions are important to answer, and I
would have liked more detail on these (this type of information is normally
found in Supplementary Information).

### Scientific Questions

Beside the questions about the simulation protocols, there are interesting
scientific questions. How do we understand a macroscopic phenomenon like
hydrophobicity in terms of atomistic mechanisms ? Certainly the results from
this paper indicate that there is some attraction between the water and the
nanotube (the water molecules form a dense layer about 0.8nm away from the
nanotube surface, with a weaker layer 1.1nm away). This could be investigated
with graphene and either a single water molecule or a sheet of molecules.
Indeed, the hydrophobicity of graphene is where the paper starts, but graphene
has no PL response, nor indeed a radial breathing mode (the frequency of the RBM
scales as the inverse of the nanotube diameter). This makes the experiments
impossible but the simulations could be done, and would be illuminating.

It would be extremely hard to model the PL response: exciton recombination is
the main source of the PL, which requires the Bethe-Salpeter equation or similar
approaches. These simulations are extremely demanding, and the best calculations
so far were semi-empirical, with a continuum dielectric. Such an apparently
simple system requires care in protocols and, potentially, sophisticated
modelling to understand. A large part of the simulator’s job is deciding how to
make a sensible model of the problem, and how to ensure that the results are
sensible.  These ideas are discussed in Chapter 10, Planning a Project.

<a name="R1">[1]</a> Phys. Rev. Lett. **110**, 157402 (2013) [DOI:10.1103/PhysRevLett.110.157402](http://dx.doi.org/10.1103/PhysRevLett.110.157402)

<a name="R2">[2]</a> J. Chem. Phys. **124**, 234708 (2006) [DOI:10.1063/1.2205852](http://dx.doi.org/10.1063/1.2205852)
