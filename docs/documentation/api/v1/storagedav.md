---
layout: page
title: StorageDAV
---

This describes the API for accesing the storage using WebDAV.

# Contents

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}

# Introduction

The Storage API manages storages and their resources.
It provides the most powerful way to interact with the storage.

The StorageDAV is an API that provides filesystem-alike operations through WebDAV on top of the Storage API.

The authentication method is Basic Auth. Token based authentication is not enabled because most of the WebDAV clients don't know how to deal with it as it isn't part of the WebDAV specification.

# Connection

To connect to the StorageDAV API the client must connect to the following URL:

`https://yourdomain.com/api/v1/storagedav/:resourceuri`

Once you have submitted you authentication credentials you can start submitting your WebDAV requests.
