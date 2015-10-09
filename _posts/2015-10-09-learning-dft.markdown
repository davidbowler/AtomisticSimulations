---
layout: post
year: 2015
month: 10
day: 09
category: tutorials
tags: DFT
summary: Introducing a series of tutorials on DFT and atomistic simulations
title: An Introduction to DFT and Atomistic Simulations
published: true
---
I am supervising four Masters projects this year, in each case involving
extensive DFT calculations.  We are meeting at least weekly, as the students
learn to do DFT calculations, and I thought that blogging what we have discussed
in each session would form a useful set of tutorials as an accompaniment to the
book (which of course covers much more material than I could here, and which
I would encourage you to buy if you want to learn about atomistic simulations
in depth).

The basic approach in any DFT code that we take to perform a simulation is to:
choose a simulation cell (a box in space which is generally periodic); place
atoms in the box at the positions that we want; solve the Kohn-Sham equations
for the wavefunctions. Once we have done this, we will have found the total
energy for the system, along with the forces on atoms, and (in DFT at least) the
eigenstates and eigenvalues, which tell us about the electronic structure of the
system.  From here, we can go on to do many things, such as relaxations and MD
runs, but how do we solve the equations ?

We start with the Kohn-Sham equations for the eigenstates \\(\psi_n(\mathbf{r})\\) :

$$
\hat{H}\psi_n(\mathbf{r}) = \left(-\frac{\hbar^2}{2m}\nabla^2 +
V_{eI}(\mathbf{r}) + V_{Ha}(\mathbf{r}) + V_{XC}(\mathbf{r})\right)
\psi_n(\mathbf{r}) = \epsilon_n\psi_n(\mathbf{r})
$$

We will consider these equations in two parts: first, the eigenstates; second,
the Hamiltonian.  The eigenstates must be represented in terms of a (finite)
basis for practical calculations, and there are many used in DFT codes.  The two
which I will discuss are: plane waves, \\(e^{i\mathbf{G}\cdot\mathbf{r}}\\); and
atomic-like orbitals, typically written as \\(R\_{l\zeta}(r) Y^{l}\_{m}(\hat{\mathbf{r}}) \\).  
I will cover atomic-like orbitals in a later
post; for now we will consider plane waves.  We write:

$$
\psi_n(\mathbf{r}) = \sum_i c_{in} \chi_{i}(\mathbf{r})
$$

where \\(\chi\_{i}(\mathbf{r})\\) is a basis function, and the basis coefficients
\\(c\_{in}\\) are what we need to find.

The smallest wavevector we can use is set by the size of the simulation cell;
the largest wavevector is a choice that we have to make.  We have to truncate
the expansion in order to have a practical calculation, but if we make it too
small, then we introduce errors.  Typically, we have to perform tests on the
properties of the system that we are interested in, and check that they are
converged with respect to the basis size (N.B. some codes set a cutoff
automatically, but this is not a guarantee of accuracy - you should always
check).  Typically, the maximum wavevector, which gives the number of plane
waves, is set by specifying a cutoff  energy, so that \\(E\_{\mathrm{cutoff}} =
\hbar^2 k\_{max}^2/2m\_e\\).  

Once we have specified the basis, and we have the Hamiltonian, we need to
solve for the ground state.  With a small basis (such as atomic-like orbitals)
we can form a Hamiltonian matrix and diagonalise; however, with plane waves
this is impractical, and we have to perform a minimisation.  Roughly speaking,
this involves:

1. Choose starting coefficients \\(c\_{in}\\) (either randomly or following a previous calculation)
1. Calculate the energy and the gradient of the energy with respect to the coefficients \\(\partial E/\partial c\_{in}\\)
1. Vary the coefficients along the gradient to minimise the energy
1. At the minimum, re-evaluate the energy and gradient
1. Choose a new search direction (typically combining the gradient with previous search directions) and return to 3.

This is a very rough description of the process - typically a code will use
something like conjugate gradients for the minimisation (see Chapter 5 for more
detail).  The key question is when to stop: we must set some criterion which the
computer can use.  This is often an energy difference or a fractional energy
difference between steps, and will affect the accuracy of the result.  We also
have to set a fail-safe: a maximum number of steps before termination (to avoid
an infinite loop).

We now turn to the Hamiltonian.  The kinetic energy term is easy to calculate,
as is the Hartree term, which is a simple electrostatic term:

$$
V_{Ha}(\mathbf{r}) = \int d\mathbf{r}^{\prime} \frac{n(\mathbf{r}^{\prime})}{\vert \mathbf{r} - \mathbf{r}^{\prime}\vert}
$$

However, there is a problem here: this potential depends on the charge density,
which is defined as \\( n(\mathbf{r}) = \sum\_{n} \vert \psi\_{n}(\mathbf{r})\vert^{2} \\).
So we require a charge density to find the eigenstates which give us the  charge
density.  We have to make an initial guess for a charge density, solve  for the
eigenstates, and then find the resulting output charge density.  From   that
density and previous densities we form a new, input density and iterate  until
the input and output densities are the same (again, there is a  termination
criterion required here).  This process is known as self-consistency,  and can
be highly non-linear and rather unstable (though modern DFT codes are generally
good at solving the problem).  The starting density is normally taken to be a
sum of atomic densities (or the charge density from a previous calculation).

The electron ion term is, in principle, also a simple electrostatic term
(effectively a sum over \\(Z/r\\) terms from all the ions), but in most solid state
DFT codes it changes.  Rather than solve for all the electrons and the full
nuclear potential, we replace the core electrons *and* the nuclear potential
with a *pseudopotential*, where the central part of the nuclear potential is
screened by the core electrons.  This makes the potential softer (having a lower
curvature) and reduces the number of eigenstates that we need to solve.  It is
important to understand that pseudopotentials are an approximation and should
be tested for the system you are modelling: there are various libraries which
are available, but they cannot guarantee accuracy.

The last term, the exchange-correlation potential, is defined as a functional
derivative of the exchange-correlation energy \\(V\_{XC}[n] = \delta
E\_{XC}[n]/\delta n\\), where \\(n(\mathbf{r})\\) is the charge density.  The two most
common approaches to this are the local density approximation (LDA) and one of
the generalised gradient approximations (GGAs).  I will write about these in a
later post.

The last thing that is needed for a standard solid state calculation is to
consider the integration over the Brillouin zone.  Since we treat the system
as periodic, the energy we calculate is a function of the reciprocal lattice
vector (read up on Bloch's theorem if you are not sure about this - see
Chapter 9 in the book, and a good solid state text book).  Most DFT codes use
a scheme such as the Monkhorst-Pack scheme to distribute k-points evenly
through the Brillouin zone, but you **must** test the convergence of the energy
with respect to k-point sampling, and note that it is not monotonic: the energy
can go up as well as down as the k-mesh density increases.

At this stage, I have set my students the task of creating input files for bulk,
crystalline silicon (in a cubic cell with eight atoms in the first place), and
exploring the convergence of energy with k-point mesh and plane-wave cutoff.
Running calculations is an important part of learning to do DFT simulations.
