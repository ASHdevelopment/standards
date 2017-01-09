# Status-Codes and REST Standards

The purpose of this document is to help both sides client/server-sides of development understand what Ember accepts.

Please Check the Docs for more information on the RESTAdapter
http://emberjs.com/api/data/classes/DS.RESTAdapter.html

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
