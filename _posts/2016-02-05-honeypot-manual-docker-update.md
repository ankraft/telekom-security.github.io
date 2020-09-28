---
title: 'Manual Docker Update for T-Pot (docker-engine 1.10)'
description: 'Manual Docker Update for T-Pot (docker-engine 1.10)'
header: 'Manual Docker Update for T-Pot (docker-engine 1.10)'
---

Yesterday Docker released version 1.10 (http://blog.docker.com/2016/02/docker-1-10/). While this release will
improve on security and bring lots of useful features the automatic upgrade within T-Pot (http://dtag-dev-sec.github.io/feature/2015/10/23/updated-docker.html) will hang.

<!--more-->


You have to manually upgrade Docker to the latest version in order for your T-Pot installation to work properly.



Please login to T-Pot and run the following commands:

sudo apt-get update -y

sudo apt-get upgrade -y

sudo apt-get upgrade docker-engine -y



Please answer the following question with (N)o for T-Pot to work correctly:

Configuration file '/etc/default/docker'

 ==> Modified (by you or by a script) since installation.

 ==> Package distributor has shipped an updated version.

   What would you like to do about it ?  Your options are:

    Y or I  : install the package maintainer's version

    N or O  : keep your currently-installed version

      D     : show the differences between the versions

      Z     : start a shell to examine the situation

 The default action is to keep your current version.

*** docker (Y/I/N/O/D/Z) [default=N] ? N



Installing new version of config file /etc/bash_completion.d/docker ...

docker start/running, process 92746

Processing triggers for libc-bin (2.19-0ubuntu6.6) ...

Processing triggers for ureadahead (0.100.0-16) ...



If everything worked Docker will calculate SHA256 checksums for docker related data (https://docs.docker.com/engine/migration/). Depending on your hardware this might take some time during which the containers will not be available. Please wait until all containers are up and running again before you reboot your machine.



If you are in doubt please run “sudo cat /etc/default/docker” and if the configs are identical:

'''# Docker Upstart and SysVinit configuration file



#Customize location of Docker binary (especially for development testing).

\#DOCKER="/usr/local/bin/docker"



# Use DOCKER_OPTS to modify the daemon startup options.

\#DOCKER_OPTS="--dns 8.8.8.8 --dns 8.8.4.4"



# If you need Docker to use an HTTP proxy, it can also be specified here.

\#export http_proxy="http://127.0.0.1:3128/"



# This is also a handy place to tweak where Docker's temporary files go.

\#export TMPDIR="/mnt/bigdrive/docker-tmp"

DOCKER_OPTS="-r=false"'''
