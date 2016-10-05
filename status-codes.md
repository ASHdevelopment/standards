#Status-Codes Standards

The purpose of this document is to help both sides client/server-sides of development understand what Ember accepts.

##Put

On a put from any Ember-App using Ember-Data I will be happy with either:

`A - 204 with No-Content`

`B - 200 with {}`

`C - 200 with Object`

###Why?
Success:
Failure:

##Delete

On a Delete from any Ember-App using Ember-Data I will be happy with:

    `204 - with no content`

from the server.

###Why?
Success:

Ember Data expects an object to be delete on the server side, all it needs is confirmation that everything happened. Which is why you respond with a 204 - No Content

Failure:
If something goes wrong with the delete on the server-side the server needs to respond with either a different status code and the error that was associated with it. 
# Links
- https://httpstatuses.com/
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Status
