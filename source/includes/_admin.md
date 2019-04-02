# Admin

Admin endpoints are a set of admin-only accessible endpoints that provide administrative functionality across a company.

## Account

The admin account endpoints allow the admin to retrieve information pertaining to the company admin account.

### Get Balance
 
```shell
curl "{base_url}/admin/account/balance/" \
  -X GET \
  -H "Authorization: Token {token}"
```

> The above command returns JSON structured like this:

```json
{
    "status": "success",
    "data": {
        "balance": 1000
    },
    "message": null
}
```

This endpoint retrieves the company account balance.

#### HTTP Request

`GET /admin/account/balance/`

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
      "results": [
        {
        "identifier": "230899032-f09832409-23580913",
        "first_name": "Testy",
        "last_name": "McTester",
        "email": "testy@getslideapp.com",
        "mobile_number": "+27821111112",
        "company": "slide",
        "groups": "user",
        "type": null,
        "status": "active",
        "reference_number": "sld-a1b2",
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

This endpoint retrieves a list of all users.

#### HTTP Request

`GET /admin/users/?first_name=john&email=johndoe@getslideapp.com`

#### Pagination Parameters
This endpoint supports pagination, with the following parameters available for use:

Parameter | Description | Type | Required
--------- | ----------- | -----| --------
`page` | Page to retrieve | Integer | No
`page_size` | Amount of results per page | Integer | No


#### Filter Parameters
This endpoint supports filtering, with the following query parameters available for use:

Parameter | Description | Type
--------- | ----------- | -----
`identifier` | User's `identifier` | String
`first_name` | User's `first_name` | String
`last_name` | User's `last_name` | String
`email` | User's `email` | String
`mobile_number` | User's `mobile_number` | String
`status` | User's status (Options are: `pending`, `active`, or `inactive`) | String
`groups` | User's group (Options are: `user` or `admin_user`) | String  

<aside class="notice">
When filtering using arguments that contain special characters, the special characters should be URL encoded. For example, + should be replaced with %2B when filtering on email or mobile_number.
</aside>

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
          "groups": "user",
          "type": null,
          "status": "active",
          "reference_number": "sld-a1b2",
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
  -H "Content-Type: application/json" \
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
        "groups": "user",
        "type": null,
        "status": "active",
        "reference_number": "sld-a1b2",
        "created": "2018-08-21T09:27:25.882898Z",
        "updated": "2018-08-29T16:58:54.647283Z"
    },
    "message": null,
    "status": "success"
}
```

This endpoint creates a user. When a new user is created they will by default be in the `user` group. Their
status defaults to `active` if none is specified.

#### HTTP Request

`POST /admin/users/`

#### POST Parameters

Parameter | Description | Type | Required
--------- | ----------- | -----| --------
`email` | User's email address | String | Yes
`first_name` | User's first name | String | No
`last_name` | User's last name | String | No
`mobile_number` | User's mobile number | String | No
`status` | User's status (Options are: `pending`, `active`, or `inactive`) | String | No
`groups` | User's group (Options are: `user` or `admin_user`) | String | No

<aside class="notice">
Formatting of `mobile_number` is such that it should be exactly 11 characters in length (9 integers prepended by `+27`).
</aside>

### Update User

```shell
curl "{base_url}/admin/users/{identifier}/" \
  -X PUT \
  -H "Content-Type: application/json" \
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
        "groups": "user",
        "type": null,
        "status": "active",
        "reference_number": "sld-a1b2",
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

#### POST Parameters

Parameter | Description | Type | Required
--------- | ----------- | ---- | --------
`email` | User's email address | String | No
`first_name` | User's first name | String | No
`last_name` | User's last name | String | No
`mobile_number` | User's mobile number | String | No
`status` | User's status (Options are: `pending`, `active`, or `inactive`) | String | No
`groups` | User's group (Options are: `user` or `admin_user`) | String | No

<aside class="notice">
Formatting of `mobile_number` is such that it should be exactly 11 characters in length (9 integers prepended by `+27`).
</aside>

### Delete User

```shell
curl "{base_url}/admin/users/{identifier}/" \
  -X DELETE \
  -H "Content-Type: application/json" \
  -H "Authorization: Token {token}" \

```
> Make sure to replace `{identifier}` with the User Id.

> The above command returns JSON structured like this:

```json
{
    "message": "deleted",
    "status": "success"
}
```

This endpoint deletes an existing user.

#### HTTP Request

`DELETE /admin/users/{identifier}/`


## Transactions

The Admin transaction endpoints allows an admin user to view or perform a transaction on the user's behalf.

<aside class="notice">
<strong>A note about fees and charges</strong>
<br /><br />
Different fee types may be imposed on different parties for each transaction type. A fee may be imposed on the <b>company</b>, on the transaction <b>user</b>, and in the case of a transfer only, the <b>recipient</b>.
<br /><br />
A transaction fee includes options for setting a <b>flat fee</b>, a <b>percentage rate</b>, or both. For the case of recipient fee on a transfer however, only a percentage rate fee is allowed. The transaction <b>total_amount_charged</b> will then equate to the sum of the transaction amount plus flate fee and percentage rate charges. In the case of a transfer the <b>total_amount_received</b> is the transfer amount minus recipient percentage rate charge.
<br /><br />
Upon the completion of each successful transaction on which fees are imposed, the respective accounts will be charged with the amount stipulated by the fee. This will be the sum of the flat fee and percentage rate for that transaction type.
Fees imposed on the user and recipients are charged to these user accounts and transferred to the company admin account. Fees imposed on the company are paid from the company admin account to Slide. For each transaction, any and all such charges can be found
under the <b>charges</b> field in the response.
</aside>



### List Transactions

```shell
curl "{base_url}/admin/transactions/?amount__gt=200" \
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
        "next": "{base_url}/admin/transactions/?page=2?page_size=50",
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
                    "reference_number": "sld-a1b2",
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
                    "reference_number": "sld-a1b2",
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

`GET /admin/transactions/`

#### Pagination Parameters
This endpoint supports pagination, with the following parameters available for use:

Parameter | Description | Type | Required
--------- | ----------- | -----| --------
`page` | Page to retrieve | Integer | No
`page_size` | Amount of results per page | Integer | No


#### Filter Parameters
This endpoint supports filtering, with the following query parameters available for use:

Parameter | Description | Type
--------- | ----------- | -----
`type` | Type of transaction (Options are: `transfer`, `withdrawal`, `deposit`, `card_deposit` or `bank_eft_deposit`) | String
`user` | User initiating transaction. Can filter on a user's `identifier`, `email` or `mobile_number` | String
`recipient` | User receiving transfer. Can filter on a recipient's `identifier`, `email` or `mobile_number` | String
`amount__gt` | Only return transactions where the amount is greater than this value, in cents | Integer
`amount__lt` | Only return transactions where the amount is less than this value, in cents | Integer
`amount__exact` | Only return transactions where the amount is equal to this value, in cents | Integer
`created__gt` | Only return transactions created after this date, in the UNIX timestamp format | Float
`created__lt` | Only return transactions created before this date, in the UNIX timestamp format | Float

<aside class="notice">
When filtering using arguments that contain special characters, the special characters should be URL encoded. For example, + should be replaced with %2B when filtering on email or mobile_number.
</aside>

### Aggregate Transactions

```shell
curl "{base_url}/admin/transactions/aggregate/" \
  -X GET \
  -H "Authorization: Token {token}"
```

> The above command returns JSON structured like this:

```json
{
    "message": null,
    "data": {
        "results": [
            {
                "deposits": {
                    "count": 118,
                    "amount": 17543
                },
                "transfers": {
                    "count": 113,
                    "amount": 17528
                },
                "withdrawals": {
                    "count": 169,
                    "amount": 16900
                },
                "date": 1549162892
            },
            {
                "deposits": {
                    "count": 168,
                    "amount": 25020
                },
                "transfers": {
                    "count": 57,
                    "amount": 8826
                },
                "withdrawals": {
                    "count": 109,
                    "amount": 10900
                },
                "date": 1549249292
            },
            {
                "deposits": {
                    "count": 0,
                    "amount": 0
                },
                "transfers": {
                    "count": 0,
                    "amount": 0
                },
                "withdrawals": {
                    "count": 0,
                    "amount": 0
                },
                "date": 1549335692
            },
            ...
        ]
    },
    "status": "success"
}
```

This endpoint retrieves a list of aggregated transaction objects, for the past 30 days, for all of the following types: `deposit`, `withdrawal` and `transfer`.

Each object in the list represents a period of one day, and the date (in UNIX epoch format) is indicated in the `date` field of each object.
Each object in the list is split into three child objects, one for each type of transaction: `deposit`, `withdrawal` and `transfer`.

`count` represents the aggregated count of that object for the given period, i.e. `count: 30` in a `deposits` object indicates there were 30 deposit for that day.

`amount` represents the aggregated amount of that object in cents for the given period, i.e. `amount: 525` in a `transfers` object indicates the sum of all the transfers for that day was 525 cents.

#### HTTP Request

`GET /admin/transactions/aggregate/`






### List Card Deposits

```shell
curl "{base_url}/admin/deposits/" \
  -X GET \
  -H "Authorization: Token {token}"
```

> The above command returns JSON structured like this:

```json
{
    "status": "success",
    "message": null,
    "data": {
        "results": [
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
                  "reference_number": "sld-a1b2",
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
              ],
              "type": "card_deposit"
            }
      ],
      "total": 1
    }
}
```

This endpoint retrieves a list of all card deposits.

#### HTTP Request

`GET /admin/deposits/`



### List Bank EFT Deposits

```shell
curl "{base_url}/admin/deposits/bank-eft/" \
  -X GET \
  -H "Authorization: Token {token}"
```

> The above command returns JSON structured like this:

```json
{
    "status": "success",
    "message": null,
    "data": {
        "results": [
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
                  "reference_number": "sld-a1b2",
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
              ],
              "type": "bank_eft_deposit"
            }
      ],
      "total": 1
    }
}
```

This endpoint retrieves a list of all card deposits.

#### HTTP Request

`GET /admin/deposits/bank-eft/`

### List Transfers

```shell
curl "{base_url}/admin/transfers/" \
  -X GET \
  -H "Authorization: Token {token}"
```

> The above command returns JSON structured like this:

```json
{
    "status": "success",
    "message": null,
    "data": {
      "results": [
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
                  "reference_number": "sld-a1b2",
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
                  "reference_number": "sld-a1b2",
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
              ],
              "type": "transfer"
        }      
      ],
      "total": 1
    }
}

```

This endpoint retrieves a list of all transfers.

#### HTTP Request

`GET /admin/transfers/`

### List Withdrawals

```shell
curl "{base_url}/admin/withdrawals/" \
  -X GET \
  -H "Authorization: Token {token}"
```

> The above command returns JSON structured like this:

```json
{
    "status": "success",
    "message": null,
    "data": {
        "results": [
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
                  "reference_number": "sld-a1b2",
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
              ],
              "type": "withdrawal"
          }
        ],
        "total": 1
      }
    }
```

This endpoint retrieves a list of all withdrawals.

#### HTTP Request

`GET /admin/withdrawals/`


### Get Transaction

```shell
curl "{base_url}/admin/transactions/{identifier}/" \
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
            "reference_number": "sld-a1b2",
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
            "reference_number": "sld-a1b2",
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
The type of the transaction is specified in the `type` field of the object.

#### HTTP Request

`GET /admin/transactions/{identifier}/`


### Get Card Deposit

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
            "reference_number": "sld-a1b2",
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
        ],
        "type": "card_deposit"
    },
    "message": null,
    "status": "success"
}
```

This endpoint retrieves the card Deposit with `identifier = {identifier}` if the logged in user is the resource owner, otherwise it returns a `404 Not Found`.

#### HTTP Request

`GET /admin/deposits/{identifier}/`


### Get Bank EFT Deposit

```shell
curl "{base_url}/admin/deposits/bank-eft/{identifier}/" \
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
            "reference_number": "sld-a1b2",
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
        ],
        "type": "bank_eft_deposit"
    },
    "message": null,
    "status": "success"
}
```

This endpoint retrieves the bank EFT Deposit with `identifier = {identifier}`, or a `404 Not Found` if it does not exist.

#### HTTP Request

`GET /admin/deposits/bank-eft/{identifier}/`

### Get Transfer

```shell
curl "{base_url}/admin/transfers/{identifier}/" \
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
            "reference_number": "sld-a1b2",
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
            "reference_number": "sld-a1b2",
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
        ],
        "type": "transfer"
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
curl "{base_url}/admin/withdrawals/{identifier}/" \
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
            "reference_number": "sld-a1b2",
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
        ],
        "type": "withdrawal"
    },
    "message": null,
    "status": "success"
}
```

This endpoint retrieves the Withdrawal with `identifier = {identifier}` if the logged in user is the resource owner, otherwise it returns a `404 Not Found`.

#### HTTP Request

`GET /admin/withdrawals/{identifier}/`

### Create Card Deposit

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
            "reference_number": "sld-a1b2",
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
        ],
        "type": "card_deposit"
    },
}
```

This endpoint creates a card deposit transaction for the specified user.

#### HTTP Request

`POST /admin/deposits/`

#### POST Parameters

Parameter | Description | Type | Required
--------- | ----------- | -----| --------
`user` | User's identifier | String | Yes
`amount` | Deposit amount (cents) | Integer | Yes


### Create Bank EFT Deposit

```shell
curl "{base_url}/admin/deposits/bank-eft/" \
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
            "reference_number": "sld-a1b2",
            "created": "2018-09-30T15:53:37.012334Z",
            "updated": "2018-11-01T12:51:29.721300Z"
        },
        "amount": 40,
        "total_amount_charged": 49,
        "currency": "ZAR",
        "created": "2018-11-01T12:54:27.467077Z",
        "updated": "2018-11-01T12:54:59.902736Z",
        "status": "Initialized",
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
                "status": "Initialized"
            }
        ],
        "type": "bank_eft_deposit"
    },
}
```

This endpoint creates a Bank EFT deposit transaction for the specified user. Following the initialization of the transaction using this endpoint, the user will then be required to make a bank EFT deposit for the total amount,
quoting their user `reference_number` as a reference. When this deposit is processed, the status will shift to `Complete` and only then will the funds will reflect in the user balance.


#### HTTP Request

`POST /admin/deposits/bank-eft/`

#### POST Parameters

Parameter | Description | Type | Required
--------- | ----------- | -----| --------
`user` | User's identifier | String | Yes
`amount` | Deposit amount (cents) | Integer | Yes

### Create Transfer

```shell
curl "{base_url}/admin/transfers/" \
  -X POST \
  -H "Content-Type: application/json" \
  -H "Authorization: Token {token}" \
  -d '{"user": "c1e21425-66c5-44d0-bc14-5352301fb7f0",
       "amount": 50,
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
            "reference_number": "sld-a1b2",
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
            "reference_number": "sld-a1b2",
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
        ],
        "type": "transfer"
    },
    "status": "success"
}
```

This endpoint creates a transfer transaction from the specified user to the specified recipient.

#### HTTP Request

`POST /admin/transfers/`

#### POST Parameters

Parameter | Description | Type | Required
--------- | ----------- | -----| --------
`user` | User's identifier | String | Yes
`amount` | Deposit amount (cents) | Integer | Yes
`recipient` | Recipient's identifier | String | Yes
`note` | Transfer note | String | Yes

### Create Withdrawal

```shell
curl "{base_url}/admin/withdrawals/" \
  -X POST \
  -H "Content-Type: application/json" \
  -H "Authorization: Token {token}" \
  -d '{"user": "c1e21425-66c5-44d0-bc14-5352301fb7f0",
       "amount": 100}'
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
            "reference_number": "sld-a1b2",
            "created": "2018-09-30T15:53:37.012334Z",
            "updated": "2018-11-01T12:51:29.721300Z"
        },
        "amount": 100,
        "total_amount_charged": 100,
        "currency": "ZAR",
        "created": "2018-11-01T13:29:14.458608Z",
        "updated": "2018-11-01T13:30:10.851876Z",
        "status": "Complete",
        "charges": [],
        "type": "withdrawal"
    },
}
```

This endpoint creates a withdrawal transaction for the specified user.

#### HTTP Request

`POST /admin/withdrawals/`

#### POST Parameters

Parameter | Description | Type | Required
--------- | ----------- | -----| --------
`user` | User's identifier | String | Yes
`amount` | Withdrawal amount (cents) | Integer | Yes


## Fees

The Admin transaction endpoints allows an admin user to view, create and update fees charged to their users.

<aside class="notice">
<strong>A note about company fees</strong>
<br /><br />
Fees set for the company are not able to be viewed but not created nor updated via the API endpoints.
</aside>

### List Fees

```shell
curl "{base_url}/admin/fees/" \
  -X GET \
  -H "Authorization: Token {token}" \
```

> The above command returns JSON structured like this:

```json
{
    "data": {
        "total": 8,
        "results": [
            {
                "description": "Deposit Fee",
                "active": false,
                "tx_type": "deposit",
                "paying_party": "recipient",
                "flat_fee": 3,
                "percentage_rate": 0.5,
                "identifier": "75f3184f-5370-4c08-8c93-d399bad2bec4"
            },
            {
                "description": "Transfer Recipient Fee",
                "active": false,
                "tx_type": "transfer",
                "paying_party": "recipient",
                "flat_fee": 0,
                "percentage_rate": 0.9,
                "identifier": "ec243d2b-e569-43f5-9006-560946f3dbce"
            },
            {
                "description": "Transfer Company Fee",
                "active": false,
                "tx_type": "transfer",
                "paying_party": "company",
                "flat_fee": 2,
                "percentage_rate": 0.5,
                "identifier": "c4e0b3a2-3b5e-4e4c-a596-87b90d1d843e"
            },
            {
                "description": "Transfer Sender Fee",
                "active": false,
                "tx_type": "transfer",
                "paying_party": "user",
                "flat_fee": 1,
                "percentage_rate": 1,
                "identifier": "10403bc5-29f6-493b-8158-bfbc3f91f8ae"
            },
            {
                "description": "withdrawal Company Fee",
                "active": false,
                "tx_type": "withdrawal",
                "paying_party": "company",
                "flat_fee": 1,
                "percentage_rate": 1,
                "identifier": "bf894445-981c-47fd-9f16-4f4af06007c4"
            },
            {
                "description": "withdrawal Fee",
                "active": false,
                "tx_type": "withdrawal",
                "paying_party": "user",
                "flat_fee": 2,
                "percentage_rate": 1,
                "identifier": "e618a867-24b8-44dc-a8cb-7c6b6a5fc3a5"
            },
            {
                "description": "Deposit Company Fee",
                "active": false,
                "tx_type": "deposit",
                "paying_party": "company",
                "flat_fee": 2,
                "percentage_rate": 1.5,
                "identifier": "89d9af37-2ecc-42f3-b25a-122ba83a173e"
            },
            {
                "description": "Deposit Fee",
                "active": false,
                "tx_type": "deposit",
                "paying_party": "user",
                "flat_fee": 2,
                "percentage_rate": 1,
                "identifier": "51900bae-af02-4f04-b589-cf8fe7ed45c1"
            }
        ]
    },
    "message": null,
    "status": "success"
}
```

This endpoint retrieves a list of all fees.

#### HTTP Request

`GET /admin/fees/`

### Get Fee

```shell
curl "{base_url}/admin/fees/{identifier}/" \
  -X GET \
  -H "Authorization: Token {token}" \
```

> The above command returns JSON structured like this:

```json
{
    "data": {
        "description": "Deposit Company Fee",
        "active": false,
        "tx_type": "deposit",
        "paying_party": "company",
        "flat_fee": 2,
        "percentage_rate": 1.5,
        "identifier": "89d9af37-2ecc-42f3-b25a-122ba83a173e"
    },
    "message": null,
    "status": "success"
}
```

This endpoint retrieves a specific fee with identifier = {identifier}.

#### HTTP Request

`GET /admin/fees/{identifier}/`

### Create Fee

```shell
curl "{base_url}/admin/fees/" \
  -X POST \
  -H "Content-Type: application/json" \
  -H "Authorization: Token {token}" \
  -d '{"description": "Fee description",
        "active": true,
        "tx_type": "withdrawal",
        "paying_party": "user",
        "flat_fee": 5,
        "percentage_rate": 3.5}'
```

> The above command returns JSON structured like this:

```json
{
    "message": "created",
    "status": "success",
    "data": {
        "active": true,
        "description": "Fee description",
        "flat_fee": 5,
        "identifier": "bbabebdf-4693-4d37-9d09-5c7a32bffb28",
        "paying_party": "user",
        "percentage_rate": 3.5,
        "tx_type": "withdrawal"
    }
}
```

This endpoint creates a fee.

#### HTTP Request

`POST /admin/fees/`

#### POST Parameters

Parameter | Description | Type | Required
--------- | ----------- | -----| --------
`description` | Fee description | String | Yes
`active` | Active status | Boolean | Yes
`tx_type` | Transaction type (Options are: withdrawal, deposit or transfer) | String | Yes
`paying_party` | Party fee is charged to (Options are: user or recipient) | String | Yes
`flat_fee` | Flat fee (cents) | Integer | Yes
`percentage_rate` | Percentage rate fee (%) | Float | Yes


### Update Fee

```shell
curl "{base_url}/admin/fees/{identifier}/" \
  -X PUT \
  -H "Content-Type: application/json" \
  -H "Authorization: Token {token}" \
  -d '{"description": "Fee test description",
        "active": true,
        "tx_type": "withdrawal",
        "paying_party": "user",
        "flat_fee": 2,
        "percentage_rate": 2.1}'
```

> The above command returns JSON structured like this:

```json
{
    "message": null,
    "status": "success",
    "data": {
        "active": true,
        "description": "Fee description",
        "flat_fee": 2,
        "identifier": "bbabebdf-4693-4d37-9d09-5c7a32bffb28",
        "paying_party": "user",
        "percentage_rate": 2.1,
        "tx_type": "withdrawal"
    }
}
```

This endpoint updates a fee.

#### HTTP Request

`PUT /admin/fees/{identifier}/`

#### POST Parameters

Parameter | Description | Type | Required
--------- | ----------- | -----| --------
`description` | Fee description | String | No
`active` | Active status | Boolean | No
`tx_type` | Transaction type (Options are: withdrawal, deposit or transfer) | String | No
`paying_party` | Party fee is charged to (Options are: user or recipient) | String | No
`flat_fee` | Flat fee (cents) | Integer | No
`percentage_rate` | Percentage rate fee (%) | Float | No
