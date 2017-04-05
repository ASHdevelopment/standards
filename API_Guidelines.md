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

### GET a single record
#### Request
URL
:   `apiHost.com/movies/2`

Ember Data Method
:   `findRecord('movie', 2)`

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

## POST

### Creating Records
#### Request
URL
:   `apiHost.com/movies`  

Ember Data Method
:  

```javascript
//create movie3 record in local store
let movie3 = get(this, 'store').createRecord('movie', {
    title: "Crimson Tide",
    year: "1995"
});

//persist movie3 via POST request to apiHost.com/movies/3
movie3.save();
```

#### Response

HTTP Status
:   201

Payload
:   
```javascript
{
  "movies": {
    "id": 3,
    "title": "Crimson Tide",
    "year": "1995"
  }
}
```


## PUT

On a PUT from any Ember-App using Ember-Data I will be happy with the following:

### On-Success:

* HTTP Status: 204
  + Response Body: Empty(No Content)
* HTTP Status: 200
  + Response Body: {}
* HTTP Status: 200
  + Response Body: JSON Object

### Why?

> The Ember App Expects a 204 with No Content because, is terminated by the first empty line after the header fields because it cannot contain a message body. A 200 response always has a payload, though an origin server MAY generate a payload body of zero length.

## DELETE

### Deleting a Record
#### Request
URL
:   `apiHost.com/movies/2`  

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

# Links
- https://github.com/emberjs/data/blob/master/addon/adapters/rest.js
- https://httpstatuses.com/
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Status
