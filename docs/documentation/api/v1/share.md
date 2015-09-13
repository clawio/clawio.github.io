---
layout: page
title: Share
---

This describes the API endpoints for managing the internal shares.

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
`iscol`|`boolean` | Indicates if the resource is a folder.
`mimetype`|`string` | The mimetype of the resource. Containers MUST return `inode/directory`.
`modified`|`number` | The modification time of the resource in UTC Unix time.
`path`|`string` | The Resource URI in Path format.
`permissions`|`object(Permission)` | The permissions for the resource.
`size`|`number` | The size of the resource. Folders MAY return the aggregated children´s size.
