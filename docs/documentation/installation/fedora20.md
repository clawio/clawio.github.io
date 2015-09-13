---
layout: page-installation
title: Fedora 20
---

Here is how to install the package on a Fedora 20 64 bits server:

# Install

<div class="marked"><pre><code><span class="hljs-built_in">sudo</span> rpm --import https://rpm.packager.io/key
<span class="hljs-built_in">echo</span> <span class="hljs-string">"[clawiod]
name=Repository for clawio/clawiod application.
baseurl=https://rpm.packager.io/gh/clawio/clawiod/fedora20/master
enabled=1"</span> | <span class="hljs-built_in">sudo</span> tee /etc/yum.repos.d/clawiod.repo
<span class="hljs-built_in">sudo</span> yum install clawiod
</code></pre></div>

# Start clawiod

`service clawiod start`

See [Getting started](/documentation/getting_started)