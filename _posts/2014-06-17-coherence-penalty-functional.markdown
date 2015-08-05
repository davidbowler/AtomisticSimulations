---
layout: post
title: Coherence penalty functional
category: Techniques
tags: ExcitedStates non-adiabatic
year: 2014
month: 06
day: 17
summary: How can we characterise the accuracy of solid-state DFT codes ?
published: true
---

As I discussed in [my last blog post]({{ site.baseurl }}/blog/non-adiabatic), probably the most common approach to non-adiabatic methods is called surface hopping.  This stochastic method has to explore configuration space, and relies on multiple trajectories.  There must be multiple electronic trajectories to sample the effects of the randomly determined jumps between electronic energy surfaces.  However, to sample the phase space properly, there should also be multiple MD trajectories, and potentially different starting energy surfaces.  This makes the cost rather large, and often it is the cost that dictates the choice of number of trajectories (though cost is often the determining factor in any ab initio simulations).

Ehrenfest dynamics, the approach where electronic degrees of freedom are propagated according to the time-dependent Schrodinger equation while nuclear degrees of freedom obey classical forces calculated from the mean-field potential energy surface, is simpler but does not allow for decoherence between energy surfaces.  The simplest way to understand this is to think about what electronic states would be found for each set of nuclear positions in a standard approach: these states define the different adiabatic surfaces.  Ehrenfest dynamics allows the electronic states to evolve in time, so they diverge from the adiabatic surfaces.  Physically, there are mechanisms that tend to collapse electrons onto specific surfaces: this is decoherence, and is missing in Ehrenfest dynamics.  Ehrenfest dynamics is simple: only a single trajectory needs to be followed; and the nuclear motion is determined by a mean-field potential energy.  However, this simplicity is offset by the lack of decoherence between states.

A recent paper[[1]](#R1) presents an alternative approach, designed to introduce decoherence into Ehrenfest dynamics.  But to understand the papers discussed in this post, we must first define some notation. We index the surfaces found above with labels i, j etc.  Then if we represent the electronic structure in terms of these surfaces, the weight for each surface is \\(c_i, c_j\\) etc.  (Note that these weights apply to the entire electronic system – the different many-body states.) Coherence in the simulation is often characterised in terms of the density matrix elements \\(c^*_i c_j\\) – a measure of how the contributions of different surfaces interfere.  Ehrenfest is well-known to preserve coherence more than it should.

The authors rewrite the time-dependent Schrodinger equation (following an approach known as MMST in the chemistry literature) as classical Hamilton dynamics (the real part of the c_i coefficients become the positions and the imaginary parts the momenta).  Having done this, they add a penalty functional to the Hamiltonian to increase the energy of system when there is coherence.  The form of the functional is not specified by they theory, and they choose a simple form: the sum over states of  the square of the matrix element defined above, each scaled by a multiplier.  These multipliers are related to the decoherence rates – which have to be calculated.  This is not a trivial problem, but the authors suggest a series of possible approximate routes to find the rates.

The tests that the authors apply, ranging from simple tests suggested by Tully in the context of the surface hopping method to large scale quantum dots on TiO2, show significant improvement over Ehrenfest, and, for the large systems, sufficient accuracy to suggest that it produces reliable results.  In particular the times for electron transfers match well with experimental results.  This offers a new route to non-adiabatic calculations for large systems, which will be important for sufficiently accurate functionals.

In some ways the most interesting idea in the paper is the analogy between their penalty functional and DFT.  The authors note that Ehrenfest dynamics is a mean-field theory, which has problems correctly describing correlations between the nuclear motion and electronic states (this is the key source of excessive coherence in Ehrenfest).  Just as DFT lacks a correct description of electron correlations, Ehrenfest lacks a correct description of electron-nuclear correlations.  They suggest that their penalty functional might offer a similarly fruitful approach to Ehrenfest dynamics as DFT has been to electronic structure.  I think that the possibility to improve Ehrenfest dynamics in this way is very exciting.

This is not the first approach to augmenting Ehrenfest with decoherence rates.  The Schwartz group produced a method known as mean-field dynamics with stochastic decoherence[[2]](#R2).  They derive decoherence rates by attaching frozen Gaussians to nuclei, and at each Ehrenfest step calculate the probability for decoherence to a single energy surface.  The method requires one external parameter, the non-adiabatic interaction width which in turn determines the nuclear size. Subotnik presented a similar method to define decoherence rates[[3]](#R3), based on a partial Wigner transform.  The method is not simple, but shows that decoherence rates based on nuclear-electronic correlations are an excellent route to improving Ehrenfest dynamics.  He also shows that the approach gives a method to monitor the accuracy of an Ehrenfest calculation.

Non-adiabatic calculations are technically demanding, and there are many different methods available, normally from the quantum chemistry literature.  The recent movement to augmenting Ehrenfest or mean-field calculations is encouraging, and offers the hope of large-scale calculations on these important processes, in the same way that DFT has allowed large-scale calculations on electronic structure.

#<a name="R1"></a>
[1] J. Chem. Phys. **140**, 194107 (2014) DOI:[10.1063/1.4875702](http://dx.doi.org/10.1063/1.4875702)

#<a name="R2"></a>
[2] J. Chem. Phys. **123**, 234106 (2005) DOI:[10.1063/1.2131056](http://dx.doi.org/10.1063/1.2131056)

#<a name="R3"></a>
[3] J. Chem. Phys. **132**, 134112 (2010) DOI:[10.1063/1.3314248](http://dx.doi.org/10.1063/1.3314248)
