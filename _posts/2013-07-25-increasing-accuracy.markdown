---
layout: post
year: 2013
month: 07
day: 25
title: Increasing accuracy of DFT with hybrids and corrections
summary: Corrections to hybrid functionals for defect energies
category: accuracy
tags: DFT hybrids accuracy
published: true
---
While density functional theory (DFT), which is discussed in detail in Chapter 8, is very widely used, it has many well-known failings. The failure to calculate band gaps correctly is key when modelling defect states in materials such as semiconductors. Hybrid functionals, such as B3LYP and PBE0, include a fraction of the Hartree-Fock exchange in place of the DFT exchange, and are often used to alleviate this problem but they are not a panacea: they don’t fix everything. Two recent papers suggest corrections to be added to a specific hybrid functional to improve energies. The functional they choose, HSE06[[1]](#R1), is a range-separated functional: exact exchange is only used for the short-ranged part of exchange and GGA exchange is used for the long-ranged part. This reduces the computational cost significantly, and can improve transferrability.

The simplest uses an occupancy correction[[2]](#R2) which is rather similar to the +U in LDA+U (where the occupancy of certain orbitals leads to an energy change). The paper is concerned with transition metal (TM) defects in silicon and silicon carbide, which they find cannot be described fully by the HSE06 functional; specifically, the highest occupied Kohn-Sham eigenvalue is not the ionization energy. This is maybe not surprising, as the defect is highly localised and the bands rather diffuse. They propose a simple correction to alleviate the problem, which closely resembles LDA+U. The results are interesting, and the final scheme seems to give a route to fitting the effective U for these systems. (An earlier paper by this group[[3]](#R3) found that for host-related defects such as vacancies and interstitials, the HSE06 functional works extremely well.)

The second paper[[4]](#R4) studies stability and mobility of self-interstitials and vacancies in silicon, using HSE06, but adding a semi-empirical van der Waals (vdW) term[[5]](#R5) which accounts for frequency-dependent polarisability by including long-range screening effects. While the hybrid functional tends to over-estimate diffusion barriers, and the original PBE GGA functional underestimates formation energies, the vdW correction mitigates both problems. Van der Waals interactions are often thought to apply only to molecules, but there is increasing evidence of their importance in solids (see, for instance, one of my papers[[6]](#R6)). This is a fertile field in DFT, with both semi-empirical vdW corrections to DFT, as well as density functionals for vdW being developed.

<a name="R1">[1]</a> J. Chem. Phys. **118**, 8207 (2003) [doi:10.1063/1.1564060](http://dx.doi.org/10.1063/1.1564060)

<a name="R2">[2]</a> Phys. Rev. B **87**, 205201 (2013) [doi:10.1103/PhysRevB.87.205201](http://dx.doi.org/10.1103/PhysRevB.87.205201)

<a name="R3">[3]</a> Phys. Rev. B **81**, 153203 (2010) [doi:10.1103/PhysRevB.81.153203](http://dx.doi.org/10.1103/PhysRevB.81.153203)

<a name="R4">[4]</a> Phys. Rev. Lett. **111**, 045501 (2013) [doi:10.1103/PhysRevLett.111.045501](http://dx.doi.org/10.1103/PhysRevLett.111.045501)

<a name="R5">[5]</a> Phys. Rev. Lett. **108**, 236402 (2012) [doi:10.1103/PhysRevLett.108.236402](http://dx.doi.org/10.1103/PhysRevLett.108.236402)

<a name="R6">[6]</a> Phys. Rev. B **83**, 195131 (2011) [doi:10.1103/PhysRevB.83.195131](http://dx.doi.org/10.1103/PhysRevB.83.195131)
