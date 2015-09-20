---
layout: page
title: OCWebDAV
---

This describes the API for accesing the storage using OwnCloud Sync Clients.

# Contents

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}

# Introduction
The OCWebDAV is an API that allows the use of [OwnCloud Sync Clients](https://owncloud.org/install/#install-clients) to manage the storage.

The authentication method is [Basic Auth](https://en.wikipedia.org/wiki/Basic_access_authentication).

# Connection

To connect to the OCWebDAV API the client must connect to the following URL:

`http://yourdomain.com/api/v1/ocwebdav/`

Steps to connect to ClawIO server:

1. Open the OwnCloud Sync Client
2. Enter the localtion of ClawIO server. Example: `https://yourdomain.com/api/v1/ocwebdav/`
3. Submit authentication credentials
4. Click *Skip folder configurations*
5. Click *Add folder to synchronize*
6. Select a local folder
7. Select a remote folder. Example: `local`
8. Click *Add folder*

The sync client should start syncing.

# Disclaimer.

**The status of the OCWebDAV API is experimental, use it at your own risk**.

The API doesn't support yet:

* Chunk upload
* Sharing
* Versions
* Trash bin
* Cookie-based authentication

The step 4: Click *Skip folder configurations* is necessary due to OwnCloud lack of flexibility to configure a remote path on the initial wizard. This constraint is imposed due to OwnCloud global namespace design.

# FAQ

## CSync failed to access ...
When this error appears in the sync client is because of the user home directory has not been created for the user.

To solve the problem, authenticate the user using the [Auth API](/documentation/api/v1/auth/#post-apiv1authgettoken)
