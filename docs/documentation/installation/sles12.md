---
layout: page-installation
title: SLES 12
---

Here is how to install the package on a SUSE Linux Enterprise Server 12 64 bits server:

# Install

<div class="marked"><pre><code><span class="hljs-built_in">sudo</span> rpm --import https://rpm.packager.io/key
<span class="hljs-built_in">sudo</span> zypper addrepo <span class="hljs-string">"https://rpm.packager.io/gh/clawio/clawiod/sles12/master"</span> <span class="hljs-string">"clawiod"</span>
<span class="hljs-built_in">sudo</span> zypper install clawiod
</code></pre></div>

# Start clawiod

`service clawiod start`

See [Getting started](/documentation/getting_started)