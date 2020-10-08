---
title: Introduction to Deutsche Telekom's Honeypot Project
description: Introduction to Deutsche Telekom's Honeypot Project
header: Introduction to Deutsche Telekom's Honeypot Project
tags: ['Honeypots']
---

Deutsche Telekom's honeypot project was started in 2010 by a small group of enthusiasts. We initially started with Lukas Rist's great [Glastopf web honeypot](http://glastopf.org/) and soon added further honeypot daemons like [kippo](https://github.com/desaster/kippo), [honeytrap](http://honeytrap.carnivore.it/) and [dionaea](http://dionaea.carnivore.it/). Over the next years we deployed more and more honeypots.

<!--more-->

Data captured on our honeypot sensors is visualized on our [Sicherheitstacho](http://www.sicherheitstacho.eu) and we further use the information
e.g. to inform our customers regarding infections.

We currently run about 180 honeypot sensors as part of Deutsche Telekom's early warning system. Individual maintenance of such large number of systems became a time consuming and painful task. As a first step we created a standardized Raspberry PI honeypot image, which is managed by Puppet and distributed across our national subsidiaries. To increase the efficiency even more, we decided to build T-Pot, a highly modular approach.


# Contact

Please understand that we cannot provide support on an individual basis. We will try to address questions, bugs and problems on our [GitHub issue list](https://github.com/dtag-dev-sec/tpotce/issues).

# Thanks

We want to thank the following people / organizations / projects for their great work.<br><br>


- **Lutz Wischmann** (our consultant / friend / main developer of Sicherheitstacho / Honeypot backend systems)
- **Honeypot developers** for their patient support for our individual feature requests
- **[The Honeynet Project](http://www.honeynet.org)** for providing great tools and a platform for discussion
- **Christian Müller / Mark Bolten from CRONON / Strato** for (as always) great support
- **Andreas Kraft & Alexander Mannecke** for their cool dsl router protection demo based on honeypot data and the mobile sicherheitstacho app 

# Deutsche Telekom's Honeypot Team

The team consists of several people, who spend some work time and private time on this project.

- Markus Schroer
- Rainer Schmidt
- Markus Schmall
- André Vorbach
- Marco Ochse
- Norbert Krummen
- another unnamed individual ;-)


### Initial Jekyll template written by Dirk Fabisch

A medium inspired Jekyll blog theme. The basic idea came from the Ghost theme 
[Readium 2.0](http://www.svenread.com/readium-ghost-theme/). I use mediator on my own blog [The Base](blog.base68.com).

You can **download** the theme here:
[https://github.com/dirkfabisch/mediator](https://github.com/dirkfabisch/mediator) 
