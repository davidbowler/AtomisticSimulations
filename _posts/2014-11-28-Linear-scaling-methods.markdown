---
layout: post
title: Linear scaling methods
category: Techniques
tags: DFT LinearScaling MD
year: 2014
month: 11
day: 28
summary: An overview of the state of linear scaling DFT methods
published: true
---

Linear scaling electronic structure techniques offer the prospect of going to very large system sizes (in comparison to standard DFT methods, which scale with the cube of the number of atoms for large systems, and other methods which scale worse).  Large systems are important for a number of reasons: reducing the approximations made with finite supercells, going to very dilute limits, and modelling nanostructures and biomolecules accurately, for instance.  There was an explosion of interest in these methods around 15-20 years ago, but the practical reality of robust, accurate implementation has meant that they have not made the impact anticipated at the time.  In this blog, I want to give some idea of the progress that we have made recently (particularly in implementing molecular dynamics[[1]](#R1)); if you are interested in more detail, I have written an extensive review of these methods[[2]](#R2) with my colleague, Tsuyoshi Miyazaki.

Linear scaling relies on the existence of a gap[[3]](#R3), and on truncating the electronic structure, usually expressed as a density matrix.  There are many different approaches to linear scaling, but three basic approaches  are widely used: divide and conquer; auxiliary density matrix; and trace correction.  Divide and conquer is a very simple idea: split the system into relatively small pieces, solve each of these exactly, and combine the resulting patchwork of electronic structure.  However, the convergence with the size of the pieces can be slow, and forces are hard to calculate.  The auxiliary density matrix approach (based on an early paper[[4]](#R4)) uses a transform proposed by McWeeny in the 1960s to force the density matrix towards idempotency (occupation numbers of either zero or one) while minimising the energy with respect to an auxiliary density matrix.  This variational method is implemented in several linear scaling codes ([CONQUEST](http://www.order-n.org/), [ONETEP](http://www.onetep.org/), and the linear scaling part of Gaussian); it usually relies on distance-based criteria to truncate the density matrix, and is an effective approach.  The trace correcting family of methods are iterative, and generally rely on a tolerance-based truncation; they are very simple to implement, and have been successfully used in various quantum chemistry codes.

Given these methods, we might ask why there are not more applications.  Part of the problem lies in the efficiency of standard DFT methods, which allow us to model systems of a few hundred atoms routinely (and we have become very good at putting problems into cells of this size – I’ve written about this [before](/blog/how-large)).  Linear scaling methods are not as robust, efficient and user-friendly as the optimised packages that are now available.  But it’s also hard to create and analyze cells larger than hundreds of atoms, and this presents a barrier.  Basis sets are not, generally, as intuitive as plane waves (or the well-documented Gaussian basis sets).  And until recently molecular dynamics hasn’t been available (for comments on exactly what constitutes MD in O(N) methods, see [here](/blog/comments-and-peer-review) and [[5]](#R5)).

We have recently[[1]](#R1) published an implementation of the extended Lagrangian MD method (I’ve written about this approach [here](/blog/Energy-conserving-ab-initio-md)), which shows that accurate, O(N) MD is possible over picoseconds, for 32,000+ atoms.  The system that we approached in the paper wasn’t very interesting (bulk silicon – the computational physicist’s system of choice) but we have also applied the method to DNA in water (and our work on hybrid functionals shows that we can do linear scaling hybrid DFT on systems with thousands or tens of thousands of atoms – though not MD yet).

So what do we need to take O(N) methods into the mainstream ? Reliable, simple basis sets with the ease of plane waves will help, as will efficient, robust implementation of structural relaxation and molecular dynamics.  Going to very large systems (millions of atoms are possible[[6]](#R6)) requires large, parallel machines, but thousands of atoms should be possible on standard workstations.  Really, we need a change of culture: a desire to make simulation cells as large as possible, to converge simulations with system size and to tackle larger problems.

#<a name="R1"></a>
[1] J. Chem. Theor. Comput. **10**, 5419 (2014) [DOI:10.1021/ct500847y](http://dx.doi.org/10.1021/ct500847y) (or arXiV [1409.6085](http://arxiv.org/abs/1409.6085))

#<a name="R2"></a>
[2] Rep. Prog. Phys. **75**, 036503 (2012) [DOI:10.1088/0034-4885/75/3/036503](http://dx.doi.org/10.1088/0034-4885/75/3/036503)

#<a name="R3"></a>
[3] With the exception of the orbital-free DFT method, which is applicable to metals, but which is not yet as accurate as standard DFT implementation.

#<a name="R4"></a>
[4] Phys. Rev. B **47**, 10891 (1993) [DOI:10.1103/PhysRevB.47.10891](http://dx.doi.org/10.1103/PhysRevB.47.10891)

#<a name="R5"></a>
[5] arXiV [1402.6828](http://arxiv.org/abs/1402.6828)

#<a name="R6"></a>
[6] J. Phys.:Condens. Matter. **22**, 074207 (2010) [DOI:10.1088/0953-8984/22/7/074207](http://dx.doi.org/10.1088/0953-8984/22/7/074207)
