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
  - v1.0.1

includes:
  - errors

search: true
---

# Introduction

> Production base URL:

```shell
https://[COMPANY].api.getslideapp.com/2
```

> Development base URL:

```shell
https://dev.[COMPANY].api.getslideapp.com/2
```

> Make sure to replace `[COMPANY]` with your Company Name and to use either of the above URLs as the `[BASE_URL]`.

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

Welcome to the public API documentation for Slide Link.

The Slide API is organized around REST. Our API has predictable, resource-oriented URLs, and uses HTTP response codes to indicate API errors. We use built-in HTTP features, like HTTP authentication and HTTP verbs, which are understood by off-the-shelf HTTP clients. JSON is returned by all API responses, including errors.

Requests made with test credentials never hit the banking networks and incur no cost.

If anything is missing or seems incorrect, please check the [GitHub issues](https://github.com/getslideapp/slate/issues) for existing known issues or create a new issue.

<aside class="notice">
You must replace <code>[COMPANY]</code> in all API Requests with your Company Name.
</aside>

## Endpoint Types

We service two different types of API endpoints.

Users: Requests that we receive directly from the client are essentially a user interacting with the service. Users will be able to view their own data or make transactions on their own behalf.

Company Admin Users: Requests by an administrator. The admin endpoints will allow Company Admin Users to retrieve data or perform transactions on behalf of users within their company.

## Authentication

> To check authorization, use this code:

```shell
curl "[BASE_URL]/user/" \
  -X GET \
  -H "Authorization: Token [TOKEN]"
```

> Make sure to replace `[TOKEN]` with your API token and `[BASE_URL]` with the desired base url.

Slide uses a token-based HTTP Authentication scheme.

Authenticate your account by including your token in API requests. Your API tokens carry many privileges, so be sure to keep them secure!

Once a user has logged in and received a token, each subsequent request should include the token in the HTTP Authorization header.

<!-- You can register a new Slide API key at our [developer portal](http://example.com/developers).You can manage your API keys in the Dashboard.  -->

All API requests must be made over HTTPS. Calls made over plain HTTP will fail. API requests without authentication will also fail.

Slide expects the API token to be included in the HTTP Authorization header in all API requests to the server:

`Authorization: Token [TOKEN]`

You can obtain a new Slide API token and Company Name by emailing us at info@getslideapp.com.

<aside class="notice">
You must replace <code>[TOKEN]</code> with your personal API token.
</aside>


### External User Authentication

> We will make a request using this format:

```shell
curl "[COMPANY_AUTHENTICATION_URL]" \
  -X GET \
  -H "Authorization: Token [TOKEN]"
```

> Where `[TOKEN]` is the authentication token received from the client.

> If authentication is successful, we expect a 200 response containing user data structured like this:

```json
{
    "data": {
        "identifier": "230899032-f09832409-23580913",
        "first_name": "Testy",
        "last_name": "McTester",
        "email": "testy@getslideapp.com",
        "mobile_number": "+27821111112",
    },
    "message": null,
    "status": "success"
}
```

> If authentication is unsuccessful, we expect a 4XX response like this:

```json
{
    "status": "error",
    "data": null,
    "message": "[YOUR_ERROR_MESSAGE]"
}
```

Either Slide can handle all the authentication for a company or the company can choose to handle their own user authentication. If Slide handles all the authentication, then Slide will authenticate all User API calls using the API token that the user receives when they log in.

If the company prefers to manage their own authentication, Users will not log in via Slide. In this case Slide will need to authenticate all User API calls with the company's server.

When a user makes an API call to Slide, Slide will check with the company's server as to whether the user should be authenticated or not.

The company will respond to Slide either authenticating the User or not.

# Users

All actions are performed by registered users. The user endpoints allow a user to view or alter their information.

## Get User

```shell
curl "[BASE_URL]/user/" \
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

#### HTTP Request

`GET /user/`


# Admin

Admin endpoints are a set of admin-only accessible endpoints that provide administrative functionality across a company.

## Users

The Admin user endpoints allows an admin user to view or alter user information the user's behalf.

### List Users

```shell
curl "[BASE_URL]/admin/users/" \
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

#### HTTP Request

`GET /admin/users/`

### Create User

```shell
curl "[BASE_URL]/admin/users/" \
  -X POST \
  -H "Authorization: Token [TOKEN]" \
  -d '{"first_name": "Joe",
       "last_name": "Shmoe",
       "mobile_number": "+27820000000",
       "email": "example@email.com"}'
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

This endpoint creates a user. When a new user is created they will by default be in the `user` group.

#### HTTP Request

`POST /admin/users/`

#### URL Parameters

Parameter | Description | Type | Required
--------- | ----------- | -----| --------
`first_name` | User's first name | String | Yes
`last_name` | User's last name | String | Yes
`mobile_number` | User's mobile number | String | Yes
`email` | User's email address | String | Yes
`groups` | User's group (Options are: `user` or `admin_user`) | String | No

<aside class="notice">
Formatting of `mobile_number` is such that it should be exactly 11 characters in length (9 integers prepended by `+27`).
</aside>

### Get User

```shell
curl "[BASE_URL]/admin/users/[IDENTIFIER]/" \
  -X GET \
  -H "Authorization: Token [TOKEN]"
```
> Make sure to replace `[IDENTIFIER]` with the User Id.

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

#### HTTP Request

`GET /admin/users/[IDENTIFIER]/`


### Update User

```shell
curl "[BASE_URL]/admin/users/[IDENTIFIER]/" \
  -X PUT \
  -H "Authorization: Token [TOKEN]" \
  -d '{"first_name": "Joe",
       "last_name": "Shmoe",
       "mobile_number": "+27820000000",
       "email": "example@email.com"}'
```
> Make sure to replace `[IDENTIFIER]` with the User Id.

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

#### HTTP Request

`PUT /admin/users/[IDENTIFIER]/`

#### URL Parameters

Parameter | Description | Type | Required
--------- | ----------- | ---- | --------
`first_name` | User's first name | String | No
`last_name` | User's last name | String | No
`mobile_number` | User's mobile number | String | No
`email` | User's email address | String | No

<aside class="notice">
Formatting of `mobile_number` is such that it should be exactly 11 characters in length (9 integers prepended by `+27`).
</aside>
