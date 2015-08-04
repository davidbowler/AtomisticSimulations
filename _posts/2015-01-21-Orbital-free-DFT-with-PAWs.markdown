---
layout: post
title: Orbital-free DFT with PAWs
category: DFT
tags: DFT LinearScaling
year: 2015
month: 1
day: 21
summary: An implementation of OFDFT using PAWs
published: true
---

Orbital-free DFT[[1]](#R1),[[2]](#R2) is a linear-scaling approach[[3]](#R3) to DFT which returns to the original spirit of DFT and Thomas-Fermi theory, in seeking only the charge density of the system.  The potential computational saving is immense, as we only have to consider one three dimensional variable rather than enough variables to describe the orbitals of the system. As there is no diagonalisation or orthogonalisation step, we achieve linear scaling.  However, there are two considerable problems with the method: first, the kinetic energy functional, which is not known; second, the potential due to the ions.  It is this second problem that a recent paper[[4]](#R4) seeks to address, by using the projector-augmented wave (PAW) method[[5]](#R5).

There has been considerable research on possible kinetic energy functionals for use in OFDFT, with two key approximations: the Thomas-Fermi (which uses a simple power of the density); and the von Weizsacker (which considers the gradient of the density as well as the density).  Most OFDFT approaches mix these at some level, and research into new functionals is on-going.

The second problem, with potentials due to the ions, is significant.  Since the only variable considered by the OFDFT method is the density, which is a local function, the potentials used must also be local. (In solid-state approaches, pseudopotentials are almost always non-local, that is functions of two points in space, which is possible as we are acting on different orbitals with different energies; the non-local form makes the pseudopotentials significantly more transferrable.)  There has been work on developing transferrable local potentials[[6]](#R6) for OFDFT, but this still presents a significant challenge.

Given this problem, an approach that uses the bare Coulomb potential is attractive.  Most OFDFT approaches use a grid-based approach, which makes the use of the bare Coulomb potential challenging, and the proposal to introduce PAWs for OFDFT is attractive.  But PAWs have the same requirement that non-local pseudopotentials do: they use multiple projector functions in multiple angular momentum channels, and are generally non-local.

The authors do not address this question clearly: there is a passing statement that all quantities  are generated as for a 1s orbital (i.e. spherically symmetric), and a single note that only one partial wave is used.  This assumption really deserves more discussion, and investigation: since the projectors are operating on the density (or more properly its square root), it is not clear whether the PAW method can be used faithfully in this context.

The results presented lead to further questions.  While isolated binary molecules are represented reasonably well, this is not a significant test of the method.  The bulk results are more concerning: the lattice constant and bulk modulus for lithium and carbon are very poorly described, and the authors introduce a new parameter to the kinetic energy, which they adjust to match the lattice constant.

While the idea of combining PAWs and OFDFT is an interesting one, the present paper leaves many unanswered questions, most relating to the implementation.  It is far from clear that the use of a single partial wave for PAW offers any advantage over the local pseudopotential approaches already proposed for OFDFT, and while the implementation in an existing code is simple, much more work is needed to test the reliability of the method.

#<a name="R1"></a>
[1] L. W. Wang and M. P. Teter, Phys. Rev. B 45, 13196 (1992) [DOI:10.1103/PhysRevB.45.13196](http://dx.doi.org/10.1103/PhysRevB.45.13196)

#<a name="R2"></a>
[2] M. Pearson, E. Smargiassi, and P. A. Madden, J. Phys.: Condens. Matter 5, 3221 (1993) [DOI:10.1088/0953-8984/5/19/019](http://dx.doi.org/10.1088/0953-8984/5/19/019)

#<a name="R3"></a>
[3] D. R. Bowler and T. Miyazaki, Rep. Prog. Phys. 75, 036503 (2012) [DOI:10.1088/0034-4885/75/3/036503](http://dx.doi.org/10.1088/0034-4885/75/3/036503)

#<a name="R4"></a>
[4] J. Lehtomäki, I. Makkonen, M. A. Caro, A. Harju, and O. Lopez-Acevedo, J. Chem. Phys. 141, 234102 (2014) [DOI:10.1063/1.4903450](http://dx.doi.org/10.1063/1.4903450)

#<a name="R5"></a>
[5] P. E. Blöchl, Phys. Rev. B 50, 17953 (1994) [DOI:10.1103/PhysRevB.50.17953](http://dx.doi.org/10.1103/PhysRevB.50.17953)

#<a name="R6"></a>
[6] S. Watson, B. J. Jesson, E. A. Carter, and P. A. Madden, Europhys. Lett. 41, 37 (1998) [DOI:10.1209/epl/i1998-00112-5](http://dx.doi.org/10.1209/epl/i1998-00112-5)
