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

#### URL Parameters

Parameter | Description | Type | Required
--------- | ----------- | -----| --------
`first_name` | User's first name | String | Yes
`last_name` | User's last name| String | Yes
`email` | User's email address | String | Yes
`mobile_number` | User's mobile number | String | No
`password1` | User selected password | String | Yes
`password2` | Password confirmation | String | Yes
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

#### URL Parameters

Parameter | Description | Type | Required
--------- | ----------- | -----| --------
`user` | User's email, mobile number or identifier | String | Yes
`password` | User password | String | Yes
`session_duration` | Sets the token duration period in milliseconds | Int | No

<aside class="notice">
The session duration, if not set, will default to 10 hours. A session duration of 0 will set an infinite token duration.
</aside>
