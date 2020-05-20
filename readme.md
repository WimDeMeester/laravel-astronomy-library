# laravel-astronomy-library

> Astronomical calculations for php / laravel

[![Logo](public/img/logo2.png)](https://www.deepskylog.org/)

[![Latest Version on Packagist][ico-version]][link-packagist]
[![Total Downloads][ico-downloads]][link-downloads]
[![Build Status][ico-travis]][link-travis]
[![StyleCI][ico-styleci]][link-styleci]

Take a look at [contributing.md](contributing.md) if you are interesting in helping out.
The laravel-astronomy-library is part of [DeepskyLog](https://www.deepskylog.org). If you are interested in helping with the development of DeepskyLog, see the [documentation](https://github.com/DeepskyLog/DeepskyLog/blob/laravel/README.md).

## Installation

AstronomyLibrary can be installed via composer:

``` bash
composer require deepskylog/laravel-astronomy-library
```

You need to publish and run the migration:

``` bash
php artisan vendor:publish --provider="deepskylog\AstronomyLibrary\AstronomyLibraryServiceProvider" --tag="migrations"
php artisan migrate
```

The database table with the delta t values can be updated using the following command:

``` bash
php artisan deltat:update
```

A job is automatically scheduled every 4 months to update the delta t value. This job can be executed using:

``` bash
php artisan schedule:run
```

It is possible to run these jobs automatically by adding the following line to your crontab:

``` bash
* * * * * cd /path-to-your-project && php artisan schedule:run >> /dev/null 2>&1
```

## Documentation

For more documentation on the mathematical background, see [docs.md](docs/docs.md).

## Usage

```php
<?php
// Use the factory to create a AstronomyLibrary instance
$coords = new GeographicalCoordinates(-70.73330, -29.25);
$astrolib = new AstronomyLibrary($carbonDate, $coords);
```

### Time methods

```php
// Get the date of the AstronomyLibrary instance.
$date = $astrolib->getDate();

// Set a new date to the AstronomyLibrary instance.
$astrolib->setDate($carbonDate);

// Get the coordinates of the AstronomyLibrary instance.
$coords = $astrolib->getGeographicalCoordinates();

// Set the coordinates of the AstronomyLibrary instance.
$astrolib->setGeographicalCoordinates($newCoords);

// Get the julian day of the AstronomyLibrary instance.
$jd = $astrolib->getJd();

// Set the julian day of the AstronomyLibrary instance. Also update the carbon date.
$astrolib->setJd($jd);

// Get delta t for the date of the AstronomyLibrary instance.
$deltat = $astrolib->getDeltaT();

// Get the dynamical time of the AstronomyLibrary instance.
$dynamicalTime = $astrolib->getDynamicalTime();

// Get the mean siderial time for AstronomyLibrary instance.
$meanSiderialTime = $astrolib->getMeanSiderialTime();

// Get the apparent siderial time for AstronomyLibrary instance.
$meanSiderialTime = $astrolib->getApparentSiderialTime();

// Get the nutation for the AstronomyLibrary instance.
$nutation = $astrolib->getNutation();
```

### Static Time methods

```php
// Convert from Carbon date to Julian day
$jd = Time::getJd($carbonDate);

// Convert from Julian day to Carbon date
$carbonDate = Time::fromJd($jd);

// Get delta T for the given date
$deltat = Time::deltaT($carbonDate);

// Get the dynamical time for the given date
$dynamicalTime = Time::dynamicalTime($carbonDate);

// Get the mean siderial time for the given date
$meanSiderialTime = Time::meanSiderialTime($carbonDate);

// Get the apparent siderial time for the given date
$meanSiderialTime = Time::apparentSiderialTime($carbonDate);

// Get the nutation for the given julian day
$nutation = Time::nutation($jd);
```

## Magnitude methods

### Static magnitude methods

```php
// Convert from Naked Eye Limiting Magnitude to SQM value
$sqm = Magnitude::nelmToSqm($sqm, $fstOffset);

// Convert from Naked Eye Limiting Magnitude to bortle scale
$bortle = Magnitude::nelmToBortle($sqm);

// Convert from SQM value to Naked Eye Limiting Magnitude
$nelm = Magnitude::sqmToNelm($sqm, $fstOffset);

// Convert from SQM value to bortle scale
$bortle = Magnitude::sqmToBortle($sqm);

// Convert from bortle scale to Naked Eye Limiting Magnitude
$nelm = Magnitude::bortleToNelm($bortle, $fstOffset);

// Convert from bortle scale to SQM value
$sqm = Magnitude::bortleToNelm($bortle, $fstOffset);
```

## Coordinate methods

### Coordinate methods on AstronomyLibrary instance

```php
// Convert from Equatorial Coordinates to Ecliptical
$coords = new EquatorialCoordinates(11.23, 32.12);
$ecliptical = $astrolib->equatorialToEcliptical($coords);

// Convert from Equatorial coordinates to horizontal coordinates.
$horizontal = $astrolib->equatorialToHorizontal($coords);

// Convert from Equatorial coordinates to Galactic coordinates.
$galactic = $astrolib->equatorialToGalactic($coords);

// Convert from Ecliptical coordinates to Equatorial coordinates.
$coords = new EclipticalCoordinates(11.23, 32.12);
$equatorial = $astrolib->eclipticalToEquatorial($coords);

// Convert from Horizontal coordinates to Equatorial coordinates.
$coords = new HorizontalCoordinates(18.5, 22.2);
$equatorial = $astrolib->horizontalToEquatorial($coords);

// Convert from Galactic coordinates to Equatorial coordinates.
$coords = new GalacticCoordinates(12.5, 45.6);
$equatorial = astrolib->galacticToEquatorial($coords);

// Get the parallactic angle
$coords = new EquatorialCoordinates(5.24, 55.55);
$parallacticAngle = $astrolib->parallacticAngle($coords);

```

### Coordinate methods on equatorial coordinates

```php
$coords = new EquatorialCoordinates(15.21, -45.21);
$coords->setRA(22.21);
$coords->setDeclination(42.37);
$ra = $coords->getRA();
$decl = $coods->getDeclination();
print ($coords->printRA());
print ($coords->printDeclination());
$ecliptical = $coords->convertToEcliptical($nutObliquity);
$ecliptical = $coords->convertToEclipticalJ2000();
$ecliptical = $coords->convertToEclipticalB1950();
$horizontal = $coords->convertToHorizontal($geo_coords, $siderial_time);
$galactic = $coords->convertToGalactic();
$parallacticAngle = $coords->getParallacticAngle($geo_coords, $siderial_time);
$hour_angle = $coords->getHourAngle($siderial_time);
```

### Coordinate methods on ecliptical coordinates

```php
$coords = new EclipticalCoordinates(15.21, -45.21);
$coords->setLongitude(22.21);
$coords->setLatitude(42.37);
$longitude = $coords->getLongitude();
$latitude = $coods->getLatitude();
print ($coords->printLongitude());
print ($coords->printLatitude());
$equatorial = $coords->convertToEquatorial($nutObliquity);
$equatorial = $coords->convertToEquatorialJ2000();
$equatorial = $coords->convertToEquatorialB1950();
```

### Coordinate methods on horizontal coordinates

```php
$coords = new HorizontalCoordinates(15.21, -45.21);
$coords->setAzimuth(22.21);
$coords->setAltitude(42.37);
$azimuth = $coords->getAzimuth();
$altitude = $coods->getAltitude();
print ($coords->printAzimuth());
print ($coords->printAltitude());
$equatorial = $coords->convertToEquatorial($geo_coords, $siderial_time);
```

### Coordinate methods on galactic coordinates

```php
$coords = new GalacticCoordinates(15.21, -45.21);
$coords->setLongitude(22.21);
$coords->setLatitude(42.37);
$longitude = $coords->getLongitude();
$latitude = $coods->getLatitude();
print ($coords->printLongitude());
print ($coords->printLatitude());
$equatorial = $coords->convertToEquatorial();
```

## Target methods

Target is a class to keep information about an object and perform calculations on it. Target is a base class and can be used for stars and deep-sky objects. There are three subclasses:

+ Moon
+ Planet
+ Sun

Moving targets (like the sun, moon or planets) need the equatorial coordinates for the given date, for the day before and the next day.

```php
// Create a new target.
$target = new Target();
// Create equatorial coordinates.
$equa = new EquatorialCoordinates(16.695, 36.460278);
// Add equatorial coordinates to the target.
$target->setEquatorialCoordinates($equa);

$date = Carbon::create(2020, 5, 13, 12);
$date->timezone('Europe/Brussels');
$geo_coords = new GeographicalCoordinates(4.86463, 50.83220);

$greenwichSiderialTime = Time::apparentSiderialTimeGreenwich($date);
$deltaT = Time::deltaT($date);

// Calculate the ephemerids for the target
$target->calculateEphemerides($geo_coords, $greenwichSiderialTime, $deltaT);

// Print the transit time of the target
echo $target->getTransit();

// Print the setting time of the target, in a specific timezone
// If the getSetting method is Null, the object does not set.
// This can be the case because the object is never visible at
// this location, or because the target is circumpolar .
echo $target->getSetting()->timezone('Europe/Brussels');

// Print the rising time of the target
// If the getRising method is Null, the object does not rise.
// This can be the case because the object is never visible at
// this location, or because the target is circumpolar .
echo $target->getRising();

// Print the maximum height of the target during the night
// If there is no astronomical night at the location, the
// maximum height is taken during nautical twilight.
// If there is no nautical night at the location, Null is
// returned.
echo $target->getMaxHeightAtNight();

// Print the maximum possible height
echo $target->getMaxHeight();

// Print the best time to observe the target
// If there is no astronomical night at the location, the
// best time is taken during nautical twilight.
// If there is no nautical night at the location, Null is
// returned.
echo $target->getBestTimeToObserve();
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

+ [The DeepskyLog Team][link-author]
+ [All Contributors][link-contributors]

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
[link-contributors]: https://github.com/DeepskyLog/laravel-astronomy-library/graphs/contributors