---
layout: post
year: 2015
month: 11
day: 16
category: accuracy
tags: testing Accuracy ExcitedStates
summary: An overview of full CI quantum Monte Carlo and a discussion of accuracy
title: How do we establish the accuracy of a method ? Full CI quantum Monte Carlo
published: true
---
One of the topics that is often discussed within the electronic structure
community is that of accuracy: how accurate is a given method.  While DFT
is efficient and widely applicable, it
has many known limitations, and rarely comes close to what is called chemical
accuracy (1 kcal/mol or around 40meV).  Recent years have seen various efforts
to improve the accuracy of DFT (I have blogged about this before:
[here]({{site.baseurl}}/blog/testing-dft-water-qmc),
[here]({{site.baseurl}}/blog/accuracy-in-dft-codes) and
[here]({{site.baseurl}}/blog/increasing-accuracy) for instance), but while
these additions have had some success, they are necessarily limited, and there
is no *systematic* way to improve accuracy in DFT.  There is, therefore, a need
for well-defined benchmarks against which DFT and other methods can be tested.
Experiment often forms one important touchstone, but we need to
be confident that the calculation we perform corresponds to the experimental
set-up (often a difficult problem).  In this blog I will discuss a recently
developed approach, full CI quantum Monte Carlo[[1]](#R5), that allows convergence to
the exact, many-body wavefunction result (for a given basis set).  This gives
both an important way to test other methods, and a powerful method for studying
problems that need this level of accuracy.

Going beyond standard DFT accuracy normally involves adding extra terms (such
as a fraction of exact exchange in [hybrid]({{site.baseurl}}/tags/hybrids)
functionals), introducing new functionality (for instance via TDDFT) or
using  DFT wavefunctions as the input to perturbative expansions (such as
[GW]({{site.baseurl}}/tags/GW)).  One advantage DFT holds over these other
methods is in the size of system that can be modelled: it generally scales with
the cube of the number of atoms, and can address systems with hundreds or
thousands of atoms (with
[linear scaling DFT]({{site.baseurl}}/tags/LinearScaling/)
we can go to millions of atoms[[2]](#R1) or
beyond).  More accurate methods generally scale more strongly with atom number
and are limited in the size of the system that they can address.

Quantum chemistry techniques differ fundamentally from DFT-based techniques in
that they work with approximations to the many-body wavefunction rather than the
charge density[[3]](#R2).  A systematic approach to improving accuracy can be
defined within this formalism (which I should note is extremely sophisticated
and requires more space than I can give here).  The starting point is
Hartree-Fock theory, which approximates the many-body wavefunction with a Slater
determinant of molecular orbitals built from some basis set (almost inevitably
Gaussian functions these days).  The simplest improvements to Hartree-Fock
invoke standard quantum mechanical perturbation theory (such as the MP2 method),
but while these methods are  powerful and reasonably accurate, they are limited.
The configuration interaction (CI) method goes beyond the single determinant of
Hartree-Fock, and adds determinants which include all possible excitations of
one, two, three (or more) electrons.  The full CI solution is prohibitively
expensive beyond about ten electrons, though this limit also depends on the
completeness of the basis set used.

The quantum Monte Carlo (QMC) family of methods provide an alternative, very
accurate approach, and seek to calculate the ground state many-body wavefunction
using a stochastic approach (see [[4]](#R3) for a recent overview of these
methods, or a review like [[5]](#R4) for more details).  However, I want to
write about a recent development: full configuration interaction QMC, or
FCI-QMC[[1]](#R5).

FCI-QMC works within the space of Slater determinants which are possible, given
the system and the basis set chosen.  Rather than adjust the coefficients of
the determinants, it evolves a stochastic set of walkers, with different
populations on different determinants, through the
operations of spawning (creating a new walker on a new determinant),
cloning (an existing walker on the same
determinant) and annihilation (removing pairs of walker with opposite
signs on the same determinant---this is needed for proper fermionic behaviour).
While the number of
walkers required is rather large, the computational and memory is very small
for each, and it can be shown that this procedure converges to the exact, full
configuration interaction result.  The only error left is that of the basis
set (this does not affect other QMC methods, which work in real space).  There
are well-established methods for extrapolating to a complete basis set limit.

In the paper which prompted me to write this post, the FCI-QMC method was
applied to various solid state problems[[6]](#R6).  The paper is remarkable for
several reasons, not the least of which is managing to get a purely
computational paper, which largely presents benchmark calculations, into
Nature.  

The authors point out that full CI calculations form an important reference
point for other quantum chemistry methods in molecular calculations, as they
give the exact result for a given basis.  However, these results are not
available in the solid state.  Recent work has seen a number of developments
that allow quantum chemistry approaches to be applied to solid state problems
(whether using traditional, gaussian basis sets, or plane wave basis sets).

A solid-state implementation of FCI-QMC is not trivial: the method scales
essentially exponentially with k points, though this can be mitigated somewhat
by ensuring that sampling between k points obeys momentum conservation.  For
small cells (LiH), even with modest k point sampling, there are approximately
\\(10^{30}\\) determinants to sample.  They demonstrate converged calculations
on a wide variety of materials, and show that the most accurate of the widely
used approximations (CCSD(T), or coupled cluster with singles, doubles and
some triples) gives excellent results compared to the exact result.

The method is not cheap: for diamond carbon, with four k points in each
direction, the calculation took 25,000 CPU hours, with a relatively modest
basis set.  However, it does allow us to test the well-established hierarchy
of quantum chemical methods in solids, and demonstrate that the best of these
go beyond chemical accuracy in solids (even for strongly correlated materials).

Why could this comparison not be made with existing QMC methods, such as
diffusion Monte Carlo ? The difference in basis sets is the key issue: DMC
works in real space, and the quantum chemistry calculations would have had to
be converged to the complete basis set limit to enable comparisons.  FCI-QMC
works in the same space as the quantum chemistry calculations, and thus gives
the exact result for any given choice of system and basis set.

There have been a number of other developments in FCI-QMC, many related to
improving the efficiency of the method, but also recently showing that it is
possible to sample excited states efficiently[[7]](#R7).  As with all QMC
methods, the FCI-QMC method parallelises with very high efficiency (in all
of these methods a given walker can operate almost independently), but it is
not possible at the moment to evaluate forces accurately.  They have a very
specific domain of applicability, but within that domain, they are quite
possibly the most accurate methods available.

<a name="R5">[1]</a> J. Chem. Phys. **131** 054106 (2009) [DOI:10.1063/1.3193710](http://dx.doi.org/10.1063/1.3193710)

<a name="R1">[2]</a> J. Phys.: Condens. Matter **22**, 074207 (2010) [DOI:10.1088/0953-8984/22/7/074207](http://dx.doi.org/10.1088/0953-8984/22/7/074207)

<a name="R2">[3]</a> Indeed, these different approaches are often referred to,
respectively, as wavefunction methods and density methods.

<a name="R3">[4]</a> J. Chem. Phys. **143**, 164105 (2015) [DOI:10.1063/1.4933112](http://dx.doi.org/10.1063/1.4933112)

<a name="R4">[5]</a> Rev. Mod. Phys. **143**, 164105 (2015) [DOI:10.1063/1.4933112](http://dx.doi.org/10.1063/1.4933112)

<a name="R6">[6]</a> Nature **493**, 365 (2013) [DOI:10.1038/nature11770](http://dx.doi.org/10.1038/nature11770)

<a name="R7">[7]</a> J. Chem. Phys. **143** 134117 (2015) [DOI:10.1063/1.4932595](http://dx.doi.org/10.1063/1.4932595)
