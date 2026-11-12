# Changelog

Versions are the package version (`PLUGIN_VERSION` in the `Makefile`).
Each release is tagged `vX.Y.Z`; see `RELEASING.md`.

## 0.2.1

Security release on top of 0.2. The findings, and how each was checked, are
described in [Lachee/opnsense-oidc#18](https://github.com/Lachee/opnsense-oidc/pull/18).

Fixed:

- Reflected XSS in the callback's error page. Errors are now generic plain
  text, with details in the `oidc` log.
- Logins into `root`, disabled, expired and system accounts are refused.
- Only servers selected under System > Settings > Administration >
  Authentication can be used for GUI login, and only those get a button.
- The session id is regenerated at login, and logins are audit-logged.
- Email is no longer matched unconditionally: the username is matched first,
  email matching is opt-in, and an email is used only when the provider
  marks it `email_verified` (with an opt-out).
- The callback host is pinned instead of taken from the `Host` header.
- ID tokens are validated with the bundled firebase/php-jwt 7.2.0: only
  RS256/384/512 with the provider's RSA signing keys, `nonce`, `exp` and
  `iat` are required, and `azp` is checked.
- `state` is checked before the authorization code is redeemed. PKCE S256
  is used when the provider advertises it.
- The icon proxy only fetches https, follows no redirects, and limits size
  and content type.
- Request parameters are read from the query string only, so cookies can no
  longer break a login.
- `fetchURL()` is https-only and follows no redirects. `stripWellKnown()`
  no longer empties provider URLs without a `.well-known/` suffix.
- The "Test" dialog and endpoint: provider data is rendered as text, and the
  endpoint requires a full admin and https URLs.
- `preauth()` no longer causes a fatal error in a captive-portal zone.

Added:

- Optional "Subject binding", which ties a local account to the provider's
  `sub`. With automatic user creation, the binding is checked before the
  account is created.
- An offline test harness for ID-token validation (`test/`).

After upgrading, review the server's settings: email matching and unverified
email are off unless ticked, and the server must be selected for GUI
authentication.

## 0.2

Upstream's first release (`v0.2.0` in Lachee/opnsense-oidc).
