---
layout: post
year: 2015
month: 10
day: 23
category: tutorials
tags: DFT
summary: A discussion of how DFT calculations scale with system size
title: Scaling of DFT calculations with system size
published: true
---
As I made clear in my [previous tutorial blog]({{site.baseurl}}/blog/why-minimisers), I think that it is important for
people using DFT codes to understand some of the internal mechanics.  This blog
will deal with another technical issue: scaling of the problem with system size.

Why should this matter ? Pragmatically, it is important to know both how long
your simulation is likely to take before starting in on it and how large a
computational resource you may need.  This will also determine whether you can
ask certain questions in your simulations: if they will require unreasonable
timescales or computer resources[[1]](#R1) then a different study should be
designed.  There are two key resources: total run time, and memory required.
I will discuss run time below; the memory required in general scales with the
square of the system size.

The overall scaling of standard DFT codes is often given as \\(N^{3}\\), where
\\(N\\) is some measure of system size (whether number of atoms, number of bands
or number of basis functions).  In plane wave codes, the basis set increases
with unit cell volume independently of number of atoms or bands, and this
affects the amount of vacuum that is used in surface or molecular studies.
However, the simple form of scaling is the only factor: the pre-factor is
important, as is the quantity that scales.

Prefactors will determine the system size at which cubic scaling will become
dominant: if the cubic scaling operation has a very small cost compared to a
quadratic or linear scaling operation, it will only become significant with
large system sizes.  This is one reason why linear scaling DFT codes are not
more widely used: the pre-factor, at the moment, is rather large.  The question
of what scales contributes to the pre-factor: to stay with the plane wave
example, the number of plane waves is much larger than the number of bands, so
an operation that scales as \\(N\_{bands} \times N\_{PW}\\) will be much cheaper
than one that scales as \\(N\_{PW} \times N\_{PW}\\).

The total energy in DFT is often found by adding different contributions, and
these scale differently.  The Hartree energy, along with the local
pseudopotential energy and the exchange-correlation energy, is found as an
integral of a potential with a charge density, and scales linearly with the
system size.  The kinetic energy requires an integral for each band, and so
scales as \\(N\_{bands} \times N\_{PW}\\) (we can substitute the number of points
on a real-space grid for the number of plane waves if this is how the
integral is performed), though this has a small pre-factor.

The most expensive part of the energy is the non-local pseudopotential energy, which also
scales as \\(N\_{bands} \times N\_{PW}\\), but has a larger pre-factor.  It is
more efficient to evaluate this energy using non-local projector functions in
real-space than in reciprocal space, but it is still a high cost.  In
plane wave codes, fast Fourier transforms (FFTs) are also expensive: they
scale as \\(N\_{bands} \times N\_{PW} ln N\_{PW}\\) when all wavefunctions are
transformed.  Fortunately, they are highly optimised on modern computers; they
do, however, involve communication between all processes on a parallel machine,
which limits their scaling with number of processes[[2]](#R2).

The cubic scaling that limits DFT approaches actually comes from the requirement
to orthogonalise the eigenstates to each other (in a code which optimises the
wavefunctions rather than diagonalising the Hamiltonian---which also scales
with the cube of the matrix size).  This operation cannot be avoided, but does
have a small pre-factor, so only becomes significant at large system sizes.

One factor which actually improves scaling is the Brillouin zone sampling.  All
of the operations described above have to be performed at *each k-point*,
giving a prefactor of \\(N\_k\\) to each cost.  As we go to larger system sizes
the Brillouin zone sampling required reduces, and the net cost of a simulation
scales more slowly than might be expected.  However, once truly large systems
have been reached, this factor goes to one and cubic scaling dominates.

It is important, therefore, to build up an understanding of how long different
calculations will take to run with small simulations, before embarking on a
larger simulation.  It is also important to realise that parallel scaling
is not perfect, and the speed-up gained from increasing the number of
processes will be lower than linear (though the memory requirements per
process will improve).  I should also note that it is possible to achieve
linear scaling of computational cost (both in time and memory), and to go
to millions of atoms[[3]](#R3).

<a name="R1">[1]</a> It is important to remember that time on high-performance
computing (HPC) resources is often restricted and awarded through grants, so
needs to be used wisely.

<a name="R2">[2]</a> Since modern CPUs are multi-core and even run more threads
than there are cores, it makes most sense to refer to the number of processes
running than the number of CPUs/cores.

<a name="R3">[3]</a> J. Phys.: Condens. Matter **22** 074207 (2010) [DOI:10.1088/0953-8984/22/7/074207](http://dx.doi.org/10.1088/0953-8984/22/7/074207)
