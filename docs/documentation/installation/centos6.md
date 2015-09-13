---
layout: page-installation
title: CentOS / RHEL 6
---

Here is how to install the package on a CentOS / RHEL 6 64 bits server:

# Install

<div class="marked"><pre><code><span class="hljs-built_in">sudo</span> rpm --import https://rpm.packager.io/key
<span class="hljs-built_in">echo</span> <span class="hljs-string">"[clawiod]
name=Repository for clawio/clawiod application.
baseurl=https://rpm.packager.io/gh/clawio/clawiod/centos6/master
enabled=1"</span> | <span class="hljs-built_in">sudo</span> tee /etc/yum.repos.d/clawiod.repo
<span class="hljs-built_in">sudo</span> yum install clawiod
</code></pre></div>

# Start clawiod

`service systemctl start clawiod`

See [Getting started](/documentation/getting_started)

