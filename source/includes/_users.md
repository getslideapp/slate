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
      "results": [
        {
          "id": 4,
          "name": "Mr T McTester",
          "account_number": "000000000",
          "type": "cheque",
          "bank_name": "nedbank",
          "branch_code": "198765",
          "primary": true,
          "identifier": "395554a4-6d14-41f8-92f2-c23db3ab3e45",
          "created": "2018-08-21T09:27:25.882898Z",
          "updated": "2018-08-29T16:58:54.647283Z"
        }
      ],  
      "total": 1
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
curl "{base_url}/user/bank-accounts/{identifier}/" \
  -X GET \
  -H "Authorization: Token {token}"
```

> The above command returns JSON structured like this:

```json
{
    "data": {
        "id": 4,
        "name": "Mr T McTester",
        "account_number": "000000000",
        "type": "cheque",
        "bank_name": "nedbank",
        "branch_code": "198765",
        "primary": true,
        "identifier": "395554a4-6d14-41f8-92f2-c23db3ab3e45",
        "created": "2018-08-21T09:27:25.882898Z",
        "updated": "2018-08-29T16:58:54.647283Z"
    },
    "message": null,
    "status": "success"
}
```

This endpoint retrieves the bank account with `identifier = {identifier}` if the logged in user is the resource owner, otherwise it returns a `404 Not Found`.

Note that `id` can also be used in place of `identifier`.

#### HTTP Request

`GET /user/bank-accounts/{identifier}/`


### Create Bank Account

```shell
curl "{base_url}/user/bank-accounts/" \
  -X POST \
  -H "Content-Type: application/json" \
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
      "identifier": "395554a4-6d14-41f8-92f2-c23db3ab3e45",
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


### Delete Bank Account

```shell
curl "{base_url}/user/bank-accounts/{identifier}/" \
  -X DELETE \
  -H "Content-Type: application/json" \
  -H "Authorization: Token {token}"
```

> The above command returns JSON structured like this:

```json
{
  "status": "success",
  "message": "deleted"
}
```

This endpoint deletes the bank account with `identifier = {identifier}`, for the logged in user.
It will automatically set the most recent remaining bank account to the primary account, if it exists.

#### HTTP Request

`DELETE /user/bank-accounts/{identifier}/`

### Set Primary Bank Account

```shell
curl "{base_url}/user/bank-accounts/{identifier}/set_primary/" \
  -X POST \
  -H "Content-Type: application/json" \
  -H "Authorization: Token {token}"
```

> The above command returns JSON structured like this:

```json
{
  "status": "success",
  "message": null,
  "data": {
      "id": 4,
      "primary": true,
      "name": "Mr T. McTester",
      "account_number": "00000000",
      "type": "savings",
      "bank_name": "nedbank",
      "bank_code": "198765",
      "branch_code": "198765",
      "identifier": "395554a4-6d14-41f8-92f2-c23db3ab3e45",
      "created": "2018-09-14T13:19:00.549216Z",
      "updated": "2018-09-14T13:19:00.549257Z"
  }
}
```

This endpoint sets the bank account with `identifier = {identifier}` to primary, for the logged in user.
All other existing bank accounts for this user will have their `primary` status set to `false`.

Note that `id` can also be used in place of `identifier`.

#### HTTP Request

`POST /user/bank-accounts/{identifier}/set_primary/`



## Cards

For user management of their own credit cards.

An authenticated user can add, verify, view, remove and set an added card as primary (the card used for payments). Users can then use registered cards to make payments.

For the case of a registered card which has not been verified, verification (via 3DSecure) will be required on a per payment basis. Cards can also be verified as a once off using the `verify` endpoint, to be used to make future payments without 3DS verification. See [Verify Card](#verify-card) for more information.

A card can have one of four verification statuses as listed below:

Status | Description
--------- | -----------
`unverified` | Card verification has not been attempted.
`pending` | The card verification process has been initiated, but has not been completed.
`failed` | The card verification process failed.
`verified` | The card has been successfully verified.

### Test cards

These test cards are provided for testing and are liable to be changed or updated regularly. Please check here for the latest cards and please let us know if they are no longer working.

Number | Type | Expiry Date | CVV | 3DS Password
--------- | -------- | -------- | ----- | -----
5286 2500 0306 1012	| mastercard | 09/19 | 742 | test123

### List Cards

```shell
curl "{base_url}/user/cards/" \
  -X GET \
  -H "Authorization: Token {token}"
```

> The above command returns JSON structured like this:

```json
{
    "data": {
        "results": [
            {
              "card_holder": "T McTester",
              "primary": true,
              "card_type": "mastercard",
              "verification_status": "pending",
              "last_four_digits": "9013",
              "expiry_year": "2018",
              "expiry_month": "10",
              "identifier": "992c19df-c084-499d-8743-b9e70fee258b",
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
              "identifier": "91a7d739-075c-4896-8090-44a196a1c319",
              "id": 17
            }
          ],  
       "total": 2    
    },
    "message": null,
    "status": "success"
}
```

This endpoint retrieves a list of all the cards registered for the logged in user.

#### HTTP Request

`GET /user/cards/`

### Get Card

```shell
curl "{base_url}/user/cards/{identifier}/" \
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
          "identifier": "91a7d739-075c-4896-8090-44a196a1c319",
          "id": 17
      },
    "message": null,
    "status": "success"
}
```

This endpoint retrieves the Card with `identifier = {identifier}` if the logged in user is the resource owner, otherwise it returns a `404 Not Found`.

Note that `id` can also be used in place of `identifier`.

#### HTTP Request

`GET /user/cards/{identifier}/`


### Register Card

```shell
curl "{base_url}/user/cards/" \
  -X POST \
  -H "Content-Type: application/json" \
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
        "identifier": "91a7d739-075c-4896-8090-44a196a1c319",
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
curl "{base_url}/user/cards/{identifier}/" \
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

This endpoint will de-register the card with `identifier = {identifier}`, if the logged in user is the resource owner, otherwise it returns a `404 Not Found`.

Note that `id` can also be used in place of `identifier`.

#### HTTP Request

`DEL /user/cards/{identifier}/`


### Verify Card

```shell
curl "{base_url}/user/cards/{identifier}/verify/" \
  -X POST \
  -H "Content-Type: application/json" \
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
      "identifier": "992c19df-c084-499d-8743-b9e70fee258b",
      "id": 26
    },
  "message": "Approved",
  "status": "success"
}
```

This endpoint will initiate a 3DSecure verification for the card with `identifier = {identifier}`, if the logged in user is the resource owner, otherwise it returns a `404 Not Found`. If the card does not require 3DSecure verification the card will be verified.

Note that `id` can also be used in place of `identifier`.

This `verification` endpoint enables a card on the system to be verified once off and then used to make payments without further verification, for better end user experience. This setting can be used at the discretion of the client who may or may not pass it on to the user. Unverified cards attempting payments will require 3DSecure verification on a per payment basis for the payment to be processed.

There are two `success` cases for the response, depending on whether the card requires 3DSecure verification or not. In the case where 3DSecure is not required the card is verified automatically and the response returns as per example given.

In the case where 3DSecure is required, however, the client will be required to submit a form with the fields populated from the response provided. An example of this form can be found below in the [Verify Form](#verify-form) Section. We'd be happy to guide you through the final step.

#### HTTP Request

`POST /user/cards/{identifier}/verify/`

Note that `id` can also be used in place of `identifier`.

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
curl "{base_url}/user/cards/{identifier}/set_primary/" \
  -X POST \
  -H "Content-Type: application/json" \
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
      "identifier": "992c19df-c084-499d-8743-b9e70fee258b",
      "id": 26
  },
  "message": null,
  "status": "success"
}
```

This endpoint will set the card with `id = {identifier}` as the primary card for the user (the card used to make payments), if the logged in user is the resource owner, otherwise it returns a `404 Not Found`.

Note that `id` can also be used in place of `identifier`.

#### HTTP Request

`POST /user/cards/{identifier}/set_primary/`
