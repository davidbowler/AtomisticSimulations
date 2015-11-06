---
layout: post
year: 2015
month: 11
day: 06
category: tutorials
tags: DFT pseudopotentials
summary: Some background to pseudopotentials and why we use them
title: An introduction to pseudopotentials
published: true
---
Pseudopotentials are widely used throughout physics (and in some parts of
quantum chemistry); in brief, they use the screening effects of the core
electrons on the valence electrons to replace the nuclear potential with a
softer[[1]](#R1) potential, and to remove the core electrons from the problem
that is being solved.  For a detailed discussion of pseudopotentials,
I recommend Richard Martin's book[[2]](#R2).

The ideas in pseudopotential theory date back at least fifty years, and come
from a solid-state background.  The difficulty with choosing a basis set for
solid-state problems comes from the two different behaviours of the
wavefunctions: near the nuclei, they are strongly bound, rapidly varying and
rather close to atomic wavefunctions; in the interstitial space, they are much
smoother, and slowly varying.

There are several methods which try to address this problem head-on, by
using a basis set with both characters: atomic-like functions near the nuclei,
supplemented with plane waves (e.g. the FLAPW method).  An early approach,
which is helpful in understanding the whole problem, is the orthogonalised
plane wave approach (OPW) where the basis functions are written as plane
waves, orthogonalised to local, atom-centred functions:

$$
\phi_q(\mathbf{r}) = e^{i\mathbf{q}\cdot\mathbf{r}} - \sum_j c_j u_j(\mathbf{r})
$$

The functions \\(u\_j(\mathbf{r})\\) are the atom-centred functions, often taken
to be the core wavefunctions from the atoms.  This formalism links directly to
the pseudopotential formalism, and gives a clear idea of the process being
undertaken.  With a pseudopotential, we only solve the Schrodinger equation for
the valence electrons, and we substitute the full potential with a
pseudopotential which is shallower and smoother than the full nuclear potential.
This potential is normally only applied up to a certain cut-off radius, beyond
which the pseudopotential is exactly equal to the full potential.

A good reproduction of the scattering of the nuclear potential often requires
that different potentials are found for each angular momentum value.  You  will
find the terms semi-local (where a potential is non-local in angular variables
but local in radius, r: \\(\hat{V} = \sum\_{lm} \vert Y\_{lm}\rangle V\_(r)
\langle Y\_{lm}\vert\\) and non-local (where the pseudopotential is written in
a separable form, such as \\(\sum\_j f_j(\mathbf{r}) g_j(\mathbf{r^\prime})\\)
which is fully non-local in all variables; this becomes an integral operator)
though a detailed discussion is beyond the scope of this blog.

### Types of pseudopotential

There are three commonly used types: norm-conserving; ultrasoft; and
projector-augmented waves (PAWs)[[3]](#R3).  It is important to understand the
basic ideas behind each of these types.  When considering pseudopotentials, it
is important to be aware of the accuracy of the potential and its
*transferability*: that is,  how accurate it is in different environments, in
particular going from the simple, atomic calculation where it is generated to
a more complex environment.

**Norm-conserving pseudopotentials** require the norm of the pseudofunctions to be
the same as the norm of the all-electron wavefunctions:

$$
\int_0^{r_c} r^2 \vert \psi^{PS}_i(\mathbf{r})\vert^2 dr =
\int_0^{r_c} r^2 \vert \psi^{AE}_i(\mathbf{r})\vert^2 dr
$$

where \\(r\_c\\) is the core radius of the pseudopotential.  This requirement
was found to make more accurate, transferrable pseudopotentials (it can be shown
that it enforces a further condition, that the energy derivative of the
logarithmic derivative of the pseudofunctions also match those of the
all-electron wavefunctions).

These pseudopotentials are still commonly used, but are challenging particularly
for first-row elements and transition metals, where there are no core electrons
to screen the potential.  Accuracy and transferability often lead to small
core radii, which in turn makes the potential harder.

**Ultrasoft pseudopotentials** relax the norm-conservation constraint, and often
introduce multiple potentials for each angular momentum channel.  This enables
a softer potential, and matching the all-electron eigenvalues at more energies,
often giving a larger core radius than is possible for a norm-conserving pseudopotential
with the same accuracy.

**Projector-augmented waves** (PAWs) give what is effectively an all-electron
method with the cost of a pseudopotential method (though there are some
subtleties here).  There is a very strong link between PAWs and ultrasoft
potentials[[4]](#R4).  The method defines a transformation between the
pseudofunctions and the all-electron wavefunctions, that changes the
pseudofunctions only within spheres centred on atoms.

Within these spheres, the wavefunctions are expanded as partial waves, with
coefficients that are found using projector functions (which are very similar
to projector functions found in pseudopotentials).  The PAW method allows for
the easy reconstruction of all-electron wavefunctions, but does not allow the
core wavefunctions to respond during the simulation.

### A caution

I want to end with a caution: there are now many libraries of pseudopotentials
that are available with electronic structure codes, and there is a strong
temptation to simply use the potential without any testing.  This is a very
dangerous thing to do, and every potential should always be tested carefully
before embarking on production calculations.  You should be clear about how
they were generated (in particular about the core radii) and how they respond
in situations of differing density or valence.  A pseudopotential is an
approximation, though a well understood and well-defined one, and that requires
testing and understanding.

<a name="R1">[1]</a> Soft when applied to potentials means smooth and shallow,
while hard means rapidly varying and generally deep.  The number of bound
states in the potential will correlate with its depth, and because higher
states are orthogonal to lower, they must have more nodes.  More nodes in a
function generally means a higher second derivative (i.e. kinetic energy)
which in turn makes representation either on a real-space grid or with
Fourier components (plane waves) more costly.

<a name="R2">[2]</a> *Electronic Structure*, Richard M. Martin, Cambridge 2004.
[Book website](http://electronicstructure.org)

<a name="R3">[3]</a> There are some people who do not class PAWs as
pseudopotentials, though this distinction is largely academic.

<a name="R4">[4]</a> G. Kresse and D. Joubert, Phys. Rev. B **59**, 1758 (1999)
[DOI:10.1103/PhysRevB.59.1758](http://dx.doi.org/10.1103/PhysRevB.59.1758)
