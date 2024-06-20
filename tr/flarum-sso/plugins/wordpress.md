---
title: WordPress Plugin
description: WordPress Plugin for Flarum SSO Extension
published: true
date: 2023-01-20T09:31:38.844Z
tags: flarum, php, sso, jwt, auth, authentication, wordpress, wp
editor: markdown
dateCreated: 2020-10-28T08:21:13.521Z
---

# Requirements

- PHP 7.3+
- PHP JSON Extension
- [Flarum SSO Extension](https://github.com/maicol07/flarum-ext-sso) installed on your Flarum

# Pre-installation

You need to create a random token (for security purposes it is better if this is long 40 characters, you can use [this tool](https://passgen.co/?pw=40\&a=1) to make one) and put it into the `api_keys` table of your Flarum database.
You only need to set the `key` column and the `user_id` one. In the first one write your new generated token and in the latter your admin user id.

# Installation

Before the following steps, be sure to have done pre-installation steps.

### Automatic installation

You can find the plugin in the [WordPress plugin Directory](https://wordpress.org/plugins/sso-flarum/).
So you can download it from your WP site in Plugins --> Add new.
**Warning! This method can fail! If the plugin activation fails then try one of the methods below**

### Manual installation

Download the zip from the [releases page](https://github.com/maicol07/flarum_sso_wp_plugin/releases) and upload it to WordPress.

### Manual installation with manual dependencies installation

1. Download the plugin from the [WP plugin repo](https://github.com/maicol07/flarum_sso_wp_plugin) via WP dashboard or manually via the browser (in this case you need to extract the zip into the plugin folder (`/wp-content/plugins/`) of your WordPress instance and rename the folder to `sso-flarum`).
2. You have to install the dependencies with Composer [(?)](https://github.com/delight-im/Knowledge/blob/master/Composer%20\(PHP\).md). So execute this command on your server (you can also do this on your local machine before uploading to the server) in the plugin folder:

```
composer install --no-dev
```

3. Upload the compiled plugin to your WordPress plugin
4. Activate the plugin from the Plugins menu.

### ⚠️ PHP versions support/drop notice

PHP versions will be supported until its [EOL](https://www.php.net/supported-versions.php).
If Flarum core changes PHP version before the official EOL, I'll update too the version accordingly to what they have chosen.

# Upgrading

You currently have two ways to upgrade the plugin. If there are breaking changes in the version you want to update to, you may also have to follow the version-specific upgrade notes below.

## Auto upgrade

Upgrade the plugin like any other plugin from the WordPress Admin Area.

## Manual upgrade

Download the zip from the [releases page](https://github.com/maicol07/flarum_sso_wp_plugin/releases) and upload it to WordPress Add Plugin page (if WP < 5.5 you must have installed the [ZIP upgrading plugin](https://wordpress.org/plugins/update-theme-and-plugins-from-zip-file)) or unzip it and upload the files/folders to the server in directory `YOUR_WP/wp-content/plugins/sso-flarum`

## Version-specific upgrade notes

### 1.x to 2.0

- You might have to check if the Verify SSL is correctly set to the right value. If the insecure option was enabled on 1.x, then you need to disable it. Otherwise enable it.
- If you bought a PRO key to use Memberpress features, you have to add it to the new Memberpress license key option.

The other breaking changes from PHP plugin are already added to the plugin, so you don't have to worry about them!

# Configuration

You can open settings of the plugin from the Settings --> Flarum SSO plugin menu. See below for help about settings.
Here are explained the options of the plugin.

- **Enable SSO**: Mark this checkbox to enable SSO
- **Flarum URL**: This is the URL of your Flarum public folder (the URL where you can see your Flarum)
- **Root domain**: The domain of the root website (the SSO one, see here for exceptions)
- **API Key**: The API key you have added in [pre-installation steps](#pre-installation).
- **Password Token**: Just a random string to encrypt Flarum passwords (you can use [this tool](https://passgen.co/?pw=32\&sy=0) to generate one)
- **Verify SSL**: Optional. This is set by default to `true`. Set this to `false` ONLY if you don't have an SSL certificate or you're developing on your local server such as XAMPP.  More details on https://docs.guzzlephp.org/en/stable/request-options.html#verify.
- **SSL Certificate absolute path**: Optional. If you have an SSL certificate you would like to use to verify SSL (see details in the link above) you can enter its absolute path here. If you'd like to use this you must disable the Verify SSL option.
- **Don't sync users avatars**: Don't sync users avatars when logging in and updating the user profile.

# Available Addons

## Memberpress (Premium)

This addon is premium, which means that requires an active subscription to be used.

Feature: Sync users' memberships with Flarum.

### Options

- **License key**: License key for this addon (your subscription ID)
- **Set groups for admins**: Set groups also for admins, or only other users.

### Pricing

You can pay with your preferred gateway.

- PayPal supports PayPal balance and Credict Cards
- Stripe supports Credit Cards and Bank Transfer (in some countries).

<div class="flex-payment-tables">
  <div>

#### Monthly

| Gateway name    | Stripe                                                                                     | PayPal                                              |
| --------------- | ------------------------------------------------------------------------------------------ | --------------------------------------------------- |
| Price           | 3,49 €                                                                                     | 3,49 €                                              |
| Commissions     | 5%                                                                                         | 10%                                                 |
| Total Price     | 3,69 €                                                                                     | 3,84 €                                              |
| Payment buttons | <a href="https://buy.stripe.com/dR67uL6sw5DSbG8cMN" class="stripe-btn">Pay with Stripe</a> | <div id="paypal-buttons-memberpress-monthly"></div> |

  </div>
  <div>

#### Yearly

| Gateway name    | Stripe                                                                                     | PayPal                                             |
| --------------- | ------------------------------------------------------------------------------------------ | -------------------------------------------------- |
| Price           | 34,99 €                                                                                    | 34,99 €                                            |
| Commissions     | 5%                                                                                         | 10%                                                |
| Total Price     | 36,74 €                                                                                    | 38,49 €                                            |
| Payment buttons | <a href="https://buy.stripe.com/5kA8yPg361nC5hK8wy" class="stripe-btn">Pay with Stripe</a> | <div id="paypal-buttons-memberpress-yearly"></div> |

  </div>
</div>

> Before buying read the [Pricing notes](#pricing-notes)!
> {.is-info}

## JWT (Premium)

This addon is premium, which means that requires an active subscription to be used.

Feature: Use JWT (Json Web Token) instead of standard API Auth in order to gain more security during the authentication procedure.

### Attributes

- **License key**: License key for this addon (your subscription ID)
- **Signer key**: Random key to sign the JWT

### Pricing

You can pay with your preferred gateway.

- PayPal supports PayPal balance and Credict Cards
- Stripe supports Credit Cards and Bank Transfer (in some countries).

<div class="flex-payment-tables">
  <div>

#### Monthly

| Gateway name    | Stripe                                                                                     | PayPal                                      |
| --------------- | ------------------------------------------------------------------------------------------ | ------------------------------------------- |
| Price           | 3,49 €                                                                                     | 3,49 €                                      |
| Commissions     | 5%                                                                                         | 10%                                         |
| Total Price     | 3,69 €                                                                                     | 3,84 €                                      |
| Payment buttons | <a href="https://buy.stripe.com/aEU7uL0487M039C148" class="stripe-btn">Pay with Stripe</a> | <div id="paypal-buttons-jwt-monthly"></div> |

  </div>
  <div>

#### Yearly

| Gateway name    | Stripe                                                                                     | PayPal                                     |
| --------------- | ------------------------------------------------------------------------------------------ | ------------------------------------------ |
| Price           | 34,99 €                                                                                    | 34,99 €                                    |
| Commissions     | 5%                                                                                         | 10%                                        |
| Total Price     | 36,74 €                                                                                    | 38,49 €                                    |
| Payment buttons | <a href="https://buy.stripe.com/4gw2ar18ceaodOgbIL" class="stripe-btn">Pay with Stripe</a> | <div id="paypal-buttons-jwt-yearly"></div> |

  </div>
</div>

> Before buying read the [Pricing notes](#pricing-notes)!
> {.is-info}

## Pricing notes

<small><i>Buying an addon includes proritary support via chat (1h/year) and proritary feature requests (1/year). The addon is valid only for one website (if you want to use this for multiple websites, you have to buy it again). Buying an addon multiple times will allow you to sum the addon features (for example, you buy 2 times an addon: you will be able to install the addon on two websites and you will get 2 hours/year of prioritary support via chat and 2 proritary feature requests/year) </i></small>

> If you want to cancel the subscription you can do it via your PayPal account if you paid with PayPal. Otherwise, check if there is an unsubscribe link in your stripe subscription confirmation email or contact the developer from his [website](https://maicol07.it) (chat in the bottom-right corner or contact form)
> {.is-warning}

# Troubleshooting

## SSO and Flarum on subdomains

If you have an SSO system located on a subdomain (for example account.example.com) and your Flarum installed on another subdomain (`forum.example.com`) you must set the _Root domain_ option to the root domain (example.com), not the subdomain (account.example.com). While this is possible, it's not possible to get this extension working on two different domains (example.com, example2.com) due to cookies limitation ([see here for more info](https://stackoverflow.com/a/6761443))

## Login fails

Do these checks (in order):

1. Is your Flarum extension installed and enabled?
   Your setup must consist of these packages:

- `maicol07/flarum-ext-sso` must be installed in Flarum
- The WP plugin must be installed in your WP site.

2. Check if there is the `flarum_token` cookie in your Flarum cookies. If yes, then check the first step (if not already done) and proceed to the next step. If not, you probably have set something wrong in the plugin settings.

3. Check if your user credentials rules are compatible with Flarum ones. Detailed rules are listed in this issue: https://tracker.maicol07.it/issue/FSSOE-13
   Flarum won't login the user if the credentials don't satisfy these rules. It is suggested to enforce these rules (or strictier ones) in WP with a dedicated plugin (not covered here).

4. If you're trying to login to Flarum with an user that existed before enabling SSO, you have to use the same Flarum password in WP. Otherwise, passwords will mismatch and login fails.

# Links

- [Github](https://github.com/maicol07/flarum_sso_php_plugin)
- [API Docs](https://maicol07.github.io/flarum_sso_php_plugin/)
  {.links-list}

# Changelog

Major changes are marked with :exclamation:
Breaking changes are marked with :boom:

## 2.2.1

- 🔥 Removed automatic composer installer due to issues
- 🐛 Fixed exceptions and errors (CA Cert path and composer autoloader)

## 2.2

- **Updated in the WP repo (should be up within minutes)**
- ✨ Automatic composer installer
- ✨ Added option to toggle avatars auto sync
- 🐛 Return a previously triggered error during login
- 🐛 Use SSL cert path when trying to verify SSL
- 🐛 Fixed user image always set to gravatar default

## 2.1

- All the changes from PHP plugin
- :bug: Fixed [FSSOE-16](https://tracker.maicol07.it/issue/FSSOE-16)

## 2.0 - The modular update (2020-11-02)

:boom: Requires PHP 7.2+ and the JSON extension

### :heavy_plus_sign: Added

- All the changes from PHP plugin
- :pencil2: When an user is updated, it will get updated in Flarum too
- :art: New settings page design
- Detached plugin settings from addons ones
- Supported login with email and password [`#FSSOE-11`](https://tracker.maicol07.it/issue/FSSOE-11)

### :star: Improvements

- :memo: Revamped docs

### :hammer_and_wrench: Fixes

- :bug: :envelope: Can't update email [`#FSSOE-12`](https://tracker.maicol07.it/issue/FSSOE-12)
- :rotating_light: Exception on login in some cases [`#FSSOE-10`](https://tracker.maicol07.it/issue/FSSOE-10)

## 1.2 (2020-04-22)

### :heavy_plus_sign: New Features

- All the new features from the PHP plugin (added option to settings)

### :star: Improvements

- Code style improvements
- Updated dependencies

### :hammer_and_wrench: Fixes

- Fixes from PHP plugin
- Removing PRO key didn't deactivate PRO features
- MEMBERPRESS: Groups weren't deleted if user has no memberships

## 1.1.2 (2020-04-20)

- Changes from PHP Plugin

## 1.1 (2020-04-20)

### :heavy_plus_sign: New Features

- All the new features from the PHP plugin
- :exclamation: Plugin has been renamed, so follow the [upgrade](https://maicol07-docs.maicol07.now.sh/docs/en/flarum_sso/plugin/upgrade) instructions.
- Memberpress Integration: change password, forgot password links now redirects to WP
- Finally available in the [WordPress Plugin Directory](https://wordpress.org/plugins/sso-flarum/)

### :star: Improvements

- Code style improvements
- Updated dependencies

### :hammer_and_wrench: Fixes

- Fixes from PHP plugin + other fixes

## 1.0 (2020-04-08)

- All the new features in PHP plugin
- New settings page
- Paid PRO features (read more on Docs, link below)
  - Memberpress plugin integration (PRO FEATURE)
- Published to the WordPress Plugin Directory!
