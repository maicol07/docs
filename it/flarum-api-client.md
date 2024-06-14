---
title: Client API per Flarum
description: Un ponte tra il tuo progetto e Flarum
published: true
date: 2020-09-18T07:58:11.642Z
tags: flarum-api-client, flarum, php
editor: markdown
dateCreated: 2020-09-09T15:22:42.357Z
---

![flarum-api-client.png](/flarum-api-client/flarum-api-client.png)
[![LUltima versione stabile](https://poser.pugx.org/maicol07/flarum-api-client/v)](//packagist.org/packages/maicol07/flarum-api-client) [![Download totali](https://poser.pugx.org/maicol07/flarum-api-client/downloads)](//packagist.org/packages/maicol07/flarum-api-client) [![Ultima versione instabile](https://poser.pugx.org/maicol07/flarum-api-client/v/unstable)](//packagist.org/packages/maicol07/flarum-api-client) [![Licenza](https://poser.pugx.org/maicol07/flarum-api-client/license)](//packagist.org/packages/maicol07/flarum-api-client)

Flarum API Client è un generico client API PHP che può essere utilizzato in qualsiasi progetto. Puoi semplicemente includere questo pacchetto come dipendenza dal tuo progetto per utilizzarlo.

Il client crea un ponte tra il tuo progetto e Flarum, permettendoti di usarlo per ottenere dati da Flarum.

# Installazione
L'unico metodo di installazione supportato è tramite [Composer](https://getcomposer.org):
```sh
composer require maicol07/flarum-api-client
```

Altri metodi di installazione *possono* funzionare, ma non sono suggeriti e non saranno supportati!

## :warning: Attenzione
- **Se usi Laravel 6 o 7 non sarai in grado di eseguire i test di PHPUnit se non installi il 7**
- **Laravel 6 e 7 saranno supportati fino a quando Laravel non li supporterà più con fix di sicurezza. [Politica di supporto di Laravel](https://laravel.com/docs/8.x/releases#support-policy)**

# Configurazione
Per iniziare ad utilizzare il client *potrebbe* essere necessario un master token di Flarum:

1. Generare un token casuale (per motivi di sicurezza è meglio se è lungo 40 caratteri; è possibile utilizzare [questo strumento](https://onlinerandomtools.com/generate-random-string) per crearne uno);
2. Aggiungerlo manualmente alla tabella `api_keys` usando phpmyadmin/adminer o un'altra soluzione.

Il master token è necessario per accedere a discussioni non pubbliche e per eseguire azioni altrimenti riservate agli amministratori Flarum. Se non si ha bisogno di questi privilegi, allora si può procedere senza alcun token

# Metodi disponibili (Documenti API)
È possibile visionare la documentazione dell'API del client qui:
- [Metodi disponibili](https://maicol07.github.io/flarum-api-client/classes/Maicol07-Flarum-Api-Fluent.html#method_token)
- [Documentazione API](https://maicol07.github.io/flarum-api-client)
{.links-list}

# Esempi
Un esempio di base:

```php
<?php

require_once "vendor/autoload.php";

use Maicol07\Flarum\Api\Client;

$api = new Client('http://example.com');

// Una raccolta di discussioni dalla prima pagina dell'indice del tuo Forum.
$discussions = $api->discussions()->request();
// Leggi una discussione specifica.
$discussion = $api->discussions()->id(1)->request();
// Leggi la prima pagina di utenti.
$users = $api->users()->request();
```

Un esempio autorizzato:

```php
<?php

require_once "vendor/autoload.php";

use Maicol07\Flarum\Api\Client;

$api = new Client('http://example.com', ['token' => '<insert-master-token>; userId=1']);
```

> `userId` si riferisce ad un utente che ha i permessi di amministrazione o all'utente per il quale si desidera eseguire azioni. L'aggiunta dell'impostazione `userId` al token funziona solo per le chiavi master.
{.is-info}

## Link
- [Github](https://github.com/maicol07/flarum-api-client)
- [Packagist](http://packagist.com/packages/maicol07/flarum-api-client)
- [Problemi](https://bugs.maicol07.it/projects/74d531ee-75e4-463f-9cfd-e81a152dbc92)
{.links-list}

## Crediti
- Flarum e Flarum Foundation sono marchi registrati in Olanda. [Sito web](https://flarum.org)
- Icona API realizzata da [Vitaly Gorbaciov](https://www.flaticon.com/authors/vitaly-gorbachev) di [Flaticon](https://www.flaticon.com/)
