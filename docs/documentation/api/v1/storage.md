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

A Resource URI is powerful addressing scheme to identify diferent storages.
It is usually referenced by the  `:resourceuri` placeholder.

The Resource URI can take two forms:

1. **Path format**: When referencing a resource by a path:

        storage-scheme://[[authinfo@]host]]/path[?query][#fragment]
        
    Examples:
    
        local:///docs/thesis.pdf
        swift://bucket1.tenant1.com/docs/thesis.pdf
        s3://user:password@bucket2.tenant2.example.com/docs/thesis.pdf
        
    The use of `authinfo@host` can be used in distributed storage systems. This is useful when you want talk to the closest geographically server, for example.
    
2. **ID format**: When referencing a resource by ID

        storage-scheme:opaque[?query][#fragment]
        
    The `opaque` part can be anything and it is defined by each storage.
    Some storages may use a number, other may use a hash and others maybe a path.
        
    Examples:
        
        local:docs/thesis.pdf
        eos:139128318809
        s3:de305d54-75b4-431b-adb2-eb6b9e546014 

# Models

## Capability
The features that the storage can offer are specified in the **Capability** model:

Name | Type | Description
-----|------|--------------
`stat`|`boolean` | Stat a reosource.
`add`|`boolean` | Add resources to a container.
`remove`|`boolean` | Remove resources from a container.
`copy`|`boolean` | Copy a resource.
`rename`|`boolean` | Rename a resource.
`3rdcopy`|`boolean` | Thrird party copy of a resource.
`3rdrename`|`boolean` | Third party rename of a resource.
`putobject`|`boolean` | Upload the full object.
`putobjectinchunks`|`boolean` | Upload the object in chunks.
`getobject`|`boolean` | Download the full object.
`getobjectbybyterange`|`boolean` | Download some bytes of the object.
`createcont`|`boolean` | Create a container.
`verifyclientchecksum`|`boolean` | Verify client checksums.
`sendserverchecksum`|`boolean` | Send server-computed checksum.

*Version features*

`listversions`|`boolean` | List versions of a resource.
`getversion`|`boolean` | Download a version.
`createversion`|`boolean` | Create a version.
`deleteversion`|`boolean` | Delete a version.
`rollbackversion`|`boolean` | Rollback to a previous version.

*Trashbin features*

`listjunk`|`boolean` | List junk files (deleted files).
`restorejunk`|`boolean` | Restore a junk a file.
`purgejunk`|`boolean` | Purge a junk file.


## Metadata
All resources have metadata attached. This information is kept in the **Metadata** model:

Name | Type | Description
-----|------|--------------
`checksum`|`string` | The checksum of the resource.
`checksumtype`|`string` | The type of checksum used for the checksum.
`children`|`array(Metadata)` | An array containing metadata about the resources inside a folder.
`etag` | `string` | The ETag of the resource
`extra`|`any` | Custom information returned by the storage. It can be any JSON type.
`id`|`string` | The Resource URI in ID format.
`iscontainer`|`boolean` | Indicates if the resource is a folder.
`mimetype`|`string` | The mimetype of the resource. Containers MUST return `inode/directory`.
`modified`|`number` | The modification time of the resource in UTC Unix time.
`path`|`string` | The Resource URI in Path format.
`permissions`|`object` | The [permissions](/documentation/api/v1/permission) for the resource.
`size`|`number` | The size of the resource. Folders MAY return the aggregated children´s size.

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


# Get the capabilities of a storage

    GET /storage/capabilities/:storagescheme

## Path parameters

Name | Type | Description
-----|------|--------------
`storagescheme`|`string` | The `<storage-scheme>` element in a Resource URI.

## Response
    GET /storage/capabilities/eos
    
    Status: 200 OK

    {
        "checksum": "",
        "checksumtype": "",
        "children": null,
        "etag": "1439495898",
        "extra": null,
        "id": "local:docs",
        "iscontainer": true,
        "mimetype": "inode/directory",
        "modified": 1439495898,
        "path": "local:///docs"",
        "permissions": {
            "add": true,
            "link": true,
            "list": true,
            "remove": true,
            "share": true,
            "stat": true
        },
        "size": 238
    }


# Get the metadata of a resource

    GET /storage/stat/:resourceuri[?children]

## Path parameters

Name | Type | Description
-----|------|--------------
`resourceuri`|`string` | A Resource URI

## Query Parameters

Name | Type | Description
-----|------|--------------
`children`|`boolean` | If the resource is a container, ask for the resources inside.

## Response


    Status: 200 OK

    {
        "checksum": "",
        "checksumtype": "",
        "children": null,
        "etag": "1439495898",
        "extra": null,
        "id": "local:docs",
        "iscol": true,
        "mimetype": "inode/directory",
        "modified": 1439495898,
        "path": "local:///docs"",
        "permissions": {
            "add": true,
            "link": true,
            "list": true,
            "remove": true,
            "share": true,
            "stat": true
        },
        "size": 238
    }

# Remove a resource

    DELETE /storage/rm/:resourceuri[?children]

## Response

    Status: 204 No Content

# Upload an object

    PUT /storage/put/:resourceuri[?checksuminfo]

## Path parameters

Name | Type | Description
-----|------|--------------
`resourceuri`|`string` | A Resource URI

## Query parameters

Name | Type | Description | Example
-----|------|-------------| -------
`cheksum`|`string` | The checksumtype and checksum in the format: `<checksumtype>:<checksum>` | md5:138019839208309

## Headers

Name | Type | Description | Example
-----|------|-------------| -------
`X-Claw-Checksum`|`string` | The checksumtype and checksum in the format: `<checksumtype>:<checksum>` | md5:138019839208309

## Body parameters
The body is the **binary data** of the object.

## Response

    Status: 201 Created

# Upload an object in chunks

    PUT /storage/putchunk/:resourceuri[?uploadid=string&offset=number]

## Path parameters

Name | Type | Description
-----|------|--------------
`resourceuri`|`string` | A Resource URI

## Query parameters

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


## Body parameters
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

## Path parameters

Name | Type | Description
-----|------|--------------
`resourceuri`|`string` | A Resource URI


## Response

    Status: 200 OK
    X-Claw-ServerChecksum: md5:138019839208309

    Binary data ...

# Create a container

    POST /storage/createcontainer/:resourceuri

## Path parameters

Name | Type | Description
-----|------|--------------
`resourceuri`|`string` | A Resource URI

## Response

    Status: 201 Created
