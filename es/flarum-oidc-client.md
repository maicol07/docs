---
title: Flarum OpenID Connect Client
description: An extension for Flarum to connect to an OpenID Provider
published: true
date: 2023-06-30T14:58:08.477Z
tags: null
editor: markdown
dateCreated: 2023-06-28T16:16:34.529Z
---

<div class="center">
  <img src="https://extiverse.com/extension/maicol07/flarum-oidc-client/card.png" alt="Extiverse card">
  <img src="/flarum-oidc-client/flarum_oidc_client.png" alt="Flarum OpenID Connect Client logo" style="margin-top: 8px;">

This extension enables users to login with an OpenID Connect (OIDC) provider. This method can be set to the only allowed method to login (SSO mode) or used as a complementary login method (like OAuth providers)

</div>

> This is a [Premium](https://extiverse.com/terms/premium-extensions) extension, not a free one. You can buy a license to use it in your Flarum through [Extiverse](https://extiverse.com/extension/maicol07/flarum-oidc-client)
> {.is-info}

## Why premium

Mostly for three reasons:

1. You can connect to any auth system, written in any language, as long as they are compliant with the OpenID Connect specs. My other [SSO extension](/en/flarum-sso/ext), which is free, allows you to connect to any auth system written in PHP.
2. It requires only configuration in the admin panel and zero code. The SSO extension requires integration with your auth system through plugins in addition to extension configuration.
3. If you want to use your auth system for other services/apps, OIDC is more reliable than my other SSO extension, since it adopts to an SSO standard other apps use.

## Screenshots

\|---|---|
\| Button in login modal | ![login\_modal.png](/flarum-oidc-client/login_modal.png) |
\| Buttons in user settings (non-SSO mode) | ![settings\_buttons\_oauth.png](/flarum-oidc-client/settings_buttons_oauth.png) |
\| Buttons in user settings (SSO mode) | ![settings\_buttons\_sso.png](/flarum-oidc-client/settings_buttons_sso.png) |

## Tested providers

> Note: This list is not exhaustive. Other generic OIDC providers should work as well.
> If you have tested this library with a provider not listed here, please contact me to add it here.

| Provider | Is tested? | Notes                                                         |
| -------- | ---------- | ------------------------------------------------------------- |
| Keycloak | ✅          | Client authenticator must be set to "Client id and secret"    |
| Casdoor  | ✅          | Code challenge must be set to S256 or PKCE should be disabled |

# Requirements

- PHP 8.1+
- The following PHP extensions installed and enabled:
  - [json](https://www.php.net/manual/en/book.json.php)
  - [mbstring](https://www.php.net/manual/en/book.mbstring.php)
  - [filter](https://www.php.net/manual/en/book.filter.php)
  - [ctype](https://www.php.net/manual/en/book.ctype.php)

Other extensions may be required due to third party dependencies. Check what composer says to know more.

> To make JWT operations faster you'll need the gmp or bcmath extension. Read [here](https://web-token.spomky-labs.com/introduction/pre-requisite) for more.
> {.is-info}

# Implemented OpenID Connect features

The extension relies on [`maicol07/oidc-client-php`](https://github.com/maicol07/oidc-client-php), a heavenly modified fork of [`JuliusPC/OpenID-Connect-PHP`](https://github.com/JuliusPC/OpenID-Connect-PHP). You can see a list of OIDC drafts and documents that have been implemented. However, the following features **aren't already implemented in the extension** at the time of writing (v3.0) and they can't be set/used:

- [OpenID Connect Dynamic Client Registration 1.0](https://openid.net/specs/openid-connect-registration-1_0.html)
- [RFC 7009: OAuth 2.0 Token Revocation](https://tools.ietf.org/html/rfc7009)
- [RFC 7662: OAuth 2.0 Token Introspection](https://tools.ietf.org/html/rfc7662)
- [Draft: OAuth 2.0 Authorization Server Issuer Identifier in Authorization Response](https://tools.ietf.org/html/draft-ietf-oauth-iss-auth-resp-00)

# How does it work?

The extension uses the authorization code flow variant of OpenID Connect. I suggest checking these resources to learn more about OIDC flow here:

- [OpenID Connect website](https://openid.net/connect/)
- [Explanation on the different OIDC flows](https://www.scottbrady91.com/OpenID-Connect/OpenID-Connect-Flows)
- [Illustrated flow example](https://developer.okta.com/blog/2019/10/21/illustrated-guide-to-oauth-and-oidc)
- [Video about OIDC flow](https://www.youtube.com/watch?v=S_sVBpEI-WQ)

## Will it work on WordPress and other CMS?

Yes, as long as you're using a plugin that provides OpenID Connect features. For WordPress, you can try this one (not tested): https://wordpress.org/plugins/miniorange-oauth-20-server/

## Do you want to disable standard signup and login?

Use this extension: [Third Party Login Only](https://discuss.flarum.org/d/28424-third-party-login-only-block-standard-login)
This way, you can only login/signup through OIDC.

> Don't enable the "Replace Sign In and Sign Up button with FoF Passport login (oAuth)" option in the Third Party Login Only since it doesn't work with this extension. Instead, you can achieve the same result with the "SSO Mode" (see [Configuration](#Configuration) below)
> {.is-warning}

# Installation

1. Be sure to check Extiverse instructions in your [subscriptions page](https://extiverse.com/premium/subscriptions) on how to install a premium extension via composer.json
2. Install by executing the command below and activate the extension in Flarum Administration area.

```
composer require maicol07/flarum-oidc-client:*
```

### ⚠️ PHP versions support/drop notice

PHP versions will be supported until its [EOL](https://www.php.net/supported-versions.php).
If Flarum core changes PHP version before the official EOL, I'll update too the version accordingly to what they have chosen.

# Upgrading

Upgrade by executing the command below, like with every other extension.

```
composer update maicol07/flarum-oidc-client:*
```

# Configuration

Here is the explanation of all the extension settings:

- **OpenID Connect provider name**: The name of your OpenID Connect provider. This will be shown on buttons in the login/signup modal and user settings.

### Client settings

- **Client ID**: The client ID your provider assigned you.
- **Client secret**: A secret key your provider assigned you when you have registered the client (Flarum).
- **Enable PKCE**: Use PKCE in your OIDC flow to mitigate authorization code interception attacks. [Learn more (RFC 7636)](https://datatracker.ietf.org/doc/html/rfc7636)
- **Enable nonce verification**: Verify the nonce in the OIDC flow to mitigate replay attacks. [Learn more (OIDC specs)](https://openid.net/specs/openid-connect-core-1_0.html#IDToken)
- **HTTP proxy**: HTTP proxy to use in requests to the provider. [Learn more](https://docs.guzzlephp.org/en/stable/request-options.html#proxy)
- **Certificate path**: Self-signed certificate path to use in requests to the provider. [Learn more](https://docs.guzzlephp.org/en/stable/request-options.html#cert)
- **Verify SSL**: Verify the SSL certificate of the provider during requests (requests will fail if the SSL certificate is not valid). [Learn more](https://docs.guzzlephp.org/en/stable/request-options.html#verify)
- **Requests timeout**: Timeout of the requests to the server. [Learn more](https://docs.guzzlephp.org/en/stable/request-options.html#timeout)
- **Use implicit flow**: Use implicit flow instead of the standard OIDC flow. This has been recognized as insecure and therefore should not be used anymore. [Learn more (OIDC specs)](https://openid.net/specs/openid-connect-core-1_0.html#ImplicitFlowAuth)
- **Skip email verified claim check**: Skip setting the email verification status property for the OIDC user if the provider doesn't send the `email_verified` claim. This can be useful for providers that are not fully compliant with the OIDC specs. [Learn more](https://openid.net/specs/openid-connect-core-1_0.html#StandardClaims)

### Provider settings

- **Provider URL**: The provider URL. This is only used to autodiscover the provider endpoints. If the URL is valid and the autodiscovery exists, some other fields will be discovered automatically.

> You don't have to fill manually the autodiscovered fields. Only fill them if they are not autodiscovered or if you want to override the autodiscovered value.
> {.is-warning}

- **Authorization endpoint URL**: URL to get the authorization code, needed to get the access token from the token endpoint
- **Token endpoint URL**: URL to get the access token, needed to get the access token from the userinfo endpoint
- **Userinfo endpoint URL**: URL to get the user infos, needed to get the access token from the token endpoint
- **End session (logout) endpoint URL**: URL to send the logout action to end user session in the provider
- **JWKS endpoint URL**: URL to get the JSON Web Keys necessary to decode the JWT token received from the token endpoint
- **Issuer URL**: URL of the issuer of the JWT provider. This must be equal to the iss value of the JWT (often autodiscovered) and often equals to the provider URL.
- **Code challenge method**: Method for the code challenge verification. S256 is more secure and therefore is recommended, but some providers might have not implemented it or they use a PLAIN code challenge.
- **Response type of provider to authorization**: The value type returned by the provider in the authorization endpoint.
- **ID token signing algorithm**: Algorithm used for signing the ID token. Should be paired with JWKS if you are using an asymmetric algorithm (i.e. RSA)
- **Authorization response iss parameter supported**: Whether the provider returns the issuer value (identified by `iss`) in the authorization endpoint response.
- **Token endpoint auth method**: Authentication method to the token endpoint. Note that some methods might not be fully supported.

### Other settings

- **Name of the provider to display on OIDC-related buttons**: Display name of the OIDC provider to use in buttons\*\*
- **Linker claim**: Claim that helps matching Flarum users with provider users. If you don't know what to digit, fill it with `sub`.
- **Account management URL**: URL where the user will be redirected whenthe Manage Account button is clicked. This button shows up in the user settings only if this setting has a valid URL.
- **Open account management in a new tab**: Open the link of the Manage Account button in a new tab
- **SSO mode**: Bypass Flarum login/signup modal and redirect the user directly to the provider auth page. This also make OIDC the only option to login to Flarum.
- **Logout from provider (when SSO mode is enabled)**: Logout from OIDC provider when logging out from Flarum. This option will work only when SSO mode is enabled.
- **Remove signup button**: Removes the signup button from Flarum frontend
- **Sync user avatar**: Sync user avatars with the ones saved in the OIDC provider
- **JWT time drift**: Period of time after the JWT expiration time that allows them to be used. (i.e. JWT expiration: 15:06, time drift: 3000 --> JWT is allowed until: 15:06)

# Troubleshoot

- **Error on signup/login: `Error: Call to undefined function GuzzleHttp\Psr7\stream_for() `**
  Solution: The extension should install the correct version of the dependency, so this error should be handled. To know more check https://discuss.flarum.org/d/28172-cant-upload-avatar/3

# Contact/Help

Since this is a premium extension, you also have premium support. So I'll help you in every error you're facing. You can contact me using the following services:

- Discuss post, check link below
- Live chat from my [personal webpage](https://maicol07.it)
- [Telegram](https://telegram.me/maicol07)
- Discord (`maicol07`)

# Links

- [Forum thread](https://discuss.flarum.org/d/28245-flarum-openid-connect-client)
- [Extiverse](https://extiverse.com/extension/maicol07/flarum-oidc-client)
  {.links-list}

# Changelog

Major changes are marked with :exclamation:

## 3.1 (2023-06-30)

- fix: Avatar uploading from user profile isn't disabled when syncing avatars

## 3.0 (2023-06-30)

### ✨ Features

- `b0dcf6c` ✨ Disable logout switch if SSO mode isn't enabled
- `8760f75` ✨ Added `email_verified` claim skip verification
- `f48608d` ✨ :exclamation: Added new SettingsPage
- `2591623` ✨ ⬆️ :exclamation: Upgraded to OIDC client v3
  Highlights:
  - :exclamation: Replaced lcobucci/jwt with web-token/jwt-\* (JWKS support!)
  - :exclamation: Enhanced autodiscovery and JWT validation (should be more resilient now)
  - PHP mbstring extension is required for JWT operations
  - PHP 8.1 enums are used for some configuration properties

### ⚡ Performance Improvements

- `6208ef6` ⚡ Removed other save calls
- `bccc86b` ⚡ Performance improvements

### 🐛 Bug Fixes

- `24951c3` Fix autodiscovery request
- `be2f738` Raised missing registered event
- `91a67a1` `joined_at` field unfilled
- `6396480` `joined_at` field unfilled
- `3daf94e` Exception when nickname extension is not enabled
- `d0de966` 🐛 Exception when the login button is clicked and the "Remove signup button" option is enabled
- `b6bac18` 🐛 Fixed a rare bug when logging in

  An exception was thrown if the user followed this steps:

  - Settings -> Connect to OIDC
  - Logout
  - Login
- `d9f0962` Restore missing provider name setting
- `245ddb7` Enforce correct types via casting
- `27c0b15` Fix default settings not working
- `3b34245` Enable buttons only when connected to OIDC provider
- `b93f683` 🐛 Manage account button not working on normal click
- `b17b439` 🐛 Remove change email and password buttons

### ♻ Code Refactoring

- `384f4fe` ♻️ Use constructor properties

### 📝 Docs changes

- `889e55a` 📝 Updated README
- `6710672` 📝 Updated docs link

### 👷 Building scripts changes

- `b43e965` 📦 Updated compiled files
- `3f30e75` Added builded files

### 👷 CI changes

- `1bbd69d` Added changelog generator
- `dd18780` 💚 Fix failing workflow

### Other changes

- `135822d` 🌐 Updated translation
- `b43766a` **deps:** ⏪ Reverted to Typescript 4
- `6044432` **deps:** ⬆️ Upgraded and cleaned dependencies

## 2.0.2 (2022-12-13)

- fix: Fix settings not working

## 2.0.1 (2022-12-12)

- fix: 🐛 Fix avatars sync when picture isn't provided
- perf: Optimized code

## 2.0 (2021-10-18)

- The internal OpenID Connect Client, which you can find [here](https://github.com/maicol07/oidc-client-php), has got a major refactor. Due to this big change, the **minimum PHP version has been bumped to PHP 8.0**
  - This will improve the overall performance of the extensions, powered by new PHP 8 features!

## 1.0.2 (2021-09-27)

- fix: 🐛 Fixed exception when not inserting provider URL
  The issue was caused by a check via autodiscovery about PKCE methods

## 1.0.1 (2021-07-21)

- Added more metadata to composer.json
- Updated README (also on Extiverse)

## 1.0 (2021-07-20)

- First version
