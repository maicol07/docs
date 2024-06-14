---
title: Flarum SSO PHP Premium Addon - Installation Instructions
description: null
published: true
date: 2022-06-18T09:29:17.068Z
tags: null
editor: markdown
dateCreated: 2021-06-11T08:14:06.994Z
---

Thank you for buying the premium addon. Here are the instructions on how to install the addon and keep it updated:

1. Login with the login details I've sent you via email to [Maicol07 Packages](https://packages.maicol07.it).
2. **(OPTIONAL BUT RECOMMENDED)** Change your password in the accout section (top bar, your username with the user icon)
3. Check if you can see the Composer repo in the Browse section (left sidebar) and the addon package inside (if there isn’t anything report me the issue and I’ll fix it).
4. Copy the repo url (we'll need it later):
   ![premium\_addon\_install\_instructions\_composer\_repo\_url.png](/flarum-sso/php/addons/premium_addon_install_instructions_composer_repo_url.png)
5. Now you can return to your project. Do one of these two methods:
   a. Run this command:
   ```
   composer config repo.foo composer {url}
   ```
   where `{url}` is the url you copied in the previous step
   b.	Add this to your composer.json:

```json
{
  "repositories": [
    {
      "type": "composer",
      "url": "{url}"
    }
  ]
}
```

where `{url}` is the url you copied in the previous step
5.	Now you can add the package to your composer packages either via `composer.json` or `composer require` command. (the package name is `maicol07/{package}` where `{package}` is the package name you can find in the composer repo this way:
![Package name location](/flarum-sso/php/addons/premium_addon_install_instructions_package_name.png)

You can always send me an email to [webmaster@maicol07.it](mailto:webmaster@maicol07.it) or chat with me from [my website](https://maicol07.it) (check the chat bubble in the bottom-right corner of the page) or from [Telegram](https://t.me/maicol07).

Now you can enjoy the addon. Check the instructions on how to use it in your code in the [PHP plugin docs](/en/flarum-sso/plugins/php#available-addons)
