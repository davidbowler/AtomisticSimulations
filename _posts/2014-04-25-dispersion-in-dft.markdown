---
layout: post
title: Approaches to dispersion in DFT
year: 2014
month: 04
day: 25
category: DFT
tags: DFT vdW
published: true
---
Dispersion is a general term referring to weak, long-range dynamic correlations in electronic structure (and includes van der Waals interactions). These interactions are not well modelled by standard DFT functionals, and the development of approaches to fix this has been a growth area in DFT recently. I will cover the most common approaches here, at a relatively non-technical level, but there are many references at the end which give much more detail. Probably the most important piece of technical information in this area is that the leading term in a distance-based expansion of the dispersion energy falls off as \\(1/r^6\\), and the expansion coefficients of this term are often known as C6 coefficients. Most practical approaches to dispersion use semi-local forms: the interaction is either between pairs of atoms or between two points in space.

We can divide approaches to dispersion in DFT into four areas, becoming steadily more accurate (and expensive) as we progress: semi-empirical, pair-wise approaches to the C6 terms, such as DFT-D2[[1]](#R1); approaches which go beyond semi-empirical, and introduce environment-dependent C6 coefficients and some ab initio information, such as DFT-D3[[2]](#R2) and TS[[3]](#R3); density functional approaches based on the vdW-DF-04[[4]](#R4) of Langreth and Lundqvist (and co-workers); and approaches which go beyond pair-wise additivity, such as many-body dispersion[[5]](#R5) and the random-phase approximation (RPA)[[6]](#R6). Many of these are available in standard DFT codes, though the functionals with which they can be paired vary from method to method (and can have a strong effect on their accuracy[[7]](#R7)).

Almost all pair-wise approaches also introduce scaling factors. A short-range scaling is used to turn off dispersion when atoms are close together (both for pragmatic and physical reasons: \\(1/r^6\\) diverges as r becomes small, and dispersion is negligible at small distances). A long-range cutoff or scaling is also common, to remove unnecessary computational load and reduce scaling; pragmatically, the \\(1/r^6\\) scaling will make this an excellent approximation.

Semi-empirical approaches, the most well-known of which is the DFT-D2 approach, fit C6 terms to ab initio or experimental data. DFT-D2 was based on a particular GGA functional (at least in the original paper, though it is now used with many others) and adds C6 terms pair-wise between all atoms, hence giving a formal scaling of \\(N^2\\). The C6 terms are calculated from atomic polarisabilities, and take no account of the environment; the approach is aimed at biomolecules in the main, where it has had significant success; it is, however, essentially a force-field.

Including environment dependence as well as ab initio data makes approaches significantly more transferrable. The TS method starts with C6 coefficients found for interactions between free atoms, calculated with self-interaction corrected TDDFT. These terms are assembled and weighted using a scheme to divide the ground state DFT charge density and assign contributions to atoms at different points in space. The DFT-D3 method also uses TDDFT calculations for polarisabilities (though with a hybrid functional – which can have a similar effect). The method calculates not just pair-wise C6 terms but also three-body C9 terms (using a geometric mean of the isolated C6 terms), and weights the C6 terms via a coordination number. Both methods are more sophisticated than a simple force field approach, and allow the geometry or DFT density to influence the calculations. They are applicable to most of the periodic table, and are accurate and fast.

The next stage in accuracy is to base the dispersion calculation on the DFT density itself. All functionals in this class are based on the work of Langreth, Lundqvist and co-workers, known as vdW-DF[[4]](#R4). The approach is to take a double integral over a pair of points in space, and at each point calculate the product of the charge densities at the two points and a kernel which depends on both points. The form of the kernel, of course, is the key point that determines the accuracy and effectiveness of the method. The original vdW-DF is based on the frequency-dependent plasmon-pole model (which is the correct physics), and is accurate for many different problems. The method was extended to be self-consistent[[8]](#R8) and has been updated[[9]](#R9) to improve the response of the functional to gradients in the charge density, making it more accurate (these functionals are commonly known as vdW-DF04 and vdW-DF10). A significant variant on the kernel used was proposed and optimised by Vydrov and van Voorhis[[10]](#R10): this approach is numerically simpler, but makes approximations which are somewhat controversial (there is a Comment and a Reply if you follow the Phys. Rev. Lett. reference below). There have been a number of papers on implementing these kernels, which are numerically demanding, the most important of which uses a factorisation of the kernel and fast fourier transforms to reduce the scaling to N log(N)[[11]](#R11). This implementation is available in SIESTA and as a stand-alone package, and is an important contribution to the community. There have been a number of investigations of how the exchange functional paired with this functional can affect energy; one of these (which I co-authored) found that chemical accuracy could be achieved[[7]](#R7).

Going beyond pair-wise interactions is challenging. The TS method has been extended to include many-body dispersion[5] by representing interactions between atomic densities in terms of quantum harmonic oscillators. The method uses the electron density from DFT, and involves the diagonalisation of a Hamiltonian with the dimension of the number of atoms. As the name suggests, the method includes terms beyond two-body, and effectively sums the coefficients to infinite order. It requires the fitting of a range-separation parameter (to prevent the small distance divergence). The other main method is the RPA (random phase approximation)[[6,12]](#R6) which is an approach to separating the one-particle and collective degrees of freedom for an electron gas. The RPA has been included into DFT, and is a good approximation to correlation, including dispersion effects; it is often applied post hoc, in other words non-self-consistently, and is computationally expensive, scaling as N^4 at best.

What can we conclude ? At present, the cheapest, reasonably reliable approaches are DFT-D3 and TS, which add little to the cost of a standard DFT calculation, and are available in various codes (both, I think, are in VASP and QuantumEspresso). An implementation of DFT-D3 and a complete set of coefficients are freely available from Stefan Grimme. The vdW-DF functionals, in whatever form you choose, are more expensive, but are more satisfying to many people, as they do not impose any division into atoms or fragments. For now, I would say that the many-body work is still experimental, and should only be tackled by experts. But overall, dispersion can now be included in DFT calculations with some confidence, and should be a part of most investigations.

<a name="R1">[1]</a> S. Grimme, J. Comput. Chem. **27**, 1787 (2006) [DOI:10.1002/jcc.20495](http://dx.doi.org/10.1002/jcc.20495)

<a name="R2">[2]</a> S. Grimme, J. Antony, S. Ehrlich & H. Krieg, J. Chem. Phys. **132**, 154104 (2010) [DOI:10.1063/1.3382344](http://dx.doi.org/10.1063/1.3382344)

<a name="R3">[3]</a> A. Tkatchenko & M. Scheffler, Phys. Rev. Lett. **102**, 073005 (2009) [DOI:10.1103/PhysRevLett.102.073005](http://dx.doi.org/10.1103/PhysRevLett.102.073005)

<a name="R4">[4]</a> M. Dion, H. Rydberg, E. Schroder, D. C. Langreth & B. I. Lundqvist, Phys. Rev. Lett. **92**, 246401 (2004) [DOI:10.1103/PhysRevLett.92.246401](http://dx.doi.org/10.1103/PhysRevLett.92.246401)

<a name="R5">[5]</a> R. A. DiStasio, Jr, O. A. von Lilienfeld and A. Tkatchenko, Proc. Nat. Acad. Sci. **109**, 14791 (2012) [DOI:10.1073/pnas.1208121109](http://dx.doi.org/10.1073/pnas.1208121109)

<a name="R6">[6]</a> J. F. Dobson & T. Gould, J. Phys.: Condens. Matter **24**, 073201 (2012) [DOI:10.1088/0953-8984/24/7/073201](http://dx.doi.org/10.1088/0953-8984/24/7/073201)

<a name="R7">[7]</a> J. Klimes, D. R. Bowler & A. Michaelides, J. Phys.: Condens. Matter **22**, 022201 (2010) [DOI:10.1088/0953-8984/22/2/022201](http://dx.doi.org/10.1088/0953-8984/22/2/022201)

<a name="R8">[8]</a> T. Thonhauser, V. R. Cooper, S. Li, A. Puzder, P. Hylgaard and D. C. Langreth, Phys. Rev. B **76** 125112 (2007) [DOI:10.1103/PhysRevB.76.125112](http://dx.doi.org/10.1103/PhysRevB.76.125112)

<a name="R9">[9]</a> K. Lee, E. D. Murray, L. Kong. B. I. Lundqvist and D. C. Langreth, Phys. Rev. B **82**, 081101(R) [DOI:10.1103/PhysRevB.82.081101](http://dx.doi.org/10.1103/PhysRevB.82.081101)

<a name="R10">[10]</a> O. A. Vydrov and T. van Voorhis J. Chem. Phys. **133**, 244103 (2010) [DOI:10.1063/1.3521275](http://dx.doi.org/10.1063/1.3521275) (see also Phys. Rev. Lett. **103** 063004 and the attendant Comment/Reply)

<a name="R11">[11]</a> G. Roman-Perez & J.M. Soler, Phys. Rev. Lett. **103**, 096102 (2009) [DOI:10.1103/PhysRevLett.103.096102](http://dx.doi.org/10.1103/PhysRevLett.103.096102)

<a name="R12">[12]</a> There is a large, complex literature; see, for instance, J. Mater. Sci. **47**, 7447 (2012) [DOI:10.1007/s10853-0012-6570-4](http://dx.doi.org/10.1007/s10853-0012-6570-4)
