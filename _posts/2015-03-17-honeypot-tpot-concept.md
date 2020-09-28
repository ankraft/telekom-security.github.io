---
title: 'Introduction into T-Pot: A Multi-Honeypot Platform'
description: 'Introduction into T-Pot: A Multi-Honeypot Platform'
header: 'Introduction into T-Pot: A Multi-Honeypot Platform'
---

We created a honeypot platform, which is based on the well-established honeypots [glastopf](http://glastopf.org/), [kippo](https://github.com/desaster/kippo), [honeytrap](http://honeytrap.carnivore.it/) and [dionaea](http://dionaea.carnivore.it/), the network IDS/IPS [suricata](http://suricata-ids.org/), [elasticsearch-logstash-kibana](http://www.elasticsearch.org/overview/), [ewsposter](https://github.com/dtag-dev-sec/ews) and some
[docker](https://www.docker.com/) magic. We want to make this technology available to everyone who is interested and release it as a Community Edition. We want to encourage you to participate.

<!--more-->

# TL;DR
1. Meet the [system requirements](#requirements). Use at least 2GB RAM and 40GB disk space as well as a working internet connection.
2. Download the [tpotce.iso](http://community-honeypot.de/tpotce.iso) or [create it yourself](#createiso). 
2. Install the system in a [VM](#vm) or on [physical hardware](#hw) with [internet access](#placement). 
3. Sit tight - [watch](http://sicherheitstacho.eu/?peers=communityPeers) and [analyze](#kibana).

We have created a nice [installation video](https://youtu.be/dWbJS_9sFNE) for you in case you run into problems. Please
be advised, that one CPU is fully sufficient to run T-Pot.

*Update:* In case you already have an Ubuntu 14.04.x running in your datacenter and are unable to install from an ISO image, we have created a [script](http://dtag-dev-sec.github.io/mediator/feature/2015/05/11/t-pot-autoinstall.html) that converts your Ubuntu base install into a full-fledged T-Pot within just a couple of minutes.

*Please note*: We will ensure the compatibility to the Intel NUC platform, as we really like this handy format.

# Documentation
- [Background](#background)
- [T-Pot](#t-pot)
- [Technical Concept](#concept)
- [System Requirements](#requirements)
- [Installation](#installation)
  - [Prebuilt ISO Image](#prebuilt)
  - [Create your own ISO Image](#createiso)
  - [Running in a VM](#vm)
  - [Running on Hardware](#hardware)
  - [First Run](#firstrun)
  - [System Placement](#placement)
- [Options](#options)
  - [Enabling SSH](#ssh)
  - [Kibana Dashboard](#kibana)
  - [Maintenance](#maintenance)
  - [Community Data Submission](#submission)
- [Roadmap](#roadmap)
- [Considerations](#considerations)
- [Q&A](#qa)
- [Contact](#contact)
- [Licenses](#licenses)


<a name="background"></a>
# Background
In the last couple of years, we at Deutsche Telekom's honeypot project have setup several honeypots in our home networks, Telekom's access networks and at partner locations all around the globe. The data gathered by those honeypots is a core component for our Early Warning System and feeds the data for the [Sicherheitstacho / "Securitydashboard"](http://sicherheitstacho.eu). 

Our experience in setting up honeypot systems at several locations showed that many people were interested in running some kind of honeypot sensor, but were a bit overwhelmed by the setup procedure and maintenance. In the past, we have gathered some experience with configuration management and finally decided to create a honeypot system that is easy to deploy, low maintenance and combines some of the best honeypot technologies in one system.

Lucky for us, a new technology called [docker](https://docker.com) emerged and we thought we'd give it a try...

<a name="t-pot"></a>
# T-Pot
Fast forward a couple of months: We finally created a multi-honeypot platform, that we want to make available as a *public beta* in order to foster a community and make this technology available to all people interested.
Aside from this, we also want to motivate people to contribute to security research and maybe take a first step towards cooperation and data exchange.

Our solution is no silver bullet to security, no intrusion prevention system and it's no fancy, cutting edge APT detection tool... <br><br>
But let's focus on what it is: Some of the best honeypot technologies available, easy to deploy and simple use. <br>

T-Pot is based on well-established honeypot daemons, IDS and tools for attack submission. <br>
The idea behind T-Pot is to create a system, whose entire TCP network range as well as some important UDP services act as honeypot, and to forward all incoming attack traffic to the best suited honeypot daemons in order to respond and process it. 

<a name="concept"></a>
# Technical Concept

T-Pot is based on a vanilla Ubuntu 14.04.02 ISO image. 
The honeypot daemons as well as other support components we used have been paravirtualized using [docker](http://docker.io).
This allowed us to run multiple honeypot daemons on the same network interface without problems make the entire system very low maintenance. <br>The encapsulation of the honeypot daemons in docker provides a good isolation of the runtime environments and easy update mechanisms.

In T-Pot we combine existing honeypots ([glastopf](http://glastopf.org/), [kippo](https://github.com/desaster/kippo), [honeytrap](http://honeytrap.carnivore.it/) and [dionaea](http://dionaea.carnivore.it/)) with the network IDS/IPS [suricata](http://suricata-ids.org/), the data monitoring and visualization triple [elasticsearch-logstash-kibana](http://www.elasticsearch.org/overview/), and our own data submission [ewsposter](https://github.com/dtag-dev-sec/ews) which now also supports hpfeeds honeypot data sharing.

<a href="/assets/images/tpot.png" target="_blank">![Architecture](/assets/images/tpot.png)</a>

All data in docker is volatile. Once a docker container crashes, all data produced within its environment is gone and a fresh instance is restarted. Hence, for some data that needs to be persistent, like config files etc., we have a persistent data storage (mounted to /data/ on the host) in order to make it available and persistent across container or system restarts.

Basically, what happens when the system is booted up is the following:

- start host system
- start all docker containers (honeypots, IDS, elk, ewsposter)
- ewsposter periodically checks the honeypot containers for new attacks and submits data to our community backend

Within the T-Pot project, we provide all the tools and documentation necessary to build your own honeypot system and contribute to our [community data view](http://sicherheitstacho.eu/?peers=communityPeers), a separate channel on our  [Sicherheitstacho](http://sicherheitstacho.eu) that is powered by this community data. 


The source code and configuration files are stored in individual github repositories, which are linked below. The docker images are tailored to be run in this environment. If you want to run the docker images separately, make sure you study the upstart scripts, as they provide an insight in how we configured them. 

The individual docker configurations etc. we used can be found here:

- [ewsposter](https://github.com/dtag-dev-sec/ews)
- [elasticsearch / logstash / kibana](https://github.com/dtag-dev-sec/elk)
- [suricata](https://github.com/dtag-dev-sec/suricata)
- [honeytrap](https://github.com/dtag-dev-sec/honeytrap)
- [kippo](https://github.com/dtag-dev-sec/kippo)
- [glastopf](https://github.com/dtag-dev-sec/glastopf)
- [dionaea](https://github.com/dtag-dev-sec/dionaea)

<a name="requirements"></a>
# System Requirements
Whether you install it on [real hardware](#hardware) or in a [virtual machine](#vm), make sure your designated T-Pot system meets the following minimum system requirements: 

- 2 GB RAM (4 GB recommended)
- 40 GB disk (64 GB SSD recommended)
- Working internet connection / network via DHCP


<a name="installation"></a>
# Installation
The installation of T-Pot is simple. Please be advised that you should have an internet connection up and running
as even the installation iso file will need the basic docker containers to be pulled from docker hub.

First, decide if you want to download our prebuilt installation ISO image [tpotce.iso](http://community-honeypot.de/tpotce.iso) ***or*** [create it yourself](#createiso). 

Second, decide where you want to let the system run: [real hardware](#hardware) or in a [virtual machine](#vm)?

<a name="prebuilt"></a>
## Prebuilt ISO Image
We provide an installation ISO image for download (~610MB), which is created using the same [tool](https://github.com/dtag-dev-sec/tpotce) you can use yourself in order to create your own image. It will basically just save you some time downloading components and creating the ISO image. 
You can download the prebuilt installation image [here](http://community-honeypot.de/tpotce.iso) and jump to the [installation](#vm) section. The ISO image is hosted by our friends from [Strato](http://www.strato.de) / [Cronon](http://www.cronon.de).

    shasum tpotce.iso
    3b8f15eba2a478b106b202726661ce75c8fe7acc tpotce.iso

<a name="createiso"></a>
## Create your own ISO Image
For transparency reasons and to give you the ability to customize your install, we provide you the [tool](https://github.com/dtag-dev-sec/tpotce) that enables you to create your own ISO installation image. 

**Requirements to create the ISO image:**

- Ubuntu 14.04.2 or 14.10 as host system (others *may* work, but remain untested)
- 2GB of free memory  
- 4GB of free storage 
- A working internet connection

**How to create the ISO image:**

1. Clone the repository and enter it. 
    
        git clone https://github.com/dtag-dev-sec/tpotce.git
        cd tpotce

2. Invoke the script that builds the ISO image. 
The script will download and install dependencies necessary to build the image on the invoking machine. It will further download the Ubuntu base image (~625MB) which T-Pot is based on. 

        sudo ./makeiso.sh
After successful build, you will find the ISO image `tpotce.iso` in your directory. 

<a name="vm"></a>
## Running in VM
You may want to run T-Pot in a virtualized environment. The virtual system configuration depends on your virtualization provider.

We successfully tested T-Pot with [VirtualBox](https://www.virtualbox.org) and [VMWare](http://www.vmware.com) with just little modifications to the default machine configurations. 

It is important to make sure you meet the [system requirements](#requirements) and assign a virtual harddisk >=40GB, >=2GB RAM and bridged networking to T-Pot. 

You need to enable promiscuous mode for the network interface for suricata to work properly. Make sure you enable it during configuration.

If you want to use a wifi card as primary NIC for T-Pot (which we do not recommend, but in some cases, it might be necessary), mind that not all network interface drivers support all wireless cards. E.g. in VirtualBox, you then have to choose the *"MT SERVER"* model of the NIC.

Last, mount the `tpotce.iso` ISO to the VM and and continue with the installation. You can now jump [here](#firstrun).




<a name="hardware"></a>
## Running on Hardware
If you decided to run T-Pot on dedicated hardware, just follow these steps:

1. Burn a CD from the ISO image or make a bootable USB stick using the image. <br>Whereas most CD burning tools allow you to burn from ISO images, the procedure to create a bootable USB stick from an ISO image depends on your system. There are various Windows GUI tools available, e.g. [this tip](http://www.ubuntu.com/download/desktop/create-a-usb-stick-on-windows) might help you.<br> On [Linux](http://askubuntu.com/questions/59551/how-to-burn-a-iso-to-a-usb-device) or [MacOS](http://www.ubuntu.com/download/desktop/create-a-usb-stick-on-mac-osx) you can use the tool *dd* or google for GUI tools. 
2. Boot from the USB stick and install. 

<a name="firstrun"></a>
## First Run
The installation requires very little interaction, only some locales and keyboard settings have to be answered. Most other things should be configured automatically. The system will reboot a couple of times. Make sure it can access the internet as it needs to download the dockerized honeypot components. Depending on your network connection, the installation may take some time. During our tests, the installation was finished within 30 minutes. 

Once the installation is finished, the system will automatically reboot and you will be presented with the T-Pot login screen. The user credentials for the first login are:

- user: *tsec* 
- pass: *tsec*

You will need to set a new password after first login.

All honeypot services are started automatically.

<a name="placement"></a>
# System Placement
Make sure your system is reachable through the internet. Otherwise it will not capture any attacks, other than the ones from your hostile internal network! We recommend you put it in an unfiltered zone, where all TCP and UDP traffic is forwarded to T-Pot's network interface. 

If you are behind a NAT gateway (e.g. home router), here is a list of ports that should be forwarded to T-Pot.

| Resonding Honeypot &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |Transport  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |Port(s) to forward|
|---|---|---|
| dionaea | TCP  | 21, 42, 135, 443, 445, 1433, 3306, 5060, 5061, 8080  |
| dionaea | UDP  | 69, 5060 | 
| kippo | TCP | 22 | 
| honeytrap | TCP | 25, 110, 139, 3389, 4444, 4899, 5900, 21000 | 
| glastopf | TCP | 80   |

<br>

Basically, you can forward as many TCP ports as you want, as honeytrap dynamically binds any TCP port that is not covered by the other honeypot daemons. 

In case you want external SSH access, forward TCP port 64295 to T-Pot, see below.

T-Pot requires outgoing http and https connections for updates (ubuntu, docker) and attack submission (ewsposter, hpfeeds).


<a name="options"></a>
# Options
The system is designed to run without any interaction or maintenance and contribute to the community.
As we know, for some this may not be enough. So here come some ways to further inspect the system and change configuration parameters.

<a name="ssh"></a>
## Enabling SSH
By default, the ssh daemon is disabled. However, if you want to be able to login via SSH, make sure you enable it like this:

    ~/.ssh_enable.sh

This will start the SSH daemon on port **64295**. It is configured to prevent password login and use pubkey-authentication instead, so make sure you get your key on the system. Just copy it to `/home/tsec/.ssh/authorized_keys` and set the appropriate permissions (`chmod 600 authorized_keys`) as well as the right ownership (`chown tsec:tsec authorized_keys`).

In addition to this - and maybe easier than getting a pubkey on the system - you can enable two-factor authentication (2fa) and use an app like [Google Authenticator](https://support.google.com/accounts/answer/1066447?hl=en) as a second authentication factor. 

Make sure that you invoke the following script as the user *tsec*. ***Do not run it as root or via sudo***. Otherwise the setup of two factor authentication will fail.

    ~/2fa_enable.sh

Don't forget to enable ssh, if you haven't done this before. If you already have enabled ssh, restart the ssh daemon `sudo service ssh restart` to make sure that the changes are active. <br>
You can then login using the password you set for the user *tsec* and use the app as the second authentication factor. 

<a name="kibana"></a>
## Kibana Dashboard
To access the kibana dashboard, make sure you have [enabled SSH](#ssh) on T-Pot, then create a [port forward](http://explainshell.com/explain?cmd=ssh+-p+64295+-l+tsec+-N+-L8080%3A127.0.0.1%3A64296+yourHoneypotsPublicIPaddress)  and make sure you leave the terminal open.

    ssh -p 64295 -l tsec -N -L8080:127.0.0.1:64296 <yourHoneypotsPublicIPaddress>

Finally, open a web browser and access [http://127.0.0.1:8080](http://127.0.0.1:8080). The kibana dashboard can be customized to fit your needs. By default, we haven't added any filtering other than outgoing ewsposter submission, because the filters depend on your setup. E.g. you might want to filter out your incoming administrative ssh connections and connections to update servers.

<a name="maintenance"></a>
## Maintenance
As mentioned before, the system was designed to be low maintenance. Basically, there is nothing you have to do but let it run. If one of the dockerized daemon fails, it will restart. If this fails, all docker container will be restarted. 

If you run into any problems, a reboot may fix it. ;) 

If new versions of the components involved appear, we will test them and build new docker images. Those new docker images will be pushed to the system and get restarted.  

<a name="submission"></a>
## Community Data Submission
We provide T-Pot in order to make it accessible to all people who are interested in honeypot deployment. By default, the data captured is submitted to a community backend. This community backend uses the data to feed a [community data view](http://sicherheitstacho.eu/?peers=communityPeers), a separate channel on our own [Sicherheitstacho](http://sicherheitstacho.eu), which is powered by our own set of honeypots.
You may opt out the submission to our community server by disabling it in the `[EWS]`-section of the config file `/data/ews/conf/ews.cfg`.

Further we support [hpfeeds](https://github.com/rep/hpfeeds). It is disabled by default as you need to supply a  channel you want to post to and enter your user credentials. To enable hpfeeds, edit the config file `/data/ews/conf/ews.cfg`, section `[HPFEED]` and set it to true.

Data is submitted in structured ews-format, a XML stucture. Hence, you can parse out the information that is relevant to you. 

We encourage you not to disable the data submission as the main purpose of the community is sharing.

The *ews.cfg* file contains many configuration parameters required for the system to run. You can - if you want - add an email address, that is sent within your submissions, in order to be able to identify your requests later. Further you can add a proxy.
Please do not change anything other than those settings and only if you absolutely need to. Otherwise, the system may not work as expected.

<a name="roadmap"></a>
# Roadmap
We planned a couple of features we haven't been able to implement yet. Those features will most likely be added through automatic updates of the docker containers.

- Import ews json log into kibana dashboard in order to visualize captured attack data
- Filter out false positives in kibana dashboard (e.g. update connections)



<a name="considerations"></a>
# Considerations
- We don't have access to your system. So we cannot remote-assist when you break your configuration. But you can simply reinstall. 
- The software was designed with best effort security, not to be in stealth mode. Because then, we probably would not be able to provide those kind of honeypot services.
- You install and you run within your responsibility. Choose your deployment wisely as a system compromise can never be ruled out.
- Honeypots should - by design - not host any sensitive data. Make sure you don't add any.
- By default, your data is submitted to the community dashboard. You can disable this in the config. But hey, wouldn't it be better to contribute to the community? 
- By default, hpfeeds submission is disabled. You can enable it in the config section for hpfeeds. This is due to the nature of hpfeeds. We do not want to spam any channel, so you can choose where to post your data and who to share it with.  
- Malware submission is enabled by default but malware is currently not processed on the submission backend. This may be added later, but can also be disabled in the ews.cfg config file. 
- The system restarts the docker containers every night to avoid clutter and reduce disk consumption. *All data in the container is then reset.* The data displayed in kibana is kept for the last 30 days.
- This software is in *beta status*, so it may contain bugs. 


<a name="qa"></a>
# Q&A
- **Q** : ***How do I enable SSH?*** <br> **A** : Run the *ssh_enable.sh* script in tsec's home directory. More [here](#ssh).
- **Q** : ***On what port is the SSH daemon listening?*** <br>**A**: OpenSSH is listening on port 64295. More [here](#ssh).
- **Q** : ***Why didn't you add honeypot XYZ?*** <br>**A** : We added the honeypot daemons which we thought were most suitable and we have good experience with. If you think a crucial honeypot daemon is missing, let us [know](#contact). Maybe someday we will add it.
- **Q** : ***Why is my installation failing?*** <br>**A** : Some people have tried to install it in VirtualBox using the default disk size of 8GB. This is not enough storage and the installation will fail. Same applies to VMWare's Easy Install. Make sure you meet the [system requirements](#requirements). T-Pot requires at least 2GB RAM and 40GB disk space. And make sure that a working internet connection is available. 
- **Q** : ***I try to login via ssh but my ssh key gets rejected?*** <br>**A**: Are you sure you are connecting to the right port? The SSH honeypot *kippo* is listening on port 22. *OpenSSH* is listening on port 64295. More [here](#ssh).
- **Q** : ***I enabled two-factor authentication, but all I get is a password prompt and it does not accept my password. What happened?*** <br>**A** : You probably invoked the `2fa_enable.sh` script (a) as *root* or (b) via *sudo*, and you should have invoked it as the user *tsec*.<br>Fix for (a): 

        sudo mv /root/.google_authenticator /home/tsec/.google_authenticator
        sudo chown tsec:tsec /home/tsec/.google_authenticator
<br>Fix for (b): 

        sudo chown tsec:tsec /home/tsec/.google_authenticator

- **Q** : ***I cannot expose T-Pot to full internet access, but I still want to participate. Can you tell me which ports should be forwarded to T-Pot?*** <br>**A** : T-Pot works best when exposed to the unfiltered internet. If you are unable to place it in that way, [here](#placement) is a list of ports that should be forwarded to T-Pot. 





...  to  be extended depending on feedback.

<a name="contact"></a>
# Contact
We provide the software as it is as a Community Edition. T-Pot is designed to run out of the box and with no maintenance effort. <br>
We hope you understand that we are unable to provide support on an individual basis. We will try to answer common  questions and problems in the Q&A section. 

Please understand that we cannot provide support on an individual basis. We will try to address questions, bugs and problems on our [GitHub issue list](https://github.com/dtag-dev-sec/tpotce/issues).

<a name="licenses"></a>
# Licenses
The software that T-Pot is built on, uses the following licenses.
<br>GPLv2: [dionaea](http://src.carnivore.it/dionaea/tree/LICENSE), [honeytrap (by Tillmann Werner)](http://src.carnivore.it/honeytrap/tree/LICENSE), [suricata](http://suricata-ids.org/about/open-source/)
<br>GPLv3: [ewsposter (by Markus Schroer)](https://github.com/dtag-dev-sec/ews/blob/master/GPL), [glastopf (by Lukas Rist)](https://github.com/glastopf/glastopf/blob/master/GPL)
<br>Apache 2 License: [elasticsearch](https://github.com/elasticsearch/elasticsearch/blob/master/LICENSE.txt), [logstash](https://github.com/elasticsearch/logstash/blob/master/LICENSE), [kibana](https://github.com/elasticsearch/kibana/blob/master/LICENSE.md), [docker] (https://github.com/docker/docker/blob/master/LICENSE)
<br>[kippo copyright disclaimer (by Upi Tamminen)](https://github.com/desaster/kippo/blob/master/doc/COPYRIGHT) 
<br>[Ubuntu licensing](http://www.ubuntu.com/about/about-ubuntu/licensing) 
