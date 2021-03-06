#HTTP API - Attachment Operations

##PUT <a id="put"></a>

Perform a PUT request to /static/{attachment key} to create the specified attachment at the given URL:

    > curl -X PUT http://localhost:8080/static/users/ayende.jpg -d "[.. binary image data ...]"

For a successful request, RavenDB will respond with the id it generated and an HTTP 201 Created response code:

    HTTP/1.1 201 Created
    Location: /static/users/ayende.jpg

It is important to note that a PUT in RavenDB will always create the specified attachment at the request URL, if necessary overwriting what was there before.

While putting an attachment, it is possible to store metadata about it using HTTP Headers. The following standard HTTP headers will be stored and sent back when the attachment is next retrieved from Raven.

* Allow
* Content-Disposition
* Content-Encoding
* Content-Language
* Content-Location
* Content-MD5
* Content-Range
* Content-Type
* Expires
* Last-Modified

In addition to that, any custom HTTP header will also be stored and sent back to the client on GET requests for the attachment.

##GET <a id="get"></a>
Raven supports the concept of attachments. Attachments are binary data that are stored in the database and can be retrieved by a key.
Retrieving an attachment is done by performing an HTTP GET on the following URL:

    > curl -X GET http://localhost:8080/static/{attachment key}

For example, the following request:

    > curl -X GET http://localhost:8080/static/users/ayende.jpg

Will retrieve an attachment whose key is "users/ayende.jpg", the response to the request is the exact byte stream that was stored in a previous [PUT](http://ravendb.net/docs/http-api/attachments/http-api-put-attachments) request.

##DELETE <a id="delete"></a>

Perform a DELETE request to delete the attachment specified by the URL:

    > curl -X DELETE http://localhost:8080/static/users/ayende.jpg

For a successful delete, RavenDB will respond with an HTTP response code 204 No Content:

    "HTTP/1.1 204 No Content"

The only way a delete can fail is if [the etag doesn't match](http://ravendb.net/docs/http-api/http-api-comcurrency), even if the attachment doesn't exist, a delete will still respond with a successful status code.