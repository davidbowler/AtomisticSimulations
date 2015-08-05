---
layout: post
year: 2013
month: 10
day: 28
title: Accelerating MD
category: techniques
tags: MD parameters
published: true
---
One of the major problems facing the scientist performing atomistic simulations is that of timescales (we discuss this problem in Sections 4.2 and 6.2 of the book).  A small timestep, of the order of a few femtoseconds at most, is needed to integrate the equations of motion accurately.  There is also a need for long timescale simulations: the standard in biochemistry has shifted to nanoseconds, for instance.  These simulations are needed for a variety of reasons: to ensure good sampling; to access infrequent processes; to model long timescale processes (which are not necessarily the same as infrequent processes); and to achieve realistic rates of motion or energy transfer (as discussed in [this previous post]({{ site.baseurl }}/blog/speeds-relative-to-experiment)).

The accurate calculation of free energies requires good sampling, and can have important consequences.  In the field of materials science, an intriguing example is given by an MD study of plastic deformation in copper[[1]](#R1).  Voids are normally the cause of failure in ductile materials, and clearly understanding their growth is an important problem.  The authors use acceleration techniques to study the kinetics of void growth for unusually slow rates: down to \\(10^6\\)/s (corresponding to 10s of microseconds).  They find that there is a stabilisation of the voids, which increases strongly with temperature.  They use a linear scaling approach to calculating the vibrational density of states to analyse the cause of this stabilisation, and find that the growth of a void would decrease the vibrational entropy in the system, leading to a free energy barrier, and stabilisation.  As well as  the relatively low strain rates that they can access, they underline the importance of free energies for volume changes.

There are many approaches available to investigating long timescales[[2]](#R2).  The hyperdynamics approach[[3]](#R3) adds a boosting potential to enable a system to climb out of deep energy valleys; it has become widely used, but suffered from a tendency to slow down with large systems.  This had been addressed with a recent paper[[4]](#R4): I think that the combination of linear scaling DFT with local hyperdynamics could be very exciting.  Metadynamics[[5]](#R5) allows reconstruction of free energy surfaces, but requires collective variables to be defined for the system in question; it has become widely used rather rapidly.  Among other methods, the parallel replica approach[[6]](#R6) harnesses the ability to run independent trajectories in parallel (an approach which is very efficient, requiring little communication, and which should scale well).  Parallel tempering[[7]](#R7) , also known as replica exchange, uses multiple copies of the system at different temperatures to explore configuration space.  These methods are effective, and widely used, but cannot disguise the difficulty of addressing long timescales, which is often only solved through brute force.

[Note added: I meant to include a reference to accelerated MD (aMD)[[8]](#R8) which has been shown to reach the equivalent of milliseconds sampling using GPUs[[9]](#R9).]

<a name="R1">[1]</a> Phys. Rev. Lett. **110**, 206001 (2013) http://dx.doi.org/10.1103/PhysRevLett.110.206001

<a name="R2">[2]</a> Annu. Rep. Comput. Chem. **5**, 79 (2009) http://dx.doi.org/10.1016/S1574-1400(09)00504-0

<a name="R3">[3]</a> Phys. Rev. Lett. **78**, 3908 (1997) http://dx.doi.org/10.1103/PhysRevLett.78.3908

<a name="R4">[4]</a> J. Chem. Phys. **139**, 144110  (2013) http://dx.doi.org/10.1063/1.4824389

<a name="R5">[5]</a> Rep. Prog. Phys. **71**, 126601 (2008) http://dx.doi.org/10.1088/0034-4885/71/12/126601

<a name="R6">[6]</a> Phys. Rev. B **57**, R13985 (1998) http://dx.doi.org/10.1103/PhysRevB.57.R13985

<a name="R7">[7]</a> Phys. Chem. Chem. Phys. **7**, 3910 (2005) http://dx.doi.org/10.1039/B509983H

<a name="R8">[8]</a> J. Chem. Phys. **120**, 11919 (2004) http://dx.doi.org/10.1063/1.1755656

<a name="R9">[9]</a> J. Chem. Theory Comput. **8**, 2997 (2012) http://dx.doi.org/10.1021/ct300284c

N.B. The references given above to methods are only representative; there are many papers on this subject, and a full literature search is an important pre-requisite to performing these simulations.
