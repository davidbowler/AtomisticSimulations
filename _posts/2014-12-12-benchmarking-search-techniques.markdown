---
layout: post
title: Benchmarking search techniques
category: Techniques
tags: searching
year: 2014
month: 12
day: 12
summary: An approach to benchmark different search techniques
published: true
---

One of the key tasks that atomistic simulations of all types perform is to characterise a potential energy surface.  This includes diverse problems, such as:

* local optimisation (finding a stable structure given a starting set of coordinates)
* global optimisation (finding the most stable structure for a given set of atoms – a very hard task !)
* transition state searches, both starting from one minimum (single-ended) and interpolating between two minima (double-ended)

These are hugely complex problems, and there are many methods which have been proposed to perform these searches. I have discussed some of these before ([here]({{ site.baseurl }}/blog/local-hyperdynamics), [here]({{ site.baseurl }}/blog/high-throughput-simulation),  and [here]({{ site.baseurl }}/blog/accelerating-md)); there is also an overview of searching methods in Chapter 5 of the book.

The paper I’m discussing in this post[[1]](#R1) proposes some benchmarks to test different methods for these optimisation problems.  They also offer a website[[2]](#R2) where the tests can be found, and new results shared.  The problems are all based around simple empirical potentials (which makes the testing relatively light).  They propose the following systems:

* a 38 atom Lennard-Jones cluster
* a 100 atom, two species Lennard-Jones cluster
* water clusters with 10, 15 and 20 atoms modelled with TIP4P
* a seven atom cluster on the (111) surface of a FCC material (Morse potential)
* a bulk sample of the same FCC material

These different problems cover a reasonable set of problems, but there is no reason why more should not be added to the set.  For instance, the present set lacks any examples of covalent bond-breaking systems.

Various methods are compared, including L-BFGS, FIRE and conjugate gradients, nudged elastic band, eigenvector following and basin hopping.  There are so many methods that it would be impossible to cover all, but I’m a little surprised that methods such as AIRSS and metadynamics weren’t tested for the global optimisation problems.

The local optimisation problem offers the simplest problem to characterise, and the conclusions are simplest here: methods that include curvature (L-BFGS and conjugate gradients) are significantly faster.  This is not particularly surprising, as these methods are known to be well optimised, and the test suite avoids any pathological problems.  The conclusions for the other challenges are less clear; the global optimisation problem in particular seems not to distinguish well between the two methods used.

The saddle point searching results surprised me.  I use the nudged-elastic band (NEB) method regularly, and one of the authors is a leading figure in the development of the method, but it did not perform particularly well.  The study did show that the performance is dependent on the initial pathway guessed, and suggested that for double-ended searches an preliminary NEB search should then be refined by the eigenvector following method.  The eigenvector following method performed best on the single-ended searches as well.

What do we learn from the paper ? While the overall conclusions confirm expectations for local optimisation and do not discriminate as much as would be hoped for the other problems, it’s an excellent start.  In particular, in seeking to offer open, standard tests for all the different methods that are available, it offers a way to move forward in this field.  It also suggests an excellent new model of characterisation-based scientific papers: a dynamic database of freely-available results.

#<a name="R1"></a>
[1] J. Chem. Theor. Comput. 10, 5476 (2014) [DOI:10.1021/ct5008718](http://dx.doi.org/10.1021/ct5008718)

#<a name="R2"></a>
[2] [http://optbench.org/](http://optbench.org/)
