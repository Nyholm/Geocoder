Geocoder
========

[![Build Status](https://travis-ci.org/geocoder-php/Geocoder.svg?branch=master)](http://travis-ci.org/geocoder-php/Geocoder)
[![Software License](https://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat-square)](LICENSE)

> **Important:** You are browsing the documentation of Geocoder **4.x** (not
> released yet).
>
> Documentation for version **3.x** is available here: [Geocoder 3.x
> documentation](https://github.com/geocoder-php/Geocoder/blob/3.x/README.md).
>
> Documentation for version **2.x** is available here: [Geocoder 2.x
> documentation](https://github.com/geocoder-php/Geocoder/blob/2.x/README.md).

---

**Geocoder** is a PHP library which helps you build geo-aware applications by
providing a powerful abstraction layer for geocoding manipulations.

* [Installation](#installation)
* [Cookbook](#cookbook)
* [Usage](#usage)
* [Providers](#providers)
* [Special Geocoders and Providers](#special-geocoders-and-providers) 
* [Dumpers](#dumpers)
* [Formatters](#formatters)
* [Versioning](#versioning)


Installation
------------

To install a Geocoder there are two things you need to know: 

1) What [Geocoder provider](https://packagist.org/providers/geocoder-php/provider-implementation) you want to use
2) What [HTTP client/adapter](https://packagist.org/providers/php-http/client-implementation) you want to use. 

### Geocoder providers

Since 4.0 we do not include providers by default. You need to select a *geocoder provider*. You will see a list of 
providers [at Packagist](https://packagist.org/providers/geocoder-php/provider-implementation)

### HTTP Clients

In order to talk to geocoding APIs, you need HTTP adapters. While it was part of
the library in Geocoder before, Geocoder 4.x and upper now relies on HTTPlug
which defines how HTTP message should be sent and received. You can use any library to send HTTP messages
that implements [php-http/client-implementation](https://packagist.org/providers/php-http/client-implementation).

Here is a list of all officially supported clients and adapters by HTTPlug: http://docs.php-http.org/en/latest/clients.html

Read more about HTTPlug in [their docs](http://docs.php-http.org/en/latest/httplug/users.html).

### Summery (Just give me the command)

To install Google Maps geocoder with Guzzle 6 you may run the following command: 

```
$ composer require geocoder-php/google-maps-provider:@beta php-http/guzzle6-adapter php-http/message geocoder-php/common-http:@beta willdurand/geocoder:@beta
```

Cookbook
--------

We have a small cookbook where you can find examples on common use cases:

* [Caching responses](/docs/cookbook/cache.md)
* [Configuring the HTTP client](/docs/cookbook/http-client.md)

Usage
-----

In the code snippet below we use GoogleMaps and Guzzle6. 

```php
$adapter  = new \Http\Adapter\Guzzle6\Client();
$provider = new \Geocoder\Provider\GoogleMaps($adapter);
$geocoder = new \Geocoder\StatefulGeocoder($provider, 'en');

$result = $geocoder->geocodeQuery(GeocodeQuery::create('Buckingham Palace, London'));
$result = $geocoder->reverseQuery(ReverseQuery::fromCoordinates(...));
```

The `Provider` interface has three methods:

* `geocodeQuery(GeocodeQuery $query):AddressCollection`
* `reverseQuery(ReverseQuery $query):AddressCollection`
* `getName():string`


The `Geocoder` interface extends the `Provider` interface and exposes two additional methods. They will
make migration from 3.x smoother.

* `geocode($streetOrIpAddress)`
* `reverse($latitude, $longitude)`


Providers
---------

Providers perform the geocoding black magic for you (talking to the APIs, fetching results, dealing with errors, etc.) 
and are highly configurable.


Provider       | Package | Features | Stats
:------------- |:------- |:-------- |:-------
[ArcGIS Online](https://github.com/geocoder-php/arcgis-online-provider) | `geocoder-php/arcgis-online-provider` | address, reverse <br> [Website](https://developers.arcgis.com/en/features/geocoding/) | [![Latest Stable Version](https://poser.pugx.org/geocoder-php/arcgis-online-provider/v/stable)](https://packagist.org/packages/geocoder-php/arcgis-online-provider) <br>[![Total Downloads](https://poser.pugx.org/geocoder-php/arcgis-online-provider/downloads)](https://packagist.org/packages/geocoder-php/arcgis-online-provider)
[Bing Maps](https://github.com/geocoder-php/bing-maps-provider) | `geocoder-php/bing-maps-provider` | address, reverse <br> [Website](http://msdn.microsoft.com/en-us/library/ff701713.aspx) | [![Latest Stable Version](https://poser.pugx.org/geocoder-php/bing-maps-provider/v/stable)](https://packagist.org/packages/geocoder-php/bing-maps-provider) <br>[![Total Downloads](https://poser.pugx.org/geocoder-php/bing-maps-provider/downloads)](https://packagist.org/packages/geocoder-php/bing-maps-provider)
[Chain](https://github.com/geocoder-php/chain-provider) | `geocoder-php/chain-provider` | Interates over multiple providers | [![Latest Stable Version](https://poser.pugx.org/geocoder-php/chain-provider/v/stable)](https://packagist.org/packages/geocoder-php/chain-provider) <br>[![Total Downloads](https://poser.pugx.org/geocoder-php/chain-provider/downloads)](https://packagist.org/packages/geocoder-php/chain-provider)
[FreeGeoIp](https://github.com/geocoder-php/free-geoip-provider) | `geocoder-php/free-geoip-provider` | IPv4, IPv6 <br> [Website](http://freegeoip.net/) | [![Latest Stable Version](https://poser.pugx.org/geocoder-php/free-geoip-provider/v/stable)](https://packagist.org/packages/geocoder-php/free-geoip-provider) <br>[![Total Downloads](https://poser.pugx.org/geocoder-php/free-geoip-provider/downloads)](https://packagist.org/packages/geocoder-php/free-geoip-provider)
[GeoIPs](https://github.com/geocoder-php/geoip-provider) | `geocoder-php/geoip-provider` | IPv4, local <br> [Website](http://www.geoips.com/en/) | [![Latest Stable Version](https://poser.pugx.org/geocoder-php/geoip-provider/v/stable)](https://packagist.org/packages/geocoder-php/geoip-provider) <br>[![Total Downloads](https://poser.pugx.org/geocoder-php/geoip-provider/downloads)](https://packagist.org/packages/geocoder-php/geoip-provider)
[GeoIP2](https://github.com/geocoder-php/geoip2-provider) | `geocoder-php/geoip2-provider` | IPv4 <br> [Website](https://www.maxmind.com/en/geoip2-databases) | [![Latest Stable Version](https://poser.pugx.org/geocoder-php/geoip2-provider/v/stable)](https://packagist.org/packages/geocoder-php/geoip2-provider) <br>[![Total Downloads](https://poser.pugx.org/geocoder-php/geoip2-provider/downloads)](https://packagist.org/packages/geocoder-php/geoip2-provider)
[GeoIPs](https://github.com/geocoder-php/geoips-provider) | `geocoder-php/geoips-provider` | IPv4 <br>  | [![Latest Stable Version](https://poser.pugx.org/geocoder-php/geoips-provider/v/stable)](https://packagist.org/packages/geocoder-php/geoips-provider) <br>[![Total Downloads](https://poser.pugx.org/geocoder-php/geoips-provider/downloads)](https://packagist.org/packages/geocoder-php/geoips-provider)
[Geonames](https://github.com/geocoder-php/geonames-provider) | `geocoder-php/geonames-provider` | address, reverse <br> [Website](http://www.geonames.org/commercial-webservices.html) | [![Latest Stable Version](https://poser.pugx.org/geocoder-php/geonames-provider/v/stable)](https://packagist.org/packages/geocoder-php/geonames-provider) <br>[![Total Downloads](https://poser.pugx.org/geocoder-php/geonames-provider/downloads)](https://packagist.org/packages/geocoder-php/geonames-provider)
[GeoPlugin](https://github.com/geocoder-php/geo-plugin-provider) | `geocoder-php/geo-plugin-provider` | IPv4, IPv6 <br> [Website](http://www.geoplugin.com/) | [![Latest Stable Version](https://poser.pugx.org/geocoder-php/geo-plugin-provider/v/stable)](https://packagist.org/packages/geocoder-php/geo-plugin-provider) <br>[![Total Downloads](https://poser.pugx.org/geocoder-php/geo-plugin-provider/downloads)](https://packagist.org/packages/geocoder-php/geo-plugin-provider)
[Google Maps](https://github.com/geocoder-php/google-maps-provider) <br> Google Maps for business | `geocoder-php/google-maps-provider` | address, reverse <br> [Website](https://developers.google.com/maps/documentation/geocoding/) | [![Latest Stable Version](https://poser.pugx.org/geocoder-php/google-maps-provider/v/stable)](https://packagist.org/packages/geocoder-php/google-maps-provider) <br>[![Total Downloads](https://poser.pugx.org/geocoder-php/google-maps-provider/downloads)](https://packagist.org/packages/geocoder-php/google-maps-provider)
[HostIp](https://github.com/geocoder-php/host-ip-provider) | `geocoder-php/host-ip-provider` | IPv4 <br> [Website](http://www.hostip.info/use.html) | [![Latest Stable Version](https://poser.pugx.org/geocoder-php/host-ip-provider/v/stable)](https://packagist.org/packages/geocoder-php/host-ip-provider) <br>[![Total Downloads](https://poser.pugx.org/geocoder-php/host-ip-provider/downloads)](https://packagist.org/packages/geocoder-php/host-ip-provider)
[IpInfoDB](https://github.com/geocoder-php/ip-info-db-provider) | `geocoder-php/ip-info-db-provider` | IPv4 <br> [Website](http://ipinfodb.com/) | [![Latest Stable Version](https://poser.pugx.org/geocoder-php/ip-info-db-provider/v/stable)](https://packagist.org/packages/geocoder-php/ip-info-db-provider) <br>[![Total Downloads](https://poser.pugx.org/geocoder-php/ip-info-db-provider/downloads)](https://packagist.org/packages/geocoder-php/ip-info-db-provider)
[MapQuest](https://github.com/geocoder-php/mapquest-provider) | `geocoder-php/mapquest-provider` | address, reverse <br> [Website](http://developer.mapquest.com/web/products/dev-services/geocoding-ws) | [![Latest Stable Version](https://poser.pugx.org/geocoder-php/mapquest-provider/v/stable)](https://packagist.org/packages/geocoder-php/mapquest-provider) <br>[![Total Downloads](https://poser.pugx.org/geocoder-php/mapquest-provider/downloads)](https://packagist.org/packages/geocoder-php/mapquest-provider)
[Mapzen](https://github.com/geocoder-php/mapzen-provider) | `geocoder-php/mapzen-provider` | address, reverse <br> [Website](https://mapzen.com/documentation/search/) | [![Latest Stable Version](https://poser.pugx.org/geocoder-php/mapzen-provider/v/stable)](https://packagist.org/packages/geocoder-php/mapzen-provider) <br>[![Total Downloads](https://poser.pugx.org/geocoder-php/mapzen-provider/downloads)](https://packagist.org/packages/geocoder-php/mapzen-provider)
[MaxMind](https://github.com/geocoder-php/maxmind-provider) | `geocoder-php/maxmind-provider` | IPv4, IPv6 <br> [Website](https://www.maxmind.com/) | [![Latest Stable Version](https://poser.pugx.org/geocoder-php/maxmind-provider/v/stable)](https://packagist.org/packages/geocoder-php/maxmind-provider) <br>[![Total Downloads](https://poser.pugx.org/geocoder-php/maxmind-provider/downloads)](https://packagist.org/packages/geocoder-php/maxmind-provider)
[MaxMind Binary](https://github.com/geocoder-php/maxmind-binary-provider) | `geocoder-php/maxmind-binary-provider` | IPv4, IPv6 <br> [Website](https://www.maxmind.com/) | [![Latest Stable Version](https://poser.pugx.org/geocoder-php/maxmind-binary-provider/v/stable)](https://packagist.org/packages/geocoder-php/maxmind-binary-provider) <br>[![Total Downloads](https://poser.pugx.org/geocoder-php/maxmind-binary-provider/downloads)](https://packagist.org/packages/geocoder-php/maxmind-binary-provider)
[Nominatim](https://github.com/geocoder-php/nominatim-provider) <br> (OpenStreetMap) | `geocoder-php/nominatim-provider` | address, reverse, IPv4 <br> [Website](http://wiki.openstreetmap.org/wiki/Nominatim) | [![Latest Stable Version](https://poser.pugx.org/geocoder-php/nominatim-provider/v/stable)](https://packagist.org/packages/geocoder-php/nominatim-provider) <br>[![Total Downloads](https://poser.pugx.org/geocoder-php/nominatim-provider/downloads)](https://packagist.org/packages/geocoder-php/nominatim-provider)
[OpenCage](https://github.com/geocoder-php/open-cage-provider) | `geocoder-php/open-cage-provider` | address, reverse <br> [Website](http://geocoder.opencagedata.com/) | [![Latest Stable Version](https://poser.pugx.org/geocoder-php/open-cage-provider/v/stable)](https://packagist.org/packages/geocoder-php/open-cage-provider) <br>[![Total Downloads](https://poser.pugx.org/geocoder-php/open-cage-provider/downloads)](https://packagist.org/packages/geocoder-php/open-cage-provider)
[TomTom](https://github.com/geocoder-php/tomtom-provider) | `geocoder-php/tomtom-provider` | address, reverse <br> [Website](https://geocoder.tomtom.com/app/view/index) | [![Latest Stable Version](https://poser.pugx.org/geocoder-php/tomtom-provider/v/stable)](https://packagist.org/packages/geocoder-php/tomtom-provider) <br>[![Total Downloads](https://poser.pugx.org/geocoder-php/tomtom-provider/downloads)](https://packagist.org/packages/geocoder-php/tomtom-provider)
[Yandex](https://github.com/geocoder-php/yandex-provider) | `geocoder-php/yandex-provider` | address, reverse <br> [Website](http://api.yandex.com/maps/) | [![Latest Stable Version](https://poser.pugx.org/geocoder-php/yandex-provider/v/stable)](https://packagist.org/packages/geocoder-php/yandex-provider) <br>[![Total Downloads](https://poser.pugx.org/geocoder-php/yandex-provider/downloads)](https://packagist.org/packages/geocoder-php/yandex-provider)

Special Geocoders and Providers
-------------------------------

### The Chain Provider

The `Chain` provider is a special provider that takes a list of providers and
iterates over this list to get information. Note that it **stops** its iteration
when a provider returns a result. The result is returned by `GoogleMaps` because
`FreeGeoIp` and `HostIp` cannot geocode street addresses. `BingMaps` is ignored.

``` php
$geocoder = new \Geocoder\ProviderAggregator();
$adapter  = new \Http\Adapter\Guzzle6\Client();

$chain = new \Geocoder\Provider\Chain([
    new \Geocoder\Provider\FreeGeoIp($adapter),
    new \Geocoder\Provider\HostIp($adapter),
    new \Geocoder\Provider\GoogleMaps($adapter, 'France'),
    new \Geocoder\Provider\BingMaps($adapter, '<API_KEY>'),
    // ...
]);

$geocoder->registerProvider($chain);

$result = $geocoder->geocodeQuery(GeocodeQuery::create('10 rue Gambetta, Paris, France'));
var_export($result);
```

Everything is ok, enjoy!

### The ProviderAggregator

The `ProviderAggregator` is used to register several providers so that you can
decide which provider to use later on.

``` php
$adapter  = new \Http\Adapter\Guzzle6\Client();
$geocoder = new \Geocoder\ProviderAggregator();

$geocoder->registerProviders([
    new \Geocoder\Provider\GoogleMaps($adapter),
    new \Geocoder\Provider\GoogleMapsBusiness($adapter, '<CLIENT_ID>'),
    new \Geocoder\Provider\Yandex($adapter),
    new \Geocoder\Provider\MaxMind($adapter, '<MAXMIND_API_KEY>'),
    new \Geocoder\Provider\ArcGISOnline($adapter),
]);

$geocoder->registerProvider(new \Geocoder\Provider\Nominatim($adapter, 'https://your.nominatim.server'));

$geocoder
    ->using('google_maps')
    ->geocodeQuery(GeocodeQuery::create( ... ));

$geocoder
    ->limit(10)
    ->reverseQuery(ReverseQuery::fromCoordinates($lat, $lng));
```

The `ProviderAggregator`'s API is fluent, meaning you can write:

``` php
$locations = $geocoder
    ->registerProvider(new \My\Provider\Custom($adapter))
    ->using('custom')
    ->limit(10)
    ->geocodeQuery(GeocodeQuery::create( ... ));
```

The `using()` method allows you to choose the `provider` to use by its name.
When you deal with multiple providers, you may want to choose one of them.  The
default behavior is to use the first one but it can be annoying.

The `limit()` method allows you to configure the maximum number of results being
returned. Depending on the provider you may not get as many results as expected,
it is a maximum limit, not the expected number of results.

### TimedGeocoder

The `TimedGeocoder` class profiles each `geocode` and `reverse` call. So you can
easily figure out how many time/memory was spent for each geocoder/reverse call.

```php
// configure your provider
$provider = // ...

$stopwatch = new \Symfony\Component\Stopwatch\Stopwatch();
$geocoder = new \Geocoder\TimedGeocoder($provider, $stopwatch);

$geocoder->geocodeQuery(GeocodeQuery::create('Paris, France'));

// Now you can debug your application
```

We use the [symfony/stopwatch](http://symfony.com/doc/current/components/stopwatch.html)
component under the hood. Which means, if you use the Symfony framework the
geocoder calls will appear in your timeline section in the Web Profiler.

### StatefulGeocoder

The `StatefulGeocoder` class is great when you want your Geocoder to hold state. Say you want to configure locale, 
limit or bounds in runtime. The `StatefulGeocoder` will append these values on each query. 

```php
// configure your provider
$provider = // ...
$geocoder = new \Geocoder\StatefulGeocoder($provider);

$geocoder->setLocale('en');
$results = $geocoder->geocodeQuery(GeocodeQuery::create('London'));
echo $results->first()->getLocality(); // London

$geocoder->setLocale('es');
$results = $geocoder->geocodeQuery(GeocodeQuery::create('London'));
echo $results->first()->getLocality(); // Londres
```

Dumpers
-------

**Geocoder** provides dumpers that aim to transform a `Location` object in
standard formats.

#### GPS eXchange Format (GPX)

The **GPS eXchange** format is designed to share geolocated data like point of
interests, tracks, ways, but also coordinates. **Geocoder** provides a dumper to
convert a `Location` object in an GPX compliant format.

Assuming we got a `$location` object as seen previously:

``` php
$dumper = new \Geocoder\Dumper\Gpx();
$strGpx = $dumper->dump($location);

echo $strGpx;
```

It will display:

``` xml
<gpx
    version="1.0"
    creator="Geocoder" version="1.0.1-dev"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns="http://www.topografix.com/GPX/1/0"
    xsi:schemaLocation="http://www.topografix.com/GPX/1/0 http://www.topografix.com/GPX/1/0/gpx.xsd">
    <bounds minlat="2.388911" minlon="48.863151" maxlat="2.388911" maxlon="48.863151"/>
    <wpt lat="48.8631507" lon="2.3889114">
        <name><![CDATA[Paris]]></name>
        <type><![CDATA[Address]]></type>
    </wpt>
</gpx>
```

#### GeoJSON

[GeoJSON](http://geojson.org/) is a format for encoding a variety of geographic
data structures.

#### GeoArray

Simple PHP array format for using with your own encoders.

#### Keyhole Markup Language (KML)

[Keyhole Markup Language](http://en.wikipedia.org/wiki/Keyhole_Markup_Language)
is an XML notation for expressing geographic annotation and visualization within
Internet-based, two-dimensional maps and three-dimensional Earth browsers.


#### Well-Known Binary (WKB)

The Well-Known Binary (WKB) representation for geometric values is defined by
the OpenGIS specification.


#### Well-Known Text (WKT)

Well-known text (WKT) is a text markup language for representing vector geometry
objects on a map, spatial reference systems of spatial objects and
transformations between spatial reference systems.

Formatters
----------

A common use case is to print geocoded data. Thanks to the `StringFormatter`
class, it's simple to format a `Location` object as a string:

``` php
// $location is an instance of Location
$formatter = new \Geocoder\Formatter\StringFormatter();

$formatter->format($location, '%S %n, %z %L');
// 'Badenerstrasse 120, 8001 Zuerich'

$formatter->format($location, '<p>%S %n, %z %L</p>');
// '<p>Badenerstrasse 120, 8001 Zuerich</p>'
```

Here is the mapping:

* Street Number: `%n`
* Street Name: `%S`
* City: `%L`
* City District: `%D`
* Zipcode: `%z`
* Admin Level Name: `%A1`, `%A2`, `%A3`, `%A4`, `%A5`
* Admin Level Code: `%a1`, `%a2`, `%a3`, `%a4`, `%a5`
* Country: `%C`
* Country Code: `%c`
* Timezone: `%T`

Versioning
----------

Geocoder follows [Semantic Versioning](http://semver.org/).

### End Of Life

#### 1.x

As of December 2014, branch `1.7` is not officially supported anymore, meaning
major version `1` reached end of life. Last version is:
[1.7.1](https://github.com/geocoder-php/Geocoder/releases/tag/1.7.1).

#### 2.x

As of December 2014, version [2.x](https://github.com/geocoder-php/Geocoder/tree/2.x)
is in a **feature frozen** state. All new features should be contributed to version 3.0
and upper. Last version is:
[2.8.1](https://github.com/geocoder-php/Geocoder/releases/tag/2.8.1).

Major version `2` will reach **end of life on December 2015**.

### Stable Version

Version `3.x` is the current major stable version of Geocoder.

### Next version

The next version is 4.0 which is currently in beta. 

Contributing
------------

See [`CONTRIBUTING`](https://github.com/geocoder-php/Geocoder/blob/master/CONTRIBUTING.md#contributing) file.


Unit Tests
----------

In order to run the test suite, install the development dependencies:

```
$ composer install --dev
```

Then, run the following command:

```
$ composer test
```

You'll obtain some _skipped_ unit tests due to the need of API keys.

Rename the `phpunit.xml.dist` file to `phpunit.xml`, then uncomment the
following lines and add your own API keys:

``` xml
<php>
    <!-- <server name="IPINFODB_API_KEY" value="YOUR_API_KEY" /> -->
    <!-- <server name="BINGMAPS_API_KEY" value="YOUR_API_KEY" /> -->
    <!-- <server name="GEOIPS_API_KEY" value="YOUR_API_KEY" /> -->
    <!-- <server name="MAXMIND_API_KEY" value="YOUR_API_KEY" /> -->
    <!-- <server name="GEONAMES_USERNAME" value="YOUR_USERNAME" /> -->
    <!-- <server name="TOMTOM_MAP_KEY" value="YOUR_MAP_KEY" /> -->
    <!-- <server name="GOOGLE_GEOCODING_KEY" value="YOUR_GEOCODING_KEY" /> -->
    <!-- <server name="OPENCAGE_API_KEY" value="YOUR_API_KEY" /> -->
</php>
```

You're done.


Credits
-------

* William Durand <william.durand1@gmail.com>
* Tobias Nyholm <tobias.nyholm@gmail.com>
* [All contributors](https://github.com/geocoder-php/Geocoder/contributors)


Contributor Code of Conduct
---------------------------

As contributors and maintainers of this project, we pledge to respect all people
who contribute through reporting issues, posting feature requests, updating
documentation, submitting pull requests or patches, and other activities.

We are committed to making participation in this project a harassment-free
experience for everyone, regardless of level of experience, gender, gender
identity and expression, sexual orientation, disability, personal appearance,
body size, race, age, or religion.

Examples of unacceptable behavior by participants include the use of sexual
language or imagery, derogatory comments or personal attacks, trolling, public
or private harassment, insults, or other unprofessional conduct.

Project maintainers have the right and responsibility to remove, edit, or reject
comments, commits, code, wiki edits, issues, and other contributions that are
not aligned to this Code of Conduct. Project maintainers who do not follow the
Code of Conduct may be removed from the project team.

Instances of abusive, harassing, or otherwise unacceptable behavior may be
reported by opening an issue or contacting one or more of the project
maintainers.

This Code of Conduct is adapted from the [Contributor
Covenant](http:contributor-covenant.org), version 1.0.0, available at
[http://contributor-covenant.org/version/1/0/0/](http://contributor-covenant.org/version/1/0/0/)


License
-------

Geocoder is released under the MIT License. See the bundled LICENSE file for
details.
