# Adhan JavaScript

[![badge-version][]][npm] [![badge-travis][]][travis] [![badge-cov][]][codecov] [![Commitizen friendly](https://img.shields.io/badge/commitizen-friendly-brightgreen.svg)](http://commitizen.github.io/cz-cli/) [![semantic-release](https://img.shields.io/badge/%20%20%F0%9F%93%A6%F0%9F%9A%80-semantic--release-e10079.svg)](https://github.com/semantic-release/semantic-release)

Adhan JavaScript is a well tested and well documented library for calculating Islamic prayer times in JavaScript using Node or a web browser. This works great all countries of the world especially in Pakistan, India, Bangladesh and neighbouring countries. 

You can even get prayer time in seconds for greater precision and your own calculations.

All astronomical calculations are high precision equations directly from the book [“Astronomical Algorithms” by Jean Meeus](http://www.willbell.com/math/mc1.htm). This book is recommended by the Astronomical Applications Department of the U.S. Naval Observatory and the Earth System Research Laboratory of the National Oceanic and Atmospheric Administration.

## Installation

Adhan was designed to work in the browser and in Node.js

### Browser

```
<script src="Adhan.js"></script>
<script>
    var prayerTimes = new adhan.PrayerTimes(coordinates, date, params, precisionOn));
</script>
```

### Node

```
npm install adhan
```

```
var adhan = require('adhan')
var prayerTimes = new adhan.PrayerTimes(coordinates, date, params, precisionOn);
```

## Migration

Migrating from version 3.x? Read the [migration guide](MIGRATION.md)

## Usage

To get prayer times initialize a new `PrayerTimes` object passing in coordinates,
date, and calculation parameters.

```js
var prayerTimes = new adhan.PrayerTimes(coordinates, date, params, precisionOn);
```

### Initialization parameters

#### Coordinates

Create a `Coordinates` object with the latitude and longitude for the location
you want prayer times for.

```js
var coordinates = new adhan.Coordinates(35.78056, -78.6389);
```

#### Date

The date parameter passed in should be an instance of the JavaScript `Date`
object. The year, month, and day values need to be populated. All other
values will be ignored. The year, month and day values should be for the date
that you want prayer times for. These date values are expected to be for the 
Gregorian calendar.

```js
var date = new Date();
var date = new Date(2015, 11, 1);
```

#### Calculation parameters

The rest of the needed information is contained within the `CalculationParameters` object.

[Calculation Parameters & Methods Guide](METHODS.md)



#### Precision On Param

If you also want to get seconds in prayer times you need to pass `precisionOn` boolean parameter to `PrayerTimes` constructor. If `true` you will get seconds in prayer times as well. Don't forget to use time format that displays seconds e.g `h:mm:ss A`
### Prayer Times

Once the `PrayerTimes` object has been initialized it will contain values
for all five prayer times and the time for sunrise. The prayer times will be
Date object instances initialized with UTC values. You will then need to format
the times for the correct timezone. You can do that by using a timezone aware 
date formatting library like [moment](https://momentjs.com/docs/).

```js
moment(prayerTimes.fajr).tz('America/New_York').format('h:mm A');

```
Or if `precisionOn` is `true`
```js
moment(prayerTimes.fajr).tz('America/New_York').format('h:mm:ss A');

```

### Full Example

```js
var date = new Date();
var coordinates = new adhan.Coordinates(35.78056, -78.6389);
var params = adhan.CalculationMethod.MuslimWorldLeague();
var precisionOn = true;
params.madhab = adhan.Madhab.Hanafi;
var prayerTimes = new adhan.PrayerTimes(coordinates, date, params, precisionOn);

var fajrTime = moment(prayerTimes.fajr).tz('America/New_York').format('h:mm:ss A');
var sunriseTime = moment(prayerTimes.sunrise).tz('America/New_York').format('h:mm:ss A');
var dhuhrTime = moment(prayerTimes.dhuhr).tz('America/New_York').format('h:mm:ss A');
var asrTime = moment(prayerTimes.asr).tz('America/New_York').format('h:mm:ss A');
var maghribTime = moment(prayerTimes.maghrib).tz('America/New_York').format('h:mm:ss A');
var ishaTime = moment(prayerTimes.isha).tz('America/New_York').format('h:mm:ss A');
```

### Convenience Utilities

The `PrayerTimes` object has functions for getting the current prayer and the next prayer. You can also get the time for a specified prayer, making it
easier to dynamically show countdowns until the next prayer.

```js
var prayerTimes = new adhan.PrayerTimes(coordinates, date, params, precisionOn);

var current = prayerTimes.currentPrayer();
var next = prayerTimes.nextPrayer();
var nextPrayerTime = prayerTimes.timeForPrayer(next);
```

### Sunnah Times

The Adhan library can also calulate Sunnah times. Given an instance of `PrayerTimes`, you can get a `SunnahTimes` object with the times for Qiyam.

```js
var sunnahTimes = new adhan.SunnahTimes(prayerTimes);
var middleOfTheNight = moment(sunnahTimes.middleOfTheNight).tz('America/New_York').format('h:mm A');
var lastThirdOfTheNight = moment(sunnahTimes.lastThirdOfTheNight).tz('America/New_York').format('h:mm A');
```

### Qibla Direction

Get the direction, in degrees from North, of the Qibla from a given set of coordinates.

```js
var coordinates = new adhan.Coordinates(35.78056, -78.6389);
var qiblaDirection = adhan.Qibla(coordinates);
```

## Contributing

Adhan is made publicly available to provide a well tested and well documented library for Islamic prayer times to all 
developers. We accept feature contributions provided that they are properly documented and include the appropriate 
unit tests. We are also looking for contributions in the form of unit tests of of prayer times for different 
locations, we do ask that the source of the comparison values be properly documented.

**Note:** Commit messages should follow the [commit message convention](./.github/COMMIT_CONVENTIONS.md) so that changelogs can be automatically generated. Commit messages will be automatically validated upon commit. **If you are not familiar with the commit message convention, you should use `npm run commit` instead of `git commit`**, which provides an interactive CLI for generating proper commit messages.

## License

Adhan is available under the MIT license. See the LICENSE file for more info.

[badge-version]: https://img.shields.io/npm/v/namaz.svg
[badge-travis]: https://travis-ci.org/goharanwar/adhan-js.svg?branch=master
[badge-cov]: https://codecov.io/gh/goharanwar/adhan-js/branch/master/graph/badge.svg
[travis]: https://travis-ci.org/goharanwar/adhan-js
[npm]: https://www.npmjs.org/package/namaz
[codecov]: https://codecov.io/gh/goharanwar/adhan-js
