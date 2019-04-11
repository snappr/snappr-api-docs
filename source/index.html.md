---
title: Snappr API Documentation

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  # - javascript

toc_footers:
  - <a href='https://www.snappr.co/partner'>Setup an enterprise account</a>
  - <a href='https://app.snappr.co/book'>Book a Snappr without the API</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Snappr API! You can use our API to commission and manage photoshoots within the Snappr marketplace network. The API is available only to organizations using the Snappr enterprise Photography Portal. If you do not currently have a Photography Portal account and are interesting in setting one up, find out more <a href="https://www.snappr.co/partner">here</a>.

We have code examples in Shell. You can them in the panel on the right. We will have language bindings for JavaScript very soon.

<!-- We have language bindings in Shell and JavaScript. You can view code examples in the panel on the right, and you can switch the programming language of the examples with the tabs on the top-right. -->

# Authentication

```shell
curl "/example-endpoint" \
  -H "Authorization: Bearer zkTvDUe5jJBJFcjc6ckwapEwax8Kbs7h3nv2SHXSgh5qGhHP22ggsu4fbdZgf25z"
```

<!-- ```javascript
const snappr = require('snappr-api');

let api = snappr.authorize(
  'zkTvDUe5jJBJFcjc6ckwapEwax8Kbs7h3nv2SHXSgh5qGhHP22ggsu4fbdZgf25z'
);
``` -->

> Replace the example key above with your API key. This fake key is used in all of the following request examples.

Snappr uses API keys to allow access to the API. You can access or regenerate your API key from your Photography Portal GUI. Each user has a single unique active API key. Regenerating your API key will deactivate your previous key. Deleting a user will deactivate their API key.

An API key needs to be included in all API requests in a header of this format:

<code>Authorization: Bearer <span class="route_param">api_key</span></code>

<aside class="notice">
You must replace <code>api_key</code> with your user API key.
</aside>

# Availability

## Get Availability

> Example request:

```shell
curl "https://api.snappr.co/availability?latitude=34.0522&longitude=-118.2437&shoottype=event&duration=120&date=2018-12-01" \
  -H 'accept-version: 1.0.0' \
  -H "Authorization: Bearer zkTvDUe5jJBJFcjc6ckwapEwax8Kbs7h3nv2SHXSgh5qGhHP22ggsu4fbdZgf25z"
```

<!-- ```javascript
const snappr = require('snappr-api');

let api = snappr.authorize(
  'zkTvDUe5jJBJFcjc6ckwapEwax8Kbs7h3nv2SHXSgh5qGhHP22ggsu4fbdZgf25z'
);
let availability = api.availability.get({
  latitude: 34.0522,
  longitude: -118.2437,
  shoottype: 'event',
  duration: 120,
  date: '2018-12-01'
});
``` -->

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

<code>GET https://api.snappr.co/availability?latitude=<span class="route_param">:latitude</span>&longitude=<span class="route_param">:longitude</span>&shoottype=<span class="route_param">:shoottype</span>&duration=<span class="route_param">:duration</span>&date=<span class="route_param">:date</span></code>

### Query Parameters

| Parameter   | Type       | Description                                                       | Required |
| ----------- | ---------- | ----------------------------------------------------------------- | -------- |
| `latitude`  | Number     | Latitude of the shoot location.                                   | Yes      |
| `longitude` | Number     | Longitude of the shoot location.                                  | Yes      |
| `shoottype` | String     | Name of the shoottype (see `Shoottypes` endpoints), e.g. "event". | Yes      |
| `duration`  | Integer    | Length of the shoot in minutes.                                   | Yes      |
| `date`      | Date (ISO) | Date for which you want to check time availability.               | Yes      |

<aside class="notice">
All available times within the range of the local date will be returned. However, the format of the returned times is always in UTC for simplicity.
</aside>

# Bookings

## Create New Booking

> Example request when start_at is provided:

```shell
curl "https://api.snappr.co/bookings" \
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

<!-- ```javascript
const snappr = require('snappr-api');

let api = snappr.authorize(
  'zkTvDUe5jJBJFcjc6ckwapEwax8Kbs7h3nv2SHXSgh5qGhHP22ggsu4fbdZgf25z'
);
let bookings = api.bookings.post({
  latitude: 34.0522,
  longitude: -118.2437,
  shoottype: 'event',
  start_at: '2018-12-01T07:30:00Z',
  duration: 120,
  location_notes:
    'Location is Emerald Theatre - ring buzzer at main entrance on arrival',
  style_notes:
    'Shots of as many members of crowd as possible; shallow depth of field where possible',
  customer_firstname: 'Mary',
  customer_surname: 'Smith',
  customer_email: 'test@snappr.co',
  customer_mobilephone: '+14153339966',
  customer_company: 'Snappr Inc.'
});
``` -->

> Example JSON response:

```json
{
  "uid": "0ccefa53-b346-4d3e-8dcb-79a914289928",
  "status": "paid",
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
  "photographer_name": "Hollie B.",
  "created_at": "2018-09-01T09:12:00Z",
  "updated_at": "2018-09-01T09:12:00Z"
}
```

> Example request when start_at is not provided (end-customer picked date and time):

```shell
curl "https://api.snappr.co/bookings" \
  -H "Authorization: Bearer zkTvDUe5jJBJFcjc6ckwapEwax8Kbs7h3nv2SHXSgh5qGhHP22ggsu4fbdZgf25z" \
  -H 'accept-version: 1.0.0' \
  -H "Content-Type: application/json" \
  --data-binary $'{
    "fields": {
      "latitude": 34.0522,
      "longitude": -118.2437,
      "shoottype": "event",
      "start_at": null,
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

<!-- ```javascript
const snappr = require('snappr-api');

let api = snappr.authorize(
  'zkTvDUe5jJBJFcjc6ckwapEwax8Kbs7h3nv2SHXSgh5qGhHP22ggsu4fbdZgf25z'
);
let bookings = api.bookings.post({
  latitude: 34.0522,
  longitude: -118.2437,
  shoottype: 'event',
  start_at: null,
  duration: 120,
  location_notes:
    'Location is Emerald Theatre - ring buzzer at main entrance on arrival',
  style_notes:
    'Shots of as many members of crowd as possible; shallow depth of field where possible',
  customer_firstname: 'Mary',
  customer_surname: 'Smith',
  customer_email: 'test@snappr.co',
  customer_mobilephone: '+14153339966',
  customer_company: 'Snappr Inc.'
});
``` -->

> Example JSON response:

```json
{
  "uid": "0ccefa53-b346-4d3e-8dcb-79a914289928",
  "status": "paid_pending_schedule",
  "credits": 249,
  "latitude": 34.0522,
  "longitude": -118.2437,
  "shoottype": "event",
  "duration": 120,
  "location_notes": "Location is Emerald Theatre - ring buzzer at main entrance on arrival",
  "style_notes": "Shots of as many members of crowd as possible; shallow depth of field where possible",
  "customer_firstname": "Mary",
  "customer_surname": "Smith",
  "customer_email": "test@snappr.co",
  "customer_mobilephone": "+14153339966",
  "customer_company": "Snappr Inc.",
  "photographer_name": "Hollie B.",
  "created_at": "2018-09-01T09:12:00Z",
  "updated_at": "2018-09-01T09:12:00Z"
}
```

This endpoint creates a new photoshoot booking.

Broadly, there are two main ways to create a new photoshoot booking, and examples are provided for each:

1. Provide all shoot details _including_ the start date and time for the shoot (`start_at`). With this option, Snappr will not seek any input on shoot details from your end-customer, they will simply be notified if the booking is placed successfully.
2. Provide all shoot details _except_ the start date and time for the shoot (`start_at` set to `null`). This will trigger an automatic process to collect the start date and time from your end-customer using the contact details (email and/or mobile phone) provided. End-customers will be able to select from all available dates and times for the shoot location, using a Snappr UI.

### HTTP Request

`POST https://api.snappr.co/bookings`

### Request (Body) Parameters

| Parameter              | Type           | Description                                                                                                                           | Required |
| ---------------------- | -------------- | ------------------------------------------------------------------------------------------------------------------------------------- | -------- |
| `latitude`             | Number         | Latitude of the shoot location.                                                                                                       | Yes      |
| `longitude`            | Number         | Longitude of the shoot location.                                                                                                      | Yes      |
| `shoottype`            | String         | Name of the shoottype (see `Shoottypes` endpoints), e.g. "event".                                                                     | Yes      |
| `start_at`             | Datetime (ISO) | Start time of the shoot in UTC. If this is set to `null`, then Snappr will automatically seek this information from the end-customer. | Yes      |
| `duration`             | Integer        | Length of the shoot in minutes.                                                                                                       | Yes      |
| `location_notes`       | String         | Details to help the photographer find the specific location and contact person at the time of the shoot.                              | No       |
| `style_notes`          | String         | Instructions, stylistic preferences and other special requests.                                                                       | No       |
| `customer_firstname`   | String         | First name of your end-customer.                                                                                                      | Yes      |
| `customer_surname`     | String         | Last name of your end-customer.                                                                                                       | No       |
| `customer_email`       | String (email) | Valid email address of your end-customer.                                                                                             | Yes      |
| `customer_mobilephone` | String         | Valid mobile phone number of your end-customer.                                                                                       | Yes      |
| `customer_company`     | String         | Name of your end-customer's company.                                                                                                  | No       |

<aside class="notice">

If <code>start_at</code> is set to <code>null</code> the system assumes you want the end-customer to pick the start date and time.

</aside>

<aside class="notice">
Always check <a href="#availability">availability</a> before trying to create a new booking with <code>start_at</code> defined. If you try to make a booking at for combination of location, date/time and shoottype for which there are no available photographers, you will receive a 400 error (see <code>Errors</code> section).
</aside>

## Get All Bookings

> Example request:

```shell
curl "https://api.snappr.co/bookings" \
  -H "Authorization: Bearer zkTvDUe5jJBJFcjc6ckwapEwax8Kbs7h3nv2SHXSgh5qGhHP22ggsu4fbdZgf25z" \
  -H 'accept-version: 1.0.0'
```

<!-- ```javascript
const snappr = require('snappr-api');

let api = snappr.authorize(
  'zkTvDUe5jJBJFcjc6ckwapEwax8Kbs7h3nv2SHXSgh5qGhHP22ggsu4fbdZgf25z'
);
let bookings = api.bookings.get({
  // no params
});
``` -->

> Example JSON response:

```json
{
  "results": [
    {
      "uid": "0ccefa53-b346-4d3e-8dcb-79a914289928",
      "status": "paid",
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
      "photographer_name": "Hollie B.",
      "created_at": "2018-09-01T09:12:00Z",
      "updated_at": "2018-09-01T09:12:00Z"
    },
    {
      "uid": "48b095fd-7fc0-41dc-b632-9b032e0a65e6",
      "status": "paid",
      "credits": 349,
      "latitude": 34.1513,
      "longitude": -118.2439,
      "shoottype": "family",
      "start_at": "2018-12-01T07:30:00Z",
      "duration": 180,
      "location_notes": "Meet near the park entance",
      "style_notes": "Variety of group shots of the family with different backgrounds",
      "customer_firstname": "John",
      "customer_surname": "Smith",
      "customer_email": "testing@snappr.co",
      "customer_mobilephone": "+14153338822",
      "customer_company": null,
      "photographer_name": "Jamie C.",
      "created_at": "2018-09-01T08:34:00Z",
      "updated_at": "2018-09-01T08:34:00Z"
    }
  ],
  "count": 2,
  "limit": 100,
  "offset": 0,
  "total": 2
}
```

This endpoint retrieves all bookings.

### HTTP Request

<code>GET https://api.snappr.co/bookings?limit=<span class="route_param">:limit</span>&offset=<span class="route_param">:offset</span></code>

### Query Parameters

| Parameter | Type    | Description                                                                                                                             | Required |
| --------- | ------- | --------------------------------------------------------------------------------------------------------------------------------------- | -------- |
| `limit`   | Integer | Maximum number of bookings to be returned (maximum of 100). Defaults to `100`.                                                          | No       |
| `offset`  | Integer | Offset used for pagination if there are more bookings than the limit (or more than 100 bookings if there is no limit). Defaults to `0`. | No       |

<aside class="notice">
The API does not currently support booking filters (e.g. filtering by a certain status), but this is planned for an upcoming release.
</aside>

## Get Single Booking

> Example request:

```shell
curl "https://api.snappr.co/bookings/0ccefa53-b346-4d3e-8dcb-79a914289928" \
  -H "Authorization: Bearer zkTvDUe5jJBJFcjc6ckwapEwax8Kbs7h3nv2SHXSgh5qGhHP22ggsu4fbdZgf25z" \
  -H 'accept-version: 1.0.0'
```

<!-- ```javascript
const snappr = require('snappr-api');

let api = snappr.authorize(
  'zkTvDUe5jJBJFcjc6ckwapEwax8Kbs7h3nv2SHXSgh5qGhHP22ggsu4fbdZgf25z'
);
let bookings = api.bookings.get({
  uid: '0ccefa53-b346-4d3e-8dcb-79a914289928'
});
``` -->

> Example JSON response:

```json
{
  "uid": "0ccefa53-b346-4d3e-8dcb-79a914289928",
  "status": "paid",
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
  "photographer_name": "Hollie B.",
  "created_at": "2018-09-01T09:12:00Z",
  "updated_at": "2018-09-01T09:12:00Z"
}
```

This endpoint retrieves a specific booking using the ID of the booking.

### HTTP Request

<code>GET https://api.snappr.co/bookings/<span class="route_param">:booking_uid</span></code>

### Request Parameters

| Parameter     | Type          | Description                    | Required |
| ------------- | ------------- | ------------------------------ | -------- |
| `booking_uid` | String (UUID) | The identifier of the booking. | Yes      |

# Images

## Get All Images For a Booking

> Example request:

```shell
curl "https://api.snappr.co/bookings/0ccefa53-b346-4d3e-8dcb-79a914289928/images" \
  -H "Authorization: Bearer zkTvDUe5jJBJFcjc6ckwapEwax8Kbs7h3nv2SHXSgh5qGhHP22ggsu4fbdZgf25z" \
  -H 'accept-version: 1.0.0'
```

<!-- ```javascript
const snappr = require('snappr-api');

let api = snappr.authorize(
  'zkTvDUe5jJBJFcjc6ckwapEwax8Kbs7h3nv2SHXSgh5qGhHP22ggsu4fbdZgf25z'
);
let bookings = api.bookings.getImages({
  uid: '0ccefa53-b346-4d3e-8dcb-79a914289928'
});
``` -->

> Example JSON response:

```json
{
  "results": [
    {
      "uid": "ed10cf86-97f9-4ce6-af6f-a01dfe891114",
      "file_name": "ZD 001.JPG",
      "url_original": "https://dev-media-snappr.s3.ap-southeast-2.amazonaws.com/ed10cf86-97f9-4ce6-af6f-a01dfe891114?AWSAccessKeyId=AKIAIIR7FMZ7RANC45MA&Expires=1586478927&Signature=IqGcYjJZXM7%2FSX%2BoHQk4mccB3FA%3D",
      "url_thumb": "https://dev-img.snappr.co/QlXCPwnEgV7P_RO4AJDLhOsq500=/fit-in/600x0/ed10cf86-97f9-4ce6-af6f-a01dfe891114"
    },
    {
      "uid": "ee9be5f8-84a8-4592-88a0-1781d0c39d0a",
      "file_name": "ZD 002.JPG",
      "url_original": "https://dev-media-snappr.s3.ap-southeast-2.amazonaws.com/ee9be5f8-84a8-4592-88a0-1781d0c39d0a?AWSAccessKeyId=AKIAIIR7FMZ7RANC45MA&Expires=1586478927&Signature=E%2BPTBIqQOEgf0MctPRy6WXLIsBM%3D",
      "url_thumb": "https://dev-img.snappr.co/rXtW9z6hGdDm3lebP9IPHGo9V5k=/fit-in/600x0/ee9be5f8-84a8-4592-88a0-1781d0c39d0a"
    },
    {
      "uid": "6b6eae3e-ebfb-4776-8a20-2b8087f76418",
      "file_name": "ZD 003.JPG",
      "url_original": "https://dev-media-snappr.s3.ap-southeast-2.amazonaws.com/6b6eae3e-ebfb-4776-8a20-2b8087f76418?AWSAccessKeyId=AKIAIIR7FMZ7RANC45MA&Expires=1586478927&Signature=cqgRr6oJDYYMxkGm2M34MKDnArM%3D",
      "url_thumb": "https://dev-img.snappr.co/gXk81aciTVokDvyi2NdWGNluhNg=/fit-in/600x0/6b6eae3e-ebfb-4776-8a20-2b8087f76418"
    }
  ],
  "count": 3,
  "limit": 1000,
  "offset": 0,
  "total": 3
}
```

This endpoint retrieves all the images of a specific booking.

### HTTP Request

<code>GET https://api.snappr.co/bookings/<span class="route_param">:booking_uid</span>/images</code>

### Request Parameters

| Parameter     | Type          | Description                    | Required |
| ------------- | ------------- | ------------------------------ | -------- |
| `booking_uid` | String (UUID) | The identifier of the booking. | Yes      |

### Query Parameters

| Parameter | Type    | Description                                                                                                                          | Required |
| --------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------ | -------- |
| `limit`   | Integer | Maximum number of images to be returned (maximum of 10000). Defaults to `1000`.                                                      | No       |
| `offset`  | Integer | Offset used for pagination if there are more images than the limit (or more than 1000 images if there is no limit). Defaults to `0`. | No       |

# Shoot Types

## Get All Shoot Types

> Example request:

```shell
curl "https://api.snappr.co/shoottypes" \
  -H "Authorization: Bearer zkTvDUe5jJBJFcjc6ckwapEwax8Kbs7h3nv2SHXSgh5qGhHP22ggsu4fbdZgf25z" \
  -H 'accept-version: 1.0.0'
```

<!-- ```javascript
const snappr = require('snappr-api');

let api = snappr.authorize(
  'zkTvDUe5jJBJFcjc6ckwapEwax8Kbs7h3nv2SHXSgh5qGhHP22ggsu4fbdZgf25z'
);
let bookings = api.shoottypes.get({});
``` -->

> Example JSON response:

```json
{
  "results": [
    {
      "name": "food",
      "display_name": "Food"
    },
    {
      "name": "real-estate",
      "display_name": "Real Estate"
    },
    ...
  ],
  "count": 10,
  "limit": 100,
  "offset": 0,
  "total": 10
}
```

This endpoint returns all available Snappr shoot types.

### HTTP Request

<code>GET https://api.snappr.co/shoottypes</code>

# Custom Webhooks

> Example JSON payload sent to webhook URL as POST:

```json
{
  "type": "update",
  "booking": {
    "uid": "0ccefa53-b346-4d3e-8dcb-79a914289928",
    "status": "paid",
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
    "photographer_name": "Hollie B.",
    "created_at": "2018-09-01T09:12:00Z",
    "updated_at": "2018-09-01T09:12:00Z"
  }
}
```

You can set a custom webhook URL in the Snappr Photography Portal GUI. If the custom webhook URL is set, we will POST to that URL every time a booking is created or updated.

### Setting URL

Click on the 'API' link in the top-right drop-down menu in your Photography Portal, or go to the following URL:

<code>https://app.snappr.co/partner/<span class="route_param">your-slug</span>/api</code>

Replace <code>your-slug</code> with your company's custom Snappr slug.

Go to the 'Custom Webhook' section and click the 'Edit' button in the URL field. Type in the receiving URL of your choice, then click the 'Save' button. If you entered a valid URL, that endpoint will now start receiving all booking updates.

<!-- <aside class="warning">Remember to be good to your mother.</aside> -->
