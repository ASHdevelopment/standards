#Status-Codes Standards

The purpose of this document is to help both sides client/server-sides of development understand what Ember accepts.

##PUT

On a PUT from any Ember-App using Ember-Data I will be happy with the following:

#On-Success:

HTTP Status: 204
Response Body: Empty(No Content)
HTTP Status: 200
Response Body: {}
HTTP Status: 200
Response Body: JSON Object

#On-Failure:

Return a Http Status Code with the Error from the server as well as the error message.

###Why?

The Ember App Expects a 204 with No Content because, is terminated by the first empty line after the header fields because it cannot contain a message body. A 200 response always has a payload, though an origin server MAY generate a payload body of zero length.

##DELETE

On a DELETE from any Ember-App using Ember-Data I will be happy with:

#On-Success:
HTTP Status: 204
Response Body: Empty(No Content)

###Why?

The Ember App Expects a 204 with No Content because, is terminated by the first empty line after the header fields because it cannot contain a message body.


# Links
- https://httpstatuses.com/
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Status
