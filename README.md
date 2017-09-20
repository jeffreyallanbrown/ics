ics
==================

The [iCalendar](http://tools.ietf.org/html/rfc5545) generator

[![npm version](https://badge.fury.io/js/ics.svg)](http://badge.fury.io/js/ics)
[![Code Climate](https://codeclimate.com/github/adamgibbons/ics/badges/gpa.svg)](https://codeclimate.com/github/adamgibbons/ics)
[![TravisCI build status](https://travis-ci.org/adamgibbons/ics.svg?branch=master)](https://travis-ci.org/adamgibbons/ics.svg?branch=master)

## Install

`npm install -S ics`

## Example Usage

Generate an iCalendar event:

```javascript
import ics from 'ics'

ics.createEvent({
  start: [2018, 4, 30, 6, 30], //  May 30, 2018 at 6:30am (footnote1)
  end: [2018, 4, 30, 15, 0], // May 30, 2018 at 3:00pm
  title: 'Bolder Boulder',
  description: 'Annual 10-kilometer run in Boulder, Colorado',
  location: 'Folsom Field, University of Colorado (finish line)',
  url: 'http://www.bolderboulder.com/',
  status: 'confirmed',
  geo: {
    lat: 40.0095,
    lon: 105.2669
  },
  attendees: [
    { name: 'Adam Gibbons', email: 'adam@example.com' },
    { name: 'Brittany Seaton', email: 'brittany@example2.org' }
  ],
  categories: ['10k races', 'Memorial Day Weekend', 'Boulder CO']
}, (error, result) => {

  console.log(result)

  //  BEGIN:VCALENDAR
  //  VERSION:2.0
  //  CALSCALE:GREGORIAN
  //  PRODID:-//Adam Gibbons//agibbons.com//ICS: iCalendar Generator
  //  BEGIN:VEVENT
  //  UID:2073c980-9545-11e6-99f9-791bff9883ed
  //  DTSTAMP:20161018T151121Z
  //  DTSTART:20160530T065000
  //  DTEND:20160530T150000
  //  SUMMARY:Bolder Boulder
  //  DESCRIPTION:Annual 10-kilometer run in Boulder, Colorado
  //  LOCATION:Folsom Field, University of Colorado (finish line)
  //  URL:http://www.bolderboulder.com/
  //  STATUS:confirmed
  //  GEO:40.0095;105.2669
  //  ATTENDEE;CN=Adam Gibbons:mailto:adam@example.com
  //  ATTENDEE;CN=Brittany Seaton:mailto:brittany@example2.org
  //  CATEGORIES:10k races,Memorial Day Weekend,Boulder CO
  //  BEGIN:VALARM
  //  ACTION:DISPLAY
  //  TRIGGER:-PT24H
  //  DESCRIPTION:Reminder
  //  REPEAT:1
  //  DURATION:PT15M
  //  END:VALARM
  //  BEGIN:VALARM
  //  ACTION:AUDIO
  //  TRIGGER:-PT30M
  //  END:VALARM
  //  END:VEVENT
  //  END:VCALENDAR



})
```

## API

### `createEvent(attributes, cb)`

Returns an iCal-compliant text string.

#### `attributes`

Object literal containing event information.
Only the `start` property is required.
The following properties are accepted:

| Property      | Description   | Example  |
| ------------- | ------------- | ----------
| start         | Array of numbers representing time at which event starts _in the server's timezone_. Array has form: `[year, month, date, hours, minutes, seconds]` | `[2000, 0, 5, 10, 0]` // January 5, 2000 at 10am
| end           | Time at which event ends | `[2000, 0, 5, 13, 5]` // January 5, 2000 at 1pm
| title         | String. Title of event. | `'Code review'`
| description   | String. Description of event. | `'A constructive roasting of those seeking to merge into master branch'`
| location      | String. Intended venue | `Mountain Sun Pub and Brewery`.
| geo           | Object literal. Geographic coordinates (lat/lon) as numbers | `{ lat: 38.9072, lon: 77.0369 }`
| url           | URL associated with event | `'http://www.mountainsunpub.com/'`
| status        | String. Must be one of: `tentative`, `confirmed`, or `cancelled` | `confirmed`
| attendees     | Array of object literals | `{name: 'Adam Gibbons', email: 'adam@example.com'}`
| categories    | Array of string | `['hacknight', 'stout month']`
| alarms        | Array of object literals | `{ action: 'DISPLAY', trigger: '-PT30M' }`

#### `cb`

Node-style callback that returns an error or formatted ical string

```
function(error, success) {
  if (error) { /* handle error */ }

  return success
}
```

## References

- [RFC 5545: Internet Calendaring and Scheduling Core Object Specification (iCalendar)](http://tools.ietf.org/html/rfc5545)
- [iCalendar Validator](http://icalendar.org/validator.html#results)
