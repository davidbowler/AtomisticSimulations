---
layout: post
title: Approaches to non-adiabatic calculations
year: 2014
month: 06
day: 14
category: techniques
tags: ExcitedStates
published: true
---
The problem of modelling electronic transitions caused by interactions with ions is formidably difficult.  Standard approaches to electronic structure assume that the electrons move on a single energy surface defined by static ions (often called the Born-Oppenheimer approximation, and discussed in Chapter 7 of [the book](http://tiny.cc/atomistic)). The most common implementations calculate the electronic structure given the ionic positions and then calculate forces on the ions from the electronic structure (it is possible to do this for excited states of the system as well as for the ground state, by promoting electrons into elevated levels, in a method known as delta-SCF[[1]](#R1)).  When performing molecular dynamics (MD) this leads to the Born-Oppenheimer MD (BOMD) method, where the electronic ground state is always maintained, though at the cost of a ground state search at every step.

The Car-Parrinello MD approach starts from a relaxed electronic ground state, and evolves the electronic states in time at the same time as the ionic states, while assigning a *fictitious* mass to the electrons.  The approach keeps the electrons close to the ground state surface while making the calculations more efficient, though it does require smaller time steps than standard MD methods.  It is in common use in the CPMD code, but is much less common than BOMD.

The Ehrenfest method goes beyond either of these other approaches and evolves the electrons according to the time-dependent  Schrödinger equation.  The method is appealing when dealing with situations where electrons are strongly affected by ions, particularly in combination with accurate methods such as TDDFT.  However, the energy surface which is being followed is now a dynamic surface which has evolved with the ions; one approach to analysing these calculations is to perform a static, ground state calculation for the system at one instant of time, and to project the dynamic state onto the ground state.

If you want to model the transfer of energy between the electrons and ions, this becomes significantly harder.  There is no provision for the change of the occupancies of different electronic states in any of the theories mentioned above, so that the transfer of energy when an electron de-excites is rarely correctly handled.  This is important: when an electron decays non-radiatively (i.e. without emitting a photon, but by putting its energy into the ionic motion) the resulting forces and velocities on the ions will determine the subsequent evolution of the system.

The most commonly used approach is called surface hopping: the system is allowed to evolve according to normal MD, but at each MD step there is a finite probability for an electron to hop between electronic surfaces (hence the name).  However, the energy transferred out of the electronic system is placed randomly into the ionic motion (except in sophisticated implementations).  Moreover, it is a stochastic method, and requires multiple trajectories, that is multiple MD simulations, to correctly sample configuration space.

What I find most interesting about all these approaches is that it is not possible to model resistive heating on an atomic level.  We showed[[2]](#R2) that the correlations between ions and electrons are needed to do this, in much the same way that it would not be possible to model Brownian motion of large particles by using a fluid with a density rather than a fluid with individual particles.  We wrote a review on solid state approaches to energy transfer between electrons and ions[[3]](#R3), and there are good overviews on surface hopping and related techniques from the chemistry literature (e.g. [[4]](#R4) and [[5]](#R5)).

I will write about one or two new ideas in this field in the next week or two.

<a name="R1"></a>
[1] U. Terranova and D. R. Bowler, J. Chem. Theory Comput. **9**, 3181 (2013) [DOI:10.1021/ct400356k](http://dx.doi.org/10.1021/ct400356k)

<a name="R2"></a>
[2] A. P. Horsfield, D. R. Bowler, A. J. Fisher, T. N. Todorov and M. J. Montgomery, J. Phys. Condens. Matter  **16**  3609  (2004) [DOI:10.1088/0953-8984/16/21/010](http://dx.doi.org/10.1088/0953-8984/16/21/010)

<a name="R3"></a>
[3] A. P. Horsfield, D. R. Bowler, H. Ness, C. G. Sanchez, T. N. Todorov and A. J. Fisher, Rep. Prog. Phys.  **69**  1195 (2006) [DOI:10.1088/0034-4885/69/1195](http://stacks.iop.org/0034-4885/69/1195)

<a name="R4"></a>
[4] O. V. Prezhdo, W. R. Duncan and V. V. Prezhdo, Prog. Surf. Sci. **84**, 30 (2009) [DOI:10.1016/j.progsurf.2008.10.005](http://dx.doi.org/10.1016/j.progsurf.2008.10.005)

<a name="R5"></a>
[5] T. van Voorhis, T. Kowalczyk, B. Kaduk, L.-P. Wang, C.-L. Cheng and Q. Wu, Annu. Rev. Phys. Chem. **16**, 149 (2010) [DOI:10.1146/annurev.physchem.012809.103324](http://dx.doi.org/10.1146/annurev.physchem.012809.103324)
