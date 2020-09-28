---
title: 'Opensourcing our Honeypot Backend (well, parts of it)'
description: 'Opensourcing our Honeypot Backend (well, parts of it)'
header: 'Opensourcing our Honeypot Backend (well, parts of it)'
---

The last years we have consistently supported the community by releasing new versions of our multi honeypot platform called T-Pot. This November we are
proud to release a new version of T-Pot with exciting new features and ...

something more.

For the last years our backend was driven by a very robust Grails code written by our former consultant / developer and friend Lutz Wischmann. The backend itself was running on a bare metal setup in the computing centre of Cronon (thanks for all your support over the years!).

In middle of 2017 we decided to rewrite the backend, as we needed new features and wanted to be more flexible in handling the data. Initially we had a closer look at Kotlin, the new language from Jetbrains, which is a really cool new language. However, as many of the people within our internal honeypot community are python lovers, we went for Python3 with the Flask framework. As the basis for our backend we now use several instances within our own [Open Telecom Cloud](https://cloud.telekom.de/en/infrastructure/open-telekom-cloud/) (all debian 9 based, fully automated installation via ansible, ...). 

In this new backend, we collect honeypot events from both our own closed DTAG honeypot network as well as community data contribution from T-Pots running all over the world. 

The data is visualized on our new [sicherheitstacho.eu](http://community.sicherheitstacho.eu) website.

The backend is designed and implemented in the way that it recieves honeypot events from T-Pot via ewsposter. The events are processed and stored in the backend and are available to be queried via an API which delivers live statistics.

The development is very agile, new features and refinements will be added over time. 

Although the backend is tailored to fullfill our own needs, we think it might be a good starting point for setting up your own centralized honeypot collection plattform.  

You can find the repository [here](https://github.com/dtag-dev-sec/PEBA).

Support will be handled same as with T-Pot via Github issues. Please bear in mind that the software is developed in the spare time next to our main job. If you have any contributions you think could be beneficial for the project, feel free to submit pull requests.

In the future we plan also to publish additional codes from the backend such as admin backend, so stay tuned for updates.

#### Thanks ####

We would like the following people and organizations

* [The Honeynet Project] (https://www.honeynet.org/)
* Lutz Wischmann ([42, The Software architects.](http://www.software-architects.de/))
* Our friends at G-Data
* Robin Verton for a source code review and other valuable input
* Simon Peters for setting up the monitoring
