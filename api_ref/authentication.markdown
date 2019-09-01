---
layout: api
title:  "API Reference: Authentication"
date:   2019-08-27 22:17:00 +0100
author: "Charlie Jonas"
permalink: /api/authentication
---

The Camdram API follows the [OAuth 2.0 specification](https://oauth.net/2/) for authenticating client access and implements the [Authorization Code](#authorization-code),
[Client Credentials](#client-credentials) and
[Refresh Token](#refresh-token) grant types.

OAuth2 authentication flows can be complicated and difficult to understand. As such, we recomend that most who are looking to make authenticated requests to the Camdram API use an existing OAuth2-compatible client rather than rolling your own from scratch. Here are a few we found online (although we don't necessarily have any experience with them):
* JavaScript: [client-oauth2](https://github.com/mulesoft/js-client-oauth2)
* Python: [Requests-OAuthlib](https://github.com/requests/requests-oauthlib)
* Ruby: [OAuth2](https://github.com/oauth-xx/oauth2)
* Java: [oauth2-essentials](https://github.com/dmfs/oauth2-essentials)
* Go: [golang.org/x/oauth2](https://github.com/golang/oauth2)

If you really want to code you own implementation then the following details may be helpful:

* The token endpoint is `/oauth/v2/token`.
* The authorize endpoint is `/oauth/v2/auth`.
* The user's account information endpoint is `/auth/account.{json|xml}`.
* The user's show infomation endpoint is `/auth/account/shows.json`.
* The user's venue & society information endpoint is `/auth/account/organisations.json`.
* The list of available scopes is as follows:
  * `user_email`
  * `user_shows`
  * `user_orgs`
  * `api_write`
  * `api_write_org`

# Authorization Code

The [Authorization Code](https://oauth.net/2/grant-types/authorization-code/) grant type is the primary way in which third-party web apps will want to interact with the Camdram API.
It is used for websites that want to authenticate users via their Camdram login and/or perform API requests on behalf of that user.

Under this strategy, a user is directed to a special page on Camdram where they login and give consent for their details to be disclosed to the third-party app, before being redirected back to a landing page on that application's website.
This process is exmplained in more details in the steps below:
1. A user clicks a "Sign in with Camdram" button in a third-party app.
2. The user is redirected to the authorization endpoint.
3. The user is show a page on Camdram where they will be asked to login and give their consent for information such as their name and email address to be disclosed to the third-party app.
4. The user clicks "Allow" on the consent screen.
5. The user is redirected once again to the third-party application's `callback_uri`.
6. A query string will be appended to the `callback_uri` with details, for example `http://www.example.com/oauth2/camdram/callback?state=entropy&code=moreEntropy`.
7. The `code` query parameter is then exchanged for an access token via a HTTP POST request to the token endpoint.

This authentication flow is complex; we recomend you read [Okta's guide](https://developer.okta.com/blog/2018/04/10/oauth-authorization-code-grant-type) to help aid your understanding.

# Client Credentials

The [Client Credentials](https://oauth.net/2/grant-types/client-credentials/) grant type is for use mainly by script-based applications which want to make API requests on behalf of the application itself or the application's owner.
In this strategy a so-called bearer token is obtained in return for the correct API application ID and secret.

A bearer token can be obtained by making the following API request, in this example by using [cURL](https://curl.haxx.se) on the command line:

```bash
curl \
--request POST \
--url 'https://www.camdram.net/oauth/v2/token' \
--header 'content-type: application/json' \
--data '{"grant_type":"client_credentials","client_id": "your_api_app_id_here","client_secret": "your_api_app_secret_here"}'
```

This will return a response similar to the following:

```json
{
  "access_token": "your_api_access_token",
  "expires_in": 3600,
  "token_type": "bearer",
  "scope": "user_email user_shows user_orgs api_write api_write_org"
}
```

The access token from that response can then be used to make further authenticated API requests, such as obtaining a list of shows that the API application's owner can edit (essentially a list of shows to which the owner has admin rights):

```bash
curl \
--header "Authorization: Bearer the_api_access_token_from_above" \
"https://www.camdram.net/auth/account/shows.json"
```

Note that at the time of writing, all API access tokens on Camdram will expire exactly one hour after they are issued. After this time you will need to obtain another access token by following the above process again.

# Refresh Token
The [Refresh Token](https://oauth.net/2/grant-types/refresh-token/) grant type works in tandem with the Authorization Code grant type.
When authenticating via Authorization Code a refresh token will be obtained in the final step when exchanging the authorization code for an access token.
This can be used as follows to obtain another access token for the user in question after the original expires:

```
curl \
--request POST \
--url 'https://www.camdram.net/oauth/v2/token' \
--header 'content-type: application/json' \
--data '{"grant_type":"refresh_token","refresh_token":"your_refresh_token_here","client_id": "your_api_app_id_here","client_secret": "your_api_app_secret_here"}'
```