---
layout: simple
title: Manifesto
---

Read the principles that have driven the creation of ClawIO.

# 1. Scalability
CERNBox is a service created at CERN that provides synchronization capabilities between a user machine (desktop PC, smart phone or tablet) and a central repository, managed by CERN IT department. CERNBox is based on OwnCloud and it provides a functionality analogous to Dropbox or similar systems.
The data are stored on EOS, the disk-storage developed and operated by the IT department for LHC computing. This allows users to access data from a variety of devices in seamless ways.
CERNBox addressed some of the OwnCloud's scalability limitations using in-house patches.
These patches have been developed for EOS storage backend and should be generalized to allow a broader usage. The current OwnCloud architecture shows some limitations which should be addressed in a definite way by replacing the core components that don't fit the requirements of huge deployments.

The goal of this research project is to create and demonstrate a new scalable architecture which overcome the existing limitations and allows broader adoption of such a service.
This new architecture has to be flexible enough to offer the same design optimizations done with EOS to a wider range of different storage backends like object storage.
The architecture has to be compatible with the sync protocol used by OwnCloud.

# 2. Standard based
Companies appear and disappear. Protocols always remain. This is a fact.

At the time of this writing there is an ongoing effort to create two industry standards: one for synchronization and one for sharing.
The protocol that deals with sharing has been started by OwnCloud and it's called [OpenCloudMesh](https://owncloud.com/lp/opencloudmesh/). This protocol allows different cloud vendors to share their data across different clouds.

The protocol that deals with synchronization is still being cooked and there isn't any draft yet.

ClawIO will implement these protocols to ensure vendor-specific implementations don't guide the protocol design, in order to achieve truly vendor agnostic specifications. 

# 3. Open Source

Open-sourcing ClawIO means sharing and learning with the community of sites that have scalability problems. There's a lot we can improve upon in ClawIO, and help is always appreciated. While we don't currently plan on building this out as a competitor to other Sync and Share servers, we will occasionally add, remove, or modify things.
