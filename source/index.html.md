---
title: Snappr API Documentation

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - javascript

toc_footers:
  - <a href='https://www.snappr.co/partner'>Setup an enterprise account</a>
  - <a href='https://app.snappr.co/book'>Book a Snappr without the API</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Snappr API! You can use our API to manage bookings conducted through the Snappr marketplace network. The API is available only to organizations using the Snappr enterprise Photography Portal. If you do not currently have a Photography Portal account and are interesting in setting one up, find out more <a href="https://www.snappr.co/partner">here</a>.

We have language bindings in Shell and JavaScript. You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

# Authentication

```shell
curl "/example-endpoint" \
  -H "Authorization: Bearer zkTvDUe5jJBJFcjc6ckwapEwax8Kbs7h3nv2SHXSgh5qGhHP22ggsu4fbdZgf25z"
```

```javascript
const snappr = require('snappr-api');

let api = snappr.authorize('zkTvDUe5jJBJFcjc6ckwapEwax8Kbs7h3nv2SHXSgh5qGhHP22ggsu4fbdZgf25z');
```

> Make sure to replace the example key above with your API key.

Snappr uses API keys to allow access to the API. You can access or regenerate your API key from your Photography Portal GUI. Each user has their own API key. Regenerating your API key will deactivate your API key. Deleting a user will deactivate their API key.

An API key to be included in all API requests in a header of this format:

<code>Authorization: Bearer <span class="route_param">api_key</span></code>

<aside class="notice">
You must replace <code>api_key</code> with your user API key.
</aside>

# Availability

## Get Availability

> Example request:

```shell
curl "http://api.snappr.co/availability?latitude=34.0522&longitude=-118.2437&shoottype=event&duration=120&date=2018-12-01" \
  -H 'accept-version: 1.0.0' \
  -H "Authorization: Bearer zkTvDUe5jJBJFcjc6ckwapEwax8Kbs7h3nv2SHXSgh5qGhHP22ggsu4fbdZgf25z"
```

```javascript
const snappr = require('snappr-api');

let api = snappr.authorize('zkTvDUe5jJBJFcjc6ckwapEwax8Kbs7h3nv2SHXSgh5qGhHP22ggsu4fbdZgf25z');
let availability = api.availability.get({
  latitude: 34.0522,
  longitude: -118.2437,
  shoottype: "event",
  duration: 120,
  date: "2018-12-01"
});
```

> Example JSON response:

```json
{
  "latitude": 34.0522,
  "longitude": -118.2437,
  "shoottype": "event",
  "duration": 120,
  "date": "2018-12-01",
  "timezone": "America/Los_Angeles",
  "available_times": [
    "2018-12-01T07:30:00Z",
    "2018-12-01T09:30:00Z",
    "2018-12-01T15:00:00Z",
    "2018-12-01T15:30:00Z"
  ]
}
```

This endpoint returns time availability (i.e. available shoot start times) for a combination of location, date, shoottype and duration.

### HTTP Request

<code>GET http://api.snappr.co/availability?latitude=<span class="route_param">:latitude</span>&longitude=<span class="route_param">:longitude</span>&shoottype=<span class="route_param">:shoottype</span>&duration=<span class="route_param">:duration</span>&date=<span class="route_param">:date</span></code>

### Query Parameters

Parameter | Type | Description | Required
--------- | ---- | ----------- | --------
`latitude` | Number | Latitude of the shoot location. | Yes
`longitude` | Number | Longitude of the shoot location. | Yes
`shoottype` | String | Name of the shoottype (see `Shoottypes` endpoints), e.g. "event". | Yes
`duration` | Integer | Length of the shoot in minutes. | Yes
`date` | Date (ISO) | Date for which you want to check time availability. | Yes

<aside class="notice">
All available times within the range of the local date will be returned. However, the format of the returned times is always in UTC for simplicity.
</aside>

# Bookings

## Create New Booking

> Example request:

```shell
curl "http://api.snappr.co/bookings" \
  -H "Authorization: Bearer zkTvDUe5jJBJFcjc6ckwapEwax8Kbs7h3nv2SHXSgh5qGhHP22ggsu4fbdZgf25z" \
  -H 'accept-version: 1.0.0' \
  -H "Content-Type: application/json" \
  --data-binary $'{
    "fields": {
      "latitude": 34.0522,
      "longitude": -118.2437,
      "shoottype": "event",
      "start_at": "2018-12-01T07:30:00Z",
      "duration": 120,
      "location_notes": "Location is Emerald Theatre - ring buzzer at main entrance on arrival",
      "style_notes": "Shots of as many members of crowd as possible; shallow depth of field where possible",
      "customer_firstname": "Mary",
      "customer_surname": "Smith",
      "customer_email": "test@snappr.co",
      "customer_mobilephone": "+14153339966",
      "customer_company": "Snappr Inc."
    }
  }'
```

```javascript
const snappr = require('snappr-api');

let api = snappr.authorize('zkTvDUe5jJBJFcjc6ckwapEwax8Kbs7h3nv2SHXSgh5qGhHP22ggsu4fbdZgf25z');
let bookings = api.bookings.post({
  latitude: 34.0522,
  longitude: -118.2437,
  shoottype: "event",
  start_at: "2018-12-01T07:30:00Z",
  duration: 120,
  location_notes: "Location is Emerald Theatre - ring buzzer at main entrance on arrival",
  style_notes: "Shots of as many members of crowd as possible; shallow depth of field where possible",
  customer_firstname: "Mary",
  customer_surname: "Smith",
  customer_email: "test@snappr.co",
  customer_mobilephone: "+14153339966",
  customer_company: "Snappr Inc."
});
```

> Example JSON response:

```json
{
  "uid": "0ccefa53-b346-4d3e-8dcb-79a914289928",
  "status": "created",
  "credits": 249,
  "latitude": 34.0522,
  "longitude": -118.2437,
  "shoottype": "event",
  "start_at": "2018-12-01T07:30:00Z",
  "duration": 120,
  "location_notes": "Location is Emerald Theatre - ring buzzer at main entrance on arrival",
  "style_notes": "Shots of as many members of crowd as possible; shallow depth of field where possible",
  "customer_firstname": "Mary",
  "customer_surname": "Smith",
  "customer_email": "test@snappr.co",
  "customer_mobilephone": "+14153339966",
  "customer_company": "Snappr Inc.",
  "created_at": "2018-09-01T09:12:00Z",
  "updated_at": "2018-09-01T09:12:00Z"
}
```

This endpoint creates a new photoshoot booking.

### HTTP Request

`POST http://api.snappr.co/bookings`

### Request (Body) Parameters

Parameter | Type | Description | Required
--------- | ---- | ----------- | --------
`latitude` | Number | Latitude of the shoot location. | Yes
`longitude` | Number | Longitude of the shoot location. | Yes
`shoottype` | String | Name of the shoottype (see `Shoottypes` endpoints), e.g. "event". | Yes
`start_at` | Datetime (ISO) | Start time of the shoot in UTC. | Yes
`duration` | Integer | Length of the shoot in minutes. | Yes
`location_notes` | String | Details to help the photographer find the specific location and contact person at the time of the shoot. | No
`style_notes` | String | Instructions, stylistic preferences and other special requests. | No
`customer_firstname` | String | First name of your end-customer. | Yes
`customer_surname` | String | Last name of your end-customer. | No
`customer_email` | String (email) | Valid email address of your end-customer. | Yes
`customer_mobilephone` | String | Valid mobile phone number of your end-customer. | Yes
`customer_company` | String | Length of the shoot in minutes. | No

<aside class="notice">
Always check availability before trying to create a new booking. If you try to make a booking at for combination of location, date/time and shoottype for which there are no available photographers, you will receive a 400 error (see <code>Errors</code> section).
</aside>

## Get All Bookings

```shell
curl "http://example.com/api/bookings"
  -H "Authorization: meowmeowmeow"
```

```javascript
const snappr = require('snappr-api');

let api = snappr.authorize('meowmeowmeow');
let bookings = api.bookings.get();
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all bookings.

### HTTP Request

`GET http://example.com/api/bookings`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include bookings that have already been adopted.

<aside class="success">
Remember — a happy booking is an authenticated booking!
</aside>

## Get a Specific Booking

```shell
curl "http://example.com/api/bookings/2"
  -H "Authorization: meowmeowmeow"
```

```javascript
const snappr = require('snappr-api');

let api = snappr.authorize('meowmeowmeow');
let max = api.bookings.get(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific booking.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/bookings/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the booking to retrieve

## Delete a Specific Booking

```shell
curl "http://example.com/api/bookings/2"
  -X DELETE
  -H "Authorization: meowmeowmeow"
```

```javascript
const snappr = require('snappr-api');

let api = snappr.authorize('meowmeowmeow');
let max = api.bookings.delete(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "deleted" : ":("
}
```

This endpoint deletes a specific booking.

### HTTP Request

`DELETE http://example.com/bookings/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the booking to delete

