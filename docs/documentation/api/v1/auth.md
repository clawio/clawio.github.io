---
layout: page
title: Auth
---

This describes the API for authenticating users.

# Contents

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}

# Introduction

The Auth API authenticates users.

A user is identified by three fields:

Name | Type | Description
-----|------|--------------
`eppn`|`string` | The [eduPersonPrincipalName](http://www.internet2.edu/media/medialibrary/2013/09/04/internet2-mace-dir-eduperson-201203.html#eduPersonPrincipalName). Usually it is like an username.
`idp`|`string` | The Identity Provider. It is usually a domain name that identifies the organization that authenticates the user.
`authid`|`string` | The authentication startegy used by the Identity Provider to authenticate the user.

In a default installation, the user with the eppn `demo` belongs to the organization identified by the idp `localhost` that uses `fileauth` as an authentication mechanism.

This API is used also to create user home directories.

When the [gettoken](/documentation/api/v1/auth/#post-apiv1authgettoken) operation is successfull, the user home directory is created in all the storages registered. It is more efficient to a have single endpoint to control the user home directory creation than checking its existence at every operation.

# Operations

## `POST /api/v1/auth/gettoken`

Get a JWT Token and creates user home directories.

    POST /api/v1/auth/gettoken

**Body parameters**

Name | Type | Description
-----|------|--------------
`eppn`|`string` | The [eduPersonPrincipalName](http://www.internet2.edu/media/medialibrary/2013/09/04/internet2-mace-dir-eduperson-201203.html#eduPersonPrincipalName). Usually it is like an username.
`idp`|`string` | The Identity Provider. It is usually a domain name that identifies the organization that authenticates the user.
`authid`|`string` | The authentication startegy used by the Identity Provider to authenticate the user.
`password`|`string` | The password used by some authentication providers like LDAP or files.
`extra`|`any` | Extra information passed to the authentication provider.

**Response**

    curl -iX POST http://localhost:57008/api/v1/auth/gettoken --data '{"eppn":"demo", "password":"demo", "authid":"fileauth"}'
    HTTP/1.1 201 Created
    Date: Sun, 13 Sep 2015 09:56:18 GMT
    Content-Length: 284
    Content-Type: text/plain; charset=utf-8

    {
        "authtoken":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJhdXRoaWQiOiJmaWxlYXV0aCIsImRpc3BsYXluYW1lIjoiRGVtbyBVc2VyIiwiZW1haWwiOiJkZW1vQGxvY2FsaG9zdCIsImVwcG4iOiJkZW1vIiwiZXhwIjoxNDQ0NzMwMTc4LCJpZHAiOiJsb2NhbGhvc3QiLCJpc3MiOiJsb2NhbGhvc3QifQ.AUr1432j5m7YGvhjgLzL0FxfUCeM_DlQS4Y_c_Mxm4M"
    }

**Possible HTTP Error Codes**

Code| Description
-----|-----------|
201 | Token created
400 | The username or password don't match.
