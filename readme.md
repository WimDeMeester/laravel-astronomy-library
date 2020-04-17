# laravel-astronomylibrary

> Astronomical calculations for php / laravel

[![Logo](public/img/logo2.png)](https://www.deepskylog.org/)

[![Latest Version on Packagist][ico-version]][link-packagist]
[![Total Downloads][ico-downloads]][link-downloads]
[![Build Status][ico-travis]][link-travis]
[![StyleCI][ico-styleci]][link-styleci]

Take a look at [contributing.md](contributing.md) if you are interesting in helping out.

## Installation

Via Composer

``` bash
composer require deepskylog/laravel-astronomy-library
```

## Usage

```php
<?php
// Use the factory to create a AstronomyLibrary instance
$astrolib = new AstronomyLibrary($carbonDate);
```

### Time methods

```php
// Get the date of the AstronomyLibrary instance
$date = $astrolib->getDate();

// Set a new date to the AstronomyLibrary instance
$astrolib->setDate($carbonDate);

// Get the julian day of the AstronomyLibrary instance
$jd = $astrolib->getJd();
```

### Static Time methods

```php
// Convert from Carbon date to Julian day
$jd = Time::getJd($carbonDate);
```

## Change log

Please see the [changelog](changelog.md) for more information on what has changed recently.

## Testing

``` bash
phpunit
```

## Contributing

Please see [contributing.md](contributing.md) for details and a todolist.

## Security

If you discover any security related issues, please email developers@deepskylog.be instead of using the issue tracker.

## Credits

- [The DeepskyLog Team][link-author]
- [All Contributors][link-contributors]

## License

GPLv3. Please see the [license file](LICENSE) for more information.

[ico-version]: https://img.shields.io/packagist/v/deepskylog/laravel-astronomy-library.svg?style=flat-square
[ico-downloads]: https://img.shields.io/packagist/dt/deepskylog/laravel-astronomy-library.svg?style=flat-square
[ico-travis]: https://img.shields.io/travis/deepskylog/laravel-astronomy-library/master.svg?style=flat-square
[ico-styleci]: https://styleci.io/repos/255550499/shield

[link-packagist]: https://packagist.org/packages/deepskylog/laravel-astronomy-library
[link-downloads]: https://packagist.org/packages/deepskylog/laravel-astronomy-library
[link-travis]: https://travis-ci.org/deepskylog/laravel-astronomy-library
[link-styleci]: https://styleci.io/repos/255550499
[link-author]: https://github.com/DeepskyLog
[link-contributors]: ../../contributors
