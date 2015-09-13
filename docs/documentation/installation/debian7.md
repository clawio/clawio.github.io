---
layout: page-installation
title: Debian 7
---

Here is how to install the package on a Debian 7 Wheezy 64bits server:

# Install

<div class="marked"><pre><code>wget <span class="hljs-attribute">-qO</span> <span class="hljs-subst">-</span> https:<span class="hljs-comment">//deb.packager.io/key | sudo apt-key add -</span>
echo <span class="hljs-string">"deb https://deb.packager.io/gh/clawio/clawiod jessie master"</span> <span class="hljs-subst">|</span> sudo tee /etc/apt/sources<span class="hljs-built_in">.</span><span class="hljs-built_in">list</span><span class="hljs-built_in">.</span>d/clawiod<span class="hljs-built_in">.</span><span class="hljs-built_in">list</span>
sudo apt<span class="hljs-attribute">-get</span> update
sudo apt<span class="hljs-attribute">-get</span> install clawiod
</code></pre></div>

# Start clawiod

`service clawiod start`

See [Getting started](/documentation/getting_started)