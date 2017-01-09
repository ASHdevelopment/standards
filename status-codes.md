# Status-Codes Standards

The purpose of this document is to help both sides client/server-sides of development understand what Ember accepts.

The REST adapter will consider a success any response with a status code of the 2xx family ("Success"), as well as 304 ("Not Modified").
Any other status code will be considered a failure. On success, the request promise will be resolved with the full response payload.

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

## JSON Structure

The REST adapter expects the JSON returned from your server to follow these conventions.

### Object Root

The JSON payload should be an object that contains the record inside a
root property. For example, in response to a `GET` request for
`/posts/1`, the JSON should look like this:
```js
{
  "posts": {
    "id": 1,
    "title": "I'm Running to Reform the W3C's Tag",
    "author": "Yehuda Katz"
  }
}
```
Similarly, in response to a `GET` request for `/posts`, the JSON should
look like this:
```js
{
  "posts": [
    {
      "id": 1,
      "title": "I'm Running to Reform the W3C's Tag",
      "author": "Yehuda Katz"
    },
    {
      "id": 2,
      "title": "Rails is omakase",
      "author": "D2H"
    }
  ]
}
```
Note that the object root can be pluralized for both a single-object response
and an array response: the REST adapter is not strict on this. Further, if the
HTTP server responds to a `GET` request to `/posts/1` (e.g. the response to a
`findRecord` query) with more than one object in the array, Ember Data will
only display the object with the matching ID.


# Links
- https://github.com/emberjs/data/blob/master/addon/adapters/rest.js
- https://httpstatuses.com/
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Status
