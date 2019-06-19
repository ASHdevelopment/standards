# ASH API Guidelines

## Table of Contents
1. **[Introduction](#introduction)**
1. **[URL Names](#url-names)**
1. **[ID's](#ids)**
1. **[GET](#get)** 
	[ [Multiple Records](#41-get-all-records) ]
	[ [Single Record](#42-get-a-single-record) ]
	[ [Querying Multiple Records](#43-get-multiple-records-using-a-query)  ]
	[ [Querying a Single Record](#44-get-a-single-record-using-a-query) ]
	[ [Querying Using an Array](#45-querying-using-an-array) ]
1. **[POST](#post)**
1. **[PUT](#put)**
1. **[DELETE](#delete)**
1. **[Model Relationships](#model-relationships)**
	[ [Side-loaded Without Query](#81-side-loaded-without-query) ]
	[ [Embedded Data Without Query](#82-embedded-data-without-query) ]
	[ [Embedded Data With Query](#83-embedded-data-with-query) ]
	[ [Async Loading](#84-async-loading) ]
1. **[Dates](#dates)**
1. **[On Failure](#on-failure)**

## Introduction
ASH adheres to REST standards and uses Ember's [RESTAdapter](http://emberjs.com/api/data/classes/DS.RESTAdapter.html). The following is a combination of REST and Ember-specific guidelines to help facilitate API development at ASH. Adhering to these guidelines will allow for the simplest and most painless use of the Ember Data library. Much of this was adopted from Ember Data's API documentation, so for more reading, check the [Ember Data documentation](http://emberjs.com/api/data/).However, Ember Data is not the only reason behind this structure, it helps to create a consistent API architecture making it easier to plug into other platforms and frameworks (e.g., backend, native apps, etc.).

**You will need to ensure that you follow the url structure, object structure, and status code. If not, the team will need to make sure adapters and serializers are set up to compensate for this in Ember.**

## URL Names
URLs should be the same for `GET`, `POST`, `PUT`, and `DELETE` - the verb should not be reflected in the endpoint URL. In the case where the client is requesting or modifying an existing record, the id should be passed after  `users`. For instance: `api/users/324` should be able to accept `GET`, `DELETE`, or `PUT` requests.

The endpoints themselves should be lowerCamelCased.

**Bad**
```sh
/api/getUsers	#GET should be the verb, not part of the endpoint
/api/deleteUser	#DELETE should be the verb
/api/User	#shouldn't be capitalized
```

**Good**
```sh
/api/users
/api/clientRecords
```

## Id's
In general, each record needs to have an id. So the API should supply one, even if it's not the real ID that is stored in the database.

## GET

### 4.1 GET all records
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

### 4.2 GET a single record
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

### 4.3 GET multiple records using a query

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

### 4.4 GET a single record using a query

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

## 4.5 Querying Using an Array

Arrays can be passed to some ember-data methods such as `query()`. By default the querystring will be serialized like so:

```javascript
store.query('person', { ids: [1, 2, 3] });

// => GET "/api/v1/person?ids%5B%5D=1&ids%5B%5D=2&ids%5B%5D=3"
// Decodes to:
// => GET "/api/v1/person?ids[]=1&ids[]=2&ids[]=3"
```

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

Payload
:
```javascript
{
  "movie": {
    "title": "Crimson Tide",
    "year": "1995"
  }
}
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

Payload
:
```javascript
{
  "title": "Goodfellers",
  "year": "1990",
  "radioheadOnSoundtrack": false
}
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

## DELETE

### Deleting a Record
#### Request
URL
: `apiHost.com/movies/2`  


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

## Model Relationships

### 8.1 Side-loaded Without Query

### Why?
> Use this method when you can safely assume that you generally want the list of actors when the `movies` endpoint is accessed

URL: `api.com/movies`

Payload:
```javascript
{
    "movies": [
        {
            "id": 1,
            "title": "Raging Bull",
            "year": "1980",
            "actors": [1,2,3]
        },
        {
            "id": 2,
            "title": "Goodfellas",
            "year": "1990",
            "actors": [1,2,4]
        },
        {
            "id": 4,
            "title": "Cape Fear",
            "year": "1991",
            "actors": [1,5,6]
        }
    ],
    "actors":[
        {
            "id":1,
            "name": "Robert De Niro"
        },{
            "id":2,
            "name": "Joe Pesci"
        },{
            "id":3,
            "name": "Cathy Moriarty"
        },{
            "id":4,
            "name": "Ray Liotta"
        },{
            "id":5,
            "name": "Nick Nolte"
        },{
            "id":6,
            "name": "Illeana Douaglas"
        }
    ]
}
```

Alternatively, you can `GET` `api.com/movies/1` and the api will only return Raging Bull and its actors. It will not return all actors for all movies.

## 8.2 Embedded Data Without Query

### Why?
> Use this method when you can safely assume that you generally want the list of actors when the `movies` endpoint is accessed.  If you have a lot of shared actors, this may result in a significantly larger payload.

Notice that the payload is larger here because shared actors (De Niro and Pesci) are repeated, whereas they are not in the side-loaded example.

URL: `api.com/movies`

Payload:
```javascript
{
    "movies": [
        {
            "id": 1,
            "title": "Raging Bull",
            "year": "1980",
            "actors":[
                {
                    "id":1,
                    "name": "Robert De Niro"
                },{
                    "id":2,
                    "name": "Joe Pesci"
                },{
                    "id":3,
                    "name": "Cathy Moriarty"
                }
            ]
        },
        {
            "id": 2,
            "title": "Goodfellas",
            "year": "1990",
            "actors":[
                {
                    "id":1,
                    "name": "Robert De Niro"
                },{
                    "id":2,
                    "name": "Joe Pesci"
                },{
                    "id":4,
                    "name": "Ray Liotta"
                }
            ]
        },
        {
            "id": 4,
            "title": "Cape Fear",
            "year": "1991",
            "actors":[
                {
                    "id":1,
                    "name": "Robert De Niro"
                },{
                    "id":5,
                    "name": "Nick Nolte"
                },{
                    "id":6,
                    "name": "Illeana Douaglas"
                }
            ]
        }
    ]
}
```

Alternatively, you can `GET` `api.com/movies/1` and the api will only return Raging Bull and its actors.

## 8.3 Embedded Data With Query

### Why?
> Use this method when you want the ability to toggle including actors when the `movies` endpoint is accessed

URL: `api.com/movies/?include=actors`

Payload:
```javascript
{
    "movies": [
        {
            "id": 1,
            "title": "Raging Bull",
            "year": "1980",
            "actors":[
                {
                    "id":1,
                    "name": "Robert De Niro"
                },{
                    "id":2,
                    "name": "Joe Pesci"
                },{
                    "id":3,
                    "name": "Cathy Moriarty"
                }
            ]
        },
        {
            "id": 2,
            "title": "Goodfellas",
            "year": "1990",
            "actors":[
                {
                    "id":1,
                    "name": "Robert De Niro"
                },{
                    "id":2,
                    "name": "Joe Pesci"
                },{
                    "id":4,
                    "name": "Ray Li otta"
                }
            ]
        },
        {
            "id": 4,
            "title": "Cape Fear",
            "year": "1991",
            "actors":[
                {
                    "id":1,
                    "name": "Robert De Niro"
                },{
                    "id":5,
                    "name": "Nick Nolte"
                },{
                    "id":6,
                    "name": "Illeana Douaglas"
                }
            ]
        }
    ]
}
```

Alternatively, you can `GET` `api.com/movies/1/?include=actors` and the api will only return Raging Bull and its actors.

## 8.4 Async Loading
URL: `api.com/movies`

Payload:
```javascript
{
    "movies": [
        {
            "id": 1,
            "title": "Raging Bull",
            "year": "1980",
            "actors":[1, 2, 3]
        },
        {
            "id": 2,
            "title": "Goodfellas",
            "year": "1990",
            "actors":[1, 2, 4]
        },
        {
            "id": 4,
            "title": "Cape Fear",
            "year": "1991",
            "actors":[1, 5, 6]
        }
    ]
}
```

The JavaScript can then make individual requests to `api.com/actors/1`, `api.com/actors/2`, `api.com/actors/3`, etc. because those id's were referenced in the original payload.

## Dates

### DateTime Options
DateTime properties should use the ISO 8601 format including timezones, as shown below:

```javascript
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


## On-Failure

For any error, the server should respond with the correct status code as well as a message in the response body. There are 2 methods available for formatting the error response:

### Standard HTTP Response

When only a single error message or result is needed, feel free to use a normal HTTP response:

```sh
GET /api/members
[500] A member with this fitnessId already exists
```

Ember will automatically parse this response for you:

```javascript
	model.save()
		.catch(err => {
			console.log(err.errors[0].status) //'500'
			console.log(err.errors[0].detail) //'A member with this fitnessId already exists'
		})
```

### ServiceStack Error Response

If you may need to return multiple messages, such as when validating a form, you can use the ServiceStack standard error response. Note that this is not directly parseable by Ember - you will need to add adapter code to serialize it.

```javascript
GET /api/members
[500]
"responseStatus": {
    "errorCode": "ArgumentException",
    "message": "Invalid Request",
    "stackTrace": "[Omitted for this example]",
    "errors": [
        {
            "errorCode": "InValid",
            "fieldName": "CurrentUsername",
            "message": "Oops! Username is incorrect."
        },
        {
            "errorCode": "InValid",
            "fieldName": "CurrentPassword",
            "message": "Oops! Password is incorrect."
        }
    ],
    "meta": null
}
```
The ServiceStack response needs to be serialized into the standard Ember error response:
```javascript
{
    "errors": [
        {
            "detail": "Oops! Username is incorrect.",
            "source": {
                "pointer": "CurrentUsername"
            }
        },
        {
            "detail": "Oops! Password is incorrect.",
            "source": {
                "pointer": "CurrentPassword"
            }
        }
    ]
}
```

```javascript
// Somewhere in Ember-land, attempting to save a model.

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

Thanks to Ember Data, we have access to our errors via our `model`.

```javascript
Ember.get(this, 'model.errors.CurrentUsername')
// => { "attribute": "CurrentUsername", "message": "Oops! Username is incorrect." }

Ember.get(this, 'model.errors.CurrentPassword')
// => { "attribute": "CurrentPassword", "message": "Oops! Password is incorrect." }
```

Or, you can render them in a template!

```hbs
{{#each model.errors as |error|}}
    {{#if error.CurrentUsername}}
        <div class="error">
            {{error.CurrentUsername.message}}
        </div>
    {{/if}}

    {{#if error.CurrentPassword}}
        <div class="error">
            {{error.CurrentPassword.message}}
        </div>
    {{/if}}
{{/each}}
```
