{your_error_message}---
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
https://{company}.api.getslideapp.com/2
```

> Development base URL:

```shell
https://dev.{company}.api.getslideapp.com/2
```

> Make sure to replace `{company}` with your Company Name and to use either of the above URLs as the `{base_url}`.

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
You must replace <code>{company}</code> in all API Requests with your Company Name.
</aside>

## Endpoint Types

We service two different types of API endpoints.

Users: Requests that we receive directly from the client are essentially a user interacting with the service. Users will be able to view their own data or make transactions on their own behalf.

Company Admin Users: Requests by an administrator. The admin endpoints will allow Company Admin Users to retrieve data or perform transactions on behalf of users within their company.

## Authentication

> To check authorization, use this code:

```shell
curl "{base_url}/user/" \
  -X GET \
  -H "Authorization: Token {token}"
```

> Make sure to replace `{token}` with your API token and `{base_url}` with the desired base url.

Slide uses a token-based HTTP Authentication scheme.

Authenticate your account by including your token in API requests. Your API tokens carry many privileges, so be sure to keep them secure!

Once a user has logged in and received a token, each subsequent request should include the token in the HTTP Authorization header.

<!-- You can register a new Slide API key at our [developer portal](http://example.com/developers).You can manage your API keys in the Dashboard.  -->

All API requests must be made over HTTPS. Calls made over plain HTTP will fail. API requests without authentication will also fail.

Slide expects the API token to be included in the HTTP Authorization header in all API requests to the server:

`Authorization: Token {token}`

You can obtain a new Slide API token and Company Name by emailing us at info@getslideapp.com.

<aside class="notice">
You must replace <code>{token}</code> with your personal API token.
</aside>


### External User Authentication

> We will make a request using this format:

```shell
curl "{company_authentication_url}" \
  -X GET \
  -H "Authorization: Token {token}"
```

> Where `{token}` is the authentication token received from the client.

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
    "message": "{your_error_message}"
}
```

Either Slide can handle all the authentication for a company or the company can choose to handle their own user authentication. If Slide handles all the authentication, then Slide will authenticate all User API calls using the API token that the user receives when they log in.

If the company prefers to manage their own authentication, Users will not log in via Slide. In this case Slide will need to authenticate all User API calls with the company's server.

When a user makes an API call to Slide, Slide will check with the company's server as to whether the user should be authenticated or not.

The company will respond to Slide either authenticating the User or not.

# Users

All actions are performed by registered users. The user endpoints allow a user to view or alter their information.

## User

For user management of their own user profiles.

### Get User

```shell
curl "{base_url}/user/" \
  -X GET \
  -H "Authorization: Token {token}"
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


## Bank Accounts

For user management of their own bank accounts.

### List Bank Accounts

```shell
curl "{base_url}/user/bank-accounts/" \
  -X GET \
  -H "Authorization: Token {token}"
```

> The above command returns JSON structured like this:

```json
{
    "data": {
        "name": "Mr T McTester",
        "account_number": "000000000",
        "type": "cheque",
        "bank_name": "nedbank",
        "branch_code": "198765",
        "primary": true,
        "created": "2018-08-21T09:27:25.882898Z",
        "updated": "2018-08-29T16:58:54.647283Z"
    },
    "message": null,
    "status": "success"
}
```

This endpoint retrieves a list of all the bank accounts for the logged in user.

#### HTTP Request

`GET /user/bank-accounts/`

### Get Bank Account

```shell
curl "{base_url}/user/bank-accounts/{id}/" \
  -X GET \
  -H "Authorization: Token {token}"
```

> The above command returns JSON structured like this:

```json
{
    "data": {
        "name": "Mr T McTester",
        "account_number": "000000000",
        "type": "cheque",
        "bank_name": "nedbank",
        "branch_code": "198765",
        "primary": true,
        "created": "2018-08-21T09:27:25.882898Z",
        "updated": "2018-08-29T16:58:54.647283Z"
    },
    "message": null,
    "status": "success"
}
```

This endpoint retrieves the bank account with `id = {id}` if the logged in user is the resource owner, otherwise it returns a `404 Not Found`.

#### HTTP Request

`GET /user/bank-accounts/{id}/`


### Create Bank Account

```shell
curl "{base_url}/user/bank-accounts/" \
  -X POST \
  -H "Authorization: Token {token}" \
  -d '{ "name": "Mr T. McTester",
        "account_number": "00000000",
        "type": "savings",
        "bank_name": "nedbank"}'
```

> The above command returns JSON structured like this:

```json
{
  "status": "success",
  "message": "created",
  "data": {
      "id": 4,
      "primary": true,
      "name": "Mr T. McTester",
      "account_number": "00000000",
      "type": "savings",
      "bank_name": "nedbank",
      "branch_code": "198765",
      "created": "2018-09-14T13:19:00.549216Z",
      "updated": "2018-09-14T13:19:00.549257Z"
  }
}
```

This endpoint creates a bank account for the logged in user. When a new bank account is created it will by default be set as the primary account for that user.

<aside class="notice">
The bank `branch_code` will be set for each bank account based on the chosen bank's universal branch code.
</aside>

#### HTTP Request

`POST /user/bank-accounts/`

#### URL Parameters

Parameter | Description | Type | Required
--------- | ----------- | -----| --------
`name` | Account name | String | Yes
`account_number` | Account number | String | Yes
`type` | Account type (Options are: `cheque` or `savings`) | String | Yes
`bank_name` | Account bank (Options are: `standard_bank`, `absa`, `fnb`, `nedbank`, `capitec`, `african_bank`, `investec`) | String | Yes

### Update Bank Account

```shell
curl "{base_url}/user/bank-accounts/{id}/" \
  -X PUT \
  -H "Authorization: Token {token}" \
  -d '{ "name": "Mr T. McTester",
        "account_number": "00000000",
        "type": "savings",
        "bank_name": "nedbank",
        "primary": true,
      }'
```

> The above command returns JSON structured like this:

```json
{
  "status": "success",
  "message": "created",
  "data": {
      "id": 4,
      "primary": true,
      "name": "Mr T. McTester",
      "account_number": "00000000",
      "type": "savings",
      "bank_name": "nedbank",
      "bank_code": "198765",
      "branch_code": "198765",
      "created": "2018-09-14T13:19:00.549216Z",
      "updated": "2018-09-14T13:19:00.549257Z"
  }
}
```

This endpoint updates the bank account with `id = {id}`, for the logged in user.
It can be used to set a secondary Bank Account back to 'primary'.

#### HTTP Request

`PUT /user/bank-accounts/{id}/`

#### URL Parameters

Parameter | Description | Type | Required
--------- | ----------- | -----| --------
`name` | Account name | String | No
`account_number` | Account number | String | No
`type` | Account type (Options are: `cheque` or `savings`) | String | No
`bank_name` | Account bank (Options are: `standard_bank`, `absa`, `fnb`, `nedbank`, `capitec`, `african_bank`, `investec`) | String | No
`primary` | Sets the account as the primary account | String | No


# Admin

Admin endpoints are a set of admin-only accessible endpoints that provide administrative functionality across a company.

## Users

The Admin user endpoints allows an admin user to view or alter user information the user's behalf.

### List Users

```shell
curl "{base_url}/admin/users/" \
  -X GET \
  -H "Authorization: Token {token}"
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


### Get User

```shell
curl "{base_url}/admin/users/{identifier}/" \
  -X GET \
  -H "Authorization: Token {token}"
```
> Make sure to replace `{identifier}` with the User Id.

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

`GET /admin/users/{identifier}/`


### Create User

```shell
curl "{base_url}/admin/users/" \
  -X POST \
  -H "Authorization: Token {token}" \
  -d '{"first_name": "Testy",
       "last_name": "McTester",
       "mobile_number": "+27820000000",
       "email": "testy@getslideapp.com"}'
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

### Update User

```shell
curl "{base_url}/admin/users/{identifier}/" \
  -X PUT \
  -H "Authorization: Token {token}" \
  -d '{"first_name": "Testy",
       "last_name": "McTester",
       "mobile_number": "+27821111112",
       "email": "testy@getslideapp.com"}'
```
> Make sure to replace `{identifier}` with the User Id.

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

`PUT /admin/users/{identifier}/`

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
