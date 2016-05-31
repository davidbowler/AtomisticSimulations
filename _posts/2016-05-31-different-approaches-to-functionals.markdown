---
layout: post
year: 2016
month: 05
day: 31
category: DFT
tags: Accuracy testing
summary: An overview of two recent meta-GGA functionals
title: Different approaches to creating (meta-GGA) DFT functionals
published: true
---
Within the DFT community, John Perdew's idea of the Jacob's ladder of
accuracy[[1]](#R1) starts with LDA, moves to GGAs with the inclusion of the
gradient of the electron density, and is further extended (to rungs three
and four in the ladder analogy) with meta-GGA, where kinetic energy density
is added, and hybrids, where exact exchange plays a role.  Although meta-GGAs
have been around for ten to fifteen years, they are only starting to become
widely used.  I will compare two examples which both seem promising, but also
encapsulate the two most common approaches to functional creation.

Meta-GGAs are promising because the kinetic energy density allows some
discrimination between areas with one or two electrons (in this way, they
are similar in some ways to the electron localisation function, or ELF, which
can be used to analyse bonding[[2]](#R2)).  This gives some hope that they
may be able to fit both strong and weak bonding as well as possibly mitigating
the self-interaction error that plagues DFT.

The Minnesota meta-GGA MN15-L[[3]](#R3) takes a very complex functional form
with 58 parameters and a non-separable form for the exchange-correlation
(adding an extra correlation functional to this) and fits it to a portion of
a very large database (I estimate that there are at least 900 entries in
the database, covering many different chemical properties, specifically
including solid state properties, transition barriers and weak interactions).
The resulting functional is local (no hybrid or non-local van der Waals terms
were included) and produces extremely small errors when compared to those parts
of the database which were not used in fitting.  Notably, it out-performs
functionals with exchange and van der Waals included.

By contrast, the SCAN functional[[4]](#R4) uses only seven parameters (close
to, if not at, the minimal number for a meta-GGA) and includes various
physically motivated norms and constraints for the electron gas.  The early
progress in GGAs was made by satisfying important constraints, so this is
seen as a good route to reliability.  This functional was also tested on
various databases, particularly focussing on solid-state and weak interactions.
It has excellent agreement with these, though was published before the MN15-L
functional so is not compared directly.  In a follow-up paper[[5]](#R5) the
performance of the SCAN functional for band gaps is found to be good, though
it does not calculate Kohn-Sham gaps, but gaps within a generalized Kohn-Sham
theory (a distinction which I don't have time to discuss here; I may write
another blog on this, as it is relevant to hybrid functionals among other
things.)

What can we learn from this ? Both functionals perform well in the tests
which are published.  The functional that you choose will depend in
part, as always, on which system you wish to study; however, both of these
functionals show some promise in being widely applicable.  Your choice will
also depend on your attitude to fitting[[6]](#R6): is a reasonable functional
form with many parameters something that you trust, or do you prefer to be more
prescriptive, and deal with fewer parameters ? Fifty years since its
inception, DFT is still developing, communities are still somewhat divided in
the approaches that they take to functional development,
but there are an increasing number of ways to achieve efficiency and accuracy.

<a name="R1">[1]</a> My colleague, Mike Gillan, reckons that we should instead
talk about wrestling Jacob when considering how to improve DFT functionals.

<a name="R2">[2]</a> J. Chem. Phys. **92**, 5397 (1990) [DOI: 10.1063/1.458517](http://dx.doi.org/10.1063/1.458517)

<a name="R3">[3]</a> J. Chem. Theor. Comput. **12**, 1280 (2016) [DOI: 10.1021/acs.jctc.5b01082](http://dx.doi.org/10.1021/acs.jctc.5b01082)

<a name="R4">[4]</a> Phys. Rev. Lett. **115**, 036402 (2015) [DOI: 10.1103/PhysRevLett.115.036402](http://dx.doi.org/10.1103/PhysRevLett.115.036402)

<a name="R5">[5]</a> Phys. Rev. B **93**, 205205 (2016) [DOI: 10.1103/PhysRevB.93.205205](http://dx.doi.org/10.1103/PhysRevB.93.205205)

<a name="R6">[6]</a> The oft-quoted maxim about fitting an elephant with
four parameters has been put into practice (see the original paper
  [here](http://dx.doi.org/DOI:10.1119/1.3254017) and a nice write-up and
  a python implementation [here](http://www.johndcook.com/blog/2011/06/21/how-to-fit-an-elephant/))
