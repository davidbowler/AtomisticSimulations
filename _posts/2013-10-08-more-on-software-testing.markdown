---
layout: post
title: More on software testing
year: 2013
month: 10
day: 08
published: true
summary: More thoughts and links on testing of scientific software
category: Techniques
tags: software testing
---
As a follow-up to a [previous post]({{ site.baseurl }}/blog/software-testing) which cited a [Perspective](http://www.sciencemag.org/content/340/6134/814) in Science, I thought that I would mention some recent developments in Software Testing. Science published a [letter](http://www.sciencemag.org/content/341/6143/236.2.short) which suggested that pre-publication peer review of code would put too much strain on the review system. I am sure that anyone who acts as a referee for journals would agree that it can consume a significant amount of time; working through someone else’s code would certainly add to the load. Moreover, I doubt that it would be practical to try and read the entire source code for large atomistic codes such as VASP, CASTEP, abinit, PWSCF, NAMD, AMBER or CHARMM. The authors of the perspective [responded](http://www.sciencemag.org/content/341/6143/237.1.full), acknowledging that it would involve more work, and suggesting that availability of the code to the reviewer, and external code review could be considered. (See also [this other article from Science](http://www.sciencemag.org/content/336/6078/159.full) on why computer codes should not be treated as black boxes.)

The Mozilla Foundation, responsible for the Firefox browser among other things, have recently announced that they will investigate the feasibility of debugging scientific code (see [this article in Nature](http://www.nature.com/news/mozilla-plan-seeks-to-debug-scientific-code-1.13812) and the Mozilla [ScienceLab](https://wiki.mozilla.org/ScienceLab)). There have been different reactions to this idea, with the most negative being that scientists will be discouraged from posting or submitting their code if they think that it will be scrutinised.

I think that code review is an excellent idea, and that scientific coders need to become more literate in computer science. The [Software Carpentry](http://software-carpentry.org/) organization is an excellent example of how to go about this, as is the [Software Sustainability Institute](http://www.software.ac.uk/). There is a growing trend in the UK to acknowledge the importance of support for scientific computing, and as computational scientists, we need to support this. If our codes are not open source, we should certainly publish details of the tests we’ve done to ensure their reliability, as well as details of the development approach and version control used. This will move us towards confidence in the results and the science generated with the codes.

Update There’s a nice piece giving a basic introduction to unit testing [here](http://drclimate.wordpress.com/2013/10/10/testing-your-code/).  Code testing is becoming something of a hot topic, as it should be.
