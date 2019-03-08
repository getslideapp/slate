# Authorization

These endpoints are used to facilitate user registration and login.

## Register

New user registration endpoint.

```shell
curl "{base_url}/auth/register/" \
  -X POST
  -H "Content-Type: application/json" \
  -d '{ "first_name": "Testy",
        "last_name": "McTester",
        "email": "testy@getslideapp.com",
        "mobile": "+27821111112",
        "password1:" "password",
        "password2:" "password",      
      }'
```

> The above command returns JSON structured like this:

```json
{
    "status": "success",
    "message": "created",
    "data": {
        "token": "bde7fd5258bbabd98723bfebcd09350bdfe09e80a86769",
        "user": {
            "identifier": "47c2dc9b-047e-4502-8f2a-271abd696f00",
            "first_name": "Testy",
            "last_name": "McTester",
            "email": "testy@getslideapp.com",
            "mobile_number": "+27821111112",
            "company": "slide",
            "groups": "user",
            "type": null,
            "status": "active",
            "created": "2019-01-25T12:28:37.605119Z",
            "updated": "2019-01-25T12:28:37.605138Z"
        }
    }
}
```

#### HTTP Request

`POST /auth/register/`

#### POST Parameters

Parameter | Description | Type | Required
--------- | ----------- | -----| --------
`email` | User's email address | String | Yes
`password1` | User selected password | String | Yes
`password2` | Password confirmation | String | Yes
`first_name` | User's first name | String | No
`last_name` | User's last name| String | No
`mobile_number` | User's mobile number | String | No
`session_duration` | Sets the token duration period in milliseconds | Int | No

<aside class="notice">
The session duration, if not set, will default to 10 hours. A session duration of 0 will set an infinite token duration.
</aside>

## Login

User login endpoint.

```shell
curl "{base_url}/auth/register/" \
  -X POST
  -H "Content-Type: application/json" \
  -d '{ "user": "testy@getslideapp.com",
        "password": "password"  
      }'
```

> The above command returns JSON structured like this:

```json
{
    "status": "success",
    "message": "created",
    "data": {
        "token": "bde7fd5258bbabd98723bfebcd09350bdfe09e80a86769",
        "user": {
            "identifier": "47c2dc9b-047e-4502-8f2a-271abd696f00",
            "first_name": "Testy",
            "last_name": "McTester",
            "email": "testy@getslideapp.com",
            "mobile_number": "+27821111112",
            "company": "slide",
            "groups": "user",
            "type": null,
            "status": "active",
            "created": "2019-01-25T12:28:37.605119Z",
            "updated": "2019-01-25T12:28:37.605138Z"
        }
    }
}
```

#### HTTP Request

`POST /auth/login/`

#### POST Parameters

Parameter | Description | Type | Required
--------- | ----------- | -----| --------
`user` | User's email, mobile number or identifier | String | Yes
`password` | User password | String | Yes
`session_duration` | Sets the token duration period in milliseconds | Int | No

<aside class="notice">
The session duration, if not set, will default to 10 hours. A session duration of 0 will create a token without an expiry.
</aside>


## Tokens

For users management of their own tokens.

### List Tokens

```shell
curl "{base_url}/auth/tokens/" \
  -X GET \
  -H "Authorization: Token {token}"
```

> The above command returns JSON structured like this:

```json
{
  "data": {
        "results": [
            {
                "expires": 1549388725521,
                "token_key": "0e6c6d0f"
            },
            {
                "expires": null,
                "token_key": "dc064946"
            },
            {
                "expires": 1549386279025,
                "token_key": "248eccda"
            },
            {
                "expires": null,
                "token_key": "de905029"
            }
        ],
        "total": 4
    }
}
```

This endpoint retrieves a list of all tokens for the user.

#### HTTP Request

`GET /auth/tokens/`


### Create Token

```shell
curl "{base_url}/auth/tokens/" \
  -X POST \
  -H "Authorization: Token {token}" \
  -d '{"password": "securepassword123"}'
```

> The above command returns JSON structured like this:

```json
{
    "data": {
        "token": "e7009c3bc9ec601bb4ca0bbfd24fa899a793ff5630d851ff2aa13bf104baf140"
    },
    "status": "success",
    "message": "created"
}
```

This endpoint creates a new permanent token for the user.

#### HTTP Request

`POST /auth/tokens/`

#### POST Parameters

Parameter | Description | Type | Required
--------- | ----------- | -----| --------
`password` | User's password for authentication | String | Yes
`duration` |  Sets the token duration period in milliseconds | Int | Yes

<aside class="notice">
The duration, if not set, will default to 10 hours. A duration of 0 will create a token without an expiry.
</aside>

### Delete Token

```shell
curl "{base_url}/auth/tokens/{token_key}/" \
  -X DELETE \
  -H "Authorization: Token {token}"
```

> The above command returns JSON structured like this:

```json
{
    "status": "success",
    "message": "deleted",
    "data": null
}
```

This endpoint deletes a token for the user where `{token_key}` is the key of the token that is to be deleted.

#### HTTP Request

`DELETE /auth/tokens/{token_key}/`
