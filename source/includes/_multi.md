# Miscellaneous API methods

## POST Multiple Request Endpoint

``POST {{ROOT}}/multi``

Retrieve or update a number of resources simultaneously. This method does not perform a transaction (where, if for example, a set of POST or PUT requests are included, if one were to fail, none would happen), but
rather just performs each request.

### Request Body

```json
{
	"requests": [
		{
			"method": "HTTP-Method",
			"resource": "Resource-URL",
			"query": {
				"Query": "Parameter"
			},
			"data": {
				"POST": "Data goes here"
			}
		}
	]
}
```

Property | Type | Description
-------- | ---- | -----------
``requests`` | Array | Array of the requests to be made
``requests[N].method`` | String | The request method to invoke. This should be a valid HTTP request method
``requests[N].resource`` | String | The full URI to the resource that the request is for. This should be relative to the API base URL (in this documentation, ``https://example.digitalxe.com/api/1``.)
``requests[N].query`` | String Map | The query parameters to append.
``requests[N].data`` | Object | The data that would be provided in the POST body. Since the API only accepts JSON bodies for POST data, the data should be the object without being JSON encoded itself.

### Response Body

```json
{
	"result": [
		{
			"responseCode": 200,
			"result": "Response-Body"
		}
	]
}
```

The result contains a list of encapsulated responses.

Response:

Property | Type | Description
-------- | ---- | -----------
``result`` | Array | The responses, in the same order that the requests were in the original request.
``result[N].responseCode`` | Number | The HTTP response code that would have been returned for the request.
``result[N].headers`` | String Map | A map of HTTP headers that would have been returned, if any.
``result[N].result`` | Any | The response body of the request.

