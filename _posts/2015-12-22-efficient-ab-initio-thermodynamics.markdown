---
layout: post
year: 2015
month: 12
day: 22
category: techniques
tags: MD algorithms DFT Techniques
summary: A discussion of a recent paper that shows a new way to improve sampling for DFT free energies
title: An efficient approach to ab initio thermodynamics
published: true
---
*Ab initio* thermodynamics is both extremely challenging and extremely important.
The challenge arises from the need to sample an energy distribution sufficiently
well to converge calculations; the importance comes from the insight that we can
gain into experimentally inaccessible situations (I have several colleagues who
work on iron in the Earth’s core which is not readily accessible
experimentally).  A new paper[[1]](#R1) suggests an approach to *ab initio*
thermodynamics that will be extremely helpful for certain calculations (and
potentially useful for general calculations).  I have written about calculations
on liquid iron in Section 4.6 of the book, and on general approaches to
thermodynamics in Chapter 6.

When finding average values of variables at finite temperature, we have to
sample over a set of micro-states which are distributed according to a potential
energy, \\(U\_1(\mathbf{r})\\), with a Boltzmann factor that depends on the
potential giving the probability of each state.  The standard approach to this
is to use either MD or Monte Carlo (MC) to sample the potential energy surface,
possibly using a weighting scheme to speed up convergence.  This tends to be
quite expensive when using *ab initio* methods where a long MD run may be
required.

The key insight of the new method is that we can perform the same averaging
using a set of micro-states that are distributed according to a different
potential energy, \\(U\_0(\mathbf{r})\\), with the Boltzmann factor now
accounting for the distribution of each state relative to the new potential,
\\(U\_1(\mathbf{r}) - U\_0(\mathbf{r})\\).  If the new potential is
significantly cheaper than the first, then we can perform a long sampling run
using this potential, and draw the micro-states from this distribution, reducing
significantly the number of expensive calculations that need to be performed.

This paper presents a careful analysis of the effect of the accuracy of the
cheap method (here taken to be a classical potential, ideally fitted to some *ab
initio* MD) and its effect on the sampling.  While the method is efficient for
standard averages, it is outstanding for thermodynamic integration, where it can
reduce the number of simulations by an order of magnitude or more.  It is clear
that it’s been developed in this context - where the absolute free energy is
required.  In the context of *ab initio* thermodynamics, this is a significant
step forward.

<a name="R1">[1]</a> Comp. Phys. Commun. **127**, 1 (2015) [DOI:10.1016/j.cpc.2015.07.008](http://dx.doi.org/10.1016/j.cpc.2015.07.008)
