---
layout: page
title: API
---

This describes the resources that make up the official ClawIO API v1. If you have any problems please contact [me](mailto:hugo@clawio.co).

# Contents

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}

# Current Version
The current version of the API is **v1**.

# Scheme
All API access should be done over HTTPS, and accessed from **https:/yourdomain.com/api/v1/**. All data is sent and received as JSON otherwise specified.


	curl -i http://localhost:57008/api/v1/storage/stat/local -u demo:demo
	HTTP/1.1 200 OK
	Date: Sun, 13 Sep 2015 09:12:38 GMT
	Content-Length: 322
	Content-Type: application/json

	{
	    "checksum": "",
	    "checksumtype": "",
	    "children": null,
	    "etag": "1442135249",
	    "extra": null,
	    "id": "local",
	    "iscontainer": true,
	    "mimetype": "inode/container",
	    "modified": 1442135249,
	    "path": "local",
	    "permissions": {
	        "add": true,
	        "federatedshare": false,
	        "get": true,
	        "link": false,
	        "list": true,
	        "remove": true,
	        "share": false,
	        "stat": true
	    },
	    "size": 4096
	}


# Blank fields
Blank fields default to these values:

Type | Default value
|------|--------------
`string`|`""`
`number`|`0`
`boolean`|`false`
`object`|`null`, instead of `{}`
`array`|`null`, instead of `[]`


# UTF-8 encoding

Every string passed to and from the ClawIO API needs to be UTF-8 encoded. For maximum compatibility, normalize to [Unicode Normalization Form C (NFC)](http://unicode.org/reports/tr15/) before UTF-8 encoding

# Timestamps
All timestamps are returned in [UTC epoch seconds](http://en.wikipedia.org/wiki/Unix_time) like:

`1439684357`

# Parameters
Many API methods take optional parameters. For GET requests, any parameters not specified as a segment in the path can be passed as an HTTP query string parameter:


	curl http://localhost:57008/api/v1/storage/stat/local?children=true -u demo:demo


In this example, the `local` value is provided for the `storagepath` path parameter meanwhile `children` is passed in the query string.

For POST, PUT, and DELETE requests, parameters not included in the URL should be encoded as JSON with a Content-Type of `application/json`:

	curl -X POST http://localhost:57008/api/v1/auth/gettoken --data '{"eppn":"demo", "password":"demo"}

<!--
# Root Endpoint

You can issue a GET request to the root endpoint to get all the endpoint categories that the API supports:


	$ curl https://yourdomain.com/api/v1 -u username:password

	{
	  "storageurl": "https://yourdomain.com/api/v1/storage",
	  "linkurl": "https://yourdomain.com/api/v1/link",
	}
-->

# Clients Errors

There are three possible types of client errors on API calls that receive request bodies:

* Sending non-UTF8 strings, invalid JSON or the wrong type of JSON values will result in a `415 Unsuported Media Type` response.


		HTTP/1.1 415 Unsupported Media Type

* Sending invalid fields will result in a `422 Unprocessable Entity` response.

		HTTP/1.1 422 Unprocessable Entity
		Content-Length: 72
		
		{
	      "resource":"Auth",
	      "field":"eppn",
	      "code":"empty_field",
		}



All error objects from `422` responses have resource and field properties so that your client can tell what the problem is.  There's also an error code to let you know what is wrong with the field.  These are the possible validation error codes:


Error Name | Description
-----------|-----------|
`empty_field` | This means a required field on a resource has not been set or has a blank value.
`invalid` | This means the formatting of a field is invalid.  The documentation for that resource should be able to give you more specific information.

Resources may also send custom validation errors (where `code` is `custom`). Custom errors will always have a `message` field describing the error, as well as a `documentation_url` field pointing to some content that might help you resolve the error.


# HTTP Redirects

API v1 uses HTTP redirection where appropriate. Clients should assume that any
request may result in a redirection. Receiving an HTTP redirection is *not* an
error and clients should follow that redirect. Redirect responses will have a
`Location` header field which contains the URI of the resource to which the
client should repeat the requests.

Status Code | Description
-----------|-----------|
`301` | Permanent redirection. The URI you used to make the request has been superseded by the one specified in the `Location` header field. This and all future requests to this resource should be directed to the new URI.
`302`, `307` | Temporary redirection. The request should be repeated verbatim to the URI specified in the `Location` header field but clients should continue to use the original URI for future requests.

Other redirection status codes may be used in accordance with the HTTP 1.1 spec.

# HTTP Verbs

Where possible, API v1 strives to use appropriate HTTP verbs for each
action.

Verb | Description
-----|-----------
`HEAD` | Can be issued against some resources to get just the HTTP header info.
`GET` | Used for retrieving resources.
`POST` | Used for creating resources.
`PUT` | Used for replacing resources or collections. For `PUT` requests with no `body` attribute, be sure to set the `Content-Length` header to zero.
`DELETE` |Used for deleting resources.

# Authentication
There are three ways to authenticate through ClawIO API.  Requests that
require authentication will return `401 Not Authorized`. In some places `404 Not Found` will be returned instead, this is to prevent the accidental leakage
of information like password protected Links to unauthorized users.

## Basic Authentication

	curl http://localhost:57008/api/v1/storage/stat/local?children=true -u demo:demo

## JWT Token (via HTTP Header)

	curl http://localhost:57008/api/v1/storage/stat/local?children=true -H "X-Auth-Token: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJhdXRoaWQiOiJmaWxlYXV0aCIsImRpc3BsYXluYW1lIjoiRGVtbyBVc2VyIiwiZW1haWwiOiJkZW1vQGxvY2FsaG9zdCIsImVwcG4iOiJkZW1vIiwiZXhwIjoxNDQ0NzI4NDc0LCJpZHAiOiJsb2NhbGhvc3QiLCJpc3MiOiJsb2NhbGhvc3QifQ.lHxZ_20Qnz_l-GLI-ATZEnqOALOoTp-3zZGtHk9tPTc"

## JWT Token (via HTTP Query Param)

	curl 'http://localhost:57008/api/v1/storage/stat/local?x-auth-token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJhdXRoaWQiOiJmaWxlYXV0aCIsImRpc3BsYXluYW1lIjoiRGVtbyBVc2VyIiwiZW1haWwiOiJkZW1vQGxvY2FsaG9zdCIsImVwcG4iOiJkZW1vIiwiZXhwIjoxNDQ0NzI4NDc0LCJpZHAiOiJsb2NhbGhvc3QiLCJpc3MiOiJsb2NhbGhvc3QifQ.lHxZ_20Qnz_l-GLI-ATZEnqOALOoTp-3zZGtHk9tPTc'

<!--
# Cross Origin Resource Sharing

The API supports Cross Origin Resource Sharing (CORS) for AJAX requests from
any origin as default.
You can read the [CORS W3C Recommendation](http://www.w3.org/TR/cors/), or
[this intro](http://code.google.com/p/html5security/wiki/CrossOriginRequestSecurity) from the
HTML 5 Security Guide.

Here's a sample request sent from a web browser application living on `http://example.com`:

	$ curl -i https://`yourdomain.com -H "Origin: http://example.com"
	HTTP/1.1 302 Found
	Access-Control-Allow-Origin: *
	Access-Control-Expose-Headers: ETag
	Access-Control-Allow-Credentials: true

This is what the CORS preflight request looks like:

	$ curl -i https://`yourdomain.com -H "Origin: http://example.com" -X OPTIONS
	HTTP/1.1 204 No Content
	Access-Control-Allow-Origin: *
	Access-Control-Allow-Headers: Authorization, Content-Type, If-Match, If-Modified-Since, If-None-Match, If-Unmodified-Since
	Access-Control-Allow-Methods: GET, POST, PATCH, PUT, DELETE
	Access-Control-Expose-Headers: ETag
	Access-Control-Max-Age: 86400
	Access-Control-Allow-Credentials: true
	-->
