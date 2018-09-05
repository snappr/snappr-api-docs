---
title: Snappr API Documentation

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - ruby
  - python
  - javascript

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://app.snappr.co/book'>Book a Snappr</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Snappr API! You can use our API to access Snappr API endpoints, which can get information on bookings conducted through the Snappr marketplace network.

We have language bindings in Shell and JavaScript. You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

# Authentication

> To authorize, use this code:

```ruby
require 'snappr'

api = Snappr::APIClient.authorize!('meowmeowmeow')
```

```python
import snappr-api

api = snappr.authorize('meowmeowmeow')
```

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: meowmeowmeow"
```

```javascript
const snappr = require('snappr-api');

let api = snappr.authorize('meowmeowmeow');
```

> Make sure to replace `meowmeowmeow` with your API key.

Snappr uses API keys to allow access to the API. You can register a new Snappr API key at our [developer portal](http://api.snappr.co/register).

Snappr expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: meowmeowmeow`

<aside class="notice">
You must replace <code>meowmeowmeow</code> with your personal API key.
</aside>

# Bookings

## Get All Bookings

```ruby
require 'snappr'

api = Snappr::APIClient.authorize!('meowmeowmeow')
api.bookings.get
```

```python
import snappr

api = snappr.authorize('meowmeowmeow')
api.bookings.get()
```

```shell
curl "http://example.com/api/bookings"
  -H "Authorization: meowmeowmeow"
```

```javascript
const snappr = require('snappr');

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

```ruby
require 'snappr'

api = Snappr::APIClient.authorize!('meowmeowmeow')
api.bookings.get(2)
```

```python
import snappr

api = snappr.authorize('meowmeowmeow')
api.bookings.get(2)
```

```shell
curl "http://example.com/api/bookings/2"
  -H "Authorization: meowmeowmeow"
```

```javascript
const snappr = require('snappr');

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

```ruby
require 'snappr'

api = Snappr::APIClient.authorize!('meowmeowmeow')
api.bookings.delete(2)
```

```python
import snappr

api = snappr.authorize('meowmeowmeow')
api.bookings.delete(2)
```

```shell
curl "http://example.com/api/bookings/2"
  -X DELETE
  -H "Authorization: meowmeowmeow"
```

```javascript
const snappr = require('snappr');

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

