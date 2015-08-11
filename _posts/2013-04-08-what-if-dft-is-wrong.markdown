---
layout: post
year: 2013
month: 04
day: 08
title: What should you do if DFT gets the order wrong ?
category: DFT
tags: parameters experiment Comparison
published: true
---
This question came up over coffee this morning, in discussion with a PhD
student, Martin Uhrin. It covers a number of useful points, which we’ll discuss
in order. Many of these ideas are discussed in Chapter 13, Tests, and Chapter
18, Comparison to Experiment.

### What do we mean by the wrong order ?

The original question was related to the stability of different phases (in
particular, phosphorus). But we need to be very careful with this question. What
is observed in experiment may well not be the thermodynamic ground state (i.e.
the lowest energy structure). An equivalent statement is that the DFT ground
state (or lowest energy structure) may not be what is found in experiment,
unless great care is taken. A classic example is the Si(111) surface, which is
often found with a (2×1) reconstruction when prepared. However, if it is
annealed to relatively high temperatures, it transforms to the well-known but
complex (7×7) reconstruction, which is the lowest energy structure.

Of course, if we’re talking about the order of different electronic states, then
we need to be very careful indeed. There is no guarantee that the Kohn-Sham
states will bear any resemblance to observed states, particularly in the
unoccupied states (also known as conduction bands in condensed matter, or
virtual orbitals in quantum chemistry). It is no surprising if the order of
these energies is wrong; equivalently, the order of excitations is often wrong
in DFT.

### Check the parameters you have used

The first check you should make is the completeness of the basis set. This is
often the source of problems, and you should investigate whether increasing the
size of the basis changes the order found. With a plane wave basis, this is
relatively easily done by increasing the plane wave cutoff, albeit at the cost
of increased computational workload. With atomic orbitals, it is harder to
increase the basis set systematically, and you will need to check carefully.
(Another effect which may occur with plane waves and different bulk phases is
Pulay stresses: a low plane wave cutoff will give unphysical energy changes with
volume changes as the basis set changes.  There are normally finite basis set
corrections that can be applied, but you should always use a large plane wave
cutoff when varying volume.)

For a periodic system, you should also check the Brillouin zone sampling (or the
k-point set you are using). Particularly when comparing different phases or
structures, this can be surprisingly sensitive. If there are delocalised states,
they will generally have complex k-space structure, and high k-point sampling
will be required.

Many DFT calculations use pseudopotentials (or effective core potentials) and
these can have an effect. It is becoming more and more standard to use a library
of pseudopotentials, and if the structures you are working with are close to
those used in testing the pseudopotential this is acceptable. But you should
test that the pseudopotential is reasonable (e.g. for the standard bulk phases
of the system you are modelling). Core radii can have a strong effect,
particularly for soft pseudopotentials. If the atomic distances vary between
structures or phases then you need to be sure that the cores do not overlap, or
come close enough to have an effect on the energy in all structures. Oxidation
states can also be important (there certainly was a time when people generated
pseudopotentials for transition metal ions in specific oxidation states, and
these tended not to be transferrable).

### Check the functional

If the energies appear wrong, you should investigate the effect of different
functionals. (As a side note, you should also check that the pseudopotential was
generated with the functional you are using, otherwise you will introduce
errors.) Does using LDA or different GGA functionals change the order ? How do
the lattice parameters vary as you change functional, and are they close to the
experimental lattice parameter ? Are you using the DFT relaxed lattice parameter
(and are you relaxing the lattice parameter for each functional) ? A notable
example here is for the semiconductor germanium, which is found to be metallic
by LDA but with a small gap for most GGAs. However, the gap can be extremely
sensitive to the lattice parameter, which in turn can depend on the basis set.
Care is needed !

You should also check the electronic structure for different functionals. Is
there a gap where there should be one ? We have alluded to germanium just now,
but ZnO is another case where the gap is far smaller than it should with
standard DFT functionals.

### Check the physics

You should consider the essential physics of the structures or phases you are
modelling carefully. Does DFT contain the correct physics to work correctly ? If
there are strong correlations in the material, then DFT will clearly not work
well, though there are approaches which can help (e.g. LDA+U or hybrid
functionals can improve these systems). Is the bonding unusual, or are there
localised states in the system ? Again, there are known errors in DFT which will
make these hard to model well.

### Conclusion

I have suggested many tests above, and this is probably a good thing.
All of science requires careful testing of the parameters and assumptions used,
and computational modelling is no different. You should have tested the effects
of your parameters and the functionals chosen. As well as the relevant chapters
(Chapter 12, The Nuts and Bolts, and Chapter 13, Tests), there are papers
available which discuss some of these issues in more detail. One of my
favourites is:

Modelling Simul. Mater. Sci. Eng. **13**, R1 (2005) [DOI: 10.1088/0965-0393/13/1/R01](
http://stacks.iop.org/MSMSE/13/R1)
