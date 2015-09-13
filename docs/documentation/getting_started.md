---
layout: simple
title: Getting started
---

This getting started is for technical users who are interested in learning about ClawIO. By following this getting started, you’ll learn fundamental ClawIO configuration features by performing some simple tasks.

# You’ll learn how to

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}

# Install clawiod
To install ClawIO check the installation instructions for your operative system [here](/documentation/installation).

# Understanding the default installation

The core component of ClawIO is `clawiod`, the API server that you have installed in the previous step.

By default, the server use a JSON file to authenticate users (`/etc/clawiod/conf.d/fileauth.conf`).

See the contents of the file: `cat /etc/clawiod/conf.d/fileauth.conf`


    [
        {"eppn":"demo", "password": "demo", "idp": "localhost", "displayname": "Demo User", "email": "demo@localhost"}
    ]

The fields are the following:

* `eppn`: The [eduPersonPrincipalName](http://www.internet2.edu/media/medialibrary/2013/09/04/internet2-mace-dir-eduperson-201203.html#eduPersonPrincipalName). It is the unique identifier for the user, like a username.
* `password`: The password. This is only used when the authentication method is not federated login, like ldap or JSON-based authentication. In this case we are authenticating the user against a JSON file so we need it.
* `idp`: The Identity Provider is responsible for authenticating the user.
* `displayname`: A user-friendly name instead. Usually the full name of the person.
* `email`: The email to reach the user identified by the `eppn`.

The default server has registered one local storage that saves data at `/home/clawiod/data/notmp`.

The contents of this directory are the following:

    root@8d6f87faba55:/# ls -l /home/clawiod/data/notmp/
    total 0

To create the user home directory we have to log in (get authentication token).


    root@522d58cc3217:/# curl -i -X POST localhost:57008/api/v1/auth/gettoken --data '{"eppn":"demo", "password":"demo"}'
    HTTP/1.1 201 Created
    Date: Sat, 12 Sep 2015 10:46:31 GMT
    Content-Length: 280
    Content-Type: text/plain; charset=utf-8

    {"auth_token":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJhdXRoaWQiOiJmaWxlYXV0aCIsImRpc3BsYXluYW1lIjoiRGVtbyBVc2VyIiwiZW1haWwiOiJkZW1vQGxvY2FsaG9zdCIsImVwcG4iOiJkZW1vIiwiZXhwIjoxNDQyMDgzNTkxLCJpZHAiOiJsb2NhbGhvc3QiLCJpc3MiOiJDbGF3SU8ifQ.DsCFtO0sJYNx9j8561I_ygqMm3infLxJJjMtX0TZlF8"}

If you are curious about what is the content of the token, copy and paste it at [jwt.io](http://jwt.io/). You will see that the signature is invalid but if you use your secret (kept in the directive `token_secret`) it will be valid.

If we see the contents of the data folder again:

    root@522d58cc3217:/# ls -l /home/clawiod/data/notmp/
    total 4
    drwxrwxr-x 3 clawiod clawiod 4096 Sep 12 10:46 fileauth

What is the fileauth directory ? The local storage implementation partition the users based on the authentication strategy used, so as user demo is using JSON-based authentication with the ID `fileauth` we have this new directory.

Let's see what is inside:

    root@522d58cc3217:/# ls -l /home/clawiod/data/notmp/fileauth/
    total 4
    drw-r--r-- 2 clawiod clawiod 4096 Sep 12 10:46 demo

Cool! We have the user home directory created for the user demo.

But this direcoty is empty, so let's add some data.

For adding data there are two APIs, the [Storage API](/documentation/api/v1/storage) and the [WebDAV API](/documentation/api/v1/webdav).

Let's start with the storage API.

We are going to create two directories:

    root@522d58cc3217:/# curl -iX POST localhost:57008/api/v1/storage/createcontainer/local/photos
    HTTP/1.1 401 Unauthorized
    Content-Type: text/plain; charset=utf-8
    X-Content-Type-Options: nosniff
    Date: Sat, 12 Sep 2015 11:08:12 GMT
    Content-Length: 13

    Unauthorized

We are receiving a 401 Unauthorized response. This is okay because we didn't provide any credentails.

To authorize the requests there are three options:

* Basic Auth: we provided the credentails using the HTTP Basic Auth mechanism.

        @7f8557a69682:/# curl -X POST localhost:57008/storage/createcontainer/local/photos -u demo:demo
        {"id":"local/photos","path":"local/photos","size":4096,"iscontainer":true,"mimetype":"inode/container","checksum":"","checksumtype":"","modified":1442060591,"etag":"\"1442060591\"","permissions":{"stat":true,"list":true,"add":true,"get":true,"remove":true,"link":false,"share":false,"federatedshare":false},"children":null,"extra":null}

* Auth Token is HTTP Header:

        

# Modify the configuration
