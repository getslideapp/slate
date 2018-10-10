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
    "data": [
      {
        "name": "Mr T McTester",
        "account_number": "000000000",
        "type": "cheque",
        "bank_name": "nedbank",
        "branch_code": "198765",
        "primary": true,
        "created": "2018-08-21T09:27:25.882898Z",
        "updated": "2018-08-29T16:58:54.647283Z"
      }
    ],
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


## Cards

For user management of their own credit cards.

An authenticated user can add, verify, view, remove and set an added card as primary (the card used for payments). Users can then use registered cards to make payments.

For the case of a registered card which has not been verified, verification (via 3DSecure) will be required on a per payment basis. Cards can also be verified as a once off using the `verify` endpoint, to be used to make future payments without 3DS verification. See [Verify Card](#verify-card) for more information.

A card can have one of three verification statuses as listed below:

Status | Description
--------- | -----------
`pending` | The card has not yet been verified.
`failed` | The card verification process failed.
`verified` | The card has been successfully verified.

### List Cards

```shell
curl "{base_url}/user/cards/" \
  -X GET \
  -H "Authorization: Token {token}"
```

> The above command returns JSON structured like this:

```json
{
    "data": [
        {
            "card_holder": "T McTester",
            "primary": true,
            "card_type": "mastercard",
            "verification_status": "pending",
            "last_four_digits": "9013",
            "expiry_year": "2018",
            "expiry_month": "10",
            "id": 21
        },
        {
            "card_holder": "T McTester",
            "primary": false,
            "card_type": "mastercard",
            "verification_status": "pending",
            "last_four_digits": "3018",
            "expiry_year": "2018",
            "expiry_month": "10",
            "id": 17
        }
    ],
    "message": null,
    "status": "success"
}
```

This endpoint retrieves a list of all the cards registered for the logged in user.

#### HTTP Request

`GET /user/cards/`

### Get Card

```shell
curl "{base_url}/user/cardss/{id}/" \
  -X GET \
  -H "Authorization: Token {token}"
```

> The above command returns JSON structured like this:

```json
{
    "data": {
          "card_holder": "T McTester",
          "primary": false,
          "card_type": "mastercard",
          "verification_status": "pending",
          "last_four_digits": "3018",
          "expiry_year": "2018",
          "expiry_month": "10",
          "id": 17
      },
    "message": null,
    "status": "success"
}
```

This endpoint retrieves the Card with `id = {id}` if the logged in user is the resource owner, otherwise it returns a `404 Not Found`.

#### HTTP Request

`GET /user/cards/{id}/`


### Register Card

```shell
curl "{base_url}/user/cards/" \
  -X POST \
  -H "Authorization: Token {token}" \
  -d '{ "number": "1111222233334444",
        "card_holder": "Mr T. McTester",
        "expiry_year": "2018",
        "expiry_month": "12",
        "cvv": "123",
      }'
```

> The above command returns JSON structured like this:

```json
{
  "data": {
        "card_type": "visa",
        "card_holder": "T McTester",
        "primary": true,
        "verification_status": "pending",
        "last_four_digits": "4444",
        "expiry_year": "2018",
        "expiry_month": "12",
        "id": 17
    },
  "message": "created",
  "status": "success"
}
```

This endpoint adds a card for the logged in user. When a new card is successfully registered it will by default be set as the primary card for that user.

<aside class="notice">
The card type will be set for each bank account based on the chosen bank's card. So the input of a card type is not necessary. Please also note that only valid Visa and Mastercard credit card numbers will be accepted.
</aside>

#### HTTP Request

`POST /user/cards/`

#### URL Parameters

Parameter | Description | Type | Required
--------- | ----------- | -----| --------
`number` | Card number | String (16 digit) | Yes
`cvv` | CVV number | String (3 digit) | Yes
`card_holder` | Card holder name | String | Yes
`expiry_year` | Year of expiry date | String (4 digit) | Yes
`expiry_month` | Month of expiry date | String (2 digit) | Yes

### Unregister Card

```shell
curl "{base_url}/user/cards/{id}/" \
  -X DEL \
  -H "Authorization: Token {token}"
```

> The above command returns JSON structured like this:

```json
{
  "status": "success",
  "message": "Card successfully removed",
  "data": null
}
```

This endpoint will de-register the card with `id = {id}`, if the logged in user is the resource owner, otherwise it returns a `404 Not Found`.


#### HTTP Request

`DEL /user/cards/{id}/`


### Verify Card

```shell
curl "{base_url}/user/cards/{id}/verify/" \
  -X POST \
  -H "Authorization: Token {token}"
```

> The above command returns JSON structured like this, for a successful response on a card which requires 3DS verification:

```json
{
  "data": {
      "threeDSecure": {
          "transactionId": "mrQSPgcMvdWksnewrf342y1RfLEU=",
          "paReq": "eJxVUttSwjAU/JUOH9AkrVDKHDID4igqTgUdn2N6gA6QlqTl4teblFbwbTd7rnsCH2uNOFmgrDRymKExYoVelg47UURpTIsOh2Q0xz2HA2qT5Yozn/oBkJbaJC3XQpUchNyPp2+cMQakwbBDPZ1wFoRALhCU2CEfC7UxqA+jpc6k8NoaQGoVZF6pUp95xCiQlkClt3xdloUZEHI8Hv3vpogvc/9HAHE6kOs8SeWQsfVOWcp3+n2Rrconiroc2994hjsvXx8+h0BcBKSiRB5Q1meUMo/1Bt1wEHaB1O8gdm4QbjW73gVD4VqMboTbB7CualSy3aNlgKciV2gjrI9/GFI0kifi/Igqq8zAW2yzFEdFEYRxt9vvURaz2NpYhwG5Lnf/5KyXpTU3CuOQ3tX219x1yqx1bqO6lSNAXAZpzkqao1v07zP8Ag+Os/4=",
          "acsUrl": "https://acsabsatest.bankserv.co.za/mdpayacs/pareq"
      },
      "reference": "20886b2d-4cd5-4fa2-8a42-a02653abfa01",
      "card_id": 26,
      "confirmation_url": "{base_url}/2/user/card/confirm3ds/"
  },
  "message": "3D Secure authentication required",
  "status": "success"
}
```

> The above command returns JSON structured like this, for a successful response on a card which does NOT require 3DS verification:

```json
{
  "data": {
      "card_type": "mastercard",
      "card_holder": "Mr Testy McTester",
      "primary": true,
      "verification_status": "verified",
      "last_four_digits": "9013",
      "expiry_year": "2018",
      "expiry_month": "10",
      "id": 26
    },
  "message": "Approved",
  "status": "success"
}
```

This endpoint will initiate a 3DSecure verification for the card with `id = {id}`, if the logged in user is the resource owner, otherwise it returns a `404 Not Found`. If the card does not require 3DSecure verification the card will be verified.

This `verification` endpoint enables a card on the system to be verified once off and then used to make payments without further verification, for better end user experience. This setting can be used at the discretion of the client who may or may not pass it on to the user. Unverified cards attempting payments will require 3DSecure verification on a per payment basis for the payment to be processed.

There are two `success` cases for the response, depending on whether the card requires 3DSecure verification or not. In the case where 3DSecure is not required the card is verified automatically and the response returns as per example given.

In the case where 3DSecure is required, however, the client will be required to submit a form with the fields populated from the response provided. An example of this form can be found below in the [Verify Form](#verify-form) Section. We'd be happy to guide you through thr final step.

#### HTTP Request

`POST /user/cards/{id}/verify/`

#### Verify Form

Upon receiving the response from the [Verify Card endpoint](#verify-card), the values should be used to populate the Verify Form in the client, which when submitted, will allow the user to enter their 3DSecure details.

Once the 3DSecure form has been submitted by the user, they will be redirected to the `TermUrl`. To confirm the `verification_status` of the card, you can make a call to the [Get Card](#get-card) endpoint.

<aside class="notice">
Note that all all the variables are mapped directly to the response, except for `TermUrl`, which should take the value of `confirmation_url` from the response.
</aside>

> The html form that needs to be submitted by the client in order for the client to be able to complete the 3DSecure process.

```html
<form method="post" target="iframe" id="threedsForm" action="{{ acsUrl }}">
    <input type=hidden name="PaReq" value="{{ paReq }}">
    <input type=hidden name="TermUrl"
           value="{{ termUrl }}">
    <input type=hidden name="MD" value="{{ transactionId }}">
</form>
```

### Set Primary Card

```shell
curl "{base_url}/user/cards/{id}/set_primary/" \
  -X POST \
  -H "Authorization: Token {token}"
```

> The above command returns JSON structured like this:

```json
{
  "data": {
      "card_type": "mastercard",
      "card_holder": "Mr Testy McTester",
      "primary": true,
      "verification_status": "pending",
      "last_four_digits": "9013",
      "expiry_year": "2018",
      "expiry_month": "10",
      "id": 26
  },
  "message": null,
  "status": "success"
}
```

This endpoint will set the card with `id = {id}` as the primary card for the user (the card used to make payments), if the logged in user is the resource owner, otherwise it returns a `404 Not Found`.


#### HTTP Request

`POST /user/cards/{id}/set_primary/`


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
