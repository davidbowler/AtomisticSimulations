---
layout: post
year: 2015
month: 10
day: 05
category: DFT
tags: DFT Accuracy hybrids
summary: A brief update
title: The accuracy of semi-local functionals
published: true
---
The great strength of density functional theory (DFT) in its purest form is that
the energy only depends on the charge density, whether its magnitude alone
(LDA), or with its gradient (GGA) or beyond. This simplicity is, of course, also
the source of some of its largest errors: poor band gap predictions, excessive
delocalisation of charge density, poor description of charge transfer, and
problems with band widths.  Many of these issues can be solved by the addition
of a judicious amount of exact exchange, giving hybrid functionals.  But
exchange is very expensive to calculate, and requires the use of orbitals in the
place of a charge density.  And there is the associated question of what
fraction of exchange to mix (there are many hybrid functionals available, each
with a different fraction of exchange) and even whether the exchange
interactions should be screened (and if so, by how much...). An interesting
recent review and extended opinion piece from one of the key practitioners in
the field[[1]](#R1) gives more details on some of these problems, and is well
worth a read.

There have been recent attempts to construct semi-local (GGA) functionals which
have at least some of the features of exact exchange, in the hope of improving
the accuracy.  The first of these[[2]](#R2) constructed the potential as a
functional of the charge density which matched the OEP (optimised effective
potential - an approach to solving the exact exchange problem) very well for
atoms.  It has been modified[[3]](#R3) to create a potential that can be used in
solids, and which gives good band gaps (known as the mBJ or TB-mBJ potential).
However, this is only a *potential*, and because of its form, it is not possible
to create a corresponding energy functional.  This limits the applicability to
post-hoc corrections of the band structure.  (The potential can also diverge
where there are large areas of vacuum, such as surfaces.)

A similar attempt has been made to create consistent potential and energy
functionals which also reproduce key features of the exact exchange[[4,5]](#R4).
One of these, the derivative discontinuity (DD), describes a change in the
gradient of the energy with respect to electron number at integer fillings; this
behaviour is missing from most semi-local functionals.  The AK13
functional[[4]](#R4) restores this by considering the asymptotic behaviour of
the functional, giving reasonable accuracy for band gaps, without the need for
fitting (the mBJ functional has parameters that are fit to improve the
accuracy).

A very recent paper, which presents a comparison of results and potentials from
these functionals[[6]](#R6), shows, as ever, that there is no single functional
that gives good results in all situations (though in principle the exact density
functional should do this).  As DFT practitioners we must choose whether we want
to use highly fitted functionals which give excellent accuracy within certain
limits, or to use functionals which respect certain important limits and
behaviours for the electron gas.  There is no universal function, just as there
is no universal basis set, and part of the practice of DFT simulations is to
quantify carefully the limitations and effects of the approaches we choose.


[<a name="R1">[1]</a> Rev. Mod. Phys. **87**, 897 (2015) [DOI:10.1103/RevModPhys.87.897](http://dx.doi.org/10.1103/RevModPhys.87.897)

[<a name="R2">[2]</a> J. Chem. Phys. **124**, 221101 (2006) [DOI:10.1063/1.2213970􏰩](http://dx.doi.org/10.1063/1.2213970􏰩)

[<a name="R3">[3]</a> Phys. Rev. Lett. **102**, 226401 (2009) [DOI:10.1103/PhysRevLett.102.226401](http://dx.doi.org/10.1103/PhysRevLett.102.226401)

[<a name="R4">[4]</a> Phys. Rev. Lett. **111**, 036402 (2013) [DOI:10.1103/PhysRevLett.111.036502](http://dx.doi.org/10.1103/PhysRevLett.111.036502)

[<a name="R5">[5]</a> Phys. Rev. B **91**, 035107 (2015) [DOI:10.1103/PhysRevB.91.035107](http://dx.doi.org/10.1103/PhysRevB.91.035107)

[<a name="R6">[6]</a> Phys. Rev. B **91**, 165121 (2015) [DOI:10.1103/PhysRevB.91.165121](http://dx.doi.org/10.1103/PhysRevB.91.165121)
