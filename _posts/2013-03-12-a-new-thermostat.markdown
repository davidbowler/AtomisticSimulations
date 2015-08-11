---
layout: post
year: 2013
month: 03
day: 12
title: A New Thermostat
category: techniques
tags: MD thermostat
published: true
---
You might ask why I chose to start with a technical paper[[1]](#R1) which
discusses ensembles, thermostats, distributions and which is, frankly, a little
nerdy. Why not start with something more spectacular, say from Science or Nature ?
Ensembles, thermostats, timesteps and related technical details matter: they are
key to performing reliable, predictive atomistic simulations, and making cotact
with experiment. The paper I will discuss is closely concerned with these
things, and so I start with a brief introduction which discusses basic concepts
(including different thermostat approaches). Those thoroughly familiar with
these matters can skip this introductory material.

### Introduction

Use of the correct ensemble and thermostat allows comparison with experiment,
which should be the bedrock of numerical simulations. Understanding how the
algorithm you have chosen works, what its limitations are, and what experiments
it can be compared to is a foundation of doing good computational science. An
experimental scientist must understand the limitations of their equipment,
sources of error and what can actually be measured. A computational scientist
must understand the assumptions behind their modelling. It is very common to
have a form of folklore about algorithms: the received wisdom within some parts
of the molecular dynamics community is that the Berendsen thermostat is good for
equilibration while Nose-Hoover is better for production. This is reasonable,
but it is rarely justified, and the reasons for the wisdom are often hard to
unpick.

We need to be clear about terminology. When performing molecular dynamics
simulations we are generally sampling a phase space which is defined by the
positions and velocities (or momenta) of the particles. Our algorithm should, in
the long time limit, sample all points in the space with equal probability (this
is known as *ergodicity*). The distribution of states of the system we generate
form an *ensemble* (defined by what macroscopic variables we hold fixed: the
canonical ensemble fixes particle number, N, volume, V, and temperature T, while
the microcanonical fixes energy, E, instead of T). We must use an *integrator* and
choose an integration *time step* when we propagate the equations of motion. When
working in anything other than the micro-canonical ensemble, we need an
algorithm to maintain temperature (and possibly pressure), which normally
involves at least one parameter that fixes the response time of the system.
Before we can start to sample any data, we must *equilibrate* the system (ensure
that all particles in the system are in equilibrium, with no memory of the
starting conditions).

Temperature is maintained with a thermostat, which either puts the system in
thermal contact with a reservoir, or introduces stochastic (or random) forces
drawn from an appropriate distribution. Thermal contact requires a response
time, but provides a quantity which should be conserved and can be monitored to
ensure accuracy. Stochastic forces do not have a conserved quantity, and do not
allow calculation of dynamical quantities, but form the basis of a number of
common approached including Langevin dynamics.

The most commonly implemented thermostats are Nose-Hoover, Andersen and
Berendsen (all discussed in the book). Andersen involves stochastic forces,
Berendsen a velocity rescaling approach (which does not lead to any ensemble)
and Nose-Hoover thermal coupling. Berendsen generally leads to fast
equilibration as the rescaling of velocities distributes energy through the
system, but also has no conserved quantity which can be monitored. The
Nose-Hoover thermostat is probably most commonly used, having a conserved
quantity and being associated with the canonical ensemble. However, it is
non-ergodic in certain limits (notably for the harmonic oscillator), which
requires implementation of the more complex Nose-Hoover chains (a chain of
thermostats).

### The Algorithm

Bussi, Donadio and Parrinello (BDP from now on) propose a new algorithm to
sample the canonical distribution, using a form of velocity rescaling. In its
simplest form, velocity rescaling involves multiplying the velocities of all
particles in the system by the same factor when the temperature moves too far
from a target temperature. This is a very simple approach, but leads to
discontinuities in the velocities, and does not obey any ensemble. BDP propose
that, rather than rescaling to a fixed target kinetic energy, they will use a
kinetic energy drawn from a canonical distribution, allowing the system to
evolve normally between rescaling steps. Neither of these procedures will
disturb the canonical distribution, so that the overall algorithm should sample
from the canonical distribution. In practice, they alternate steps which evolve
the particles and steps which update the kinetic energy, rescaling the
velocities after the kinetic energy step. Stochastic dynamics are used to evolve
the kinetic energy.

The kinetic energy is updated with a first-order differential equation, which
involves a parameter to set the time scale of the response of the thermostat.
This equation has clear, well-defined limits: it reduces to the Berendsen
thermostat if the stochastic term is set to zero, to pure stochastic velocity
rescaling in the limit of zero time scale, and Hamiltonian dynamics
(microcanonical ensemble) in the limit of infinite time scale. The algorithm
should give rapid equilibration and correct canonical sampling at equilibrium.
The authors also demonstrate how the formalism parallels the Nose-Hoover scheme.

Most interestingly, BDP introduce a conserved quantity, which allows the
fidelity of the simulation to be monitored. They point out that, during a
simulation, we are generating a sequence of points through phase space, and they
associate with each point a weight which measures the probability that the point
is drawn from the correct, target ensemble. They show that an effective energy
can be written in terms of the weight at each point, and derive an analytic
formula for this energy, in terms of an integral along the overall trajectory.
The conservation of this effective energy can be used to monitor the effect of
integration timesteps on the simulation.

### Applications

The algorithm is tested for two systems: a Lennard-Jones potential for argon,
and water using the TIP4P potential. They check the sampling of the canonical
ensemble by monitoring fluctuations, and compare to the Nose-Hoover thermostat
for both solid and liquid Lennard-Jones systems. The two approaches compare well
for various values of the time scale, though the non-ergodicity of the
Nose-Hoover thermostat appears for large values of this parameter. For this
simple system, they find that relatively large timesteps (up to 5fs) lead to
stable dynamics.

For water, a more challenging system, the timestep has to be reduced to 1fs. The
BDP approach is more efficient than Nose-Hoover for small and large values of
the time scale of the thermostat. For moderate values, the two approaches agree
well. They also show that dynamic properties of the system can be calculated by
finding the vibrational spectrum of ice 1h at 120K (using a smaller timestep of
0.5fs). Two very different values of the time scale give essentially identical
spectra, which also agree with the micro-canonical results. The diffusion
coefficient of water is also well calculated, only showing significant errors
for very small values of the time scale.

Overall, this new thermostat appears to be both efficient and robust. It obeys
ergodicity, samples the canonical ensemble and could be extended to allow
constant pressure simulations. The authors have suggested a way to monitor the
quality of numerical integration which involves the accuracy of sampling rather
than the accuracy of trajectories. This makes the monitor applicable to all
stochastic methods, which will benefit from a simple way to check errors
introduced by numerical integration.

[1] J. Chem. Phys. **126**, 014101 (2007) [DOI: 10.1063/1.2408420](http://dx.doi.org/10.1063/1.2408420)
