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

A user belongs to an authentication provider.

The authentication provider authenticates the user against its user database with the `username`, `password` and `extra` attributes passed in the request.

The user database can vary from a simple JSON file to a complex LDAP server.

Different authentication providers can be registered in the server to provide some QoS or to have more control about user privileges.

In the future, Single Sign-On using Shibboleth will be supported.

The fact of having multiple authentication providers registered allows the storage to be more efficient in managing resources.

For example, imagine that the storage configured is an object store like Swift with multiple tenants (one in Sidney and the other in Berlin).
Users coming from auth provider A will have their reosurces in a tenant in Sidney and users coming from auth provider B will have their resources in a tenant in Berlin.

# Get a JWT Token

    POST /auth/login

## Body parameters

Name | Type | Description
-----|------|--------------
`id`|`string` | The username
`password`|`string` | The password
`authid`|`string` | The authentication provider ID
`extra`|`any` | Extra information passed to the authentication provider.

## Response

	Status: 201 Created

	{
    	"token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9.TJVA95OrM7E2cBab30RMHrHDcEfxjoYZgeFONFh7HgQ"
    }

