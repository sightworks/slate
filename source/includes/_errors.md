# OAuth API Errors

TODO

# Platform Errors
```
HTTP/1.1 500 Internal Server Error
Content-Type: application/json

{
	"type": [ "Error" ],
	"error": {
		"code": "UNKNOWN_ERROR",
		"message": "An unknown error has occurred."
	},
	"debug": {
		"Debug": "Info Here"
	}
}
```

When the API needs to report an error, you will get back an Error object as the response.

## Properties of an Error object

Name | Type | Description
--- | --- | ---
``error.code`` | String | The error code.
``error.message`` | String | A human-readable string describing the error.
``debug`` | Object | Debugging information that may be useful if the error is to be reported to us.

Errors are also reported with appropriate HTTP response codes. If using the [``multi``](#post-multiple-request-endpoint) endpoint, then the HTTP error code will be
returned as the ``responseCode`` property on the relevant portion of the response.

## Unknown Errors (``UNKNOWN_ERROR``)

This is the error that is used when no other is appropriate. It uses HTTP error code 500 (``Internal Server Error``) indicating that the problem is likely on
our side of the connection.

## Invalid Request Method (``INVALID_REQUEST_METHOD_ERROR``)

```
HTTP/1.1 405 Method Not Allowed
Allow: GET POST
Content-Type: application/json

{
	"type": [ "Error" ],
	"error": {
		"code": "INVALID_REQUEST_METHOD",
		"message": "Invalid request method PUT."
		"detail": {
			"allowedMethods": [ "GET", "POST" ]
		}
	},
	"debug": {}
}
```

You will get this object back when you attempt to perform a disallowed operation via an HTTP method - for example, trying to PUT a [Collection](#collections-collection).

Properties:

Name | Type | Description
--- | --- | ---
``error.detail.allowedMethods`` | Array of Strings | The HTTP request methods that are allowed. This mirrors the contents of the ``Allow`` header in the response.

## Invalid Request (``INVALID_REQUEST_ERROR``)

```
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
	"type": [ "Error" ],
	"error": {
		"code": "INVALID_REQUEST",
		"message": "Invalid request."
	},
	"debug": {}
}
```

This can occur at any time that the request itself doesn't make sense.

## Forbiden (``FORBIDDEN``)

```
HTTP/1.1 403 Forbidden
Content-Type: application/json

{
	"type": [ "Error" ],
	"error": {
		"code": "FORBIDDEN",
		"message": "You cannot do that."
	},
	"debug": {}
}
```

This is returned when you are not allowed to perform the specified operation.

This is currently used when attempting to delete an AppGroup object with children, or delete the top-level AppGroup object
in an app.

## Invalid Parameter Value (``INVALID_PARAMETER_VALUE``)

```
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
	"type": [ "Error" ],
	"error": {
		"code": "INVALID_PARAMETER_VALUE",
		"message": "Invalid parameter value for depth."
	},
	"debug": {}
}
```

This happens when using a query parameter whose value does not make sense for the parameter:
- Specifying a negative ``depth``
- Specifying a ``limit`` of 0 or a negative value

## Invalid Request Body (``INVALID_BODY``)

```
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
	"type": [ "Error" ],
	"error": {
		"code": "INVALID_BODY",
		"message": "Invalid body content.",
		"detail": {
			"path": "data.meta"
		}
	}
}
```

This happens when the request body is syntactically well-formed but the server refuses to handle it because it fails validation.

Name | Type | Description
--- | --- | ---
``error.detail.path`` | String | A string that represents the approximate path to the error in the request body.

## Conflict (``CONFLICT``)

```
HTTP/1.1 409 Conflict
Content-Type: application/json

{
	"type": [ "Error" ],
	"error": {
		"code": "CONFLICT",
		"message": "Update conflict. Resolve the conflict and try again.",
		"detail": {
			"conflictField": "meta.updated",
			"originalValue": "2015-11-21T12:34:45Z",
			"newValue": "2015-11-21T12:45:56Z"
		}
	}
}
```

This happens when an update has happened to an object since you last retrieved it, to prevent two users from clobbering each other's work.

Name | Type | Description
--- | --- | ---
``error.detail.conflictField`` | String | The path to the field that triggered the error.
``error.detail.originalValue`` | String | The value in the currently stored record.
``error.detail.newValue`` | String | The value in the record you are trying to store.

