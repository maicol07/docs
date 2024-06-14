---
title: Version specific upgrade notes
description: 
published: true
date: 2021-04-09T14:35:05.759Z
tags: 
editor: markdown
dateCreated: 2021-04-09T13:28:20.522Z
---

You should always check the example folder to see how the changes in this release can be implemented. Here are some notes with the major breaking changes:

## 2.0 to 3.0
### Gravity: Major
- You can't create an User with `$user = new User($username, $flarum);` anymore. Now the Flarum class will handle the user creation for you:
```php
use Maicol07\SSO\Flarum;

$flarum = new Flarum($options); // $options is an array with the required options
$user = $flarum->user($username) // Create the user if it doesn't exists with the user method

// Retrieve the user with the user method **after** its creation
$user_alias = $flarum->user(); // $user_alias contains the same object stored in $user
```
- `lifetime` option is now `remember`. His behaviour is changed:
	`remember` is a bool property and it should contain the value of the Remember me checkbox. If you want to remember the user (Flarum keeps the session active for 5 years) then set this to `true`, `false` otherwise (1 hour the first one. It's renewed every time the user visits Flarum).
	It can be set in the initial options array or via the [`isSessionRemembered`](https://flarum-sso-php.api.docs.maicol07.it/Maicol07/SSO/Flarum.html#method_isSessionRemembered) method (set the parameter with the new value to change/set it; to retrieve the current value call the method without any arguments).
- Laravel 6 & 7 are not supported anymore (Guzzle 6 support has also been dropped in the API Client), although they may work without any issues. It's recommended to upgrade to the newest versions (Laravel 8 and Guzzle 7)

### Gravity: Medium/Minor
- PHP 7.3+ required! PHP 7.2 is no longer supported.
- The `fetchUser` method of the User class has been renamed to [`fetch`](https://flarum-sso-php.api.docs.maicol07.it/Maicol07/SSO/User.html#method_fetch).
- `displayname` attribute is now read-only. Use `nickname` to change the display name of a user (Doesn't work if the Flarum Nicknames extension is disabled).
- The `getForumLink` method has been removed. You can use the `url` property to get the Flarum URL.
- Cookie refactor:
	- Cookies are generated on the fly ([`generateCookie()`](https://flarum-sso-php.api.docs.maicol07.it/Maicol07/SSO/Traits/Cookies.html#method_generateCookie) method) and are no longer stored in the Flarum class (as a property);
  - `setCookie()` method has been removed. Now there are 4 new methods that replace this: 
  	- [`setRememberCookie`](https://flarum-sso-php.api.docs.maicol07.it/Maicol07/SSO/Traits/Cookies.html#method_setRememberCookie)
    - [`deleteRememberCookie`](https://flarum-sso-php.api.docs.maicol07.it/Maicol07/SSO/Traits/Cookies.html#method_deleteRememberCookie)
    - [`setSessionTokenCookie`](https://flarum-sso-php.api.docs.maicol07.it/Maicol07/SSO/Traits/Cookies.html#method_setSessionTokenCookie)
    - [`deleteSessionTokenCookie`](https://flarum-sso-php.api.docs.maicol07.it/Maicol07/SSO/Traits/Cookies.html#method_deleteSessionTokenCookie)
    - [`setLogoutCookie`](https://flarum-sso-php.api.docs.maicol07.it/Maicol07/SSO/Traits/Cookies.html#method_setLogoutCookie)
    - [`deleteLogoutCookie`](https://flarum-sso-php.api.docs.maicol07.it/Maicol07/SSO/Traits/Cookies.html#method_deleteLogoutCookie)
- The `getLifeTimeSeconds` method has been removed (the `lifetime` property doesn't exists anymore)!
- Addon methods renamed:
	- `addAddon` is now [`loadAddon`](https://flarum-sso-php.api.docs.maicol07.it/Maicol07/SSO/Traits/Addons.html#method_loadAddon)
  - `removeAddon` is now [`unloadAddon`](https://flarum-sso-php.api.docs.maicol07.it/Maicol07/SSO/Traits/Addons.html#method_unloadAddon)
  - `setAddonAttributes` is now [`setAddonProperties`](https://flarum-sso-php.api.docs.maicol07.it/Maicol07/SSO/Traits/Addons.html#method_setAddonProperties)
- New API docs! Now it is easier to search for properties and methods. Check it out: https://flarum-sso-php.api.docs.maicol07.it (The old domain will redirect to the new one).

## 1.x to 2.0
- You need to convert the Flarum object parameters to the new array options form
- The `insecure` option is now `verify_ssl`. Its behaviour has changed. Check the Options section below to know what you have to set to `verify_ssl`.
- You need to create the User object and work with that to login/signup and do other stuff with the user
- Almost all the methods that involve the user have been moved to the User class. Check what you need to change in the API Docs (for example, the login and signup methods have been moved).
- The `removeGroups()` method has been removed. Use Users relationships to manage groups (set to blank array to remove them).
- If you use the `getUsersList()` method, you may want to know that this now returns a Laravel collection and no more an array.