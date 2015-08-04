---
layout: post
title: Energy-conserving ab initio molecular dynamics
category: Techniques
tags: DFT MD
year: 2014
month: 11
day: 14
summary: Ab initio MD with absolute energy convergence using an extended Lagrangian
published: true
---

Energy conservation in molecular dynamics is an important test of the propagators and parameters being used.  In first principles Born-Oppenheimer molecular dynamics (BOMD) this is complicated because of the need for self-consistent iteration to a consistent ground state charge density and potential.  The self-consistency cycle introduces errors since it is not possible to achieve a perfect ground state.  Moreover, to improve efficiency, the starting charge density is often predicted from previous charge densities; while this can results in impressive speed gains, it breaks the time-reversibility of the dynamics and will introduce an unavoidable energy drift.

This blog will discuss a set of papers[[1-4]](#R1) from Anders Niklasson in Los Alamos, which seek to remove this problem, and which have also shown a way to avoid the self-consistency cycle entirely.  The work is perhaps less well-known that it should be, and offers an excellent route to efficiency.  The recent linear scaling DFT molecular dynamics that we have published (going to over 32,000 atoms with DFT)[[5]](#R5) used the time-reversible formulation to conserve energy.

The time reversibility is built on an extended Lagrangian (and is known as XL-BOMD) which introduces a new dynamical variable, or set of variables, for the electronic degrees of freedom (whether charge density or density matrix) alongside the nuclear degrees of freedom, in a trick reminiscent of Car-Parrinello MD[[6]](#R6).  This degree of freedom, written below as P, evolves harmonically in time, depending on a frequency (set by the user) and the difference to the ground state charge density (or whatever is being used).

This degree of freedom, P, can be propagated in a time-reversible manner with an accuracy that only depends on the timestep.  The BOMD can thus be made time-reversible by starting the self-consistency cycle from P – resulting in stability and energy conservation in MD over long timescales.  On its own, this is a very valuable advance.

However, in a set of papers[[2-4]](#R2), Niklasson takes the idea further.  He suggests that, given the right functional for the energy, the self-consistency cycle can be removed.  This involves performing a single diagonalisation/minimisation without self-consistent update, and using P as the input charge density.  The scheme has been developed, with the latest paper[[4]](#R4) generalising the earlier work, and showing that the approach is applicable to systems regardless of gap, provided that an adiabatic separation between electronic and nuclear degrees of freedom can be made.

This work provides techniques to accelerate ab initio MD significantly; we have found that, in combination with linear scaling DFT, we can access picoseconds of MD for tens of thousands of atoms even with a simple initial implementation.  The two approaches together promise to make large-scale, long time accurate MD significantly more affordable.

#<a name="R1"></a>
[1] Phys. Rev. Lett. **100**, 123004 (2008) [DOI:10.1103/PhysRevLett.100.123004](http://dx.doi.org/10.1103/PhysRevLett.100.123004)

#<a name="R2"></a>
[2] J. Chem. Phys. **139**, 214102 (2013) [DOI:10.1063/1.4834015](http://dx.doi.org/10.1063/1.4834015)

#<a name="R3"></a>
[3] J. Chem. Phys. **140**, 044117 (2014) [DOI:10.1063/1.4862907](http://dx.doi.org/10.1063/1.4862907)

#<a name="R4"></a>
[4] J. Chem. Phys. **140**, 164123 (2014) [DOI:10.1063/1.4898803](http://dx.doi.org/10.1063/1.4898803)

#<a name="R5"></a>
[5] J. Chem. Theor. Comput. **10**, 5419 (2014) [DOI:10.1021/ct500847y](http://dx.doi.org/10.1021/ct500847y) (or arXiV [1409.6085](http://arxiv.org/abs/1409.6085))

#<a name="R6"></a>
[6] Phys. Rev. Lett. **55**, 2471 (1985) [DOI:10.1103/PhysRevLett.55.2471](http://dx.doi.org/10.1103/PhysRevLett.55.2471)
