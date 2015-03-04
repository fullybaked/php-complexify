php-complexify
===============

PHP port of Dan Palmer's [jquery.complexify.js](https://github.com/danpalmer/jquery.complexify.js/)

## Fork

This fork of [mcrumley/php-complexify](https://github.com/mcrumley/php-complexify) adds no new code,
but adds support to use this library as a composer package in your projects.

## Composer

As Complexify is not yet on Packagist.org to install it via composer you need to add a `repositories`
section to your `composer.json` as well as adding it to the required packages list.  Once it is on
Packagist this document will be updated with the more standard install instructions.

```json
"repositories": [
    {
        "type": "vcs",
        "url": "https://github.com/fullybaked/php-complexify"
    }
],
"require": {
    "fullybaked/php-complexify": "dev-master"
}
```

## Usage

```php
use Complexify\Complexify;

$check = new Complexify();
$result = $check->evaluateSecurity('correct horse battery staple');

echo round($result->complexity, 1) . '% ';
if ($result->valid) {
    echo 'VALID';
} else {
    echo 'NOT VALID: '.implode(', ', $result->errors);
}
```

## Configuration

You can override the default configuration by passing an array to the constructor.

```php
$check = new \Complexify\Complexify(array(
    'minimumChars' => 8,          // the minimum acceptable password length
    'strengthScaleFactor' => 1,   // scale the required password strength (higher numbers require a more complex password)
    'bannedPasswords' => array(), // override the default banned password list
    'banmode' => 'strict',        // strict == don't allow substrings of banned passwords, loose == only ban exact matches
    'encoding' => 'UTF-8',        // password string encoding
));
```

## Return value

The `evaluateSecurity` method returns an object containing the following properties:

- valid - `TRUE` if the password passes all checks
- complexity - The calculated complexity as a percent of the maximum (25 characters with at least one from each set)
- errors - Zero or more strings explaining the checks that did not pass
  - banned
  - tooshort
  - toosimple
