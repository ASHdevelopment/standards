# ASH API Guidelines

ASH adheres to REST standards and uses Ember's [RESTAdapter](http://emberjs.com/api/data/classes/DS.RESTAdapter.html). The following is a combination of REST guidelines and Ember guidelines to help facilitate API development at ASH. Much of this was adopted from Ember Data's API documentation, so for more reading, check the [Ember Data documentation](http://emberjs.com/api/data/).

**You will need to ensure that you follow the url structure, object structure, and status code. If not, the team will need to make sure adapters and serializers are set up to compensate for this in Ember.**

## URL Names
URLs should be the same for `GET`, `POST`, `PUT`, and `DELETE`. For instance, `api/users` not `api/getUsers` or `api/deleteUser`. In the case where the client is requesting or modifying an existing record, the id should be passed after  `users`. For instance: `api/users/324` should be able to accept `GET`, `DELETE`, or `PUT` requests.

## Id's
In general, each record needs to have an id. So the API should supply one, even if it's not the real ID that is stored in the database.

## GET

### GET all records
#### Request
URL
:   `apiHost.com/movies`  
Ember Data Method
:   `findAll('movies')`

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

### GET a single record
#### Request
URL
:   `apiHost.com/movies/2`
Ember Data Method
:   `findRecord('movies', 2)`

#### Response

HTTP Status
:   200

Payload
:   
```javascript
{
  "movies": {
    "id": 2,
    "title": "Goodfellas",
    "year": "1990"
  }
}
```

### GET multiple records using a query
**Need documentation**. In the mean time [Finding Records](https://guides.emberjs.com/v2.10.0/models/finding-records/)

## PUT

> `PUT` requests update records that already exist with new or updated information

### Request
URL
:   `apiHost.com/movies/2`

Ember Data Method
:   
```javascript
//lookup record in the local store
let movie = store.findRecord('movie', 2); // returns record of {"id": 2, "title": "Goodfellas", "year": "1990"}
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
  "movies": {
    "id": 2,
    "title": "Goodfellers", //title has been updated
    "year": "1990",
    "radioheadOnSoundtrack": false //property has been added
  }
}
```

The api can also return a `204` with an empty payload, but **this is not preferred**. It's preferred to use a `200` so the API can compute or serialize any data and send back to the front end.

## DELETE

On a DELETE from any Ember-App using Ember-Data I will be happy with:

### On-Success:
* HTTP Status: 204
  + Response Body: Empty(No Content)

### Why?

>The Ember App Expects a 204 with No Content because, is terminated by the first empty line after the header fields because it cannot contain a message body.

## On-Failure

For any error, the server should respond with the correct status code as well as a message in the response body.



### Errors

If a response is considered a failure, the JSON payload is expected to include
a top-level key `errors`, detailing any specific issues. For example:

```js
{
  "errors": {
    "msg": "Something went wrong"
  }
}
```

# Links
- https://github.com/emberjs/data/blob/master/addon/adapters/rest.js
- https://httpstatuses.com/
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Status
