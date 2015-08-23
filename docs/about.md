---
layout: simple
title: About
---

Learn more about this project, particularly how it's built and who maintains it.

### What and why
I was working on CERN developing the CERNBox project, a sync and share platform based on OwnCloud.

During the development phase I used all the knowledge learned to write my bachelor thesis on how to scale OwnCloud and how we did it with CERNBox.

CERNBox fixed some of the most un-scalable components in OwnCloud architecture like the SQL namespace and the sharing module.

Those fixes  were made using direct patches to the code without having the possibility to merge the CERNBox code base with the upstream product at the time of the CERNBox development.

OwnCloud has serious design problems regarding scalability that are pretty hard to fix without affecting the usability and backward compatibility of the product.

ClawIO was born as a prototype implementation to assert my ideas about a better scalable architecture for a sync and share server like OwnCloud.

ClawIO architecture is very innovative. ClawIO has ben developed using an API-driven development approach, to create a server that just focus on one thing: provide the necesary REST APIs to sync and share data. No fancy user interfaces or Calendar-alike apps are involved in the development of the ClawIO Daemon.


Open-sourcing ClawIO means sharing and learning with the community of sites that have scalability problems. There's a lot we can improve upon in ClawIO, and help is always appreciated. While we don't currently plan on building this out as a competitor to other Sync and Share servers, we will occasionally add, remove, or modify things.

### Platform support

ClawIO Daemon is supported on Unix based systems. Windows is not supported and never will be.

ClawIO WebApp currently supports Internet Explorer 9+ and the latest two versions of Chrome, Safari, and Firefox on OS X and Windows. While not a responsive or mobile-focused project, Mobile Safari and Chrome for Android should render just fine. Support for Linux-based browsers is not strictly guaranteed, but accounted for whenever possible.

### Future updates

See the [roadmap](/roadmap) for a rough outline on what's slated for future versions of ClawIO.

### Who

Currently maintained by [@labkode](https://github.com/labkode).
