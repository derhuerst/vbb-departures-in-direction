# hafas-departures-in-direction

Pass in a [`vbb-hafas`](https://github.com/derhuerst/vbb-hafas#vbb-hafas)-compatible [HAFAS](https://de.wikipedia.org/wiki/HAFAS) API client and **get departures at a station in a certain direction.**

[![npm version](https://img.shields.io/npm/v/hafas-departures-in-direction.svg)](https://www.npmjs.com/package/hafas-departures-in-direction)
[![build status](https://img.shields.io/travis/derhuerst/hafas-departures-in-direction.svg)](https://travis-ci.org/derhuerst/hafas-departures-in-direction)
![ISC-licensed](https://img.shields.io/github/license/derhuerst/hafas-departures-in-direction.svg)
[![chat on gitter](https://badges.gitter.im/derhuerst.svg)](https://gitter.im/derhuerst)


## Installing

```shell
npm install hafas-departures-in-direction
```


## Usage

Pass in your [`vbb-hafas`](https://github.com/derhuerst/vbb-hafas#vbb-hafas)-compatible [HAFAS](https://de.wikipedia.org/wiki/HAFAS) API client. In this case, we're going to use [`vbb-hafas`](https://github.com/derhuerst/vbb-hafas#vbb-hafas) itself:

```js
const setup = require('hafas-departures-in-direction')
const hafas = require('vbb-hafas')

const depsInDirection = setup(hafas.departures, hafas.journeyLeg)
```

Specify the direction as the *next station* after the one you're querying departures for. `depsInDirection` will then query departures, advancing in time until it found enough results or sent enough requests.

```js
const friedrichstr = '900000100001' // where to get departures
const brandenburgerTor = '900000100025' // direction

depsInDirection(friedrichstr, brandenburgerTor)
.then(console.log)
.catch(console.error)
```

The results will look similar to [those of `vbb-hafas`](https://github.com/derhuerst/vbb-hafas/blob/master/docs/departures.md).

## API

```js
depsInDirection = setup(fetchDepartures, fetchJourneyLeg)
```

`fetchDepartures(stationId, opt)` should be API-compatible with [`vbb-hafas.departures`](https://github.com/derhuerst/vbb-hafas/blob/master/docs/departures.md). `fetchJourneyLeg(ref, lineName, opt)` should be API-compatible with [`vbb-hafas.journeyLeg`](https://github.com/derhuerst/vbb-hafas/blob/master/docs/journey-leg.md). Both should return valid [*FPTF* `1.0.1`](https://github.com/public-transport/friendly-public-transport-format/blob/1.0.1/spec/readme.md).

```js
depsInDirection(station, direction, [opt])
```

`opt` overrides the following defaults:

```js
{
	concurrency: 4, // max nr. of parallel requests
	results: 10, // nr. of results to collect
	maxQueries: 10, // max nr. of requests
	when: 0 // time in ms since epoch
}
```

Returns a `Promise` that resolves with an array of departures.


## Contributing

If you have a question or have difficulties using `hafas-departures-in-direction`, please double-check your code and setup first. If you think you have found a bug or want to propose a feature, refer to [the issues page](https://github.com/derhuerst/hafas-departures-in-direction/issues).
