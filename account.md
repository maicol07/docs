---
title: Maicol07 Account
description: What is Maicol07 Account?
published: true
date: 2020-09-10T07:20:18.021Z
tags: maicol07-account
editor: markdown
dateCreated: 2020-09-09T11:48:47.968Z
---

Maicol07 Account is an SSO authentication and closed-source user management system developed by [Maicol Battistini (maicol07)](https://maicol07.it).

# Features
## Network status
The dashboard offers a status section, in which you can see the current status of the Maicol07 Network. This also include the issues history of the network. The data is obtained from [Maicol07 Status](https://status.maicol07.it).

## Profile management
You can manage your profile, which will be shared across all the sites of the network. At the moment of writing, these are the only things you can add/change:
- Name
- Surname
- Image (via [Gravatar](https://gravatar.com))
- Username
- Email

## Account security
Security is the most important feature for an SSO system. To ensure your account security, Maicol07 Account gives you some options, although most of them are optional. The following fields can be changed from the Security tab of your account area:
- Username
- Email
- Password

To speed up the login is possible to sign in with a social provider. This requires that you have signed up on the social provider website. Currently, you can use your account from the following social providers to login:
- Google
- Facebook
- Twitter
- Discord

While these are standard settings, they are not enough to secure your account properly. So, you can pair your Maicol07 Account with an authenticator app to approve the login. At the moment, the only authenticator apps supported are:
- [Authy OneTouch](https://authy.com/)
- [Telegram](https://telegram.org/)
- Standard authenticator app such as [Authy](https://authy.com/), [Google Authenticator](https://support.google.com/accounts/answer/1066447) or [Microsoft Authenticator](https://www.microsoft.com/it-it/account/authenticator)

Since your privacy is important to us, you have full control on what a network component can get from your account: you can revoke an authorization, but required ones won't be revokable, as they guarantee the correct functioning of the component. If you want to remove your account from a network component, revoking all the authorizations you've given to the component, you'll need to unlink it.

Your data is also important: for us, to provide you a customized experience, for you because they're personal. According to the GDPR regolamentation, you can review our privacy policy (the same for all our components), request your data or delete your account from the database.

## SSO (Single Sign On)
SSO, acronym of Single Sign On, allows you to login only one time on Maicol07 Account, to get signed in automatically into all the components of the network, when you need them. With this system you only need one account, stored in Maicol07 Account database and shared with the component that requests your data (though, this data requires your authorization to be requested).
