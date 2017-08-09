# ASH API Guidelines

## Table of Contents
1. [GET](#get)
    - [Multiple Records](#get-multiple)
    - [Single Record](#get-single)
    - [Querying Multiple Records](#queryMultiple)
    - [Querying a Single Record](#querySingle)
1. [POST](#post)
1. [PUT](#put)
1. [DELETE](#delete)
1. [Dates](#date)
1. [Error Formatting](#errors)

ASH adheres to REST standards and uses Ember's [RESTAdapter](http://emberjs.com/api/data/classes/DS.RESTAdapter.html). The following is a combination of REST guidelines and Ember guidelines to help facilitate API development at ASH. Much of this was adopted from Ember Data's API documentation, so for more reading, check the [Ember Data documentation](http://emberjs.com/api/data/).

**You will need to ensure that you follow the url structure, object structure, and status code. If not, the team will need to make sure adapters and serializers are set up to compensate for this in Ember.**

## URL Names
URLs should be the same for `GET`, `POST`, `PUT`, and `DELETE`. For instance, `api/users` not `api/getUsers` or `api/deleteUser`. In the case where the client is requesting or modifying an existing record, the id should be passed after  `users`. For instance: `api/users/324` should be able to accept `GET`, `DELETE`, or `PUT` requests.

## Id's
In general, each record needs to have an id. So the API should supply one, even if it's not the real ID that is stored in the database.

<a name="get"></a>
## GET

<a name="get-multiple"></a>
### [1.1](#get-multiple): GET all records
#### Request
URL
:   `apiHost.com/movies`

Request Method
: `GET`

Ember Data Method
:   `findAll('movie')`

#### Response

HTTP Status
:   200

Payload
:
```javascript
{
  "movies": [
    {
      "id": 1,
      "title": "Raging Bull",
      "year": "1980"
    },
    {
      "id": 2,
      "title": "Goodfellas",
      "year": "1990"
    }
  ]
}
```

Payload (If no data is found, then an empty array is returned)
:
```javascript
{
  "movies": []
}
```

<a name="get-single"></a>
### [1.2](#get-single): GET a single record
#### Request
URL
:   `apiHost.com/movies/2`

Request Method
: `GET`

Ember Data Method
:   `findRecord('movie', 2)`

#### Response

HTTP Status
:   200

Payload
:
```javascript
{
  "movie": {
    "id": 2,
    "title": "Goodfellas",
    "year": "1990"
  }
}
```

#### Response when no data is found

HTTP Status
:   404

Payload
:

Content should be an error and may differ, as error style is defined by the server. 

<a name="queryMultiple"></a>
### [1.3](#queryMultiple): GET multiple records using a query

### Why?
> To get multiple records based on parameter criteria.

#### Request
URL
:   `apiHost.com/movies?year=1990`

Request Method
: `GET`

Ember Data method
:   `query('movie', { year: '1990' })`

### Response

HTTP Status
:   200

Payload
:   
```javascript
{
  "movies": [{
    "id": 2,
    "title": "Goodfellas",
    "year": "1990"
  },
  {
    "id": 5,
    "title": "Red Riding Hood",
    "year": "1990"
  }]
}
```

Payload (If no data is found, then an empty array is returned)
:
```javascript
{
  "movies": []
}
```

<a name="querySingle"></a>
### [1.4](#querySingle): GET a single record using a query

### Why?
> To get a single record based on parameter criteria when the result is known to be one record.

#### Request
URL
:   `apiHost.com/movies?title=Goodfellas`

Request Method
: `GET`

Ember Data method
:   `queryRecord('movie', { title: 'Goodfellas' })`

### Response

HTTP Status
:   200

Payload
:   
```javascript
{
  "movie": {
    "id": 2,
    "title": "Goodfellas",
    "year": "1990"
  }
}
```

Payload (If no data is found, then an empty array is returned)

:
```javascript
{
  "movie": {}
}
```

<a name="post"></a>
## POST

### Creating Records
#### Request
URL
:   `apiHost.com/movies`

Request Method
: `POST`

Ember Data Method
:  

```javascript
//create movie3 record in local store
let movie3 = get(this, 'store').createRecord('movie', {
    title: "Crimson Tide",
    year: "1995"
});

//persist movie3 via POST request to apiHost.com/movies
movie3.save();
```

#### Response

HTTP Status
:   201

Payload
:   
```javascript
{
  "movie": {
    "id": 3,
    "title": "Crimson Tide",
    "year": "1995"
  }
}
```

## PUT

> `PUT` requests update records that already exist with new or updated information

### Request
URL
:   `apiHost.com/movies/2`


Request Method
: `PUT`

Ember Data Method
:   
```javascript
//lookup record in the local store
let movie = get(this, 'store').findRecord('movie', 2); // returns record of {"id": 2, "title": "Goodfellas", "year": "1990"}
movie.set('title', 'Goodfellers'); //update an existing property
movie.set('radioheadOnSoundtrack', false); //add a new property
// set method only updates the record in the local store without making a network request yet.
movie.save(); //save() initiates a PUT request to apiHost.com/movies/2
```

#### Response
HTTP Status
:   200

Payload
:   
```javascript
{
  "movie": {
    "id": 2,
    "title": "Goodfellers", //title has been updated
    "year": "1990",
    "radioheadOnSoundtrack": false //property has been added
  }
}
```
While you can add a new property in the `PUT` request, it's not good practice, since your app should be working off a schema rather than arbitrarily adding properties.

The api can also return a `204` with an empty payload, but **this is not preferred**. It's preferred to use a `200` so the API can compute or serialize any data and send back to the front end.

<a name="delete"></a>
## DELETE

### Deleting a Record
#### Request
URL
:   `apiHost.com/movies/2`  


Request Method
: `DELETE`

Ember Data Methods:

*Examples are assuming you have already set movie to a record in your store using findRecord('movie', 2) or a similar method*

**Deletes Only (you must save to persist)**  
```javscript
movie.deleteRecord(); //Deletes it from the local store, but no network request to the API yet
movie.save() //DELETE network request to apiHost.com/movies/2
```

**Deletes and Persists**   
```javscript
movie.destroyRecord(); //Deletes it from the local store and sends a DELETE network request to apiHost.com/movies/2
```

#### Response

HTTP Status
:   204

Payload
:   Empty (No Content)

### Why?

> The Ember App Expects a 204 with No Content because, is terminated by the first empty line after the header fields because it cannot contain a message body.

<a name="date"></a>
## Dates

### DateTime Options
DateTime properties should use the ISO 8601 format below.

```javascript
//No matter where the user is, they will see the time as 3:26pm
//local to their timezone
var local ='2017-05-10T15:26';

//The Z at the end means that this is 3:26 UTC time. Depending
//on how this date is implimented client-side, the user will
//see their local conversion. For instance, 11:26am EST during
//daylight saving time or 10:26 after daylight saving time ends.
var utc = '2017-05-10T15:26Z';

//In this case, the datetime is 3:26 EST, so it will convert
//to 12:26 PST if the user is in San Diego.
var offset =  '2017-05-10T15:26-0400'

//In this case, the datetime is 2:26 EST because daylight saving
//time has ended (the month was changed to December), so it will
//convert to 11:26 PST if the user is in San Diego.
var offsetWithoutDaylightSaving =  '2017-12-10T15:26-0400'
```

### Daylight Saving Time
It's important to remember that UTC is different in the USA any given date, depending on if we are in the middle of daylight saving or not.

### Dates Only (without time)
If the API passes a date without a time, it will be converted to that date at midnight UTC.

```javascript
var newYearsDay = '2017-01-01';
new Date(newYearsDay); //Sat Dec 31 2016 16:00:00 GMT-0800 (Pacific Standard Time)
```

Because PST is 8 hours behind UTC, the date will display 12/31/2016.

<a name="errors"></a>
## On-Failure

For any error, the server should respond with the correct status code as well as a message in the response body.

### Errors

If a response is considered a failure, the JSON payload is expected to include
a top-level key `errors`, detailing any specific issues. If the JSON payload does not
include a top level `errors` key, then we will need to munge the data.

```js

// Somewhere in Ember-land, attempting to save a model.

// Omitted for brevity...

actions: {
  save(model) {
    model.save().then(() => {
      // For successful save
    }).catch((error) => {
      // For unsuccessful save

      // Model had server errors for attributes: `firstName` and `email`
    });
  }
}

```

```js

// Ideal JSON error payload

{
  "errors": [
    {
      "detail": "First name is required",
      "source": {
        "pointer": "data/attributes/first-name"
      }
    },
    {
      "detail": "Email is required",
      "source": {
        "pointer": "data/attributes/email"
      }
    }
  ]
}

```

Thanks to Ember Data, we have access to our errors via our `model`.

```js

// Somewhere in Ember-land...

// Omitted for brevity...

Ember.get(this, 'model.errors.first-name')
// returns a `first-name` error object!
// => { "attribute": "firstName", "message": "First name is required" }

Ember.get(this, 'model.errors.email')
// returns an `email` error object!
// => { "attribute": "email", "message": "Email is required" }

```

Or, you can render them in a template!

```hbs

{{!-- Somewhere in Handlebars --}}

{{#each model.errors as |error|}}
  {{#if error.firstName}}
    <div class="error">
      {{error.firstName.message}}
    </div>
  {{/if}}

  {{#if error.lastName}}
    <div class="error">
      {{error.lastName.message}}
    </div>
  {{/if}}
{{/each}}

```
