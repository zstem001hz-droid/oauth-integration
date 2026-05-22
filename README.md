# OAuth Integration

A written reflection on real-world OAuth 2.0 vulnerabilities and security considerations for production implementations.

## Reading Assignment

[Common OAuth Vulnerabilities — Doyensec Blog](https://blog.doyensec.com/2025/01/30/oauth-common-vulnerabilities.html)

## Reflection Questions

1. **CSRF and the `state` Parameter** — Explain how an attacker could perform 
a CSRF attack on an OAuth flow and how the `state` parameter prevents it.

2. **Redirect URI Attacks** — Describe a hypothetical scenario where leaky 
`redirect_uri` validation could be exploited to steal an authorization code.

3. **User Experience vs. Security** — Describe one key trade-off a development 
team must consider when deciding to implement OAuth.

## Submission

- [reflection.md](./reflection.md)