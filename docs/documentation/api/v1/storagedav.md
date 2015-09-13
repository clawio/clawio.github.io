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

`https://yourdomain.com/api/v1/webdav/:storagepath`

Once you have submitted you authentication credentials you can start submitting your WebDAV requests.

The majority of OS has built-in support for WebDAV:

* Mac OSX: use the Finder
* Windows: use Network Drives
* Linux: use a fuse mount with [davfs2](http://savannah.nongnu.org/projects/davfs2) or other clients
* Third-party apps like [Cyberduck](https://cyberduck.io/)

Example using curl:

    curl -X PROPFIND http://localhost:57008/api/v1/webdav/local -H "Depth: 1" -u demo:demo
    <?xml version="1.0" encoding="UTF-8"?>
    <d:multistatus xmlns:d="DAV:">
        <d:response>
            <d:href>/api/v1/webdav/local</d:href>
            <d:propstat>
                <d:prop>
                    <d:resourcetype>
                        <d:collection />
                    </d:resourcetype>
                    <d:getcontentlength>4096</d:getcontentlength>
                    <d:getlastmodified>Sun, 13 Sep 2015 14:41:08 UTC</d:getlastmodified>
                    <d:getetag>"1442155268"</d:getetag>
                    <d:getcontenttype>inode/container</d:getcontenttype>
                </d:prop>
                <d:status>HTTP/1.1 200 OK</d:status>
            </d:propstat>
        </d:response>
        <d:response>
            <d:href>/api/v1/webdav/local/passwd2</d:href>
            <d:propstat>
                <d:prop>
                    <d:resourcetype>
                        <d:collection />
                    </d:resourcetype>
                    <d:getcontentlength>4096</d:getcontentlength>
                    <d:getlastmodified>Sun, 13 Sep 2015 14:20:30 UTC</d:getlastmodified>
                    <d:getetag>"1442154030"</d:getetag>
                    <d:getcontenttype>inode/container</d:getcontenttype>
                </d:prop>
                <d:status>HTTP/1.1 200 OK</d:status>
            </d:propstat>
        </d:response>
        <d:response>
            <d:href>/api/v1/webdav/local/passw2</d:href>
            <d:propstat>
                <d:prop>
                    <d:getcontentlength>1046</d:getcontentlength>
                    <d:getlastmodified>Sun, 13 Sep 2015 13:08:02 UTC</d:getlastmodified>
                    <d:getetag>"1442149682"</d:getetag>
                    <d:getcontenttype>application/octet-stream</d:getcontenttype>
                </d:prop>
                <d:status>HTTP/1.1 200 OK</d:status>
            </d:propstat>
        </d:response>
    </d:multistatus>
