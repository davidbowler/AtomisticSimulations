---
layout: post
year: 2015
month: 10
day: 23
category: tutorials
tags: DFT
summary: Basis sets in DFT
title: A discussion of two of the most common basis sets used in DFT
published: true
---
There are many different basis sets used to represent the wavefunctions in DFT,
and it is important to understand their advantages and limitations.  In this
tutorial, I will discuss two of the most commonly used basis sets: plane
waves; and atomic orbitals[[1]](#R1).  Unlike the basis sets generally used in
undergraduate quantum mechanics courses, these basis sets cannot be complete,
and the effects of using a finite basis set must be both understood and tested
carefully.

Plane waves are the foundation of almost all of the computational solid state
DFT modelling that has been done for the last thirty years.  They have two key
advantages: the completeness of the basis set can be increased systematically;
and they offer great computational efficiency.  The completeness is improved
simply by increasing the largest wavevector used in the plane wave expansion.
This is often expressed as an energy, \\(E\_{\mathrm{cutoff}} =
\hbar^2 k\_{max}^2/2m\_e\\), which represents the largest kinetic energy that
can be sustained by the basis (and is also related to the finest spatial
features that can be resolved).  

The mathematical and computational simplicity comes partly from the orthgonality
of the basis functions, and also from the exceptional efficiency of the fast
Fourier transform (FFT) as implemented on modern computers.  Any transformation
from real to reciprocal space, or vice versa, relies on FFTs, and these are
generally very fast[[2]](#R2).

However, plane waves are not a very good representation of the electronic
wavefunctions in molecules and solids, particularly in areas where the
potential varies rapidly (for instance near the nuclear core).  Generally
they are used either with pseudopotentials (which I will write about soon) or
with an auxiliary basis set that describes the rapid variation well.  They also
extend over all space, which can affect performance with very large system
sizes.

Atomic orbitals (which I will refer to as AOs - often we have either numerical
AOs, NAOs, or pseudo-AOs, PAOs) consist of a radial function multiplied by a
spherical harmonic.  The radial function can be a gaussian (as used extensively
in quantum chemistry) or the solution to an atom confined in a potential, to
ensure that its wavefunctions go to zero at a finite distance.  The question of
how to choose the confinement is complex, and there is no good, single answer.
Typically, AOs use several radial functions in each angular momentum channel
(each value of \\(l\\)) with different radii, to allow for flexibility.  The angular
momenta for the valence electrons must be included, but it is common to add
higher empty angular momentum shells (called polarisation orbitals).

As well as the problem of confinement, there is no systematic way to improve
the completeness of an AO basis set.  It is not clear whether the number of
radial functions (often referred to as the number of zetas) or the maximum
angular momentum would offer better convergence, or if both should be
increased.  In solid state calculations, the double zeta plus polarisation (DZP)
basis set is often used as a standard, with two radial functions for the
valence electrons and a single function for the first empty shell[[3]](#R3).

PAOs are not an orthogonal basis set, which changes the matrix representation
of the Schrodinger equation.  If we write \\(\vert \phi_{i\alpha}\rangle\\) for
a PAO on atom \\(i\\) with \\(\alpha\\) a combined index for zeta and angular
momentum, and expand the eigenstates in this basis, we find:

$$
\vert \psi_{n}\rangle = \sum_{i\alpha} c^{n}_{i\alpha}\vert \phi_{i\alpha}\rangle\\
\hat{H}\vert \psi_{n}\rangle = \epsilon_n \vert \psi_{n}\rangle \\
\sum_{j\beta} H_{i\alpha j\beta} c^{n}_{j\beta} = \epsilon_{n} \sum_{j\beta} S_{i\alpha j\beta} c^{n}_{j\beta}\\
S_{i\alpha j\beta} = \langle \phi_{i\alpha} \vert \phi_{j\beta}\rangle\\
H_{i\alpha j\beta} = \langle \phi_{i\alpha} \vert \hat{H} \vert \phi_{j\beta}\rangle
$$

This is a generalised eigenvalue equation, which can be solved in a variety of
ways.  It's important to note that the number of PAOs is generally much smaller
than the number of PWs (13 per atom for a DZP basis for C or Si compared to
several hundred or several thousand plane waves), so that the ground state can
often be found by direct diagonalisation, rather an a minimisation.  The
locality of the PAOs is also important: the Hamiltonian and overlap matrices
(H and S above) are sparse, and the construction of them can be made to scale
linearly with system size, even if the solution does not.

There is no ideal basis set for electronic structure calculations, but it is
important to know about the basis set you use, and to characterise it.  The
more you learn and think about the calculations you are doing, the better
the science that you are performing will be.

<a name="R1">[1]</a> Note that in many cases we actually use the orbitals that
come from using a pseudopotential with the atom, giving pseudo-atomic orbitals.

<a name="R2">[2]</a> FFTs are very good for small and medium sized systems;
however, they require a certain level of all-to-all communication when implemented
in parallel, which ultimately limits their scaling to very large systems or
numbers of processes.

<a name="R3">[3]</a> The correlation consistent basis sets of quantum chemistry
form a group of basis sets where, for each new level of accuracy, an extra
radial function is added to every existing angular momentum, and a new
angular momentum channel is added.  This leads to a series: single zeta (SZ);
double zeta plus polarisation (DZP); triple zeta, double polarisation plus f
(TZDPF); etc.
