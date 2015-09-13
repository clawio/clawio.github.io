---
layout: page
title: Storage
---

This describes the API for managing the storage.

# Contents

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}

# Introduction

The Storage API manages storages and their resources.

A resource can be: 

* `Object`: it contains binary data. Depending on the storage it can be a file, a BLOB object or anything that is not a Container.
* `Container`: it contains other resources. Depending on the storage it can be a folder, a bucket or anything that is not an Object.

Every resource live inside a storage. To improve performance ClawIO prefixes every resource with the prefix of the storage. In order to manage resources the `storagepath` format is used to have **Universal Content-Addresable** resources.

The format of the `storagepath` is: `<storageprefix>/<pathtoresource>`

Imagine there is a directory `photos` inside a local storage that has the prefix `local`. The `resourcepath` for this resource will be `local/photos`.

# Models

## Capability
The features/capabilities a storage can offer are specified in the **Capability** model:

Name | Type | Description
-----|------|--------------
`stat`|`boolean` | Can get resource metadata.
`remove`|`boolean` | Can remove resources.
`copy`|`boolean` | Can copy a resource inside the same storage.
`rename`|`boolean` | Can rename a resource inside the same storage.
`thirdpartycopy`|`boolean` | Can thrird party copy of a resource.
`thirdpartyrename`|`boolean` | Can third party rename of a resource.
`putobject`|`boolean` | Can upload the full object.
`putobjectinchunks`|`boolean` | Can upload the object in chunks.
`getobject`|`boolean` | Can download the full object.
`getobjectbybyterange`|`boolean` | Can download some bytes of the object.
`createcontainer`|`boolean` | Can create a container.
`listversions`|`boolean` | Can list versions of a resource.
`getversion`|`boolean` | Can download a version.
`createversion`|`boolean` | Can create versions.
`rollbackversion`|`boolean` | Can rollback to a previous version.
`listdeletedresources`|`boolean` | Can list deleted resources.
`restoredeletedresource`|`boolean` | Can restore a resource from the junk.
`purgeresource`|`boolean` | Can purge a resource.
`verifyclientchecksum`|`boolean` | Can verify client checksums.
`sendchecksum`|`boolean` | Can send checksums to clients.
`supportedchecksum`|`string` |The checksum supported on the server.
`createuserhomedirectoryonlogin` |`boolean` | Create user home directory on login


## Metadata
All resources have metadata attached. This information is kept in the **Metadata** model:

Name | Type | Description
-----|------|--------------
`checksum`|`string` | The checksum of the resource.
`checksumtype`|`string` | The type of checksum used for the checksum.
`children`|`array(Metadata)` | An array containing [Metadata](/documentation/api/v1/storage/#metadata) about the resources inside a container.
`etag` | `string` | The ETag of the resource
`extra`|`any` | Custom information returned by the storage. It can be any JSON type.
`id`|`string` | The ID of the resource.
`iscontainer`|`boolean` | Indicates if the resource is a container.
`mimetype`|`string` | The mimetype of the resource. Containers MUST return `inode/container`.
`modified`|`number` | The modification time of the resource in UTC Unix time.
`path`|`string` | The path to the resource with the storage prefix.
`permissions`|`object` | The [Permissions](/documentation/api/v1/storage/#permission) for the resource.
`size`|`number` | The size of the resource. Containers MAY return the aggregated children´s size.

## Permission
The operations that can be done on a resource are governed by the **Permission** model:

Name | Type | Description
-----|------|--------------
`stat`|`boolean` | Grants getting metadata from a resource.
`list`|`boolean` | Grant listing of resources inside a container.
`add`|`boolean` | Grants adding resources to the container.
`get`|`boolean` | Grants downloading an object.
`remove`|`boolean` | Grants removing resources from the container.
`link`|`boolean` | Grants creating links for the resource.
`share`|`boolean` | Grants internal sharing for the container.
`fedshare`|`boolean` | Grants federated (external) sharing for the container.

# Operations

## `GET /api/v1/storage/getcapabilities`

Get the capabilities of a storage

    GET /api/v1/storage/capabilities/:storageprefix

**Path Parameters**

Name | Type | Description
-----|------|--------------
`storageprefix`|`string` | The prefix of the storage

**Response**

    curl -i http://localhost:57008/api/v1/storage/getcapabilities/local -u demo:demo
    HTTP/1.1 200 OK
    Content-Type: application/json
    Date: Sun, 13 Sep 2015 11:44:52 GMT
    Content-Length: 631

    {
        "putobject": false,
        "putobjectinchunks": false,
        "getobject": false,
        "getobjectbybyterange": false,
        "stat": false,
        "remove": false,
        "createcontainer": false,
        "copy": false,
        "rename": false,
        "thirdpartycopy": false,
        "thirdpartyrename": false,
        "listversions": false,
        "getversion": false,
        "createversion": false,
        "rollbackversion": false,
        "listdeletedresources": false,
        "restoredeletedresource": false,
        "purgedeletedresource": false,
        "verifycientchecksum": false,
        "sendchecksum": false,
        "supportedchecksum": "",
        "createuserhomedirectory": true
    }

**Possible HTTP Error Codes**

Code| Description
-----|-----------|
200 | Capabilities returned

## `GET /api/v1/storage/get`

Download an object

    GET /api/v1/storage/get/:storagepath

**Path Parameters**

Name | Type | Description
-----|------|--------------
`storagepath`|`string` | The storage path.

**Response**

    curl -i http://localhost:57008/api/v1/storage/get/local/passwd -u demo:demo
    HTTP/1.1 200 OK
    Content-Disposition: attachment; filename=passwd
    Content-Type: application/octet-stream
    Date: Sun, 13 Sep 2015 13:22:22 GMT
    Content-Length: 1046

    ... binary data ...

**Possible HTTP Error Codes**

Code| Description
-----|-----------|
200 | Binary object returned.
501 | Download of containers not supported.

## `GET /api/v1/storage/stat`

Get the metadata of a resource

    GET /api/v1/storage/stat/:storagepath

**Path Parameters**

Name | Type | Description
-----|------|--------------
`storagepath`|`string` | The storage path.

**Query Parameters**

Name | Type | Description
-----|------|--------------
`children`|`boolean` | If the resource is a container, ask for the resources inside.

**Response**

    curl -i http://localhost:57008/api/v1/storage/stat/local?children=true -u demo:demo
    HTTP/1.1 200 OK
    Date: Sun, 13 Sep 2015 12:41:28 GMT
    Content-Length: 1836
    Content-Type: text/plain; charset=utf-8

    {
        "id": "local",
        "path": "local",
        "size": 4096,
        "iscontainer": true,
        "mimetype": "inode/container",
        "checksum": "",
        "checksumtype": "",
        "modified": 1442148066,
        "etag": "1442148066",
        "permissions": {
            "stat": true,
            "list": true,
            "add": true,
            "get": true,
            "remove": true,
            "link": false,
            "share": false,
            "federatedshare": false
        },
        "children": [
            {
                "id": "local/photos",
                "path": "local/photos",
                "size": 4096,
                "iscontainer": true,
                "mimetype": "inode/container",
                "checksum": "",
                "checksumtype": "",
                "modified": 1442148066,
                "etag": "1442148066",
                "permissions": {
                    "stat": true,
                    "list": true,
                    "add": true,
                    "get": true,
                    "remove": true,
                    "link": false,
                    "share": false,
                    "federatedshare": false
                },
                "children": null,
                "extra": null
            }
        ],
        "extra": null
    }

**Possible HTTP Error Codes**

Code| Description
-----|-----------|
200 | Metadata returned

## `PUT /api/v1/storage/put`

    PUT /api/v1/storage/put/:storagepath

**Path Parameters**

Name | Type | Description
-----|------|--------------
`storagepath`|`string` | The storage path.

**Query Parameters**

Name | Type | Description | Example
-----|------|-------------| -------
`x-checksum`|`string` | The checksumtype and checksum in the format: `<checksumtype>:<checksum>` | `md5:138019839208309`

**Headers**

Name | Type | Description | Example
-----|------|-------------| -------
`X-Checksum`|`string` | The checksumtype and checksum in the format: `<checksumtype>:<checksum>` | `md5:138019839208309`

**Body Parameters**

The body is the **binary data** of the object.

**Response**

    curl -i -X PUT http://localhost:57008/api/v1/storage/put/local/passwd --data-binary @/etc/passwd -u demo:demo
    HTTP/1.1 201 Created
    Date: Sun, 13 Sep 2015 13:08:02 GMT
    Content-Length: 502
    Content-Type: application/json

    {
        "id": "local/passwd",
        "path": "local/passwd",
        "size": 1046,
        "iscontainer": false,
        "mimetype": "application/octet-stream",
        "checksum": "",
        "checksumtype": "",
        "modified": 1442149682,
        "etag": "1442149682",
        "permissions": {
            "stat": true,
            "list": false,
            "add": false,
            "get": true,
            "remove": true,
            "link": false,
            "share": false,
            "federatedshare": false
        },
        "children": null,
        "extra": null
    }

    Status: 201 Created

**Possible HTTP Error Codes**

Code| Description
-----|-----------|
201 | Object created and Metadata returned.
412 | The checksum provided does not match the server checksum.


## `POST /api/v1/storage/createcontainer`

    POST /api/v1/storage/createcontainer/:storagepath

**Path Parameters**

Name | Type | Description
-----|------|--------------
`storagepath`|`string` | The storage path.

**Response**

    curl -i -X POST http://localhost:57008/api/v1/storage/createcontainer/local/photos -u demo:demo
    HTTP/1.1 201 Created
    Content-Type: application/json
    Date: Sun, 13 Sep 2015 13:28:16 GMT
    Content-Length: 490

    {
        "id": "local/photos",
        "path": "local/photos",
        "size": 4096,
        "iscontainer": true,
        "mimetype": "inode/container",
        "checksum": "",
        "checksumtype": "",
        "modified": 1442150896,
        "etag": "1442150896",
        "permissions": {
            "stat": true,
            "list": true,
            "add": true,
            "get": true,
            "remove": true,
            "link": false,
            "share": false,
            "federatedshare": false
        },
        "children": null,
        "extra": null
    }

**Possible HTTP Error Codes**

Code| Description
-----|-----------|
201 | Container created and Metadata returned.
405 | The container already exists.


## `DELETE /api/v1/storage/rm`

Remove a resource
    
    DELETE /api/v1/storage/rm/:storagepath

**Path Parameters**

Name | Type | Description
-----|------|--------------
`storagepath`|`string` | The storage path.

**Response**

    curl -i -X DELETE http://localhost:57008/api/v1/storage/rm/local/photos -u demo:demo
    HTTP/1.1 204 No Content
    Date: Sun, 13 Sep 2015 14:08:11 GMT

**Possible HTTP Error Codes**

Code| Description
-----|-----------|
204 | Resource deleted.

## `POST /api/v1/storage/mv`

Moves a resource
    
    POST /api/v1/storage/mv

**Query Parameters**

Name | Type | Description 
-----|------|-------------
`from`|`string` | The source storage path.
`to`|`string` | The target storage path .

**Response**

    curl -i -X POST 'http://localhost:57008/api/v1/storage/mv?from=local/photos&to=local/photos.bak' -u demo:demo
    HTTP/1.1 200 OK
    Content-Type: application/json
    Date: Sun, 13 Sep 2015 14:20:34 GMT
    Content-Length: 498

    {
        "id": "local/photos.bak",
        "path": "local/photos.bak",
        "size": 4096,
        "iscontainer": true,
        "mimetype": "inode/container",
        "checksum": "",
        "checksumtype": "",
        "modified": 1442154030,
        "etag": "1442154030",
        "permissions": {
            "stat": true,
            "list": true,
            "add": true,
            "get": true,
            "remove": true,
            "link": false,
            "share": false,
            "federatedshare": false
        },
        "children": null,
        "extra": null
    }

**Possible HTTP Error Codes**

Code| Description
-----|-----------|
200 | Resource moved and Metadata returned.
412 | Canntot overwrite an object with a container and viceversa.



<!--
# Upload an object in chunks

    PUT /storage/putchunk/:resourceuri[?uploadid=string&offset=number]

## Path Parameters

Name | Type | Description
-----|------|--------------
`resourceuri`|`string` | A Resource URI

## Query Parameters

Name | Type | Description 
-----|------|-------------
`uploadid`|`string` | The ID for the chunk upload transaction.
`offset`|`string` | The start offset of the data being transmitted.

In the first chunk the `uploadid` and `offset` MUST NOT be sent.

With the `offset` parameter and the `Content-Length` header is easy to figure out the byte range of the chunk in the original object.

**Typical usage**:

1. Send a PUT request to **/storage/putchunk/:resourceuri** with the first chunk of the file without setting `uploadid` nor `offset`, and receive an `uploadid` in return.
2. Repeatedly (even in parallel) PUT subsequent chunks to **/storage/putchunk/:resourceuri?uploadid=string&offset=number** using the `uploadid` to identify the upload in progress and an `offset` representing the number of bytes transferred so far.
3. After the last chunk, POST to **/commitchunkupload[?checksuminfo=string]** to complete the upload.

**The implementation of this spec MUST handle parallel chunk uploads.   **


## Body Parameters
The body is the **binary data** of the object.

## Response

    Status: 201 Created

    {
        "uploadid": "550e8400-e29b-41d4-a716-446655440000"
    }


## Example
    curl -u username https://yourdomain.com/api/v1/storage/put/local:///docs/new-thesis.pdf?checksum=md5:d6df47d400edf3f738b3015378f6ea53 --data-binary @/etc/passwd

Typical usage:

Send a PUT request to /chunked_upload with the first chunk of the file without setting upload_id, and receive an upload_id in return.
Repeatedly PUT subsequent chunks using the upload_id to identify the upload in progress and an offset representing the number of bytes transferred so far.
After each chunk has been uploaded, the server returns a new offset representing the total amount transferred.
After the last chunk, POST to /commit_chunked_upload to complete the upload.
# Download an object

    GET /storage/get/:resourceuri

## Path Parameters

Name | Type | Description
-----|------|--------------
`resourceuri`|`string` | A Resource URI


## Response

    Status: 200 OK
    X-Claw-ServerChecksum: md5:138019839208309

    Binary data ...

# Create a container

    POST /storage/createcontainer/:resourceuri

## Path Parameters

Name | Type | Description
-----|------|--------------
`resourceuri`|`string` | A Resource URI

## Response

    Status: 201 Created

-->
