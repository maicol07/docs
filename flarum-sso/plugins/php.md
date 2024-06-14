---
title: PHP Plugin
description: PHP Plugin for Flarum SSO Extension
published: true
date: 2023-01-20T09:33:42.539Z
tags: flarum, php, sso, extension, jwt, auth, authentication
editor: markdown
dateCreated: 2020-10-27T09:12:48.855Z
---

# Requirements
- PHP 7.3+
- [Flarum SSO Extension](https://github.com/maicol07/flarum-ext-sso) installed on your Flarum

# Pre-installation
You need to create a random token (for security purposes it is better if this is long 40 characters, you can use [this tool](https://passgen.co/?pw=40&a=1) to make one) and put it into the `api_keys` table of your Flarum database.
You only need to set the `key` column and the `user_id` one. In the first one write your new generated token and in the latter your admin user id.

# Installation
The only supported way to install this plugin is through [Composer](https://getcomposer.org):
```
composer require maicol07/flarum-sso-plugin
```

### ⚠️ PHP versions support/drop notice
PHP versions will be supported until its [EOL](https://www.php.net/supported-versions.php).
If Flarum core changes PHP version before the official EOL, I'll update too the version accordingly to what they have chosen.

# Upgrading
Update composer packages as usual:
```
composer update
```
There may be some breaking changes in the update you are trying to install, so check the following version-specific upgrade notes to check how to handle these:

- [Version-specific upgrade notes](/en/flarum-sso/plugins/php/upgrade)
{.links-list}

# Configuration
I suggest you to follow the examples in the example folder along with the following steps. API documentation can be found [here](https://maicol07.github.io/flarum-sso-php-plugin/) or inside the `src/Flarum.php` class.

Basically, you need to do this:
1. Create a Flarum object with your configuration. See [here](#options) for options explanation.
```php
use Maicol07\SSO\Flarum;

$flarum = new Flarum($options);
```
> `$options` is an associative array with the options listed below
{.is-info}

2. Create the flarum user object.
```php
use Maicol07\SSO\User;

$user = $flarum->user($username) // Create the user object if it doesn't exists with the user method

// Retrieve the user object with the user method **after** its creation
$user_alias = $flarum->user(); // $user_alias contains the same object stored in $user
```
> `$username` is a string (the username of the user)
> `$flarum` is the Flarum object created in the previous step
{.is-info}

3. Now, time to fill in some user details.
```php
$flarum_user->attributes->email = 'user@example.com';
$flarum_user->attributes->password = 'userpassword';
```
You can check other attributes in the [API Docs](http://flarum-sso-php.api.docs.maicol07.it/Maicol07/SSO/User/Attributes.html) or via your IDE autocompletion. Note that extensions may add other attributes that are not listed natively.

4. If you want to use an addon, such as the Groups one, let's add it to the Flarum object and setup some groups for the user:
```php
use Maicol07\Flarum\SSO\Addons\Groups;

$flarum->loadAddon(Groups::class);
$flarum->setAddonProperties(Groups::class, ['set_groups_admins' => true]); // if the addon has some attributes they can be added through this method
$flarum_user->relationships->groups = ['group1', 'group2']
```
Like the attributes, there can be other relationships (that do not require an addon)

5. Do your action (login, logout or delete). There is a method in this lib for any of these actions. (Check the User methods in the [api docs](http://flarum-sso-php.api.docs.maicol07.it/Maicol07/SSO/User.html) (check the Traits for additional methods you can use))
```php
$flarum_user->login();
```

# Options
Here are explained the options of the plugin.
- **`url` - Flarum URL**: This is the URL of your Flarum public folder (the URL where you can see your Flarum)
- **`root_domain` - Root domain**: The domain of the root website (the SSO one, see [here]() for exceptions)
- **`api_key` - API Key**: The API key you have added in [pre-installation steps](#pre-installation).
- **`password_token` - Password Token**: Just a random string to encrypt Flarum passwords (you can use [this tool](https://passgen.co/?pw=32&sy=0) to generate one)
- **`remember` - Remember user**: Optional. This is set by default to `false`. It indicates if the token must be remembered across sessions or not (session periods are decided by Flarum. Currently they are 5 years with remembered session, else 1 hour renewable each time the user visits Flarum).
- **`verify_ssl` - Verify SSL**: Optional. This is set by default to `true`. Set this to `false` ONLY if you don't have an SSL certificate or you're developing on your local server such as XAMPP. You can also set this to the path of an SSL certificate.  More details on https://docs.guzzlephp.org/en/stable/request-options.html#verify.
- **`cookies_prefix` - Prefix for cookies name**: String to prefix the cookie name when creating remember/auth tokens. Default: "flarum"

# Available Addons
## Groups (Bundled)
This addon is bundled into the plugin. So no installation required, only add it via the `loadAddon` method and set its attribuetes, if required.

Feature: Sync groups of your Auth system with Flarum.

## JWT (Premium)
This addon is premium, which means that requires an active subscription to be used.

Feature: Use JWT (Json Web Token) instead of standard API Auth in order to gain more security during the authentication procedure.

### Attributes
- **`key`**: Random base64 encoded key to sign the JWT. You can generate one with this tool: [Cryptokey](https://github.com/AndrewCarterUK/CryptoKey)
- **`signer`**: Algorithm used to sign the JWT. This can be **an object** of any of the algorithm classes defined under the `Lcobucci\JWT\Signer\Hmac` namespace. Some accepted classes examples:
	- `Lcobucci\JWT\Signer\Hmac\Sha256` (default; fastest on x86 system)
  - `Lcobucci\JWT\Signer\Hmac\Sha384`
  - `Lcobucci\JWT\Signer\Hmac\Sha512` (stronger, fastest on x64 systems, slower on x86 ones)

### Pricing
You can pay with your preferred gateway.
- PayPal supports PayPal balance and Credict Cards
- Stripe supports Credit Cards and Bank Transfer (in some countries).
<div class="flex-payment-tables">
  <div>
    
#### Monthly
| Gateway name    | Stripe | PayPal |   |   |
|-----------------|--------|--------|---|---|
| Price           | 2,99 € | 2,99 € |   |   |
| Commissions     | 5%     | 10%    |   |   |
| Total Price     | 3,15 € | 3,29 € |   |   |
| Payment buttons | <a href="https://buy.stripe.com/dR63ev6sw2rG11ucMR" class="stripe-btn">Pay with Stripe</a> | <div id="paypal-buttons-monthly"></div> |  |   |
  </div>
  <div>
    
#### Yearly
| Gateway name    | Stripe  | PayPal  |   |   |
|-----------------|---------|---------|---|---|
| Price           | 29,99 € | 29,99 € |   |   |
| Commissions     | 5%      | 10%     |   |   |
| Total Price     | 31,49 € | 32,99 € |   |   |
| Payment buttons | <a href="https://buy.stripe.com/28o6qH7wAc2g4dG9AG" class="stripe-btn">Pay with Stripe</a> |   <div id="paypal-buttons-yearly"></div> |   |
  </div>
</div>

> Before buying read the [Pricing notes](#pricing-notes)!
{.is-info}

## Pricing notes
<small><i>Buying an addon includes proritary support via chat (1h/year) and proritary feature requests (1/year). The addon is valid only for one website (if you want to use this for multiple websites, you have to buy it again). Buying an addon multiple times will allow you to sum the addon features (for example, you buy 2 times an addon: you will be able to install the addon on two websites and you will get 2 hours/year of prioritary support via chat and 2 proritary feature requests/year) </i></small>

> If you want to cancel the subscription you can do it via your PayPal account if you paid with PayPal. Otherwise, check if there is an unsubscribe link in your stripe subscription confirmation email or contact the developer from his [website](https://maicol07.it) (chat in the bottom-right corner or contact form)
{.is-warning}

# Troubleshooting
## SSO and Flarum on subdomains
If you have an SSO system located on a subdomain (for example account.example.com) and your Flarum installed on another subdomain (`forum.example.com`) you must set the `root_domain` option to the root domain (example.com), not the subdomain (account.example.com). While this is possible, it's not possible to get this extension working on two different domains (example.com, example2.com) due to cookies limitation ([see here for more info](https://stackoverflow.com/a/6761443))

## Login fails
Do these checks (in order):
1. Is your Flarum extension installed and enabled?
Your setup must consist of these packages:
- `maicol07/flarum-ext-sso` must be installed in Flarum
- `maicol07/flarum_sso_plugin` must be installed in your auth system

2. Check if there is the `flarum_token` cookie in your Flarum cookies. If yes, then check the first step (if not already done) and proceed to the next step. If not, you probably have set something wrong in your config/options.

3. Check if your user credentials rules are compatible with Flarum ones. Detailed rules are listed in this issue: https://tracker.maicol07.it/issue/FSSOE-13
Flarum won't login the user if the credentials don't satisfy these rules. It is suggested to enforce these rules (or strictier ones) in your auth system.

4. If you're trying to login to Flarum with an user that existed before enabling SSO, you have to use the same Flarum password in your auth system. Otherwise, passwords will mismatch and login fails.

# Links
- [Github](https://github.com/maicol07/flarum-sso-php-plugin)
- [API Docs](https://flarum-sso-php.api.docs.maicol07.it)
{.links-list}

# Changelog
Major changes are marked with :exclamation:
Breaking changes are marked with :boom:

<a name="3.1"></a>
## [3.1](https://github.com/maicol07/flarum-sso-php-plugin/compare/3.0.1...3.1)

> Released on October 01, 2022
### ✨ Features
- [`b923d57`](https://github.com/maicol07/flarum-sso-php-plugin/commit/b923d5714300ef64b727acdd127d523b189585f8) ✨ Added ability to specify the cookie name prefix
  
    - Standardized the PHPDoc
  
### 🐛 Bug Fixes
  - [`fff7f46`](https://github.com/maicol07/flarum-sso-php-plugin/commit/fff7f46d1c46f4fc2635cf47e1088f7dd0047ae4) Fix API docs action config file path
  - [`3aad2d5`](https://github.com/maicol07/flarum-sso-php-plugin/commit/3aad2d5d3d5db6905446f78246d4a81317befe67) Fix API docs action name
  
### Other changes
  - [`f701ef4`](https://github.com/maicol07/flarum-sso-php-plugin/commit/f701ef4b3d0f0dcf4a9d233e369f716010808569) 👷 Added API docs action
  - [`fcf2d86`](https://github.com/maicol07/flarum-sso-php-plugin/commit/fcf2d86afad76a72f1e2a92a2112c010650d58c5) 👷 Added changelog generator action
  - [`d9a76dd`](https://github.com/maicol07/flarum-sso-php-plugin/commit/d9a76dd05711597b46d13d12beea06624ca57108) Delete issue template
  - [`85938c3`](https://github.com/maicol07/flarum-sso-php-plugin/commit/85938c3bfd24e21ef51152ee7f71fe5e69d0ef03) Don't allow new issues on Github
  - [`f3ed428`](https://github.com/maicol07/flarum-sso-php-plugin/commit/f3ed4281601ddc232bb2cbb4beffc49e7765a533) **deps:** Support Laravel 9

## [3.0.1](https://github.com/maicol07/flarum-ext-sso/compare/3.0...3.0.1)

> Released on May 22, 2021
{.is-info}

### 🐛 Bug Fixes

- [`0dd2465`](https://github.com/maicol07/flarum-ext-sso/commit/0dd2465d90f093cb18f889dad55de8da552d6275) 🐛 Groups created even if they already exists in Flarum

### Other changes

- [`f3c9c50`](https://github.com/maicol07/flarum-ext-sso/commit/f3c9c505336bd5ccd5081170f2dfdc9fb9b44244) 🔥 Removed set_groups_admins as no more used

  Implementation of this should be made by the user

## [3.0](https://github.com/maicol07/flarum_sso_php_plugin/compare/2.0...3.0)

> Released on April 06, 2021
{.is-info}

### ✨ Features
- [`d173cb1`](https://github.com/maicol07/flarum_sso_php_plugin/commit/d173cb1c2b6c5e048567f167a65d02e141456d5d) ✨ Addons can now specify what addons are required to be loaded before it
- [`c0dc540`](https://github.com/maicol07/flarum_sso_php_plugin/commit/c0dc5403ee64a9e89140def8ba81167b1b0b74c4) ✨ Allow to change the remember property via the `isSessionRemembered` method
- [`f152d95`](https://github.com/maicol07/flarum_sso_php_plugin/commit/f152d950cb79ca9af033a232b02c6126f6aae89c) ✨ 💥 New user() method
    - Replaces the current user object creation
	- User property is now private. You can only access to the user via this method
	- Improved examples
- [`99c0594`](https://github.com/maicol07/flarum_sso_php_plugin/commit/99c059459d1f264fd4c15d6162a7fbcef697e2d8) 💥 ✨ 🚚 Moved Addons and Cookies features to traits
    - Removed class cookie. Now all the necessary cookies are generated on the fly.
	- Addons initialization in the constructor is moved to the initAddons() method in the Addons trait.
	- Login: now the logout cookie is deleted (if it exists), the session token or remember token is created
	- Logout: now the session token and remember token cookies are deleted (if they exist), a new logout cookie (flarum_logout) is created.
	
	New methods:
	- setRememberCookie
	- deleteRememberCookie
	- setSessionTokenCookie
	- deleteSessionTokenCookie
	- setLogoutCookie
	- deleteLogoutCookie
	- generateCookie
	
	Renamed methods:
	- addAddon is now loadAddon
	- removeAddon is now unloadAddon
	
	Removed methods:
	- setCookie

- [`613b5b5`](https://github.com/maicol07/flarum_sso_php_plugin/commit/613b5b5317a174b74199c28e94a4515fb992f4fa) ✨ Added Remember me checkbox to example + some visual improvements
- [`5865c51`](https://github.com/maicol07/flarum_sso_php_plugin/commit/5865c51d8ae57ace218d8c6648afd0612902d4ce) ✨ Changed `lifetime` to `remember`
    Lifetime is deprecated in beta16.
	Remember should be set to true when you want to login the user with a "Remember me" option.
- [`edd34eb`](https://github.com/maicol07/flarum_sso_php_plugin/commit/edd34eb1356fe28baf77c54be1b69b8cb0e2bcbe) ✨ Initial attempt to beta16 compatibility
    - BREAKING CHANGE: 💥 Replaced the `lifetime` setting with `remember`
	- BREAKING CHANGE: 💥 Removed the `getLifeTimeSeconds` method
	- BREAKING CHANGE: 💥 PHP 7.3 required
	- WARNING! illuminate/support pinned to ^8 (removed support for Laravel 6 & 7)

### 📝 Documentation changes
- [`2b2dcb3`](https://github.com/maicol07/flarum_sso_php_plugin/commit/2b2dcb38b547443bfb73a72399fe4079a4ebd14b) 📝 Updated docs
- [`94a2a84`](https://github.com/maicol07/flarum_sso_php_plugin/commit/94a2a84d3e270190c2ad4d3adfe26dd9c26e700b) 📝 Updated docs
- [`37ce682`](https://github.com/maicol07/flarum_sso_php_plugin/commit/37ce682eadd9a4e2ee1fc24c3af49499909f8d06) 📝 Updated docs
	Now using doctum with a modified version of the Flarum docs theme
	- 🙈 Updated .gitignore
- [`7b7cbb8`](https://github.com/maicol07/flarum_sso_php_plugin/commit/7b7cbb8d95eb4ec2c37b32ae98842cd8c1ad541f) 📝 PHPDoc fix

### 🐛 Bug Fixes
- [`6c262d8`](https://github.com/maicol07/flarum_sso_php_plugin/commit/6c262d898f3ccc68ee20a93dbd0c12ebe4182236) 👽 Nickname attribute instead of display name

### 🔄 Updates
- [`443658d`](https://github.com/maicol07/flarum_sso_php_plugin/commit/443658d6ca64b7a22a443866a82f8361434f8096) 🔥 Removed the getForumLink
	URL is accessible via the url property
- [`3f31ea6`](https://github.com/maicol07/flarum_sso_php_plugin/commit/3f31ea614a15c9e409f6cd31dc0792801788b377) ✨ Updated user `update` method

    - ✨ Added check if id is set. If not set, it will be fetched automatically.
	- ✨ Response is now saved and passed as argument to the after_update method hook.
	- ✨ The method now returns a bool. True if the user has been updated (the response correctly reports the user id); false if the user can't be fetched (if the user id doesn't exists) or the response id is different from user id
- [`7b14a69`](https://github.com/maicol07/flarum_sso_php_plugin/commit/7b14a69e8b8853096a90e560957423f656d80ffa) 🚚 💥 Renamed the `fetchUser` method to simply `fetch`
- [`e9c1c9e`](https://github.com/maicol07/flarum_sso_php_plugin/commit/e9c1c9ed309ead47182e465284900c042f4edca4) 🚚 Moved and Renamed the Basic trait to the Auth trait in the Maicol07\SSO\User\Traits namespace
- [`80d1f7e`](https://github.com/maicol07/flarum_sso_php_plugin/commit/80d1f7eb6991b195eb2dcd2ee4438345cf9e1937) Minor improvements
- [`c1e71eb`](https://github.com/maicol07/flarum_sso_php_plugin/commit/c1e71eba648d64c2607eb4b7f3e1ea64db0fcc41) **addons:** 🚚 Renamed master property to flarum (consistency)
- [`e886c59`](https://github.com/maicol07/flarum_sso_php_plugin/commit/e886c596b6e6bdc9c8cf3543bffb732617c8e118) **example:** Updated example
- [`204e0a2`](https://github.com/maicol07/flarum_sso_php_plugin/commit/204e0a26d33e76c541d11c93d92a8e3f27537301) **examples:** ✨ Added users list on the delete page

### ♻ Code Refactoring
- [`7f5f752`](https://github.com/maicol07/flarum_sso_php_plugin/commit/7f5f752005d6645194a8445b0bdc7cb6e20453a6) ♻️ 🚚 Moved delete and update methods out of the basic trait
- [`e3c00de`](https://github.com/maicol07/flarum_sso_php_plugin/commit/e3c00dede0d2ee171e9a1a0d09a5dbcbf5601b40) ♻️ Refactor doctum.config.php
- [`a72432d`](https://github.com/maicol07/flarum_sso_php_plugin/commit/a72432d75f7b0b5c930fad708fcda143b0613347) ♻️ Refactor doctum.config.php
- [`6331b3c`](https://github.com/maicol07/flarum_sso_php_plugin/commit/6331b3c2fc92746c85a6e2bb8a9c6b931eceb8f2) ♻️ General refactor

### Rename
- [`768152a`](https://github.com/maicol07/flarum_sso_php_plugin/commit/768152a6cd531f7b8d460c0177352b3ad275e172) **addons:** 🚚 `setAddonAttributes` renamed to `setAddonProperties`

### 🎨 Code styling
- [`1b6448b`](https://github.com/maicol07/flarum_sso_php_plugin/commit/1b6448bd8f3a79c609e07c05ef8958735386ac4e) 💄 Minor example styling improvement

### 🔀 Pull Requests

- [`befa080`](https://github.com/maicol07/flarum_sso_php_plugin/commit/befa08099f3818c9c2e57ffec26a55c05ebd40ce) Merge pull request [#9](https://github.com/maicol07/flarum_sso_php_plugin/issues/9) from richstandbrook/patch-1
    - feat: ✨ Be able to programmatically signup users

## 2.0 - The modular update (2020-11-02)
:boom: Requires PHP 7.2+
### :heavy_plus_sign: Added
- :boom: Hooks/Addons system. Now you can install addons and add them to the PHP plugin! This way they can customize the behaviour of the plugin and add additional features.
- Compatibility with Laravel 8
- :boom: Changed constructor parameters format (now using an array)
- :boom: New User management (removed `removeGroups()` method. Edit user groups to remove them)
- :boom: `getUsersList()` now returns a Laravel collection instead of an array
- `getUsersList()` now supports more than one filter
- :boom: The old insecure mode is now the Verify SSL option. Its behaviour has changed, so check the docs to know what has changed

### :star: Improvements
- Revamped examples
- :memo: Revamped docs
- :zap: Optimized login (should be faster)

### :hammer_and_wrench: Fixed
- :bug: Redirect not working when no URL scheme is specified

### :fire: Removed
Removed `setCookie()` function 

## 1.2.2 (2020-08-27)
- Fixed a typo

## 1.2.1 (2020-08-27)
- Fixed "Unknown class" exception on new installations

## 1.2.0 (2020-04-22)
### :heavy_plus_sign: Added
- New parameter `$set_groups_user` to not set groups to an admin user (check API docs)
- Updated [API Docs](https://maicol07.github.io/flarum_sso_php_plugin)

### :star: Improvements
- Optimized/reduced login times
- Code style improvements

### :hammer_and_wrench: Fixes
- :exclamation: User can't login if his id was > 20
- Admin might not login in some cases

## 1.1.1 (2020-04-20)
- Changed license to MIT

## 1.1.0 (2020-04-20)
### :heavy_plus_sign: Added
- :exclamation: Update user username, email or password on Flarum (check API documentation)
- Set user groups on signup
- :exclamation: New API Docs: You can now check all the methods available and their related docs [here](https://maicol07.github.io/flarum_sso_php_plugin/)

### :star: Improvements
- Code style improvements

### :hammer_and_wrench: Fixes
- :exclamation: Fixed the `not_authenticated` error (https://discuss.flarum.org/d/21666-single-sign-on-sso-with-wordpress-integration/157)
- Fixed logout on Flarum (see [#FSSOE-1](https://bugs.maicol07.it/issue/FSSOE-1))

## 1.0 (2020-04-08)
- :boom: PHP 7+ required
- :boom: New request system: now using the great Flagrow API client
- New Cookie management: now using the awesome Cookie library by Delight-im
- New option: insecure mode (principally for local development, read in docs for more)
- Added groups settings for users: you can now set a group for a user and, if doesn't exists, it will be created!
- :boom: Deleted sendRequest and get methods as no more used.
- Code and performance improvements
- Various fixes (see also the bug tracker)

