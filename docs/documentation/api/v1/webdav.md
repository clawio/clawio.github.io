---
layout: page
title: WebDAV
---

This describes the API for accessing the storage using WebDAV.

# Contents

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}

# Introduction

The WebDAV is an API that provides filesystem-alike operations through [WebDAV](https://en.wikipedia.org/wiki/WebDAV) protocol.

The authentication method is [Basic Auth](https://en.wikipedia.org/wiki/Basic_access_authentication).

# Connection

To connect to the WebDAV API the client must connect to the following URL:

`http://yourdomain.com/api/v1/webdav/:storagepath`

Where `:storagepath` is one of the storages defined.

Example: `http://yourdomain.com/api/v1/webdav/local`

Once you have submitted you authentication credentials you can start submitting your WebDAV requests.

The majority of OS has built-in support for WebDAV:

* Mac OSX: use the Finder
* Windows: use Network Drives
* Linux: use a fuse mount with [davfs2](http://savannah.nongnu.org/projects/davfs2) or other clients
* Third-party applications like [Cyberduck](https://cyberduck.io/)

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
