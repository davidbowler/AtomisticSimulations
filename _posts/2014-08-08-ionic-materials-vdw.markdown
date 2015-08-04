---
layout: post
title: Ionic materials and van der Waals
category: DFT
tags: DFT vdW
year: 2014
month: 08
day: 08
summary: How well DFT with vdW methods model ionic materials
published: true
---
One of the areas which has grown explosively in DFT in recent years is modelling van der Waals interactions (I’ve written about this [before](/blog/dispersion-in-dft)).  The semi-empirical approach originated by Tkatchenko and Scheffler[[1]](#R1) (normally known as TS) uses the calculated DFT charge density, which is divided between the atoms in the system to give an effective volume occupied by each atom; the ratio of this volume to a free atom volume is then used to rescale the C6 coefficients and polarisabilities found for free atoms.  This brings us to an ever-present problem with ab initio methods: how to divide a continuous charge density or wavefunction between the atoms in the system.

TS use Hirshfeld partitioning[[2]](#R2), which creates a distance-dependent weight for each atom according to the ratio between the free atom charge density for the atom and the sum of free atom charge densities for the whole system.  This can be used to give a volume and hence the relevant vdW quantities (see Eq. 7-9 in [[1]](#R1) for more detail).  But this is not the only way to divide space (and hence charge density) between atoms: Voronoi polyhedra, Bader’s atoms-in-molecules[[3]](#R3), and Becke’s integral partitioning[[4]](#R4) are only some of the methods in common use, along with Mulliken charges as a way of assigning charge to atoms.  We discuss these methods in Section 17.2 of [the book](http://tiny.cc/atomistic), and show an example of how different methods and basis sets can change the charge by more than half an electron.

The papers I want to discuss in this blog[[5,6]](#R5) address another problem with the Hirshfeld approach: as it uses free, neutral atoms as the references to divide the charge density, it performs poorly for ionic materials.  Instead, they use an update of Hirshfeld partitioning which iterates the charge density decomposition, and interpolates between reference densities of the free atoms in different charge states.  It is a relatively small change to make to the process, but has a very strong effect on ionic materials, improving agreement with experiment and high-level theory markedly[[5]](#R5).

Whenever a change is made to an approach, it is important to characterise the effect on the existing performance; in an extensive follow-up paper, the authors do this[[6]](#R6), looking at an large collection of test systems.  The main criticism that can be made of the new, iterative approach is that it worsens the modelling of molecular interactions (particularly those which have an obvious van der Waals component).  However, the improvement of other systems, particularly ionic solids and molecules interacting with charges on surfaces, is sufficiently strong that this would be well worth using in most circumstances.  The authors make the parameters they have used clear (k point meshes, plane wave cutoffs), and the approach is available in VASP (though I would have preferred to have seen it made freely available !).

#<a name="R1"></a>
[1] Phys. Rev. Lett. **102**, 073005 (2009) DOI:[10.1103/PhysRevLett.102.073005](http://dx.doi.org/10.1103/PhysRevLett.102.073005)

#<a name="R2"></a>
[2] Theor. Chim. Acta **44**, 129 (1977) DOI:[10.1007/BF00549096](http://dx.doi.org/10.1007/BF00549096)

#<a name="R3"></a>
[3] [R. F. W. Bader](http://www.chemistry.mcmaster.ca/bader/aim/), [Atoms in Molecules: A Quantum Theory](http://ukcatalogue.oup.com/product/9780198558651.do), Oxford University Press (1990).

#<a name="R4"></a>
[4] J. Chem. Phys., **88**, 2547 (1988) DOI:[10.1063/1.454033](http://dx.doi.org/10.1063/1.454033)

#<a name="R5"></a>
[5] J. Chem. Theory Comput. **9**, 4293 (2013) DOI:[10.1021/ct400694h](http://dx.doi.org/10.1021/ct400694h)

#<a name="R6"></a>
[6] J. Chem. Phys. **141**, 034114 (2014) DOI:[10.1063/1.4890003](http://dx.doi.org/10.1063/1.4890003)
