---
title: Full Overview about all of Deutsche Telekom's Honeypot Projects
description: Full Overview about all of Deutsche Telekom's Honeypot Projects
header: Full Overview about all of Deutsche Telekom's Honeypot Projects
tags: ['honeypot project']
---

Over the time, we developed a number of projects, which we mostly published on [Github](https://github.com/dtag-dev-sec/).

Parts of them were developed in "one day a month" projects, others in spare time of dedicated persons.
Our "one day a month" approach from the central security organization basically means, that the
security employees can work one day a month on a technical topic, aside from day-to-day business. The topic must be somehow related to their work, but does not necessarily have to be security-focussed.

<!--more-->

# Public Projects

## T-Pot CE

T-Pot CE (Community Edition) is a honeypot framework, which is based on existing honeypots ([glastopf](http://glastopf.org/), [kippo](https://github.com/desaster/kippo), [honeytrap](http://honeytrap.carnivore.it/) and [dionaea](http://dionaea.carnivore.it/)), the network IDS/IPS [suricata](http://suricata-ids.org/), [elk](http://www.elasticsearch.org/overview/), [ewsposter](https://github.com/dtag-dev-sec/ews) and some
[docker](https://www.docker.com/) magic. A detailed description can be found in [this article](http://dtag-dev-sec.github.io/mediator/feature/2015/03/11/concept.html).

## Nodepot

[Nodepot](https://github.com/schmalle/Nodepot) is our [glastopf](http://glastopf.org/) copycat, a web application honeypot based on NodeJS. Code is available [here](https://github.com/schmalle/Nodepot).

## NodeJS HP Feed Module

As an option for Nodepot you can use [hpfeeds](https://github.com/rep/hpfeeds) to share data.
We wrote a small NPM module to support hpfeeds that can be found [here](https://github.com/schmalle/nodejs-hpfeeds).

## EWSposter

[EWSposter](https://github.com/dtag-dev-sec/ews) is a python application that collects information from multiple honeypot sensors and posts it to central collection services like the DTAG early warning system or hpfeeds.


## IP-Blacklist

Our Early Warning System provides an API which can be queried for IP addresses that have been associated with malicious activities in our sensors.
The Java example code for the IP blacklist is available [here](https://github.com/dev-t-sec/BadIPFetch).

If you want to get an account for retrieving data, please contact us at cert @ telekom.de.

# Notable Internal Projects
Some of our projects have been research driven proof-of-concepts that never reached a status worth publishing, others were fulfilling purely internal purposes. However, to give you an idea of what we do for the Early Warning System of Deutsche Telekom, we wanted to mention the following projects.

## sicherheitstacho.eu - securitydashboard.eu
The ["Sicherheitstacho" / "Securitydashboard"](http://sicherheitstacho.eu) is our eye-candy vizualization frontend that displays live cyberattacks in a manager-friendly way. It provides live statistics from the sensor network we setup in the last years.  <br>
... but of course, we also have a more technical system suitable for some analysis. ;-)

## *honeypi* - Raspberry PI Honeypots 
We are all fans of cool technologies, and we love playing with new toys. When the Raspberry PI got released, we built a custom ISO image with preinstalled [dionaea](http://dionaea.carnivore.it/), [honeytrap](http://honeytrap.carnivore.it/) and [kippo](https://github.com/desaster/kippo). We added central management to the system using [puppet](https://puppetlabs.com/), and <a href="/assets/images/rpi.jpg" target="_blank">rolled out</a> approx. 80 to the international subsidaries of Deutsche Telekom. It was our first try to create low-efffort, easy-to-deploy honeypots to broaden our sensor base.

We further added a <a href="/assets/images/honeypi-display1.jpg" target="blank">display</a> that provides some statistics and later a <a href="/assets/images/honeypi-display2.jpg" target="blank">touchscreen</a> .

As the image is configured to be used internally, we have not planned to make it publicly available. 


## Smartphone Honeypots
In early 2011 we setup a mobile honeypot project wich simulates the network footprint of smartphones (iOS and Android). [kippo](https://github.com/desaster/kippo) was modified so that the filesystem and commands matched those of an Android device as well as a jailbroken iPhone. Other honeypot daemons like [dionaea](http://dionaea.carnivore.it/) and  [honeytrap](http://honeytrap.carnivore.it/) were added. After attaching them to the 3G network, they were able to capture some device-specific attacks, e.g. some attack directly aiming for the addessbook and photos of the jailbroken iPhone.<br>
Some results are documented [here](http://arxiv.org/pdf/1301.7257v1.pdf). Further, if you understand German, you can find some more information including a short video [here](http://www.heise.de/security/meldung/Smartphone-Honeypots-im-Mobilfunknetz-der-Telekom-1630359.html).


##  *honeydroid* - Android Honeypot
As a proof-of-concept, we modified <a href="/assets/images/honeydroid.jpg" target="blank">Android phones</a> so that their cellular network stack acts as a honeypot and displays some attack statistics. And yep, *<a href="/assets/images/app.jpg" target="blank">there's an app for that</a>* (kindof). German speaking people can find some more background information [here](http://www.heise.de/security/meldung/Honeydroid-Android-Handy-wird-zur-Hackerfalle-1980058.html). 

## RDP Honeypot
We are currently developing a RDP honeypot that captures RDP scans and login attempts. 

## CVEmapper
Another project we have been working on is the CVEmapper. The CVEmapper takes information sumitted by our EWSposter as input and tries to determine if a known vulnerability (CVE) is currently being exploited. 
