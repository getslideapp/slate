---
title: Slide API Documentation

language_tabs: # must be one of https://git.io/vQNgJ
  - shell : cURL
#  - ruby
#  - python
#  - javascript

toc_footers:
  - <a href='https://dashboard.getslideapp.com'>Production Slide Dashboard</a>
  - <a href='https://dev.dashboard.getslideapp.com'>Development Slide Dashboard</a>

includes:
  - errors

search: true
---

# Introduction

> Production API Endpoint:

```shell
https://[COMPANY].api.getslideapp.com/2/
```

> Development API Endpoint:

```shell
https://dev.[COMPANY].api.getslideapp.com/2/
```

> Make sure to replace `[COMPANY]` with your Company Id.

> The format of a standard success response is structured like this:

```json
{
    "status": "success",
    "data": {
        /* Application-specific data would go here. */
    },
    "message": null /* Or optional success message */
}
```

> The format of a standard error response is structured like this:

```json
{
    "status": "error",
    "data": null /* or optional error payload */,
    "message": "Error xyz has occurred"
}
```

The Slide API is organized around REST. Our API has predictable, resource-oriented URLs, and uses HTTP response codes to indicate API errors. We use built-in HTTP features, like HTTP authentication and HTTP verbs, which are understood by off-the-shelf HTTP clients. JSON is returned by all API responses, including errors.

Requests made with test credentials never hit the banking networks and incur no cost.

<aside class="notice">
You must replace <code>[COMPANY]</code> in all API Requests with your Company Id.
</aside>


# Authentication

> To authorize, use this code:

```shell
curl "https://[COMPANY].api.getslideapp.com/2" \
  -H "Authorization: [TOKEN]"
```

> Make sure to replace `[TOKEN]` with your API token.

Slide uses a token-based HTTP Authentication scheme.

Authenticate your account by including your token in API requests. Your API tokens carry many privileges, so be sure to keep them secure!

Once a user has logged in and received a token, each subsequent request should include the token in the HTTP Authorization header.

<!-- You can register a new Slide API key at our [developer portal](http://example.com/developers).You can manage your API keys in the Dashboard.  -->

All API requests must be made over HTTPS. Calls made over plain HTTP will fail. API requests without authentication will also fail.

Slide expects the API token to be included in the HTTP Authorization header in all API requests to the server:

`Authorization: Token [TOKEN]`

You can obtain a new Slide API key by emailing us at info@getslideapp.com.

<aside class="notice">
You must replace <code>[TOKEN]</code> with your personal API token.
</aside>

# Users

## Get User

```shell
curl "https://[COMPANY].api.getslideapp.com/2/user/" \
  -X GET \
  -H "Authorization: Token [TOKEN]"
```
> The above command returns JSON structured like this:

```json
{
    "data": {
        "identifier": "230899032-f09832409-23580913",
        "first_name": "Testy",
        "last_name": "McTester",
        "email": "testy@getslideapp.com",
        "mobile_number": "+27821111112",
        "company": "slide",
        "group": "user",
        "created": "2018-08-21T09:27:25.882898Z",
        "updated": "2018-08-29T16:58:54.647283Z"
    },
    "message": null,
    "status": "success"
}
```

This endpoint retrieves the profile details for the logged in user.

### HTTP Request

`GET https://[COMPANY].api.getslideapp.com/2/user/`

### Query Parameters

None

# Admin

Admin endpoints are a set of admin-only accessible endpoints that provide administrative functionality across a company.

## List Users

```shell
curl "https://[COMPANY].api.getslideapp.com/2/admin/users/" \
  -X GET \
  -H "Authorization: Token [TOKEN]"
```

> The above command returns JSON structured like this:

```json
{
    "data": {
        "identifier": "230899032-f09832409-23580913",
        "first_name": "Testy",
        "last_name": "McTester",
        "email": "testy@getslideapp.com",
        "mobile_number": "+27821111112",
        "company": "slide",
        "group": "user",
        "created": "2018-08-21T09:27:25.882898Z",
        "updated": "2018-08-29T16:58:54.647283Z"
    },
    "message": null,
    "status": "success"
}
```

This endpoint retrieves a list of all users.

### HTTP Request

`GET https://[COMPANY].api.getslideapp.com/2/admin/users/`

### URL Parameters

None

## Create User

```shell
curl "https://[COMPANY].api.getslideapp.com/2/admin/users/" \
  -X POST \
  -H "Authorization: Token [TOKEN]"
```

> The above command returns JSON structured like this:

```json
{
    "data": {
        "identifier": "230899032-f09832409-23580913",
        "first_name": "Testy",
        "last_name": "McTester",
        "email": "testy@getslideapp.com",
        "mobile_number": "+27821111112",
        "company": "slide",
        "group": "user",
        "created": "2018-08-21T09:27:25.882898Z",
        "updated": "2018-08-29T16:58:54.647283Z"
    },
    "message": null,
    "status": "success"
}
```

This endpoint creates a user.

### HTTP Request

`POST https://[COMPANY].api.getslideapp.com/2/admin/users/`

### URL Parameters

Parameter | Description | Default | Required
--------- | ----------- | ------- | --------
first_name | User's first name | null | true
last_name | User's last name | null | true
mobile_number | User's mobile number | null | true
email | User's email address | null | true

## Get User

```shell
curl \
"https://[COMPANY].api.getslideapp.com/2/admin/users/{identifier}/" \
  -X GET \
  -H "Authorization: Token [TOKEN]"
```

> The above command returns JSON structured like this:

```json
{
    "data": {
        "identifier": "230899032-f09832409-23580913",
        "first_name": "Testy",
        "last_name": "McTester",
        "email": "testy@getslideapp.com",
        "mobile_number": "+27821111112",
        "company": "slide",
        "group": "user",
        "created": "2018-08-21T09:27:25.882898Z",
        "updated": "2018-08-29T16:58:54.647283Z"
    },
    "message": null,
    "status": "success"
}
```

This endpoint retrieves a user.

### HTTP Request

`GET https://[COMPANY].api.getslideapp.com/2/admin/users/{identifier}/`

### URL Parameters

None

## Update User

```shell
curl \
"https://[COMPANY].api.getslideapp.com/2/admin/users/{identifier}/" \
  -X PUT \
  -H "Authorization: Token [TOKEN]"
```

> The above command returns JSON structured like this:

```json
{
    "data": {
        "identifier": "230899032-f09832409-23580913",
        "first_name": "Testy",
        "last_name": "McTester",
        "email": "testy@getslideapp.com",
        "mobile_number": "+27821111112",
        "company": "slide",
        "group": "user",
        "created": "2018-08-21T09:27:25.882898Z",
        "updated": "2018-08-29T16:58:54.647283Z"
    },
    "message": null,
    "status": "success"
}
```

This endpoint updates an existing user.

### HTTP Request

`PUT https://[COMPANY].api.getslideapp.com/2/admin/users/{identifier}/`

### URL Parameters

Parameter | Description | Default | Required
--------- | ----------- | ------- | --------
first_name | User's first name | null | false
last_name | User's last name | null | false
mobile_number | User's mobile number | null | false
email | User's email address | null | false
