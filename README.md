# Utopia Fastly

> [!IMPORTANT]
> This repository is archived. Utopia Fastly now lives in `packages/fastly` of Appwrite's Edge repository, its only consumer, where it is developed and loaded directly. No further releases are published here.

A small [Fastly](https://www.fastly.com) API client for PHP 8.4+, built on
[utopia-php/client](https://github.com/utopia-php/client). It currently exposes
surrogate-key purging.

## Install

```bash
composer require utopia-php/fastly
```

## Usage

```php
<?php

use Utopia\Client\Client;
use Utopia\Client\Adapter\Curl\Client as CurlAdapter;
use Utopia\Fastly\Fastly;

require __DIR__ . '/vendor/autoload.php';

$fastly = new Fastly(
    new Client(new CurlAdapter()), // any PSR-18 client
    serviceId: 'SU1Z0isxPaozGVKXdv0eY',
    token: 'your-fastly-api-token',
);

// Invalidate every response tagged with this surrogate key.
$fastly->purge('homepage');
```

`purge()` issues `POST {endpoint}/{serviceId}/purge/{surrogateKey}` with the API
token in the `Fastly-Key` header. A non-2xx response raises
`Utopia\Fastly\Exception\PurgeException`; network failures surface as the
underlying PSR-18 client exceptions.

The endpoint (`https://api.fastly.com/service`) and token header (`Fastly-Key`)
are constructor arguments, so the client also works against a compatible proxy:

```php
$fastly = new Fastly($client, $serviceId, $token, 'https://cdn.internal/service', 'X-Purge-Key');
```
