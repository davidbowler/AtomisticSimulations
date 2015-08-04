---
layout: post
title: Local hyperdynamics
category: Techniques
tags: MD Size Thermostat LinearScaling
year: 2014
month: 07
day: 25
summary: An approach to speeding up sampling of rare events
published: true
---
One of the key desires in atomistic simulations of all kinds, whether chemistry, biology, physics, materials science or earth sciences, is to be able to model accurately the long-time evolution of large systems.  There have been good advances in recent years in linear scaling approaches to DFT and quantum chemistry[[1]](#R1), and significant progress in time acceleration (e.g. parallel replica dynamics[[2]](#R2) and hyperdynamics[[3]](#R3)) as well as excellent advances in methods to allow sampling of conformational space (e.g. metadynamics[[4]](#R4) or parallel tempering[[5]](#R5)).

The paper I will discuss here[[6]](#R6) points to a way to combine these two efforts, so that significant time acceleration can be applied to large systems.  In combination with linear scaling electronic structure methods, this will allow us to achieve linear scaling in both size and time.  The method, local hyperdynamics (LHD), is a development of the original hyperdynamics (HD) method, which is designed to increase the rate at which a system escapes from energy valleys.  The idea behind HD is fairly simple, though elegant: a boost potential is added to the energy surface of the system being modelled so as to raise the bottom of the valleys in the system.  This boost potential must go to zero if the system approaches a transition state; this requirement ensures that the rates of escape are not changed relative to each other, but leads to poor scaling with system size.  As a larger system is modelled, it is more likely that a transition state will be found in some part of the system, and the boost becomes zero more and more of the time.  The boosted time or hypertime can be evaluated during the simulation and related to the true system dynamics.

LHD defines local regions (centred normally on bonds) within which a local boost is applied.  This allows the dynamics of the entire system to be boosted, and as the regions are independent, when one region goes through a transition, the other regions will not be affected.  As the boost is only local, the dynamics are no longer conservative, though the authors make good arguments to suggest that, on average, using a Langevin thermostat, dynamics which are very close to conservative are followed.

The regions need to be large enough to encompass the characteristic length-scale of interactions and transitions being modelled, otherwise there will may be incorrect boosts from neighbouring domains.  Once the system size is larger than the region size, the authors demonstrate that the boost factor achieved is constant with system size.

As each region has its own boost factor, some care has to be applied to ensure that boosting is uniform throughout the system.  Given a target boost factor, the local boosting can be monitored and adjusted to ensure that it matches the target on average.  The authors suggest that this time-evolving correction to the boost should be called a boostostat.  There are careful, detailed statistical arguments which are worth careful reading in the paper.

The paper is also extremely careful in exploring the effects of the assumptions imposed.  There will be force mismatches at the region edges, along with the lack of conservative dynamics.  However, plausible arguments are made that these should average out for relatively uniform systems.  A challenging system such as water, which contains both weak and strong bonds, might well show more significant deviation from these assumptions.

The authors tested the method with an embedded atom method (EAM) for Ag(100) with adatoms, vacancies and steps, and show excellent agreement with normal MD.  For rare events, they show that boost of 10^6 are possible and give good agreement with transition state theory, though normally they work on boost factors around 100.

The [Conquest](http://www.order-n.org/) developers (of which I am one) have recently performed full DFT molecular dynamics on 32,768 atoms for 2ps, and static calculations on over 1,000,000 atoms; in combination with LHD, we can see that DFT MD should be possible on 100,000+ atoms for nanoseconds and beyond.

N.B. I am grateful to [Art Voter](http://cmime.lanl.gov/bios/art_voter.shtml) for pointing out some inaccuracies in the article above, which have now been fixed.

#<a name="R1"></a>
[1] Rep. Prog. Phys. **75** 036503 (2012) DOI:[10.1088/0034-4885/75/3/036503](http://dx.doi.org/10.1088/0034-4885/75/3/036503)

#<a name="R2"></a>
[2] Phys. Rev. B **57**, R13985 (1998) DOI:[10.1103/PhysRevB.57.R13985](http://dx.doi.org/10.1103/PhysRevB.57.R13985)

#<a name="R3"></a>
[3] Phys. Rev. Lett. **78**, 3908(1997) DOI:[10.1103/PhysRevLett.78.3908](http://dx.doi.org/10.1103/PhysRevLett.78.3908)

#<a name="R4"></a>
[4] Rep. Prog. Phys. **71**, 126601 (2008) DOI:[10.1088/0034-4885/71/12/126601](http://dx.doi.org/10.1088/0034-4885/71/12/126601)

#<a name="R5"></a>
[5] Phys. Chem. Chem. Phys. **7**, 3910 (2005) DOI:[10.1039/B509983H](http://dx.doi.org/10.1039/B509983H)

#<a name="R6"></a>
[6] J. Chem. Phys., **139**, 144110 (2013) DOI:[10.1063/1.4824389](http://dx.doi.org/10.1063/1.4824389)
