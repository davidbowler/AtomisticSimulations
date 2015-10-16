---
layout: post
year: 2015
month: 10
day: 16
category: tutorials
tags: DFT
summary: Covering cell size and periodicity
title: Modelling infinite problems with finite resources
published: true
---
Much of this tutorial covers the problems that we face from trying to model
infinite systems with finite computational resources, and relies on a
reasonable background understanding of Bloch's theorem and periodicity.  It is
very much from a solid-state physics background (which is my background, and
the area where I work).

When using an electronic structure code that relies on periodicity, the
Brillouin zone sampling (or k-point mesh - the two are essentially
synonymous) is a key parameter, and is important to test.  One useful exercise
is to compare a basic cubic diamond cell with the same cell doubled in one
direction (say along x).  If we use a k-point mesh for the second which has
half the number of k-points along x that the first has, the total energy per
atom for the two cells should agree.  This is a basic consequence of Bloch's
theorem; however, the underlying concepts are important to understand.  It is
also crucial to note that increasing the k-point mesh can cause the energy to
go up **or** down: we are changing the sampling of a numerical integration
method, and there is no variational principle.  You must test the convergence
of energies, energy differences and forces with k-point mesh.

Another consequence of periodicity is the interactions between images.  If we
have a defect in the cell, then we are not modelling an isolated defect, as we
might want, but actually a periodic array of defects.  My favourite example of
this at the moment is doping in semiconductors: the metal-insulator transition
in silicon lies at a dopant concentration of around \\(10^{19} cm^{-3}\\),
which is roughly one dopant for 50,000 atoms of silicon.  So almost all
density functional theory calculations of doped systems will be past the
metal-insulator transition.  This has huge consequences for the electronic
structure, and the interaction of dopant states.

There are also long-ranged strain and electrostatic interactions between
periodic images that have to be acknowledged (if not removed).  Elastic
interactions fall off as \\(1/r \rightarrow 1/r^3\\) depending on the
defect, and can cause distortions in the structure and stability of systems
that are being modelled.  This needs to be tested.  Electrostatic interactions
in periodic systems are a real problem: a charged defect would have an infinite
electrostatic energy, so is usually compensated (practically, the net charge
of the system is set to zero) but that can leave dipolar and higher order
interactions.  There are schemes to deal with these for high symmetry cells,
but they must be used with great caution.  You should always check the effect
of increasing cell size on your results.

Modelling reduced dimensionality problems requires approximation.  Surfaces  are
modelled using a slab of material, with either two identical surfaces  (and the
middle of the slab approximating the infinite bulk) or with one  surface
terminated in a bulk-like manner and the other surface forming the problem.  (On
elemental semiconductors, hydrogen termination is often used for bulk-like
termination, while for other systems simply leaving the bulk structure is often
reasonable.)  In both cases, it is important to test for convergence with slab
thickness; there will be both strain and electronic effects.  The strain  will
be clearest in systems such as semiconductors which reconstruct, and  is often
absorbed with ten to fifteen layers of material, but must be checked.  The
electronic  effects of the base must be tested for carefully, as the depth of
slab required will depend on the magnitude and sensitivity of the properties
being modelled.  I have seen some electronic effects still clearly visible even
with 30-40 layers of silicon.

There is also the problem of vacuum: the gap below and above the slab in the
unit cell must be large enough that the two surfaces do not interact.  It is
also important to account for the size of any molecules to be added to a surface
when testing for vacuum thickness.  There is no need to sample the Brillouin
zone along the direction normal to the surface: we want to remove any
periodicity in this direction.  This can in fact be a useful test of the
isolation across the vacuum: if changing the k-point sampling along the normal
to the surface changes the energy, then something is interacting.

We can also model one- and zero-dimensional systems, such as wires and isolated
clusters or molecules.  In these cases, there are two and three directions with
vacuum gaps, respectively, which must be tested, and the k-point sampling
becomes simpler (reducing to gamma point sampling for isolated systems).  It may
seem odd to model an isolated system with periodic boundaries; the simplicity of
solving the electrostatic problem with fast Fourier transforms (FFTs), along
with the compatibility with solid state problems, is the key driver for doing
this.  Careful consideration of the correct termination of surfaces of wires and
clusters is just as important as for surfaces, as it can have a strong effect on
the results.

These types of technical test can often seem rather tedious, and even
unnecessary (after all, many people have already tested these things, and we
might as well use their expertise).  Nevertheless, reproducibility is a
cornerstone of science, and understanding the effects of the approximations you
are making in your simulations is key; they can often be subtle.  When
performing simulations, it is just as vital to know the sources of error as when
performing experiments, and characterising the likely effects of termination or
sample size is a significant part of the preparation for any simulation.
