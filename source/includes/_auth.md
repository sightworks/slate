# Authentication

## Get API credentials

Your Sightworks support representative will provide you with unique API credentials.

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
GET {{ROOT}}/channels
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

This is just an example of using the token.

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


