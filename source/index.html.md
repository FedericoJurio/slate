---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

#toc_footers:
#  - <a href='#'>Sign Up for a Developer Key</a>
#  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

search: true
---

# Interacting with the API

## Status codes

Code | Description
--------- | -----------
200 OK | Successful request
201 Created | New object saved
204 No content | Object deleted
400 Bad Request | Returns JSON with the error message
401 Unauthorized | Couldn't authenticate your request
403 Invalid scope | User hasn't authorized necessary scope
404 Not Found | No such object
422 Unprocessable Entity | Unable to process the contained instructions
500 Internal Server Error | Something went wrong
503 Service Unavailable | Your connection is being throttled or the service is down for maintenance

## Making requests
As per RESTful design patterns, Monrovia API implements following HTTP verbs

Code | Description
---- | -----------
GET | Read resources
POST | Create new resources
PUT | Modify existing resources
DELETE | Remove resources

When making requests, arguments can be passed as params, form data or JSON with correct Content-Type header.

## CORS
Monrovia API supports cross-origin HTTP requests which is commonly referred as [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS). This means that you can call API resources using Javascript from any browser. While this allows many interesting use cases, it’s important to remember that you should never expose private API keys to 3rd parties.

# Authentication

Monrovia uses API keys to allow access to the API.

## Generate access token

To authenticate a user you must provide the API credentials in the Authorization header and the user credentials in the payload. If the request succeed, the API will provide a Bearer token that expects to be included in all API requests to the server in a header that looks like `Authorization: Bearer <token>`

> Request

```shell
curl --request POST 'https://monrovia.herokuapp.com/auth/token' \
--header 'Content-Type: application/json' \
--header 'Authorization: Basic <credentials>' \
--data-raw '{
	"email": "me@organization.com",
	"password": "secret"
}'
```

> Response

```shell
{
    "access_token": "<access_token>",
    "refresh_token": "<refresh_token>"
}
```

### URL
POST https://monrovia.herokuapp.com/auth/token

### Arguments
Parameter |	Type |	Required |	Description
--------- | ---- | --------- | ------------
email | string | Required | User email address
password | string | Required | User password

### Attributes
Name |	Description
---- | ------------
access_token | Token that expires after 60 minutes.
refresh_token | Token that expires after 30 days.

### Response Codes
Code |	Description
---- | ------------
201 | Successful authentication.
400 | Missing data for required field.
400 | Invalid type.

## Refresh access token

> Request

```shell
curl --request POST 'https://monrovia.herokuapp.com/auth/refresh' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer <refresh_token>'
```

> Response

```shell
{
    "access_token": "<access_token>"
}
```

### URL
POST https://monrovia.herokuapp.com/auth/refresh

### Attributes
Name |	Description
---- | ------------
access_token | Token that expires after 60 minutes.

### Response Codes
Code |	Description
---- | ------------
201 | Successful refresh.
422 | Only access tokens are allowed.
422 | Not enough segments.
422 | Signature verification failed.

## Revoke access token

> Request

```shell
curl --request DELETE 'https://monrovia.herokuapp.com/auth/token' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer <access_token>'
```

> Response

```shell
{
    "message": "Access token revoked"
}
```

### URL
DELETE https://monrovia.herokuapp.com/auth/token

### Response Codes
Code |	Description
---- | ------------
200 | Access token revoked.
422 | Only access tokens are allowed.
422 | Not enough segments.
422 | Signature verification failed.

## Revoke refresh token

> Request

```shell
curl --request DELETE 'https://monrovia.herokuapp.com/auth/refresh' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer <refresh_token>'
```

> Response

```shell
{
    "message": "Refresh token revoked"
}
```

### URL
DELETE https://monrovia.herokuapp.com/auth/refresh

### Response Codes
Code |	Description
---- | ------------
200 | Refresh token revoked.
422 | Only access tokens are allowed.
422 | Not enough segments.
422 | Signature verification failed.
