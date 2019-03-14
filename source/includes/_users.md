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
        "groups": "user",
        "type": null,
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

#### POST Parameters

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
5239 0900 0011 6015	| mastercard | 04/22 | 722 | test123
5286 2500 0002 5044	| mastercard | 08/20 | 188 | test123

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

#### POST Parameters

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


## Transactions

For user management of their transactions.

This endpoint returns transactions of any of the following types: `deposit`, `withdrawal` and `transfer`.

### List Transactions

```shell
curl "{base_url}/user/transactions/" \
  -X GET \
  -H "Authorization: Token {token}" \
  -d '{
       "page": 1,
       "page_size": 50
     }'
```

> The above command returns JSON structured like this:

```json
{
  "data": {
        "count": 55,
        "next": "{base_url}/user/transactions/?page=2?page_size=50",
        "previous": null,
        "results": [
            {
                "user": {
                    "identifier": "652f3dd5-d00f-48d2-9c37-0779d55bee37",
                    "first_name": "Testy",
                    "last_name": "McTester",
                    "email": "testy@getslideapp.com",
                    "mobile_number": "+27811223321",
                    "company": "slide_dev",
                    "groups": "user",
                    "type": null,
                    "status": "active",
                    "created": "2018-12-04T11:14:51.924388Z",
                    "updated": "2019-01-17T10:06:46.021888Z"
                },
                "recipient": {
                    "identifier": "9fa066cd-cd6c-47db-a8de-ff64942343df",
                    "first_name": "Testy 2",
                    "last_name": "McTester 2",
                    "email": "testy2@getslideapp.com",
                    "mobile_number": "+27811223325",
                    "company": "slide_dev",
                    "groups": "user",
                    "type": null,
                    "status": "active",
                    "created": "2018-12-04T11:17:37.522889Z",
                    "updated": "2018-12-13T13:12:55.896326Z"
                },
                "identifier": "217b5bd1-f28b-4869-a922-ed0c31c2ae61",
                "total_amount_charged": 100,
                "amount": 99,
                "currency": "ZAR",
                "created": "2019-01-11T08:40:45.514764Z",
                "updated": "2019-01-17T09:36:17.609090Z",
                "status": "Complete",
                "total_amount_received": 99,
                "note": null,
                "charges": [
                    {
                        "type": "user",
                        "identifier": "652f3dd5-d00f-48d2-9c37-0779d55bee37",
                        "amount": 1,
                        "flat_fee_charge": 1,
                        "percentage_rate_charge": 0,
                        "currency": "ZAR",
                        "created": "2019-01-11T08:40:45.514764Z",
                        "updated": "2019-01-11T08:45:20.876294Z",
                        "status": "Complete"
                    }
                  ],
                "type": "transfer"
            },
      ],
    }
}
```

This endpoint retrieves a list of all transactions of any of the following types: `deposit`, `withdrawal` and `transfer`.
The type of each transaction is specified in the `type` field of the object.

#### HTTP Request

`GET /user/transactions/`

#### POST Parameters
This endpoint supports pagination, with the following parameters available for use:

Parameter | Description | Type | Required
--------- | ----------- | -----| --------
`page` | Page to retrieve | Integer | No
`page_size` | Amount of results per page | Integer | No


<!-- ### List Deposits
```shell
curl "{base_url}/user/deposits/" \
  -X GET \
  -H "Authorization: Token {token}"
```
> The above command returns JSON structured like this:

```json
{
    "status": "success",
    "message": null,
    "data": [
        {
            "identifier": "68796fab-7edb-413c-9881-55e1101d9777",
            "user": {
                "identifier": "c1e21425-66c5-44d0-bc14-5352301fb7f0",
                "first_name": "Testy",
                "last_name": "McTester",
                "email": "testy@getslideapp.com",
                "mobile_number": "+27821111112",
                "company": "slide_dev",
                "groups": "user",
                "type": null,
                "status": "active",
                "created": "2018-09-30T15:53:37.012334Z",
                "updated": "2018-10-31T09:48:38.299689Z"
            },
            "amount": 40,
            "total_amount_charged": 49,
            "currency": "ZAR",
            "created": "2018-10-31T14:20:47.628274Z",
            "updated": "2018-10-31T14:21:24.875560Z",
            "status": "Complete",
            "charges": [
                {
                    "type": "user",
                    "identifier": "729c086e-1dab-4f39-9679-1cdaee6f5c06",
                    "amount": 9,
                    "flat_fee_charge": 5,
                    "percentage_rate_charge": 4,
                    "currency": "ZAR",
                    "created": "2018-10-31T14:20:48.095841Z",
                    "updated": "2018-10-31T14:21:38.732972Z",
                    "status": "Complete"
                }
            ]
        }
      ]  
    }
```
This endpoint retrieves a list of all the user's deposits.
#### HTTP Request
`GET /user/deposits/` -->

### List Transfers

```shell
curl "{base_url}/user/transfers/" \
  -X GET \
  -H "Authorization: Token {token}"
```

> The above command returns JSON structured like this:

```json
{
    "status": "success",
    "message": null,
    "data": [
        {
            "identifier": "a56b9f50-81b7-4de6-8111-550686b997f9",
            "user": {
                "identifier": "c1e21425-66c5-44d0-bc14-5352301fb7f0",
                "first_name": "Testy",
                "last_name": "McTester",
                "email": "testy@getslideapp.com",
                "mobile_number": "+27821111112",
                "company": "slide_dev",
                "groups": "user",
                "type": null,
                "status": "active",
                "created": "2018-09-30T15:53:37.012334Z",
                "updated": "2018-10-31T09:48:38.299689Z"
            },
            "recipient": {
                "identifier": "899b55e1-65a9-4969-b282-b8f0ok01f76b",
                "first_name": "Ray",
                "last_name": "Serpent",
                "email": "ray@getslideapp.com",
                "mobile_number": null,
                "company": "slide",
                "groups": "user",
                "type": null,
                "status": "active",
                "created": "2018-07-24T10:50:42.364399Z",
                "updated": "2018-07-24T10:50:48.329165Z"
            },
            "note": "testy",
            "amount": 50,
            "total_amount_charged": 65,
            "total_amount_received": 45,
            "currency": "ZAR",
            "created": "2018-10-31T13:55:34.418860Z",
            "updated": "2018-10-31T13:56:01.651581Z",
            "status": "Complete",
            "charges": [
                {
                    "type": "recipient",
                    "identifier": "b16e0f99-7ae2-46fe-b375-194a90a0f23c",
                    "amount": 5,
                    "flat_fee_charge": 0,
                    "percentage_rate_charge": 5,
                    "currency": "ZAR",
                    "created": "2018-10-31T13:55:34.530633Z",
                    "updated": "2018-10-31T13:56:07.719340Z",
                    "status": "Complete"
                },
                {
                    "type": "user",
                    "identifier": "0a56cea7-9d57-4fe8-8f8d-8e22aeb8174f",
                    "amount": 15,
                    "flat_fee_charge": 10,
                    "percentage_rate_charge": 5,
                    "currency": "ZAR",
                    "created": "2018-10-31T13:55:34.497813Z",
                    "updated": "2018-10-31T13:56:19.209375Z",
                    "status": "Complete"
                }
            ]
        }
      ]
    }
```

This endpoint retrieves a list of all the user's transfers.

#### HTTP Request

`GET /user/transfers/`

### List Withdrawals

```shell
curl "{base_url}/user/withdrawals/" \
  -X GET \
  -H "Authorization: Token {token}"
```

> The above command returns JSON structured like this:

```json
{
    "status": "success",
    "message": null,
    "data": [
        {
            "identifier": "90123aaf-cb6c-4435-aa78-c093de616d84",
            "user": {
                "identifier": "c1e21425-66c5-44d0-bc14-5352301fb7f0",
                "first_name": "Testy",
                "last_name": "McTester",
                "email": "testy@getslideapp.com",
                "mobile_number": "+27821111112",
                "company": "slide_dev",
                "groups": "user",
                "type": null,
                "status": "active",
                "created": "2018-09-30T15:53:37.012334Z",
                "updated": "2018-10-31T09:48:38.299689Z"
            },
            "amount": 100,
            "total_amount_charged": 115,
            "currency": "ZAR",
            "created": "2018-10-31T14:08:08.021608Z",
            "updated": "2018-10-31T14:08:50.753947Z",
            "status": "Complete",
            "charges": [
                {
                    "type": "user",
                    "identifier": "9947b78a-a7d0-4da6-a7ce-f2a447a732a0",
                    "amount": 15,
                    "flat_fee_charge": 5,
                    "percentage_rate_charge": 10,
                    "currency": "ZAR",
                    "created": "2018-10-31T14:08:08.119462Z",
                    "updated": "2018-10-31T14:09:02.457248Z",
                    "status": "Complete"
                }
            ]
        }
      ]
    }
```

This endpoint retrieves a list of all the user's withdrawals.

#### HTTP Request

`GET /user/withdrawals/`

<!-- ### Get Deposit
```shell
curl "{base_url}/admin/deposits/{identifier}/" \
  -X GET \
  -H "Authorization: Token {token}"
```
> The above command returns JSON structured like this:

```json
{
    "data": {
        "identifier": "68796fab-7edb-413c-9881-55e1101d9777",
        "user": {
            "identifier": "c1e21425-66c5-44d0-bc14-5352301fb7f0",
            "first_name": "Testy",
            "last_name": "McTester",
            "email": "testy@getslideapp.com",
            "mobile_number": "+27821111112",
            "company": "slide_dev",
            "groups": "user",
            "type": null,
            "status": "active",
            "created": "2018-09-30T15:53:37.012334Z",
            "updated": "2018-10-31T09:48:38.299689Z"
        },
        "amount": 40,
        "total_amount_charged": 49,
        "currency": "ZAR",
        "created": "2018-10-31T14:20:47.628274Z",
        "updated": "2018-10-31T14:21:24.875560Z",
        "status": "Complete",
        "charges": [
            {
                "type": "user",
                "identifier": "729c086e-1dab-4f39-9679-1cdaee6f5c06",
                "amount": 9,
                "flat_fee_charge": 5,
                "percentage_rate_charge": 4,
                "currency": "ZAR",
                "created": "2018-10-31T14:20:48.095841Z",
                "updated": "2018-10-31T14:21:38.732972Z",
                "status": "Complete"
            }
        ]
    },
    "message": null,
    "status": "success"
}
```
This endpoint retrieves the Deposit with `identifier = {identifier}` if the logged in user is the resource owner, otherwise it returns a `404 Not Found`.
#### HTTP Request
`GET /admin/deposits/{identifier}/` -->

### Get Transaction

```shell
curl "{base_url}/user/transactions/{identifier}/" \
  -X GET \
  -H "Authorization: Token {token}"
```

> The above command returns JSON structured like this:

```json
{
    "status": "success",
    "message": null,
    "data": {
        "user": {
            "identifier": "652f3dd5-d00f-48d2-9c37-0779d55bee37",
            "first_name": "Testy",
            "last_name": "McTester",
            "email": "testy@getslideapp.com",
            "mobile_number": "+27811223321",
            "company": "slide_dev",
            "groups": "user",
            "type": null,
            "status": "active",
            "created": "2018-12-04T11:14:51.924388Z",
            "updated": "2019-01-17T10:06:46.021888Z"
        },
        "recipient": {
            "identifier": "9fa066cd-cd6c-47db-a8de-ff64942343df",
            "first_name": "Testy 2",
            "last_name": "McTester 2",
            "email": "testy2@getslideapp.com",
            "mobile_number": "+27811223325",
            "company": "slide_dev",
            "groups": "user",
            "type": null,
            "status": "active",
            "created": "2018-12-04T11:17:37.522889Z",
            "updated": "2018-12-13T13:12:55.896326Z"
        },
        "identifier": "217b5bd1-f28b-4869-a922-ed0c31c2ae61",
        "total_amount_charged": 100,
        "amount": 99,
        "currency": "ZAR",
        "created": "2019-01-11T08:40:45.514764Z",
        "updated": "2019-01-17T09:36:17.609090Z",
        "status": "Complete",
        "total_amount_received": 99,
        "note": null,
        "charges": [
            {
                "type": "user",
                "identifier": "652f3dd5-d00f-48d2-9c37-0779d55bee37",
                "amount": 1,
                "flat_fee_charge": 1,
                "percentage_rate_charge": 0,
                "currency": "ZAR",
                "created": "2019-01-11T08:40:45.514764Z",
                "updated": "2019-01-11T08:45:20.876294Z",
                "status": "Complete"
            }
          ],
        "type": "transfer"
    }
}
```

This endpoint retrieves a specific transaction with `identifier = {identifier}` of any of the following types: `deposit`, `withdrawal` and `transfer`.
If the logged in user is not the resource owner, it returns a `404 Not Found`.
The type of the transaction is specified in the `type` field of the object.

#### HTTP Request

`GET /user/transactions/{identifier}/`

### Get Transfer

```shell
curl "{base_url}/user/transfers/{identifier}/" \
  -X GET \
  -H "Authorization: Token {token}"
```

> The above command returns JSON structured like this:

```json
{
    "data": {
        "identifier": "a56b9f50-81b7-4de6-8111-550686b997f9",
        "user": {
            "identifier": "c1e21425-66c5-44d0-bc14-5352301fb7f0",
            "first_name": "Testy",
            "last_name": "McTester",
            "email": "testy@getslideapp.com",
            "mobile_number": "+27821111112",
            "company": "slide_dev",
            "groups": "user",
            "type": null,
            "status": "active",
            "created": "2018-09-30T15:53:37.012334Z",
            "updated": "2018-10-31T09:48:38.299689Z"
        },
        "recipient": {
            "identifier": "899b55e1-65a9-4969-b282-b8f0ok01f76b",
            "first_name": "Ray",
            "last_name": "Serpent",
            "email": "ray@getslideapp.com",
            "mobile_number": null,
            "company": "slide",
            "groups": "user",
            "type": null,
            "status": "active",
            "created": "2018-07-24T10:50:42.364399Z",
            "updated": "2018-07-24T10:50:48.329165Z"
        },
        "note": "testy",
        "amount": 50,
        "total_amount_charged": 65,
        "total_amount_received": 45,
        "currency": "ZAR",
        "created": "2018-10-31T13:55:34.418860Z",
        "updated": "2018-10-31T13:56:01.651581Z",
        "status": "Complete",
        "charges": [
            {
                "type": "recipient",
                "identifier": "b16e0f99-7ae2-46fe-b375-194a90a0f23c",
                "amount": 5,
                "flat_fee_charge": 0,
                "percentage_rate_charge": 5,
                "currency": "ZAR",
                "created": "2018-10-31T13:55:34.530633Z",
                "updated": "2018-10-31T13:56:07.719340Z",
                "status": "Complete"
            },
            {
                "type": "user",
                "identifier": "0a56cea7-9d57-4fe8-8f8d-8e22aeb8174f",
                "amount": 15,
                "flat_fee_charge": 10,
                "percentage_rate_charge": 5,
                "currency": "ZAR",
                "created": "2018-10-31T13:55:34.497813Z",
                "updated": "2018-10-31T13:56:19.209375Z",
                "status": "Complete"
            }
        ]
    },
    "message": null,
    "status": "success"
}
```

This endpoint retrieves the Transfer with `identifier = {identifier}` if the logged in user is the resource owner, otherwise it returns a `404 Not Found`.

#### HTTP Request

`GET /admin/transfers/{identifier}/`

### Get Withdrawal

```shell
curl "{base_url}/user/withdrawals/{identifier}/" \
  -X GET \
  -H "Authorization: Token {token}"
```

> The above command returns JSON structured like this:

```json
{
    "data": {
        "identifier": "90123aaf-cb6c-4435-aa78-c093de616d84",
        "user": {
            "identifier": "c1e21425-66c5-44d0-bc14-5352301fb7f0",
            "first_name": "Testy",
            "last_name": "McTester",
            "email": "testy@getslideapp.com",
            "mobile_number": "+27821111112",
            "company": "slide_dev",
            "groups": "user",
            "type": null,
            "status": "active",
            "created": "2018-09-30T15:53:37.012334Z",
            "updated": "2018-10-31T09:48:38.299689Z"
        },
        "amount": 100,
        "total_amount_charged": 115,
        "currency": "ZAR",
        "created": "2018-10-31T14:08:08.021608Z",
        "updated": "2018-10-31T14:08:50.753947Z",
        "status": "Complete",
        "charges": [
            {
                "type": "user",
                "identifier": "9947b78a-a7d0-4da6-a7ce-f2a447a732a0",
                "amount": 15,
                "flat_fee_charge": 5,
                "percentage_rate_charge": 10,
                "currency": "ZAR",
                "created": "2018-10-31T14:08:08.119462Z",
                "updated": "2018-10-31T14:09:02.457248Z",
                "status": "Complete"
            }
        ]
    },
    "message": null,
    "status": "success"
}
```

This endpoint retrieves the Withdrawal with `identifier = {identifier}` if the logged in user is the resource owner, otherwise it returns a `404 Not Found`.

#### HTTP Request

`GET /user/withdrawals/{identifier}/`

<!-- ### Create Deposit
```shell
curl "{base_url}/admin/deposits/" \
  -X POST \
  -H "Content-Type: application/json" \
  -H "Authorization: Token {token}" \
  -d '{
      "user": "c1e21425-66c5-44d0-bc14-5352301fb7f0",
       "amount": 40
     }'
```
> The above command returns JSON structured like this:

```json
{
  "status": "success",
    "message": "created",
    "data": {
        "identifier": "138cf55f-cbcb-40a8-9db6-c153e7f3be81",
        "user": {
            "identifier": "c1e21425-66c5-44d0-bc14-5352301fb7f0",
            "first_name": "Testy",
            "last_name": "McTester",
            "email": "testy@getslideapp.com",
            "mobile_number": "+27654329999",
            "company": "slide_dev",
            "groups": "user",
            "type": null,
            "status": "active",
            "created": "2018-09-30T15:53:37.012334Z",
            "updated": "2018-11-01T12:51:29.721300Z"
        },
        "amount": 40,
        "total_amount_charged": 49,
        "currency": "ZAR",
        "created": "2018-11-01T12:54:27.467077Z",
        "updated": "2018-11-01T12:54:59.902736Z",
        "status": "Complete",
        "charges": [
            {
                "type": "user",
                "identifier": "9a8dc48c-cd4d-4f6a-9705-981e687e3106",
                "amount": 9,
                "flat_fee_charge": 5,
                "percentage_rate_charge": 4,
                "currency": "ZAR",
                "created": "2018-11-01T12:54:27.618507Z",
                "updated": "2018-11-01T12:55:11.583940Z",
                "status": "Complete"
            }
        ]
    },
}
```
This endpoint creates a deposit transaction for the specified user.
#### HTTP Request
`POST /admin/deposits/`
#### POST Parameters
Parameter | Description | Type | Required
--------- | ----------- | -----| --------
`user` | User's identifier | String | Yes
`amount` | Deposit amount (cents) | Integer | Yes -->

### Create Transfer

```shell
curl "{base_url}/user/transfers/" \
  -X POST \
  -H "Content-Type: application/json" \
  -H "Authorization: Token {token}" \
  -d '{"amount": 50,
       "recipient": "899b55e1-65a9-4969-b282-b8f02671f76b",
       "note": "testy"}'
```

> The above command returns JSON structured like this:

```json
{
    "message": "created",
    "data": {
        "identifier": "97d3c3d8-b47c-456d-86db-f10a1122079b",
        "user": {
            "identifier": "c1e21425-66c5-44d0-bc14-5352301fb7f0",
            "first_name": "Testy",
            "last_name": "McTester",
            "email": "testy@getslideapp.com",
            "mobile_number": "+27654329999",
            "company": "slide_dev",
            "groups": "user",
            "type": null,
            "status": "active",
            "created": "2018-09-30T15:53:37.012334Z",
            "updated": "2018-11-01T12:51:29.721300Z"
        },
        "recipient": {
            "identifier": "899b55e1-65a9-4969-b282-b8f02671f76b",
            "first_name": "Agent",
            "last_name": "Smith",
            "email": "agent.smith@matrix.com",
            "mobile_number": null,
            "company": "slide_dev",
            "groups": "user",
            "type": null,
            "status": "active",
            "created": "2018-07-24T10:50:42.364399Z",
            "updated": "2018-07-24T10:50:48.329165Z"
        },
        "note": "testy",
        "amount": 50,
        "total_amount_charged": 65,
        "total_amount_received": 45,
        "currency": "ZAR",
        "created": "2018-11-01T13:24:39.407243Z",
        "updated": "2018-11-01T13:25:08.589063Z",
        "status": "Complete",
        "charges": [
            {
                "type": "user",
                "identifier": "d94b5dd0-0e25-4de0-a828-0ba777259444",
                "amount": 15,
                "flat_fee_charge": 10,
                "percentage_rate_charge": 5,
                "currency": "ZAR",
                "created": "2018-11-01T13:24:39.911105Z",
                "updated": "2018-11-01T13:25:27.501781Z",
                "status": "Complete"
            },
            {
                "type": "recipient",
                "identifier": "db6686d2-decd-44fd-966a-b4523eaa575c",
                "amount": 5,
                "flat_fee_charge": 0,
                "percentage_rate_charge": 5,
                "currency": "ZAR",
                "created": "2018-11-01T13:24:39.943320Z",
                "updated": "2018-11-01T13:25:15.709067Z",
                "status": "Complete"
            }
        ]
    },
    "status": "success"
}
```

This endpoint creates a transfer transaction from the specified user to the specified recipient.

#### HTTP Request

`POST /user/transfers/`

#### POST Parameters

Parameter | Description | Type | Required
--------- | ----------- | -----| --------
`amount` | Deposit amount (cents) | Integer | Yes
`recipient` | Recipient's identifier | String | Yes
`note` | Transfer note | String | Yes

### Create Withdrawal

```shell
curl "{base_url}/user/withdrawals/" \
  -X POST \
  -H "Content-Type: application/json" \
  -H "Authorization: Token {token}" \
  -d '{"amount": 100}'
```

> The above command returns JSON structured like this:

```json
{
    "status": "success",
    "message": "created",
    "data": {
        "identifier": "ac630b80-3efd-459d-9599-8c1491246315",
        "user": {
            "identifier": "c1e21425-66c5-44d0-bc14-5352301fb7f0",
            "first_name": "Testy",
            "last_name": "McTester",
            "email": "testy@getslideapp.com",
            "mobile_number": "+27654329999",
            "company": "slide_dev",
            "groups": "user",
            "type": null,
            "status": "active",
            "created": "2018-09-30T15:53:37.012334Z",
            "updated": "2018-11-01T12:51:29.721300Z"
        },
        "amount": 100,
        "total_amount_charged": 100,
        "currency": "ZAR",
        "created": "2018-11-01T13:29:14.458608Z",
        "updated": "2018-11-01T13:30:10.851876Z",
        "status": "Complete",
        "charges": []
    },
}
```

This endpoint creates a withdrawal transaction for the specified user.

#### HTTP Request

`POST /user/withdrawals/`

#### POST Parameters

Parameter | Description | Type | Required
--------- | ----------- | -----| --------
`amount` | Withdrawal amount (cents) | Integer | Yes
