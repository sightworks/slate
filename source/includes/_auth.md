# Authentication

## Get API credentials

Your Sightworks support representative will provide you with the required unique API credentials. If you have not yet received your credentials, lost your credentials or feel that they might have been compromised, please contact your SightWorks support representative at (503) 223-4184 Mon. - Fri. 9:00 AM to 5:00 PM Pacific Time or <a href="mailto:support@sightworks.com">support@sightworks.com</a>. 

## Get a Bearer token

### Request

```
POST /oauth2/token
Authorization: Basic a2V5OnRva2Vu
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials
```

Given your set of API credentials, make a request to /oauth2/token on your platform's instance. 

- Authorization: Basic *token* 
  The HTTP Auth header. The username and password for this are the API credentials you got above.
- The content body: ``grant_type=client_credentials``
  This specifies the type of token to retrieve. The access tokens provided here are application access tokens - all changes
  will show that they were done by the application, not by any specific user.

### Response

```
HTTP/1.1 200 OK
Content-Type: application/json

{
	"token_type": "bearer",
	"access_token": "eyJ0b2tlbiI6ImtleSIsImtleSI6InRva2VuIn0="
}
```

This JSON response body indicates:

- The token is a 'Bearer' token (``"token_type": "bearer"``)
- The included access token to use.

In the future, this API may return other token types (possibly an OAuth token for the app using the 'OAuth' mechanism for HTTP Auth).

## Use the Bearer token in a request

```
GET {{ROOT}}/channels HTTP/1.1
Authorization: Bearer eyJ0b2tlbiI6ImtleSIsImtleSI6InRva2VuIn0=

HTTP/1.1 200 OK
Content-Type: application/json

{
    "resource": "{{ROOT}}/channels",
    "resources": {
        "{{ROOT}}/channels": {
            "id": "{{ROOT}}/channels", 
            "collection": {
                "length": 1,
                "items": [
                    {
                        "$Resource": "{{ROOT}}/channels/44bc0861e2ad8f8b158a2842c7207009"
                    }
                ]
            },
            "$type": [
                "Resource",
                "Collection",
                "ChannelCollection"
            ]
        }
    },
    "debug": {}
}
```

The JSON example code to the right provides a basic example of using the token.

## Alternative: Use the Bearer token in the query string

```
GET {{ROOT}}/channels?access_token=eyJ0b2tlbiI6ImtleSIsImtleSI6InRva2VuIn0%3D HTTP/1.1
```

Use the ``access_token`` query string parameter on any request to pass in the access token.

If this method is used on a request to [``.../multi``](#post-multiple-request-endpoint), the ``access_token`` parameter must be passed in both
the query string and ``requests[N].query.access_token``.

## Invalidate the Bearer token

```
POST /oauth2/invalidate_token
Authorization: Basic a2V5OnRva2Vu
Content-Type: application/x-www-form-urlencoded

access_token=eyJ0b2tlbiI6ImtleSIsImtleSI6InRva2VuIn0%3D

HTTP/1.1 200 OK
Content-Type: application/json

{
	"access_token": "eyJ0b2tlbiI6ImtleSIsImtleSI6InRva2VuIn0="
}
```

<span class='warning'>This actually doesn't do anything currently except return success with the passed access token.</span>

This API will invalidate the provided access token, making it no longer usable.


