---
layout: post
year: 2015
month: 10
day: 30
category: tutorials
tags: DFT
summary: A little more on DFT formalism
title: A discussion of the background theory to DFT
published: true
---
These blogs are aimed at fourth year undergraduates/Masters students, with a
relatively restricted time for projects (six months or so); we do not have
time to discuss DFT in full detail, but it is important to cover the background
theory sufficiently thoroughly to understand its limitations and capabilities.
This week we covered DFT theory in more depth (some of this has been covered
in the first blog of this
series, [here](http://davidbowler.github.io/AtomisticSimulations/blog/learning-dft/) ).

The most important thing to understand about DFT is that it can be shown that
the ground state properties of a system depend only on the charge density,
and not on the full details of the many-body wavefunction (this is essentially
what the Hohenberg-Kohn theorems tell us).  The idea dates back to Dirac if
not earlier[[1]](#R1).  As a result, we can write the total energy of the system
as a functional (a function of a function) of the charge density.  

The energy can be helpfully written as the sum of a number of terms: kinetic
energy of the electrons; electron-ion energy; electron-electron energy; and
the ion-ion energy.  It is vital to include this last term, as it determines
the stability of our system.  We will work with classical ions and within the
Born-Oppenheimer approximation (decoupling electron and ionic degrees of
freedom: these are not always good approximations but simplify the problem
considerably).  In this context, the ion-ion interaction is simply the
classical electrostatics of a set of point charges[[2]](#R2).

The kinetic energy can, in principle, be written in terms of the charge density,
and indeed this was the approach of Thomas and Fermi (the Thomas-Fermi
functional is still used, and shows that the kinetic energy can be found as
\\(E_{KE} \propto \int n^{5/3}(\mathbf{r}) d\mathbf{r}\\), with
\\(n(\mathbf{r})\\) the electron density ).  But the accuracy  of this approach
is poor, and almost all DFT uses the Kohn-Sham approach, which  writes the
kinetic energy in terms of a set of non-interacting electrons:

$$
E_{KE} = -\frac{\hbar^2}{2m} \sum_n \langle \psi^{KS}_{n} \vert \nabla^2 \vert
\psi^{KS}_{n}\rangle
$$

The density is then written as \\(n(\mathbf{r}) = \sum\_{n} \vert
\psi^{KS}\_{n}\vert^2 \\). It is important to note that this kinetic energy is
**not** the same as the kinetic energy for the many-body wavefunction (while the
operator is the same,  the wavefunction is different) and this difference is
included in another term.

The electron-ion interaction involves a sum over the potentials from each ion,
using either the bare Coulomb potential or a pseudopotential (which I will
discuss in my next blog).  While the details of pseudopotential generation and
implementation are complex and important, the potential and its associated
energy are quite simple: \\(V(\mathbf{r}) = \sum_I V_I(\mathbf{r})\\),
\\(E_{eI} = \int d\mathbf{r} V(\mathbf{r}) n(\mathbf{r}) \\).

The electron-electron interaction is split into two terms: the classical
electrostatic energy of a charge density, often called the Hartree energy, and
the exchange-correlation energy.  The Hartree energy is written:

$$
E_{Har} = \frac{1}{2}\int\int \frac{n(\mathbf{r})n(\mathbf{r}^\prime)}{\vert
\mathbf{r} - \mathbf{r}^\prime\vert} d\mathbf{r} d\mathbf{r}^\prime
$$

This is normally found, along with the potential, using fast Fourier transforms
(FFTs) on an even grid throughout real-space.  It conceals one of the nastier
errors in DFT: the self-interaction error.  The energy for each electron
contains the effect of that electron interacting with itself - which is quite
wrong.  It gives a tendency for DFT to delocalise charge more than it should
be[[3]](#R3).

The final term in the energy is the exchange and correlation energy, which
accounts for all the many-body terms that have been left out so far; it
can be shown that this reformulation is, in principle exact - the problem is
that we do not know the correct form of the exchange-correlation functional.  
There is a vast number of XC functionals available, each of which have both
advantages and disadvantages.  I will briefly list the hierarchy now.

* Local density approximation (LDA).  This was the approximation originally
 proposed (and comes in part from classical DFT, which is used to model liquid
 flow), and assumes that the energy can be written as the integral over all
 space of the charge density multiplied by the exchange-correlation energy of
 a uniform electron gas with the density at that point in space.  This energy
 can be found from quantum Monte Carlo calculations (among others).  LDA tends
 to over-bind, giving lattice constants that are too small, and binding energies
 that are too large.  It is remarkably successful, mainly because it is not
 as crude as it seems (see a recent review[[4]](#R4) for some details,
 especially part IV C, and the literature for full details).
* Generalised gradient approximations (GGA).  These extend the LDA, and consider
 functionals that depend both on the charge density *and* its gradient.  They
 often give better binding energies and overall results, but the wealth of
 different functionals should give a clue to the fact that there is no single
 perfect functional.  Commonly used functionals include PBE (and a version
 which was reparameterised for solid-state, PBEsol) and BLYP.
* Meta-GGAs add the kinetic energy density of the electron gas as a further
 variable.  They offer improvements in some areas, but as far as I am aware,
 there is no concensus on whether they are always better.
* Hybrid functionals.  Here, some Hartree-Fock exchange (calculated using the
  Kohn-Sham eigenstates) is mixed into the functional.  Hybrids often improve
  on band gaps (which are notoriously poor in LDA and GGA) and reaction
  barriers, but are computationally expensive (particularly for plane wave
  implementations).  It should be noted that the fraction of exchange to be
  mixed in is not defined *a priori*, and forms a parameter.  PBE0 and B3LYP
  are probably the two most widely used hybrid functionals.  In solid state
  codes, screened hybrids (where exact exchange is only used for short-range
  exchange and standard LDA/GGA exchange is used at long ranges) are common
  and improve the efficiency, though again the range for screening must be
  fitted somehow.

It is very important to understand what functional you are using, and how it
is limited, rather than simply using what seems best or easiest.  DFT is a
very powerful method, but has significant limitations.  A significant part of
any paper or thesis should be to consider what errors the choice of
functional and/or parameters could make in the results.

<a name="R1">[1]</a> He makes this point in his excellent book on quantum
mechanics as well as other places.

<a name="R2">[2]</a> For a periodic system, this is not a trivial problem to
solve, as the electrostatic interaction is long-ranged.  The standard approach
is to use the Ewald method, which splits the problem into short-range (solved
in real space) and long-range (solved in reciprocal space).

<a name="R3">[3]</a> In Hartree-Fock theory, this self-interaction is exactly
cancelled by an equivalent term in the exchange energy; however, DFT writes
the exchange in terms of the density and so does not cancel the term.

<a name="R4">[4]</a> Rev. Mod. Phys. **87**, 897 (2015) [DOI:10.1103/RevModPhys.87.897](http://dx.doi.org/10.1103/RevModPhys.87.897)
