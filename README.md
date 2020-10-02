# Fallback for `php 8.0` using `symfony-polyfill`

Provides a metapackage for library needing `php 8.0` 
that fallback to a default implementation when user does not meet
the initial requirements.

## Usage

```shell
composer require "composer-fallback/php-80.symfony-polyfill:*"
```

Composer will, by preference:
- check if user has `php>=8.0`
- otherwise, fallbacks to `symfony/polyfill-php80`

## How does it works

This package contains 2 versions:

1. The highest [1.1](https://github.com/composer-fallback/php-80.symfony-polyfill/blob/1.1/composer.json) needs `php: >=8.0`.

1. The lowest [1.0](https://github.com/composer-fallback/php-80.symfony-polyfill/blob/1.0/composer.json) triggers install of `symfony/polyfill-php80`.

Composer will choose this highest version when user has the right version of php.
Otherwise, composer will choose the lowest version and in that case will 
download the following packages: `symfony/polyfill-php80`.

## What problem does it solve?

You are maintaining a library that need an implementation of `php 8.0`,
but you don't want requiring a specific implementation. 

ie. you need function defined in `php 8.0`, but polyfill exists and would be enough.

When end users requires you library with the following code 
```json
{
    "name": "end-user/app",
    "require": {
      "acme/lib": "^1.0"
    }
}
```

They probably ends with such error

```shell
composer up

Your requirements could not be resolved to an installable set of packages.

  Problem 1
    - Installation request for acme/lib ^1.0 -> satisfiable by acme/lib[1.0].
    - acme/lib 1.0 requires php >=8.0 -> no matching package found.
```

You can ask user to install a random package, it works, but the DX is very bad,
and confusing at first.

By using `composer-fallback/php-80.symfony-polyfill`, 
users will provide the native implementation 
or fallback to your default choice

### Example of user that meet the preferred requirements

```json
{
    "name": "end-user/app",
    "require": {
        "acme/lib": "^1.0",
        "third-party/provide-implementation": "^1.0"
    }
}
```
```shell
composer up
...
Package operations: 2 installs, 0 updates, 0 removals
  - Installing acme/lib (1.0)
  - Installing composer-fallback/php-80.symfony-polyfill (1.1)
```

### Example of user that fallback to your recommendations

```json
{
    "name": "end-user/app",
    "require": {
        "acme/lib": "^1.0"
    }
}
```
```shell
composer up
...
Package operations: 3 installs, 0 updates, 0 removals
  - Installing acme/lib (1.0)
  - Installing composer-fallback/php-80.symfony-polyfill (1.0)
  - Installing symfony/polyfill-php80 (1.0)
```

## Contributing

This repository is automatically generated. If you want contributing and 
submitting an issue or a Pull Request, please use 
[composer-fallback/generator](https://github.com/composer-fallback/generator).
