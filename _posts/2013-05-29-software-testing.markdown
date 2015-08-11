---
layout: post
year: 2013
month: 05
day: 29
title: Software testing
summary: Thoughts on how we should test large, complex scientific codes
category: techniques
tags: parameters software testing
published: true
---
In Chapter 10, Section 10.6, we discuss how you should write about a simulation, and we touched on the idea of reproducible research. A recent paper in Science[[1]](#R1) touches on a related, important problem: how can you validate scientific codes and modelling protocols (if indeed you do try) ?

We tend to expect large, complex codes such as atomistic simulation codes to work without errors, but that is probably unrealistic. Alarmingly, in the Science article, nearly 30% of the ecologists who were surveyed used a particular software because it had been “validated against other methods in peer-reviewed publications”, but only 8% claimed to have validated the software against other methods (when asking about the primary choice for using a particular package). Significant numbers (around 10%) relied on trust or recommendation as a basis for choosing a package. I suspect that a similar profile would be found in the atomistic simulation community (though cost or availability of a particular package might also play a significant role). The scientific method might be taken to require a more cautious approach.

Should this be a cause for concern ? And how much testing should we do ? With any atomistic simulation, regardless of the code you have chosen, there are many things that need testing and validating. Naturally this will depend on the technique that you are using. There must be sanity checks on the parameters and models you are using (for example with DFT, testing the effect of increasing the basis set on simple material properties, as well as testing that the pseudopotentials you are using are accurate). There should also be tests against material parameters. These checks are an essential part of a study based on atomistic simulations, and are often not done or not done carefully (they should certainly form the basis of first year PhD studies).

But beyond this there is an important role for checking between codes in a given area. You should always compare results of your simulations to existing results (and, if possible, to results from experiment and more accurate techniques). If there are discrepancies, it is important to understand where these come from: are they due to differences in models, in parameters chosen, or could there be an error or a bug in the code ? Moreover, where is the error: in our results, or in the previously published ones ? We should not flinch from questioning other studies, as well as our own: this is an essential part of the scientific process.

Any atomistic simulation described in a paper should give the following details (whether in methods or in supplementary information): the package used, including version and (ideally) platform and compiler; the model chosen (including enough details to allow reproduction of the research, where possible); the tests made on the system, and the reasons for parameters chosen; ideally, any benchmark results used for comparisons and sanity checks. Very few papers include these (including my own). Naturally, if the paper is building on previous work, then repetition of testing is not always needed, but it does pay to go back and check that assumptions (or compromises) made a few years ago do not overly affect results.

I also think that it would be very interesting to find test cases for specific atomistic simulation codes to run and compare against. Having a suite that can be checked would allow confidence in the community, and provide important tests for new codes and approaches. However, this would require storage and more importantly time to set up, run and maintain these results, along with the inputs used.

[There is a further interesting paper on this question[[2]](#R2), though it makes suggestions more about scientific computing than running simulations.]

<a name="R1">[1]</a> Science **340**, 814 (2013) [DOI:10.1126/science.1231535](http://dx.doi.org/10.1126/science.1231535)

<a name="R2">[2]</a> arXiV [1210.0530](http://arxiv.org/abs/1210.0530)
