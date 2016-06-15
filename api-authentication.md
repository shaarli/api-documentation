# Shaarli API Authentication

## Assumptions and preconditions

1. there is a webserver,
2. there is a shaarli instance,
3. it should be **KISS** – both on server and client side,
4. it should be **as secure as shaarli** itself (ban after fails),
5. there is only **one user**, so there's no finer authorisation

## Approaches

1. Webserver does the auth or
2. get a token from the shaarli webinterface or
3. do a full cycle auth with uid+pwd and e.g. HTTP Digest, OAuth2 or Kerberos.

I'd propose 2., salt the token and send it in a custom HTTP Request Header, e.g. `SHAARLI_SALTED_AUTH_TOKEN`
