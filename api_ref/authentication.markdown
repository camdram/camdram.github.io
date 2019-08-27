---
layout: api
title:  "API Reference: Authentication"
date:   2019-08-27 22:17:00 +0100
author: "Charlie Jonas"
permalink: /api/authentication
---

The Camdram API follows the [OAuth 2.0 specification](https://oauth.net/2/) for authenticating client access and implements the Authorization Code, Client Credentials and Refresh Token strategies.
For most who are looking to make authenticated requests to the Camdram API, we recomend using an existing OAuth2-compatible client rather than rolling your own from scratch:
* JavaScript: [client-oauth2](https://github.com/mulesoft/js-client-oauth2)
* Python: [Requests-OAuthlib](https://github.com/requests/requests-oauthlib)
* Ruby: [OAuth2](https://github.com/oauth-xx/oauth2)
* Java: [oauth2-essentials](https://github.com/dmfs/oauth2-essentials)
* Go: [golang.org/x/oauth2](https://github.com/golang/oauth2)

# Authorization Code

The *Authorization Code* strategy is the primary way in which third-party web apps will want to interact with the Camdram API.
It is used for websites that want to authenticate users via their Camdram login and/or perform API requests on behalf of that user.

Under this strategy, a user is directed to a special page on Camdram where they login and give consent for their details to be disclosed to the third-party, before being redirected back to the application's landing page.
This process is exmplained in more details in the steps below:
1. User clicks "Sign in with Camdram" button in a third-party app.
2. User is redirected to a Camdram webpage where they will be asked to login and give their consent for information such as their name and email address to be disclosed to the third-party app.
3. The user clicks "Allow" on the consent screen.
4. The user is redirected once again to the third-party application's `callback_uri`.

# Client Credentials

The *Client Credentials* authentication strategy is for use mainly by script-based applications which want to make API requests on behalf of the application itself or the application's owner.
In this strategy a so-called bearer token can be obtained in return for the correct API application ID and secret.

A bearer token can be obtained by making the following API request, in this example by using cURL:

```
curl --request POST --url 'https://www.camdram.net/oauth/v2/token' \
--header 'content-type: application/json' \
--data '{"grant_type":"client_credentials","client_id": "your_api_app_id_here","client_secret": "your_api_app_secret_here"}'
```

This will return a response similar to the following:

```
{"access_token":"api_access_token","expires_in":3600,"token_type":"bearer","scope":"user_email user_shows user_orgs api_write api_write_org"}
```

The access token from that response can then be used to make further authenticated API requests, such as obtaining a list of shows that the API application's owner can edit (essentially a list of shows to which the owner has admin rights):

```
curl --header "Authorization: Bearer the_api_access_token_from_above" "https://www.camdram.net/auth/account/shows.json"
```

Note that at the time of writing, all API access tokens on Camdram will expire exactly one hour after they are issued.
