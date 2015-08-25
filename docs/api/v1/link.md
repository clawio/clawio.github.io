---
layout: page
title: Link
---

This describes the API for managing the links.

# Contents

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}

# Introduction

The Link API manages links.

A link is method to share resources without having to be a user of the system. Using links you can expose your internal resources to external collaborators with a mix of security considerations:

+ **Password protected links**: A password has to be provided to access the resources inside the link.
+ **Expiration time**: The links can have a expiration date.
+ **Permissions**: The operations that can be done with a link are enforced by a set of permissions.

# Models

## Link Permission
The operations that can be done on a link are governed by the **Link Permission** model:

Name | Type | Description
-----|------|--------------
`stat`|`boolean` | Grants getting metadata from a resource.
`list`|`boolean` | Grant listing resources inside a container.
`add`|`boolean` | Grants adding resources to the container.
`get`|`boolean` | Grants downloading an object.
`remove`|`boolean` | Grants removing resources from the container.

There are two types of links:

1. **Object link**: When the link points to an object the only link permissions that apply are : `stat`, `get` and `remove`.
    
2. **Container link**: When the link points to a container all the permissions applies, following these rules:

`stat`|`boolean` | Grants getting metadata from the container and ALL resources inside.
`list`|`boolean` | Grant listing resources inside a container and ALL sub-containers.
`add`|`boolean` | Grants adding resources to the container and in ALL sub-containers.
`get`|`boolean` | Grants downloading objects inside the container and in ALL sub-containers.
`remove`|`boolean` | Grants removing resources from the container and in ALL sub-containers.

**Besides the link permissions, the last word is dictated by the storage permissions. For example, if you set `add` permission to a container in the link permission model but the storage permission model forbids this permission, then you cannot `add`**.

## Link Metadata
The **Link Metadata** model is an extension of the [Storage Metadata](/api/v1/storage/#metadata):

Name | Type | Description
-----|------|--------------
`checksum`|`string` | The checksum of the resource.
`checksum_type`|`string` | The type of checksum used for the checksum.
`children`|`array` | An array containing [link metadata](#link-metadata) about the resources inside a container.
`etag` | `string` | The ETag of the resource
`extra`|`any` | Custom information returned by the storage. It can be any JSON type.
`id`|`string` | The Resource URI in ID format.
`is_container`|`boolean` | Indicates if the resource is a container.
`mimetype`|`string` | The mimetype of the resource. Containers MUST return `inode/directory`.
`modified`|`number` | The modification time of the resource in UTC Unix time.
`path`|`string` | The Resource URI in Path format.
`permissions`|`object` | The [permissions](/api/v1/storage/#permission) for the resource.
`size`|`number` | The size of the resource.
`opaque_path`|`string` | An opaque path to access sub-resources.

When getting link metadata, the `id` and `path` fields SHOULD be blanked to avoid disclosure of information.

The `opaque_path` is the path that end users MUST use to do storage-alike operations. The `opquep_ath` structure is : `<token>/[path]`.

# Create a link

The token that identifies the link MUST be **unique** across all the links in the system. The link SHOULD change if it is regenerated (create-delete-create).

    POST /link/create

## Body parameters

Name | Type | Description
-----|------|--------------
`resource_uri`|`string` | The Resource URI of the resource.
`password`|`string` | A password to protect the link.
`expiration`|`number` | A expiration date in UTC Unix time for the resource.
`permissions`|`object` | The [permissions](#link-permission) for the link.

## Response

    Status: 201 Created

    {
        "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9.TJVA95OrM7E2cBab30RMHrHDcEfxjoYZgeFONFh7HgQ"
    }


# Update a link

Just the owner of the link has the right to update it.

**The link token after an update MUST remain the same**

    PUT /link/update/:token

## Path parameters

Name | Type | Description
-----|------|--------------
`token`|`string` | The link token.

## Body parameters

Name | Type | Description
-----|------|--------------
`password`|`string` | A password to protect the link.
`expiration`|`number` | A expiration date in UTC Unix time for the resource.
`permissions`|`object(LinkPermission)` | The permissions for the link.

## Response

    Status: 200 OK

# Delete a link

Just the owner of the link has the right to delete it.

    DELETE /link/delete/:token

## Path parameters

Name | Type | Description
-----|------|--------------
`token`|`string` | The link token.

## Response

    Status: 204 No Content


# Get the metadata of a resource using the link

    GET /link/stat/:token/[:path][?children=boolean]

## Path parameters

Name | Type | Description
-----|------|--------------
`token`|`string` | The link token.
`path`|`string` | The path for reaching sub-resources.

## Query Parameters

Name | Type | Description
-----|------|--------------
`children`|`boolean` | If the resource is a container, ask for the resources inside.

## Response

    GET /link/stat/eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9.TJVA95OrM7E2cBab30RMHrHDcEfxjoYZgeFONFh7HgQ/

    Status: 200 OK

    {
        "checksum": "",
        "checksum_type": "",
        "children": null,
        "etag": "1439495898",
        "extra": null,
        "id": "",
        "is_container": false,
        "mimetype": "application/pdf",
        "modified": 1439495898,
        "path": "",
        "permissions": {
            "add": false,
            "link": false,
            "list": false,
            "remove": true,
            "share": false,
            "stat": true
        },
        "size": 238,
        "opaque_path": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9.TJVA95OrM7E2cBab30RMHrHDcEfxjoYZgeFONFh7HgQ",
    }

*Stating a link pointing to a container*

    GET /link/stat/eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9.TJVA95OrM7E2cBab30RMHrHDcEfxjoYZgeFONFh7HgQ/photos
    Status: 200 OK

    {
        "checksum": "",
        "checksum_type": "",
        "children": null,
        "etag": "1439495898",
        "extra": null,
        "id": "",
        "is_container": true,
        "mimetype": "inode/directory",
        "modified": 1439495898,
        "path": "",
        "permissions": {
            "add": true,
            "link": false,
            "list": true,
            "remove": true,
            "share": false,
            "stat": true
        },
        "size": 0,
        "opaque_path": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9.TJVA95OrM7E2cBab30RMHrHDcEfxjoYZgeFONFh7HgQ/photos",
    }

# Remove a resource using the link

    DELETE /link/rm/:token/[:path]

## Path parameters

Name | Type | Description
-----|------|--------------
`token`|`string` | The link token.
`path`|`string` | The path for reaching sub-resources.

## Response

    Status: 204 No Content

# Upload an object using the link

    PUT /link/put/:token/[:path][?checksuminfo]

## Path parameters

Name | Type | Description
-----|------|--------------
`token`|`string` | The link token.
`path`|`string` | The path for reaching sub-resources.

## Query parameters

Name | Type | Description | Example
-----|------|-------------| -------
`cheksum`|`string` | The checksum type and checksum in the format: `<checksum_type>:<checksum>` | md5:138019839208309

## Headers

Name | Type | Description | Example
-----|------|-------------| -------
`X-Claw-Checksum`|`string` | The checksum type and checksum in the format: `<checksum_type>:<checksum>` | md5:138019839208309

## Body parameters
The body is the **binary data** of the object.

## Response

    Status: 201 Created

## Example
    curl https://yourdomain.com/api/v1/link/put/eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9.TJVA95OrM7E2cBab30RMHrHDcEfxjoYZgeFONFh7HgQ/photos/bunny.png?checksum=md5:d6df47d400edf3f738b3015378f6ea53 --data-binary @/etc/passwd

# Download an object using the link

    GET /link/get/:token/[:path]

## Path parameters

Name | Type | Description
-----|------|--------------
`token`|`string` | The link token.
`path`|`string` | The path for reaching sub-resources.


## Response

    Status: 200 OK
    X-Claw-Checksum: md5:138019839208309

    Binary data ...

# Create a container using the link

    POST /link/createcontainer/:token/[:path]

## Path parameters

Name | Type | Description
-----|------|--------------
`token`|`string` | The link token.
`path`|`string` | The path for reaching sub-resources.

## Response

    Status: 201 Created
