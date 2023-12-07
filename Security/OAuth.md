# What is OAuth  

<b>

It's an open slandered for authorization. OAuth is a standard that apps can use to provide client applications with “secure delegated access”. OAuth works over HTTPS and authorizes devices, APIs, servers, and applications with access tokens rather than credentials.

</b>

# Why OAuth

  

OAuth was created as a response to the direct authentication pattern. This pattern was made famous by HTTP Basic Authentication, where the user is prompted for a username and password.  

  

instead of sending a username and password to the server with each request, the user sends an API key ID and secret.

  

**Single-Sign On:** the user talk to their identity provider and the identity provider response with crypto token which hands off to the app to Authenticate the user.

  
  

![SSO](https://developer.okta.com/assets-jekyll/browser_spa_implicit_flow-9116158c9299208718b42a75921acd10a60e4c829edee55a8f14a9ce8de40028.png)

  
  

# OAuth and APIs

  

most developers have moved to REST and stateless APIs. REST is, in a nutshell, HTTP commands pushing JSON packets over the network.

  

in the old days you would requested to enter username/password directory and the app would login directly as you. This gave rise to the **delegated authorization problem**.

  

this can be achieved using OAuth.

OAuth is a delegated authorization framework for REST/APIs. It enables apps to obtain limited access (scopes) to a user’s data without giving away a user’s password.

  

you need to to do an authentication process at the front desk to get it. After authenticating and obtaining the key card, you can access resources across the hotel.

  

# OAuthing Process

- App requests authorization from User

- User authorizes App and delivers proof

- App presents proof of authorization to server to get a Token

- Token is restricted to only access what the User authorized for the specific App

  

# OAuth Component

- Scopes and Consent

- Actors

- Clients

- Tokens  

- Authorization server

- Flow

  

# Scope

the resource that you asked to accept peremission upon it. They’re bundles of permissions asked for by the client when requesting a token. and developed by the application developer.

  
  

You have to capture this consent. This is called trusting on first use, The consent can vary based on the application.

One thing to watch for when you consent is that the app can do stuff on your behalf.

You often have the ability to log in to a dashboard to see what applications you’ve given access to and to revoke consent.

  
  

# OAuth Actors

- **Resource Owner:** the person who owns the data on the server

- **Resource Server:** the server that holds data the application want to access.  

- **Client:** the application wants access.

- **Authorization Server:** the main engine of Auth.

  

![Actors](https://developer.okta.com/assets-jekyll/blog/oauth/oauth-actors-cd8b4861e839037400d8521e97c5d8cf0cb029add65d1036488991c7e85dcb72.png)

  
  

# OAuth Tokens

  

## Access Tokens  

tokens the client use to access the resources server, they'r meant to be short lived. they can not be revoked.  

  

## Refresh Tokens

This can be used to get new access token, this require confidential clients with authentication, they can be revoked.

  

<b>

tokens are retrived from endpoints on the authorization server.

</b>

  

Each time you refresh your access token you get a new cryptographically signed token. Key rotation is built into the system.

  

OAuth spec does'nt define the type of token. but usulally it is a **JWT json web tokens**

  

# JWT

is a secure trustworthy standerd for token authentications. it allows to sign information with a signature that can be verfied later with a secret signing key.

  
  

# Authorization flow

- go to authorize endpoint to get consnet and authorization from the user and

- this return authorization grant that says the user has consented to it

- the authorization grant is passed to the token endpoint

- token endpoint process the authorization grant and says here is your refresh token and access token.

- you can access the api then with the aut token, one it expire you can go back to the auth endpoint with refresh token to get new acess token.

  

![Flow](https://developer.okta.com/assets-jekyll/blog/oauth/authorization-server-99a4ad01368a4c8e407917358d4394d573a6c0e3c9fa10c01a59d1a54c4938cf.png)

  

# Pitfall for developer  

you have to manage the refersh tokens. you get the benfits of key rotaions but the pain of satat managment.

  

there is platforms that help with token managment.

  
  

# Front channel and Back Channel

  

![fornt-back](https://developer.okta.com/assets-jekyll/blog/oauth/flow-channels-5d8996b3706cab1e4ac8f9ed716d6529d3970f91cbc3cb5ff5a21a94389d0e1d.png)

  

## Front Channel

The front channel is what goes over the browser. The browser redirected the user to the authorization server, the user gave consent.

  

## Front Flow

- RO starts the flow to delegate acess to protected resource

- client send auth request with desired scope via browser, browser redirect to the authorize endpoint on the authoriztion server.

- authoriztion server return **consent dialog** to do access upon, you need to be authenticaed to the application.

- the auth grant is passed back to the application via browser redirect.  

  

![flow](https://developer.okta.com/assets-jekyll/blog/oauth/front-channel-flow-4eb727aa0e67884935dd1642749fac60a7194815b8a1724aa11a0ce9adb80fa1.png)

  

## Rquest Responce

### Request

  

```

GET https://accounts.google.com/o/oauth2/auth?scope=gmail.insert gmail.send

&redirect_uri=https://app.example.com/oauth2/callback

&response_type=code&client_id=812741506391

&state=af0ifjsldkj

```

- **Scopes:** from gmail api for insert and send

- **redirect_uri:** the URL of the application that the grant will returned to.

- **responce type:** vaires the OAuth flow.

- **client_id:** from the reg process

- **state:** security flag to insure **XRFS**

  

### Responce

```

HTTP/1.1 302 Found

Location: https://app.example.com/oauth2/callback?

code=MsCeLvIaQm6bTrgtp7&state=af0ifjsldkj

```

- HTTP 302 Redirect  

- Location the application that the grant will returned to

- **code:** the Auth grant

- **State:** to ensure it's not forged and it's from the same source.

  

## Back Channel

<b>

exchange the authorization code for acess token.

</b>

  

- the client send access token request to the token endpoint.

- the token endpoint exchange the grant code  with access token.

- client can acess the protected resources  then.

  

![flow](https://developer.okta.com/assets-jekyll/blog/oauth/back-channel-flow-7a2fb837d1950d5718ee788c2d904247edc5b06b9e3f399253da78636197cbbc.png)

  

## Request

  

```

POST /oauth2/v3/token HTTP/1.1

Host: www.googleapis.com

Content-Type: application/x-www-form-urlencoded

  

code=MsCeLvIaQm6bTrgtp7&client_id=812741506391&client_secret={client_secret}&redirect_uri=https://app.example.com/oauth2/callback&grant_type=authorization_code

```

  

- **The grant_type** is the extensibility part of OAuth.

## Responce

```

{

  "access_token": "2YotnFZFEjr1zCsicMWpAA",

  "token_type": "Bearer",

  "expires_in": 3600,

  "refresh_token": "tGzv3JOkF0XG5Qx2TlKWIA"

}

```

<b> You can then use access token to access portected resources. </b>  

  
  

# OAuth Flow