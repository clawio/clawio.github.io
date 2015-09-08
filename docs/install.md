---
layout: simple
title: Install
---

Learn how to install ClawIO.

# Contents

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}

# CentOS / RHEL 6
Here is how to install the package on a CentOS / RHEL 6 64 bits server:

`sudo rpm --import https://rpm.packager.io/key`

`echo "[clawiod]
name=Repository for clawio/clawiod application.
baseurl=https://rpm.packager.io/gh/clawio/clawiod/centos6/master
enabled=1" | sudo tee /etc/yum.repos.d/clawiod.repo`

`sudo yum install clawiod`

# CentOS / RHEL 7
Here is how to install the package on a CentOS / RHEL 6 64 bits server:

`sudo rpm --import https://rpm.packager.io/key`

`echo "[clawiod]
name=Repository for clawio/clawiod application.
baseurl=https://rpm.packager.io/gh/clawio/clawiod/centos7/master
enabled=1" | sudo tee /etc/yum.repos.d/clawiod.repo`

`sudo yum install clawiod`

# Debian 7
Here is how to install the package on a Debian 7 Wheezy 64 bits server:

`wget -qO - https://deb.packager.io/key | sudo apt-key add -`

`echo "deb https://deb.packager.io/gh/clawio/clawiod wheezy master" | sudo tee /etc/apt/sources.list.d/clawiod.list`

`sudo apt-get update`

`sudo apt-get install clawiod`

# Debian 8
Here is how to install the package on a Debian 7 Wheezy 64 bits server:

`wget -qO - https://deb.packager.io/key | sudo apt-key add -`

`echo "deb https://deb.packager.io/gh/clawio/clawiod jessie master" | sudo tee /etc/apt/sources.list.d/clawiod.list`

`sudo apt-get update`

`sudo apt-get install clawiod`

# Fedora 20
Here is how to install the package on a Fedora 20 64 bits server:

`sudo rpm --import https://rpm.packager.io/key`

`echo "[clawiod]
name=Repository for clawio/clawiod application.
baseurl=https://rpm.packager.io/gh/clawio/clawiod/fedora20/master
enabled=1" | sudo tee /etc/yum.repos.d/clawiod.repo`

`sudo yum install clawiod`

# SLES 12
Here is how to install the package on a SUSE Linux Enterprise Server 12 64 bits server:

`sudo rpm --import https://rpm.packager.io/key`

`sudo zypper addrepo "https://rpm.packager.io/gh/clawio/clawiod/sles12/master" "clawiod"`

`sudo zypper install clawiod`

# Ubuntu 12.04
Here is how to install the package on a Ubuntu 12.04 Precise 64 bits server:

`wget -qO - https://deb.packager.io/key | sudo apt-key add -`

`echo "deb https://deb.packager.io/gh/clawio/clawiod precise master" | sudo tee /etc/apt/sources.list.d/clawiod.list`

`sudo apt-get update`

`sudo apt-get install clawiod`

# Ubuntu 14.04
Here is how to install the package on a Ubuntu 14.04 Trusty 64 bits server:

`wget -qO - https://deb.packager.io/key | sudo apt-key add -`

`echo "deb https://deb.packager.io/gh/clawio/clawiod trusty master" | sudo tee /etc/apt/sources.list.d/clawiod.list`

`sudo apt-get update`

`sudo apt-get install clawiod`

