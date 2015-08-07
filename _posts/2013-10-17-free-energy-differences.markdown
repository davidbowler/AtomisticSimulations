---
layout: post
title: Free energy differences
year: 2013
month: 10
day: 17
category: techniques
tags: MD software
summary: A novel approach to finding the free energy difference between two phases of a material
published: true
---
A calculation of the free energy difference between two phases of a material can be used to explore first-order transitiona between phases, such as melting. We cover free energy differences in the book in secion 6.5, with a very relevant example also discussed in section 4.6, on the melting of iron under the conditions at the Earth’s core. The standard ab initio approach to free energy differences is to use thermodynamic integration, effectively making the transition from a simple system with a known free energy to the complex system of interest. This requires an appropriate simple system – often a model potential is used, if a sufficiently reliable potential is available. An alternative, which has become more viable as computers have become more powerful, is direct simulation of coexistence: a series of large simulations, set at different conditions, can be used to find the thermodynamic variables which allow co-existence (this approach is exactly what’s discussed in Section 4.6 and the source paper[[1]](#R1)).

Here I will discuss an alternative approach suggested in [[2]](#R2) (this paper was suggested and discussed by the Thomas Young Centre @ UCL coffee club). Rather than seeking coexistence, which is found by looking for the conditions where the interface between the two phases is static and both phases have the same free energy, the authors apply an external, harmonic potential to the interface and compute the force required to keep it static. This allows them to run standard equilibrium simulations, which are relatively simple, while still recovering data on coexistence. The free energy difference can be derived from the harmonic force, and the coexistence point can then be sought by varying the conditions and using a standard numerical root finder.

This approach has the advantage of simplicity, and certainly performs well in the tests that are presented (all on liquid-solid coexistence, for a Lennard-Jones system and DFT calculations of Na, Mg, Al and Si). The authors note that, as the interface is made explicit, the method may suffer from larger finite-size effects and longer equilibration times than a direct approach.

The key problem is to define an order parameter that identifies the two phases and the interface. The authors demonstrate that a parameter based on a sum of phases (using reciprocal lattice vectors near the Bragg peaks of the crystal) is effective for liquid-solid coexistence. Given that their set-up fixes two dimensions of the unit cell and allows only one to vary, it’s clear that there is more work to be done in both defining order parameters, and creating viable simulations. For instance, it’s not clear how the present approach would work for a highly anisotropic system.

This approach is interesting, and clearly has a great deal in common with the direct approach; in both cases, relatively large systems (several hundreds of atoms) will be needed, along with methods to prepare a cell with the appropriate phases. It’s not yet clear whether the freedom given by the restraint of the interface will be significantly more efficient or more widely applicable than the direct approach.

<a name="R1">[1]</a> Phys. Rev. B, 65, 165118 (2002) [DOI:10.1103/PhysRevB.65.165118](http://dx.doi.org/10.1103/PhysRevB.65.165118)

<a name="R2">[2]</a> Phys. Rev. B 88, 094101 (2013) [DOI:10.1103/PhysRevB.88.094101](http://dx.doi.org/10.1103/PhysRevB.88.094101)
