---
title: Flarum API Client
description: A PHP client to interact with Flarum REST API
published: true
date: 2024-03-19T10:11:39.345Z
tags: flarum-api-client, flarum
editor: markdown
dateCreated: 2020-09-09T12:57:10.022Z
---

<div class="center">

  <img alt="flarum-api-client.png" src="/flarum-api-client/flarum-api-client.png" width="450">

[![Latest Stable Version](https://poser.pugx.org/maicol07/flarum-api-client/v)](//packagist.org/packages/maicol07/flarum-api-client) [![Total Downloads](https://poser.pugx.org/maicol07/flarum-api-client/downloads)](//packagist.org/packages/maicol07/flarum-api-client) [![Latest Unstable Version](https://poser.pugx.org/maicol07/flarum-api-client/v/unstable)](//packagist.org/packages/maicol07/flarum-api-client) [![License](https://poser.pugx.org/maicol07/flarum-api-client/license)](//packagist.org/packages/maicol07/flarum-api-client)

</div>

Flarum API Client is a generic PHP API client you can use in any project. You can simply include this package as a dependency to your project to use it.

It creates a bridge between your project and Flarum, allowing you to use it to get data from Flarum.

# Installation

The only supported installation method is via [Composer](https://getcomposer.org):

```sh
composer require maicol07/flarum-api-client
```

Other installation methods _may_ be possible, but are not suggested and they won't be supported!

## :warning: Warnings

### Since 1.1.2

- **Guzzle 6 will be supported during until its EOL. [Guzzle Version Guidance](https://github.com/guzzle/guzzle#version-guidance)** (support dropped from 1.3)

### Since 1.1.1

- **If you use Laravel 6 you won't be able to run PHPUnit tests**
- **<i class="mdi mdi-laravel"></i> Laravel 6 & 7 will be supported until Laravel won't supports them anymore with security fixes. [Laravel Support Policy](https://laravel.com/docs/8.x/releases#support-policy)** (support dropped from 1.3)

# Configuration

In order to start working with the client you _might_ need a Flarum master key:

1. Generate a random token (for security purposes it is better if this is long 40 characters; you can use [this tool](https://onlinerandomtools.com/generate-random-string) to make one)
2. Manually add it to the `api_keys` table using phpmyadmin/adminer or another solution.

The master key is required to access non-public discussions and running actions otherwise reserved for Flarum administrators. If you don't need these privileges, then you can go ahead without any token

# Available methods (API Docs)

You can find the client API Docs here:

- [Available Methods](https://maicol07.github.io/flarum-api-client/classes/Maicol07-Flarum-Api-Fluent.html#method_token)
- [API Docs](https://maicol07.github.io/flarum-api-client)
  {.links-list}

# Examples

A basic example:

```php
<?php

require_once "vendor/autoload.php";

use Maicol07\Flarum\Api\Client;

$api = new Client('http://example.com');

// A collection of discussions from the first page of your Forum index.
$discussions = $api->discussions()->request();
// Read a specific discussion.
$discussion = $api->discussions()->id(1)->request();
// Read the first page of users.
$users = $api->users()->request();
```

An authorized example:

```php
<?php

require_once "vendor/autoload.php";

use Maicol07\Flarum\Api\Client;

$api = new Client('http://example.com', ['token' => '<insert-master-token>']);
```

<!-- > `userId` refers to a user that has admin permissions or the user you want to run actions for. Appending the userId setting to the token only works for Master keys.
{.is-info} -->

# Links

- [Github](https://github.com/maicol07/flarum-api-client)
- [Packagist](http://packagist.org/packages/maicol07/flarum-api-client)
- [Issues](https://tracker.maicol07.it/projects/74d531ee-75e4-463f-9cfd-e81a152dbc92)
  {.links-list}

# Credits

- Flarum and Flarum Foundation are trademarks registered in Netherlands. [Website](https://flarum.org)
- API Icon made by [Vitaly Gorbachev](https://www.flaticon.com/authors/vitaly-gorbachev) from [Flaticon](https://www.flaticon.com/)
