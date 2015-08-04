---
layout: post
title: "Band offsets in semiconductors: GW and hybrid functionals"
category: Techniques
tags: DFT testing parameters GW hybrids
year: 2014
month: 06
day: 27
summary: How can we characterise the accuracy of solid-state DFT codes ?
published: true
---

Fixing the well-known problems with DFT band gaps can be done, but there is no universal solution.  Hybrid functionals have long been used in the chemistry community (where B3LYP is very popular) while solid state approaches tend to use the PBE0 functional (which mixes 25% exact exchange into the PBE functional in its original form).  There are also many-body perturbation theory approaches such as GW, which tend to be complex but are generally very good at getting band structures correct.

The paper I’m writing about[[1]](#R1) considers how to calculate offsets between both the valence and conduction bands in different bulk semiconductors.  These have obvious applications in areas such as photovoltaics and electronics, and a consistent way to calculate them would be very valuable.  This isn’t the only paper which has been written about this problem; however, this paper is really very careful, and goes to some lengths to be open.  They give details of all the relevant parameters used in the calculations, even giving details of the pseudopotentials used.  This is unusual, and highly laudable: it makes the science they do open and reproducible.

They consider non-polar interfaces, and a range of group IV, III-V and II-VI semiconductors.  What do they find ? They use the common one-shot G0W0 method (which calculates corrections to DFT bands without any form of self-consistency) and find excellent agreement with experiment.  For PBE0, the hybrid functional, they find that the original mixing parameter of 0.25 gives poor results; they optimise it by fitting either to experimental gaps or to GW gaps.  When modelling the interfaces, they find that the experimental gap fitting process (taking an average of the values for the two semiconductors) is much better.  Interestingly, they find that the valence band offsets are modelled extremely well by PBE alone, though they have applied a similar method to semiconductor/insulator interfaces where PBE fared rather poorly[[2]](#R2).

Overall, the conclusions add to our understanding of hybrids and GW.  A single hybrid mixing parameter for all systems performs fairly poorly; however, fitting the parameter to (say) bulk properties can be successful.  GW is generally reliable, though it cannot calculate electron-hole effects, and can depend on input bands (in particular, small gap systems in DFT can form a poor starting point for GW calculations – see, for instance [[3]](#R3)).  But I really like their approach, constructing pseudopotentials very carefully and publishing their protocols.


#<a name="R1"></a>
[1] Phys. Rev. B **89**, 205309 (2014) doi:[10.1103/PhysRevB.89.205309](http://dx.doi.org/10.1103/PhysRevB.89.205309)

#<a name="R2"></a>
[2] Phys. Rev. Lett. **101**, 106802 doi:[10.1103/PhysRevLett.101.106802](http://dx.doi.org/10.1103/PhysRevLett.101.106802)

#<a name="R3"></a>
[3] New J. Phys. **7**, 126 (2005) doi:[10.1088/1367-2630/7/1/126](http://dx.doi.org/10.1088/1367-2630/7/1/126)
