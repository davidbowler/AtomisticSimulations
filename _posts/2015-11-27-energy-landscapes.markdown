---
layout: post
year: 2015
month: 11
day: 27
category: tutorials
tags: searching Techniques
summary: A discussion of how to find energetic minima and transition states
title: Exploring energy landscapes
published: true
---
Once you have mastered the basics of performing DFT calculations (or other
atomistic methods for finding the energy of a structure), you need to move on
to understanding the system that you are investigating.  Largely speaking, this
will involve exploring its energy landscape: the local minima define its
thermodynamic properties, while the barriers between the minima define the
kinetic behaviour.  (The topics in this blog are covered in parts of Chapters 5,
12, 15 and 18 in the book.)

This distinction is very important, and it's also important to realise that
many experiments are not actually operating in the thermodynamic minimum.  My
favourite example taken from my PhD research relates to the growth of germanium
on silicon: when depositing it, a fascinating series of reconstructions is
seen, which arise from the strain mismatch between the two materials.  But a
thin layer of Ge on Si is not the lowest energy structure, as we found when we
left a sample annealing over a few days: the lowest energy structure is a
dilute alloy of Ge dispersed through Si, which gives a rather featureless
surface and little of interest to the surface scientist.

In general, the problem of exploring an energy landscape is extremely difficult,
as shown by the wealth of approaches that have been developed (I covered some
of these in a blog on [high throughput methods]({{site.baseurl}}/blog/high-throughput-simulation/)).
We cannot simply perform a simple molecular dynamics simulation to explore the
landscape, because of the timescales involved: MD covers at most microseconds.
We can simply prepare simulations in structures close to obvious, intuitive
minima (often guided by experiment), though this risks missing structures.
Various algorithms for searching for stable structures exist: random structure
searching and genetic algorithms both seek to find only the minima, and are
often effective (though are limited in system size because of the complexity
of the problem).  I've written about how we
[benchmark search techniques]({{site.baseurl}}/blog/benchmarking-search-techniques/)
 before.

There are approaches that [accelerate MD]({{site.baseurl}}/blog/accelerating-md/)
including the [hyperdynamics]({{site.baseurl}}/blog/local-hyperdynamics/) though
these are not guaranteed to explore the entire energy surface.  Methods that
explore both transition states as well as minima (such as metadynamics and the
dimer method) are growing in both popularity and predictive power, but can be
rather expensive computationally.

If you have two minima, there are well-established ways to find the transition
state between the two.  The simplest approach is simply to decide on some
variable that will define a path, and constrain that variable at various points
between the start and the end.  This is best suited to simple problems such
as the diffusion of one atom, and risks missing important behaviour if the
energy landscape is at all complicated.

The nudged elastic band method is one of the most widely used approaches to
transition state searching.  The idea is elegant: we construct a series of
replicas of the system, interpolated between the start and end points, and
join the images of each atom with springs (or elastic bands).  We then minimise
the energy of the entire, composite system; the springs stop the atoms that
are moving from falling back to the start of the end, and we map out the
transition state, at least approximately.  

Of course it is not that simple: there is a tendency for images to slide down
energy hillsides, so nudging is introduced: the force perpendicular to the
local tangent to the pathway is projected out.  We can also invert the
component of the force along the pathway for the image with the highest energy,
which will force it to *climb up* to the top of the barrier.

The question of how many images to use is very difficult: the computational
effort increases with images, but so does the accuracy of the description of
the surface.  I was involved in some work[[1]](#R1) which showed that one image
can give a good estimate of the barrier height, but that accuracy only improves
for a significant number of images (between 5 and 8, depending on the system).

There is no simple prescription for this problem: it is one of the most complex
problems within physics, chemistry and materials science.  The areas where it
is applied vary between the structure of nanoparticle, surface reconstructions
and protein folding.  You must be careful to be honest about how you have
explored the system, and where you might have made errors or missed structures.
This is an area where fruitful collaboration with experiment can be a great
help, giving data to test against possible structures.

<a name="R1">[1]</a> J. Phys.:Condens. Matter **22**, 074203 (2010) [DOI:10.1088/0953-8984/22/7/074203](http://dx.doi.org/10.1088/0953-8984/22/7/074203)
