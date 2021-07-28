---
title: OpenID Connect vs Oauth
date: 2021-07-23
tags: ["network"]
---


# [OpenID Connect - OAuth 2.0 Simplified](https://www.oauth.com/oauth2-servers/openid-connect/)
- OAuth 2.0 does not provide a mechanism to say who a user is or how they authenticated, it just says that a user delegated an application to act on their behalf.
- OpenID Connect takes the OAuth 2.0 framework and adds an identity layer on top. It provides information about the user, as well as enables clients to establish login sessions. 



## [Authorization vs Authentication - OAuth 2.0 Simplified](https://www.oauth.com/oauth2-servers/openid-connect/authorization-vs-authentication/)
- OAuth 2.0 was intentionally designed to provide authorization without providing user identity and authentication, as those problems have very different security considerations that don’t necessarily overlap with those of an authorization protocol.
- Treating authentication and identity separately allows the OAuth 2.0 framework to be used as part of building an authentication protocol.



## [Building an Authentication Framework - OAuth 2.0 Simplified](https://www.oauth.com/oauth2-servers/openid-connect/building-an-authentication-framework/)
- Typically when a single provider attempts to add things to OAuth 2.0 to create an authentication and identity protocol, this results in another snowflake API with varying degrees of security.
- OpenID Connect takes the shared knowledge gained from many different implementations and standardizes it into a protocol suitable for enterprise grade implementations.



## [ID Tokens - OAuth 2.0 Simplified](https://www.oauth.com/oauth2-servers/openid-connect/id-tokens/)
- OpenID Connect’s ID Tokens take the form of a JWT (JSON Web Token), which is a JSON payload that is signed with the private key of the issuer, and can be parsed and verified by the application.
- [OpenID Connect  |  Google Identity  |  Google Developers](https://developers.google.com/identity/protocols/oauth2/openid-connect)


# Reference
- [OpenID Connect debugger](https://oidcdebugger.com/)
- [OAuth 2.0 Playground](https://www.oauth.com/playground/)
- [Signing in with Google - OAuth 2.0 Simplified](https://www.oauth.com/oauth2-servers/signing-in-with-google/)
- [Spring Security and OpenID Connect | Baeldung](https://www.baeldung.com/spring-security-openid-connect)
- [Authenticate with a backend server  |  Google Sign-In for Websites](https://developers.google.com/identity/sign-in/web/backend-auth)
- [Google Sign-In for Websites: Authentication with backends - YouTube](https://www.youtube.com/watch?v=j_31hJtWjlw&t=1s)
- [OpenID Connect  |  Google Identity  |  Google Developers](https://developers.google.com/identity/protocols/oauth2/openid-connect#discovery)

