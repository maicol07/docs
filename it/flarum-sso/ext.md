---
title: Flarum SSO Extension
description: null
published: true
date: 2023-03-10T13:57:11.838Z
tags: null
editor: markdown
dateCreated: 2020-10-02T09:48:18.844Z
---

<div class="center">
  <img src="/flarum-sso/flarum_sso.png" alt="Flarum SSO">

![Supports latest Flarum version?](https://flarum-badge-api.davwheat.dev/v1/compat-latest/maicol07/flarum-ext-sso) [![Latest Stable Version](https://poser.pugx.org/maicol07/flarum-ext-sso/v)](//packagist.org/packages/maicol07/flarum-ext-sso) [![Total Downloads](https://poser.pugx.org/maicol07/flarum-ext-sso/downloads)](//packagist.org/packages/maicol07/flarum-ext-sso) [![Latest Unstable Version](https://poser.pugx.org/maicol07/flarum-ext-sso/v/unstable)](//packagist.org/packages/maicol07/flarum-ext-sso) [![License](https://poser.pugx.org/maicol07/flarum-ext-sso/license)](//packagist.org/packages/maicol07/flarum-ext-sso)

This extension equips Flarum with [Single Sign On (shortly SSO)](https://en.wikipedia.org/wiki/Single_sign-on). Basically, this extension will act as a bridge between Flarum and your SSO/Auth system. The extension is useful if you run Flarum on a subdomain but you want to use the login mechanism of your main website.

</div>

> If you have an OpenID Connect provider/auth system you can use my [OpenID Connect Client Extension](https://discuss.flarum.org/d/28245-flarum-openid-connect-client), that requires zero coding from your side. You only have to configure it and you're ready to go!
> {.is-info}

# Requirements

- PHP 7.3+
- PHP JSON extension enabled
- The following PHP extensions installed and enabled if you want to use JWT features (premium):
  - Hash
  - Mbstring
  - OpenSSL
  - Sodium

# How does it work?

> Workflow based on this [post](https://discuss.flarum.org/d/2808-how-i-implemented-cross-authentication-with-flarum).

The user wants to login to your Auth system. Once his login attempt is successful, a POST request is sent to Flarum API from one of the extension plugins to retrieve the user access token (verifying his credentials). He will be created in the Flarum database (through an API request) if he isn't signed up on Flarum.
Then, the access token is saved in a cookie to be used when the user visits Flarum (this cookie keeps the login active).

# Extension, Plugins & Addons

This section covers the difference between plugins and addons.

## Extension

This is the standard Flarum extension, installable on Flarum following the [Installation](#installation) instructions. This is required since it activates the user when he is added to the database, manage his logout, changes the Login and Signup links destination, ...

## Plugins

A plugin is a library/package that you install on your Auth system to communicate with Flarum. This allows you to login, signup, update or logout the user (_these are only examples, there may be other features along these ones_). Some plugins are already developed for you and needs only to add the proper settings, like the WordPress plugin.

Examples of plugins are the PHP and WordPress ones. You can find them in the "Plugins" section in the left sidebar.

## Addons

Addons are additional features that can be added to the plugin(s). The installation method changes with the plugin you are using. For example, with the PHP plugin you have to add an addon calling an object method, while on WordPress you need to install it through the plugins screen.

Examples of addons are the Groups and the JWT ones.

# Installazione

Install by executing the command below and activate the extension in Flarum Administration area.

```
composer require maicol07/flarum-ext-sso
```

You'll also need a plugin to get it work with your auth system. You can choose one from the "Plugins" section in the left sidebar.

### ⚠️ PHP versions support/drop notice

PHP versions will be supported until its [EOL](https://www.php.net/supported-versions.php).
If Flarum core changes PHP version before the official EOL, I'll update too the version accordingly to what they have chosen.

# Upgrading

Upgrade by executing the command below, like with every other extension.

```
composer update maicol07/flarum-ext-sso
```

# Configurazione

Here is the explanation of all the extension settings:

- **Signup URL**: URL where the user will be redirected when the Signup button is clicked

- **Login URL**: URL where the user will be redirected when the Login button is clicked

- **Logout URL**: URL where the user will be redirected when the Logout button is clicked

- **Manage account URL (available in 1.8+)**: URL where the user will be redirected whenthe Manage Account button is clicked. This button shows up in the user settings only if this setting has a valid URL.

- **Open account management in a new tab (available in 1.8+)**: Open the link of the Manage Account button in a new tab

- **Remove login button**: Removes the login button from Flarum frontend

- **Remove signup button**: Removes the signup button from Flarum frontend

- **Cookies name prefix**: Prefix of the token cookie. Set by default to "flarum"

- **Provider mode**: Enable this option to use the provider mode (SSO between Flarum instances).
  Allows other Flarum instances to login an user of this Flarum instance.
  This will disable the standard SSO feature with other websites.

Default values for WordPress are:

- **Signup url**: `https://example.com/wp-login.php?action=register`
- **Login url**: `https://example.com/wp-login.php?redirect_to=forum` (The `redirect_to=forum` part is important as it will redirect your users back to the forum)
- **Logout url**: `https://example.com/wp-login.php?action=logout`

## Addons related settings

### JWT Addon

- **Issuer domain**: The `iss` claim of your JWT. This is the domain that issues the JWT. Typically, this corresponds with the root domain.
- **Signing algorithm**: Algorithm used in the JWT addon to sign the token.
- **Signer key**: The base64 encoded signer key of your JWT. You can generate one with this tool: [Cryptokey](https://github.com/AndrewCarterUK/CryptoKey)

#### ⚠️ Package required

This addon requires the `lcobucci/jwt:>=4.1` package added (and installed) to Flarum `composer.json` to work.

## Provider mode

> Sponsored by @kuaza from https://sorucevap.com
> {.is-info}

You can enable provider mode to register Flarum instances as clients and login users with your provider Flarum account.
To do so, you must obtain an API key and a password token for each Flarum client you want to add (check in the [PHP plugin options](https://docs.maicol07.it/en/flarum-sso/plugins/php#options) how to obtain them).
Then you have to enable the "Provider mode" in the main flarum instance you want to be the provider and register clients with their parameters:

- **Name**: Name of the client Flarum instance
- **URL**: URL of the client Flarum instance
- **API Key**: URL of the client Flarum instance (refer to the PHP plugin instructions)
- **Password token**: Password token of the client Flarum instance (refer to the PHP plugin instructions)
- **Verify SSL**: Verify the SSL certificate of the client Flarum instance when making requests to it.

In the client Flarum you have to set it up normally, but you have to set the Cookie prefix setting this way: if the client name is filled in provider, then the cookie prefix is the client name, otherwise it will be equal to the value of the URL field of the client.

> Beware that using a client name that contains spaces or special characters might cause problems when an user tries to login!
> {.is-warning}

# Link

- [Forum thread](https://discuss.flarum.org/d/21666-php-and-wordpress-single-sign-on-sso)
- [Extiverse](https://extiverse.com/extension/maicol07/flarum-ext-sso)
- [Packagist](https://packagist.org/packages/maicol07/flarum-ext-sso)
- [Github](https://github.com/maicol07/flarum-ext-sso)
  {.links-list}

# Changelog

Major changes are marked with :exclamation:

<a name="1.11.5"></a>

## [1.11.5](https://github.com/maicol07/flarum-ext-sso/compare/1.11.4...1.11.5)

> Released on March 10, 2023

### 🐛 Bug Fixes

- [`2795bce`](https://github.com/maicol07/flarum-ext-sso/commit/2795bced519c1114f1e308f9e73fe0d228943a44) 🐛 Fix wrong event type when deleting an user

  ### 📝 Docs changes

- [`7fd176c`](https://github.com/maicol07/flarum-ext-sso/commit/7fd176c3ff98dba74ea84de9c299be6452b7d02a) 📝 Removed bazaar references

  ### Other changes

- [`91a8e68`](https://github.com/maicol07/flarum-ext-sso/commit/91a8e68bcde793d35565d9c625ca24937098464a) 🙈 Added composer.lock in .gitignore
  - [`f957f86`](https://github.com/maicol07/flarum-ext-sso/commit/f957f86063ee9b687b5916e6fea519b5bea5c273) Added IDE settings
  - [`d389a1c`](https://github.com/maicol07/flarum-ext-sso/commit/d389a1c5df684b1a55bfba9bbad4d97f4afd6b28) Added composer.lock
  - [`55baf16`](https://github.com/maicol07/flarum-ext-sso/commit/55baf1640071ebef30ca98b02b3e588be30aadca) **deps:** ⬆️ Upgraded dependencies
  ### ⏪ Reverts

- [`b109334`](https://github.com/maicol07/flarum-ext-sso/commit/b10933460fe2b438067ad0dd31e342ab615352fe) chore: Added composer.lock

  ```
  This reverts commit d389a1c5df684b1a55bfba9bbad4d97f4afd6b28.
  ```

<a name="1.11.4"></a>

## [1.11.4](https://github.com/maicol07/flarum-ext-sso/compare/1.11.3...1.11.4)

> Released on December 13, 2022

### ⚡ Performance Improvements

- [`f074256`](https://github.com/maicol07/flarum-ext-sso/commit/f074256583433e5600634fa6fa3f90188d69d6f3) ⚡ Improved responsiveness of settings page

### 🐛 Bug Fixes

- [`769495a`](https://github.com/maicol07/flarum-ext-sso/commit/769495aeeb47f55352550d7df7af6bfd0b0baaf4) 🐛 Fix initializer not loaded on page refresh
- [`2619af0`](https://github.com/maicol07/flarum-ext-sso/commit/2619af0f13b10a7e9c7aa51722fbf34fa845185c) 💬 Fix untraslated button

### Other changes

- [`168fa9d`](https://github.com/maicol07/flarum-ext-sso/commit/168fa9d8803be47ed05b86f69dc696de2c8ad4a7) Added casting to settings function
- [`5b9567c`](https://github.com/maicol07/flarum-ext-sso/commit/5b9567c53fdd60e32ccafcb48e0fc7e77476d806) **deps:** ⬆️ Updated dependencies

<a name="1.11.3"></a>

## [1.11.3](https://github.com/maicol07/flarum-ext-sso/compare/1.11.2...1.11.3)

> Released on November 16, 2022

### 🐛 Bug Fixes

- [`de71fb1`](https://github.com/maicol07/flarum-ext-sso/commit/de71fb1639a551e78af1aba3ab146281ebfd5add) Re-added missing Logout URL setting

<a name="1.11.2"></a>

## [1.11.2](https://github.com/maicol07/flarum-ext-sso/compare/1.11.1...1.11.2)

> Released on October 26, 2022

### ✨ Features

- [`8ea8f8f`](https://github.com/maicol07/flarum-ext-sso/commit/8ea8f8fed95b7ffd4a42cca4d2f5027f28eb3f27) ✨ Fix login redirects in embeded mode

  ### 🔀 Pull Requests

- [`da54638`](https://github.com/maicol07/flarum-ext-sso/commit/da546382d47199b79903f4fe58a249e32ffe4407) Merge pull request [#17](https://github.com/maicol07/flarum-ext-sso/issues/17) from ruslanbelziuk/embed_support

<a name="1.11.1"></a>

## [1.11.1](https://github.com/maicol07/flarum-ext-sso/compare/1.11...1.11.1)

> Released on October 21, 2022

### 🐛 Bug Fixes

- [`aa58e2b`](https://github.com/maicol07/flarum-ext-sso/commit/aa58e2bf0b1602c510df58b6472f38d9499ecdd1) 🐛 logout middleware redirect

### 🔀 Pull Requests

- [`86e0a09`](https://github.com/maicol07/flarum-ext-sso/commit/86e0a09080c117ccf2d5c03fc564f08989486bec) Merge pull request [#16](https://github.com/maicol07/flarum-ext-sso/issues/16) from ruslanbelziuk/fix_logout_middleware

<a name="1.11"></a>

## [1.11](https://github.com/maicol07/flarum-ext-sso/compare/1.10.3...1.11)

> Released on October 02, 2022

### ✨ Features

- [`2d7f772`](https://github.com/maicol07/flarum-ext-sso/commit/2d7f772a1a6223bc235ffb043bd398bee6a2f0af) ✨ Added provider mode

  ```
  Sponsored by @kuaza from https://sorucevap.com
  ```
- [`a15b3e9`](https://github.com/maicol07/flarum-ext-sso/commit/a15b3e94a8529695514bbb370d72728597adb290) ✨ Added settings page

### 🐛 Bug Fixes

- [`d10faf5`](https://github.com/maicol07/flarum-ext-sso/commit/d10faf5006a23baefff0d2b52037a59bf2be03b8) Fix method deprecation

### ♻ Code Refactoring

- [`f7e1368`](https://github.com/maicol07/flarum-ext-sso/commit/f7e1368b65bc01d1bf31c55f7f36b11ff699c916) 🎨 Formatted code
- [`eac0f58`](https://github.com/maicol07/flarum-ext-sso/commit/eac0f5814bd779a1e1a2b24b654a95bdacb3612e) ♻️ Standardized JS config and gitignore

### 👷 CI changes

- [`4674f1b`](https://github.com/maicol07/flarum-ext-sso/commit/4674f1b530c42285e2545514bcbc58f294427db2) Removed build action

  ```
  Already included in frontend
  ```
- [`bdf9671`](https://github.com/maicol07/flarum-ext-sso/commit/bdf9671f0a377f8977511c50d63f1716966c3581) 💚 Fix JS build action
- [`699ec7f`](https://github.com/maicol07/flarum-ext-sso/commit/699ec7f55dd5461f38d8bf8ed01bfccb49ab958b) Disable TS checks
- [`d771657`](https://github.com/maicol07/flarum-ext-sso/commit/d771657407f0e71004b938a9abcc2056dcf50fa2) Added backend action
- [`ff8df07`](https://github.com/maicol07/flarum-ext-sso/commit/ff8df07e5ecda4c2338c14c31e3c85f4fbcf9313) Improved changelog generation

### Other changes

- [`990e838`](https://github.com/maicol07/flarum-ext-sso/commit/990e83847c137b624c3cad6cb6b7510fd60ed0d6) Updated JS to TS, CI and config to latest update
- [`dc62612`](https://github.com/maicol07/flarum-ext-sso/commit/dc626126f9ac2b1227361290f6b0419df46dd0c2) **deps:** ⬆️ Upgraded dependencies
- [`ea3861b`](https://github.com/maicol07/flarum-ext-sso/commit/ea3861b8be3480b75fc077564dc9b224eb42718a) **deps:** ⬆️ Updated dependencies
- [`9a233fb`](https://github.com/maicol07/flarum-ext-sso/commit/9a233fb99c093a49b1bfc527441e5385a7bc8725) **deps:** ⬆️ Updated dependencies
- [`bec87c5`](https://github.com/maicol07/flarum-ext-sso/commit/bec87c59eab9b3ab98132152ef09094b34868a44) **deps:** Added bundlewatch

## [1.10.3](https://github.com/maicol07/flarum-ext-sso/compare/1.10.2...1.10.3)

> Released on October 21, 2021

### 🐛 Bug Fixes

- [`5b7afa3`](https://github.com/maicol07/flarum-ext-sso/commit/5b7afa3d0dbd9f57a481942f867a4bcdb3c1fc5c) Unblock other extensions routes

  It was impossible for other extensions, loaded after SSO to register new routes
- [`ecc0676`](https://github.com/maicol07/flarum-ext-sso/commit/ecc0676dbd3b2d4f6aed7a6a673865c28dd971ee) Signup button points to logout URL

## [1.10.2](https://github.com/maicol07/flarum-ext-sso/compare/1.10.1...1.10.2)

> Released on August 09, 2021
> {.is-info}

### ✨ Features

- [`fd9bc60`](https://github.com/maicol07/flarum-ext-sso/commit/fd9bc60f9eef914f637aecdc4fd6be16c0af4f88) ✨ Added Typescript config to get Flarum typing definitions
- [`8b70076`](https://github.com/maicol07/flarum-ext-sso/commit/8b70076deea992c7a69ae45a10d49077edaaef77) **code_tools:** ✨ Added Prettier instead of ESLint

### ⚡ Performance Improvements

- [`6fbd7f9`](https://github.com/maicol07/flarum-ext-sso/commit/6fbd7f990dd55f2a74dcd34e82f92521f55a1c65) ⚡ Better frontend settings helper

### 🐛 Bug Fixes

- [`3e24b49`](https://github.com/maicol07/flarum-ext-sso/commit/3e24b492adb415f6b6dd1508523421155dedefdf) New Discussion button opening the Login modal
  - Also did some major refactor
- [`f9cb9f4`](https://github.com/maicol07/flarum-ext-sso/commit/f9cb9f4c957fd0ffb9ee3b2b8ff88b19e3b1cd51) 🐛 Wrong app namespace

### ♻ Code Refactoring

- [`3dce5dc`](https://github.com/maicol07/flarum-ext-sso/commit/3dce5dc3b0d87dff62e7013b6adbe36519551823) Reformatted build action
- [`edbd507`](https://github.com/maicol07/flarum-ext-sso/commit/edbd507f36832ec95f88eeb83898408e42f804d2) ♻️ Removed old Webpack config
- [`f17a9a5`](https://github.com/maicol07/flarum-ext-sso/commit/f17a9a5057cb1241f9c56a9118a277ba5526bfa5) ♻️ Removed ESLint comments

### 👷 CI changes

- [`3233f94`](https://github.com/maicol07/flarum-ext-sso/commit/3233f943c4988562c5ce48c82eb58e2fdee3f7a5) Trigger changelog workflow when JS build has finished
- [`f244d0f`](https://github.com/maicol07/flarum-ext-sso/commit/f244d0f4d99085740c1ae633a0d1db3aeec3b069) Updated build action
- [`b8e32e2`](https://github.com/maicol07/flarum-ext-sso/commit/b8e32e23cd3f562af0de63082134504989dadfb6) 👷 Added Flarum Bot to automatically compile JS
- [`646203c`](https://github.com/maicol07/flarum-ext-sso/commit/646203ce9c6c98d9b9f6c747d82c420e36903c92) 👷 Added conditional commit messages to changelog action
- [`086bdea`](https://github.com/maicol07/flarum-ext-sso/commit/086bdeae4e9b61b79857eea269ff2640fd10c088) 👷 Updated changelog generation

### Other changes

- [`4145636`](https://github.com/maicol07/flarum-ext-sso/commit/41456364e5f6dc6b086eba83bb33beee4268a5fc) **deps:** ⬆️ Upgraded Flarum Webpack config
- [`3e99300`](https://github.com/maicol07/flarum-ext-sso/commit/3e993006fb9e97d839010ffa39d7939f8424a5e9) **meta:** Updated extension icon colors for better contrast

## [1.10.1](https://github.com/maicol07/flarum-ext-sso/compare/1.10...1.10.1)

> Released on June 12, 2021
> {.is-info}

### ⚡ Performance Improvements

- [`b74c90c`](https://github.com/maicol07/flarum-ext-sso/commit/b74c90cb94c6cf3323614228dcf600f3b1fdf7c4) ⚡ Use dot notation to set array value

### 🐛 Bug Fixes

- [`1f0c404`](https://github.com/maicol07/flarum-ext-sso/commit/1f0c404e6098ade4d8e18004f1621feed0bc6135) 🐛 JWT signup not working
- [`8b1673f`](https://github.com/maicol07/flarum-ext-sso/commit/8b1673fdfd0951c7e5206b8adab9696bc4bbc679) 🐛 login middleware redirect ([#12](https://github.com/maicol07/flarum-ext-sso/issues/12)) [[FSSOE-19](https://tracker.maicol07.it/issue/FSSOE-19)]

### 📝 Docs changes

- [`5e84bf4`](https://github.com/maicol07/flarum-ext-sso/commit/5e84bf42ceb3977329d5ded195f316294173d06c) 📝 Fixed PHPDocs

### 👷 Building scripts changes

- [`8f3d699`](https://github.com/maicol07/flarum-ext-sso/commit/8f3d69952c5565635eddab00f3a4f4963673ef83) Allow to run changelog action manually to set the next version
- [`5cb1154`](https://github.com/maicol07/flarum-ext-sso/commit/5cb1154d10ee0a86a4a25225316e1a08cde73de6) Add git credentials
- [`a10765e`](https://github.com/maicol07/flarum-ext-sso/commit/a10765e7d1900020e21247eff1427d068e52bb81) Add missing commit
- [`c009c2e`](https://github.com/maicol07/flarum-ext-sso/commit/c009c2e1b8b93099e06f824e376b27d6cb49a569) Add missing token to push back changes
- [`8ab1c6f`](https://github.com/maicol07/flarum-ext-sso/commit/8ab1c6ff262d0645dccb470962aff860e1393ffa) Push CHANGELOG.md back to repo
- [`b705c4f`](https://github.com/maicol07/flarum-ext-sso/commit/b705c4f391edfc94303e72b44a86de601ac7bbd0) Ensure changelog is saved to file
- [`ec11208`](https://github.com/maicol07/flarum-ext-sso/commit/ec11208a9a936b565f1c9f4bb7ffc5b8474d16bc) Fix changelog action branch
- [`801fc90`](https://github.com/maicol07/flarum-ext-sso/commit/801fc9073a4c1e06f55abcc0b923153e45db2a47) Missing checkout action

### Other changes

- [`69b9e7e`](https://github.com/maicol07/flarum-ext-sso/commit/69b9e7e21207a4213b8b4ea096f4777550fb7247) Added more author metadata to `composer.json`
- [`fcd76e6`](https://github.com/maicol07/flarum-ext-sso/commit/fcd76e6dd1bdc7fbeb0617dbcae781b12378c8d5) Added flarum version support badge
- [`5de2a76`](https://github.com/maicol07/flarum-ext-sso/commit/5de2a76cdbb8a2c85c36bd431e50183fda94d13a) **deps:** Moved JWT package to suggestions
- [`7a4ce8c`](https://github.com/maicol07/flarum-ext-sso/commit/7a4ce8c3fd2aa157fe4ea33748f8b359b0dec2b9) **locale:** Delete pl.yml ([#11](https://github.com/maicol07/flarum-ext-sso/issues/11))

## [1.10](https://github.com/maicol07/flarum-ext-sso/compare/1.9...1.10)

> Released on May 22, 2021
> {.is-info}

### ✨ Features

- [`eda72a0`](https://github.com/maicol07/flarum-ext-sso/commit/eda72a07455daf67a29c3b5b821db9ecff9f3e6c) [`5485b84`](https://github.com/maicol07/flarum-ext-sso/commit/5485b84fd2cdccb1ac5e892bf82e036d82d5cf39) [`5e58a02`](https://github.com/maicol07/flarum-ext-sso/commit/5e58a02a9c7d0d06433c730c8e13ec3672b8e5f3) ✨ Compatibility with Flarum 1.0

## [1.9](https://github.com/maicol07/flarum-ext-sso/compare/1.8.1...1.9)

> Released on April 08, 2021
> {.is-info}

### :sparkles: Features

- [`6c802df`](https://github.com/maicol07/flarum-ext-sso/commit/6c802dff583aa417dc009da4473a6df52b626459) :sparkles: Allow updating user avatar via `avatarUrl` attribute
- [`bc56ed3`](https://github.com/maicol07/flarum-ext-sso/commit/bc56ed373661a9512b2aba278e0d38feabb4f0d0) :sparkles: New Login middleware
- [`3e033f8`](https://github.com/maicol07/flarum-ext-sso/commit/3e033f8c8d1969152da8250ddcd900b552e76c2d) :sparkles: Initial compatibility with beta16

### :bug: Bug Fixes

- [`830def6`](https://github.com/maicol07/flarum-ext-sso/commit/830def6f9e6da958cc3bc82e3e3ebfee742c7ff7) :bug: Exception when updating user and `avatarUrl` is `null`
- [`978e1de`](https://github.com/maicol07/flarum-ext-sso/commit/978e1de28786a2219ec1c6f4b1396d41d31530ea) :bug: Fixed issue with the Laravel Cookie helper

### :zap: Performance Improvements

- [`3cf884f`](https://github.com/maicol07/flarum-ext-sso/commit/3cf884fe7a8b060cba274f46612287415aa1f4c8) :zap: Improved subscribers and listeners handling

### 🔄 Updates

- [`27b284e`](https://github.com/maicol07/flarum-ext-sso/commit/27b284eb0e6562906757c45ee71d3ee115627bc4) :boom: Updated JWT SSO to beta16
  Major changes:
  - 💥 Signer key must be plain text now. It will be encoded to base64 automatically
  - 💥 Login is no longer done with the login method (which is now named getToken) but will rely on the new middleware

### 👷 Building scripts changes

- [`da6515b`](https://github.com/maicol07/flarum-ext-sso/commit/da6515bdc70ebce4eacc7ebcaf7bcec1fa70fbd0) 💚 Fixed changelog generation
- [`563336b`](https://github.com/maicol07/flarum-ext-sso/commit/563336ba2b8980efee4a60c19dc28540b4bb46b4) 👷 Added CHANGELOG
- [`5ec3529`](https://github.com/maicol07/flarum-ext-sso/commit/5ec3529006686d44caae8a2b84bb0ca53d45a328) 📦 Missing bundled JS

### Other changes

- [`e0df539`](https://github.com/maicol07/flarum-ext-sso/commit/e0df5393aed028d510bb0fbacc0696b6f17e8b87) Updated composer.json metadata
- [`9ea9834`](https://github.com/maicol07/flarum-ext-sso/commit/9ea9834602a5497e109c6dc4e24cdd6799099eaa) **release:** 🔖 1.9 final changelog

## 1.8.1 (2021-01-24)

### :heavy_plus_sign: Added

- Added JWT Signing Algorithm option

### Changed

- Migrated to [lcobucci/jwt](https://github.com/lcobucci/jwt) v4 (compatible with v3.4.2 if already installed)

## 1.8 - Beta 15 support (2020-12-23)

### :heavy_plus_sign: Added

- :fire::exclamation: Added support for Beta 15 (now the extension **requires** beta 15 to run)
- :lipstick::exclamation: New settings page
- Added Manage Account button in user settings

### :sparkles: Improvements

- Improved code and removed outdated one

### :hammer_and_wrench: Fixed

- :bug: Fixed login modal showing up when user is not logged in and clicks the Start discussion button ([PR #7](https://github.com/maicol07/flarum-ext-sso/pull/7))
- Login modal not showing when extension is enabled but no login url is set

## 1.7 - Beta 14 support (2020-11-02)

### :heavy_plus_sign: Added

- :fire: :exclamation: Added support to Beta 14 (now the extension **requires** beta 14 to run)
- :key: Compatibility with Json Web Tokens

### :star: Improvements

- :memo: Revamped docs

### :hammer_and_wrench: Fixed

- :bug: :exclamation: Fixed logout ([#FSSOE-1](https://tracker.maicol07.it/issue/FSSOE-1))

## 1.6 - Beta 13 support (2020-05-13)

- :exclamation: Beta 13 support

## 1.5 (2020-04-08)

- Moved plugins into its own repos

## 1.4.6 (2020-03-09)

- Allow installation on beta 12

## 1.4.4 (2020-02-05) & 1.4.5 (2020-02-06)

- :bug: Fixed settings modal not showing up

## 1.4.3 (2020-01-20)

- Fix for version constaint

## 1.4.1 & 1.4.2 (2020-01-05)

- :exclamation: Critical fix to solve crash at Flarum startup
- Extension settings modal construction moved to @fof/components (thanks to @datitisev for its lib)
- Code enhancements

## 1.4.0 (2020-01-05)

- :exclamation: Added "Remove signup and login button" option in settings
- SSO won't work if a URL isn't set
- Fixed and changed icon

## 1.3.2 (2019-10-16)

- :exclamation: Fixed a critical issue that caused the extension not to work
- Updated sample website example

## 1.3.1 (2019-10-14)

- Minor tweak to `composer.json`

## 1.3.0 (2019-10-13)

- :exclamation: Added new `delete` method (not fully tested, but should working almost all the times)
- Added new `getFlarumLink` method that returns flarum link set in `config.php` file
- :exclamation: Added detailed comments in every method
- Added Italian translation
- :exclamation: Changed Curl requests with HTTP requests (Curl failed all the times with my tests)
- Minor tweaks and enhancements

## 1.2.0

- Compatibility with beta 8.1
- Added Polish language

## 1.1.2 (2017-08-09)

- Removed account section from settings

## 1.1.1 (2017-07-09)

- Login redirect

## 1.1.0 (2017-03-29)

- Added WordPress plugin

## 1.0.1 (2017-03-11)

- Added token lifetime to config
- Extended token lifetime

## 1.0 (2017-03-01)

- First version
