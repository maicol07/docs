---
title: Flarum OpenID Connect Client
description: An extension for Flarum to connect to an OpenID Provider
published: true
date: 2023-06-30T10:14:02.374Z
tags: null
editor: markdown
dateCreated: 2021-07-19T18:20:38.774Z
---

<div class="center">
  <img src="https://extiverse.com/extension/maicol07/flarum-oidc-client/card.png" alt="Extiverse card">
  <img src="/flarum-oidc-client/flarum_oidc_client.png" alt="Flarum OpenID Connect Client logo" style="margin-top: 8px;">

![Supports latest Flarum version?](https://flarum-badge-api.davwheat.dev/v1/compat-latest/maicol07/flarum-oidc-client)

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

Button in login modal:
![login\_modal.png](/flarum-oidc-client/login_modal.png)

Buttons in user settings (non-SSO mode):
![settings\_buttons\_oauth.png](/flarum-oidc-client/settings_buttons_oauth.png)

# Requirements

- PHP 8.0+ (starting from v2.0)
- The following PHP extensions installed and enabled:
  - [json](https://www.php.net/manual/en/book.json.php)
  - [curl](https://www.php.net/manual/en/book.curl.php)

# Implemented OpenID Connect features

The extension relies on [`maicol07/oidc-client-php`](https://github.com/maicol07/oidc-client-php), a heavenly modified fork of [`JuliusPC/OpenID-Connect-PHP`](https://github.com/JuliusPC/OpenID-Connect-PHP). You can see a list of OIDC drafts and documents that have been implemented. However, the following features aren't already implemented in the extension at the time of writing (v1.0) and they can't be set/used:

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

### Provider settings

- **Provider URL**: The provider URL. This is only used to autodiscover the provider endpoints.

### Provider endpoints

**:warning: You don't have to fill these endpoints if your provider supports endpoints autodiscovery and you have filled the provider URL setting above**

- **Authorization endpoint**: URL where the extension will get the authorization code, needed to get the access token from the token endpoint
- **Token URL**: URL where the extension will get the access token, needed to get the access token from the userinfo endpoint
- **Userinfo URL**: URL where the extension will get the user infos, needed to get the access token from the token endpoint
- **End session (logout) URL**: URL where the extension will send the logout action to end user session in the provider

### Other settings

- **Issuer URL**: URL of the response payload issuer. This often matches the provider URL.
- **Deactivate the nonce verify**: Allow to skip the nonce verification. Can be useful if provider doesn't support it.
- **Linker claim**: Claim that helps matching Flarum users with provider users. If you don't know what to digit, fill it with `sub`.
- **Account management URL**: URL where the user will be redirected whenthe Manage Account button is clicked. This button shows up in the user settings only if this setting has a valid URL.
- **Open account management in a new tab**: Open the link of the Manage Account button in a new tab
- **SSO mode**: Bypass Flarum login/signup modal and redirect the user directly to the provider auth page. This also make OIDC the only option to login to Flarum.
- **Logout from provider (when SSO mode is enabled)**: Logout from OIDC provider when logging out from Flarum. This option will work only when SSO mode is enabled.
- **Remove signup button**: Removes the signup button from Flarum frontend
- **Sync user avatar**: Sync user avatars with the ones saved in the OIDC provider

# Troubleshoot

- **Error on signup/login: `Error: Call to undefined function GuzzleHttp\Psr7\stream_for() `**
  Solution: The extension should install the correct version of the dependency, so this error should be handled. To know more check https://discuss.flarum.org/d/28172-cant-upload-avatar/3

# Contact/Help

Since this is a premium extension, you also have premium support. So I'll help you in every error you're facing. You can contact me using the following services:

- Discuss post, check link below
- Live chat from my [personal webpage](https://maicol07.it)
- [Telegram](https://telegram.me/maicol07)
- Discord (`maicol07#6156`)

# Links

- [Forum thread](https://discuss.flarum.org/d/28245-flarum-openid-connect-client)
- [Extiverse](https://extiverse.com/extension/maicol07/flarum-oidc-client)
  {.links-list}

# Changelog

Major changes are marked with :exclamation:

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
