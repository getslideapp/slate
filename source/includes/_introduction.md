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

> The format of a standard success response data is structured like this:

```json
{
    "status": "success",
    "data": {
        /* Application-specific data would go here. */
    },
    "message": null /* Or optional success message */
}
```

> The format of a standard error response data is structured like this:

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

Users: Requests that we receive directly from the client are essentially a user interacting with the service. Users will be able to view their own data or make transactions on their own behalf. Users are authenticated as per the [User Authentication](#user-authentication) below.

Company Admin Users: Requests by an administrator. The admin endpoints will allow Company Admin Users to retrieve data or perform transactions on behalf of users within their company. Admin Users are authenticated as per the [Admin Authentication](#admin-authentication) below.

## Authentication

> To check authorization, use this code:

```shell
curl "{base_url}/user/" \
  -X GET \
  -H "Authorization: Token {token}"
```

> Make sure to replace `{token}` with your API token and `{base_url}` with the desired base url.

Slide uses a token-based HTTP Authentication scheme.

Authenticate your account by including a token in API requests. API tokens carry many privileges, so be sure to keep them secure!

Once a user has logged in and received a token, each subsequent request should include the token in the HTTP Authorization header.

<!-- You can register a new Slide API key at our [developer portal](http://example.com/developers).You can manage your API keys in the Dashboard.  -->

All API requests must be made over HTTPS. Calls made over plain HTTP will fail. API requests without authentication will also fail.

Slide expects an API token to be included in the HTTP Authorization header in all API requests to the server:

`Authorization: Token {token}`


There are two types of authentication offered: [User Authentication](#user-authentication) and [Admin Authentication](#admin-authentication). Please refer to each section for further details.

<aside class="notice">
You must replace <code>{token}</code> with your personal API token.
</aside>


### User Authentication

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

Slide can handle the user authentication for a company or the company can choose to handle their own user authentication. If Slide handles all the user authentication, Slide will authenticate all [User API calls](#user) using the API token that the user receives from Slide when they log in.

If the company prefers to manage their own user authentication, users will not log in via Slide and hence will not receive a token from Slide. In this case Slide will need to authenticate all User API calls with the company's server.

When a user makes an API call to Slide, Slide will check with the company's server as to whether the token included in the API call header is valid.

### Admin Authentication

Slide will authenticate all [Admin API calls](#admin) using an Admin API token that is generated by Slide.

You can obtain a new Slide Admin API token or a Company Name by emailing us at info@getslideapp.com.
