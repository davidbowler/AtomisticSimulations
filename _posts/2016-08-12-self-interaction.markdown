---
layout: post
year: 2016
month: 08
day: 12
category: dft
tags: DFT Accuracy
summary: A discussion of a recent paper testing self-interaction correction for atomisation energies
title: Self-interaction correction in DFT
published: true
---
One of the biggest problems facing DFT is that of self-interaction: each
electron effectively interacts with itself, because the potential derives from
the total charge density of the system.  This is not an issue for the exact
(unknown) density functional, or for Hartree-Fock, but is the cause of
significant error in many DFT functionals.  Approaches such as
DFT+U[[1]](#R1),[[2]](#R2),[[3]](#R3) and hybrid functionals (far too many to
reference !) are aimed in part at fixing this problem.

Probably the earliest attempt to remove this error is the self-interaction
correction of Perdew and Zunger[[4]](#R4) which corrects the potential for each
Kohn-Sham orbital, complicating the calculation considerably over a standard DFT
calculation.  (Ironically, this paper, which has over 11,000 citations, is best
known for its appendix C, where a parameterisation of the LDA XC energy is
given.)  However, this process is notoriously slow to converge and is not widely
used.

A recent paper[[5]](#R5) showed that, even for isolated molecules, complex orbitals
were required to achieve convergence, and this approach has now been tested for
atomisation energies of a standard set of 140 molecules[[6]](#R6).  The tests compare
the new complex SIC implementation against the standard, real implementation, as
well as various GGAs, hybrid functionals and meta-GGAs.  The complex SIC, when
coupled with the PBEsol functional[[7]](#R7), gives good results (though ironically the
PBEsol functional was developed to improve PBE for solids).  Not surprisingly,
the best results are from hybrids, but meta-GGA improves the energies almost as
well.

This study highlights the problem with DFT at the moment: there are many
different approaches, which often work well for specific problems. SIC is
cheaper than hybrid calculations, and can be important for charge transfer
problems (and Rydberg states).  The results for convergence and complex orbitals
are interesting, but based on these results, I would use meta-GGA for
atomisation energies, as a good compromise between accuracy and cost (almost the
same as GGA).

<a name="R1">[1]</a> Phys. Rev. B **52**, R5467 (1995) [DOI:10.1103/PhysRevB.52.R5467](http://dx.doi.org/10.1103/PhysRevB.52.R5467)

<a name="R2">[2]</a> Phys. Rev. B **57**, 1505 (1998) [DOI:10.1103/PhysRevB.57.1505](http://dx.doi.org/10.1103/PhysRevB.57.1505)

<a name="R3">[3]</a> Int. J. Quantum Chemistry **114**, 14 (2014) [DOI:10.1002/qua.24521](http://dx.doi.org/10.1002/qua.24521)

<a name="R4">[4]</a> Phys. Rev. B. **23**, 5048 (1981) [DOI:10.1103/PhysRevB.23.5048](http://dx.doi.org/10.1103/PhysRevB.23.5048)

<a name="R5">[5]</a> J. Chem. Theory Comput. **12**, 3195 (2016) [DOI:10.1021/acs.jctc.6b00347](http://dx.doi.org/10.1021/acs.jctc.6b00347)

<a name="R6">[6]</a> J. Chem. Theory Comput. *in press* (2016) [DOI:10.1021/acs.jctc.6b00622](http://dx.doi.org/10.1021/acs.jctc.6b00622)

<a name="R7">[7]</a> Phys. Rev. Lett. **100**, 136406 (2008) [DOI:10.1103/PhysRevLett.100.136406](http://dx.doi.org/10.1103/PhysRevLett.100.136406)
