## 1. CSRF and the `state` Parameter

A Cross-Site Request Forgery attack on an OAuth flow works by tricking a victim's browser into completing an authorization flow that was initiated by the attacker. The attacker begins the OAuth process with a provider like GitHub, stops before the final redirect, and sends that redirect URL to the victim. When the victim clicks it, their browser completes the flow using the attacker's authorization code — linking the victim's account to the attacker's identity without their knowledge.

The `state` parameter prevents this by acting as a session-bound token. When the authorization request is initiated, the client generates a random value and stores it in the user's session. GitHub includes this value in the callback redirect. The client then verifies that the returned `state` matches the stored value before accepting the authorization code. Since the attacker cannot access the victim's session, they cannot forge a matching `state` value — the verification fails and the attack is blocked.

## 2. Redirect URI Attacks

A redirect URI attack exploits loose validation of the callback URL registered with the OAuth provider. Consider a scenario where a legitimate application registers `https://innovateinc.com` as its domain, and the authorization server validates only that the redirect URI starts with that domain — allowing any path beneath it.

An attacker could craft a redirect URI pointing to an open redirect or a path they control on the same domain, such as `https://innovateinc.com/logout?next=https://attacker.com`. When the authorization server redirects the victim after consent, the authorization code is appended to this URL. The open redirect then forwards the victim — and the authorization code in the URL parameters — to the attacker's server. The attacker captures the code from their server logs and exchanges it for an access token before the legitimate application can.

The fix is exact string matching on redirect URIs — not domain-only validation, not wildcard subdomains, and not prefix matching. The registered  URI and the requested URI must match character for character.

## 3. User Experience vs. Security

Adding third-party login like "Login with GitHub" significantly lowers friction for users — no password to create, no email to verify, and no credentials to forget. From a user experience standpoint, this is a clear win. However, it introduces a dependency on an external identity provider that the development team doesn't control.

The key trade-off is trust delegation. When a user authenticates via GitHub, the application is trusting GitHub to verify that user's identity. If GitHub experiences a breach, an outage, or changes its OAuth API, the application's entire authentication flow is directly impacted — through no fault of the application itself. Users who registered exclusively via GitHub have no fallback login method if the OAuth flow breaks.

A responsible implementation accounts for this by supporting both local and third-party authentication, linking accounts by email so users aren't locked out, and monitoring for provider-side changes that could affect the integration. Convenience for the user must be balanced against the application's responsibility to maintain reliable and secure access — regardless of what happens upstream.