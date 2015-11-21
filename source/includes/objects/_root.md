## Root (``Root``)

```json
{
	"id": "{{ROOT}}",
	"data": null,
	"children": {
		"channels": {
			"$Resource": "{{ROOT}}/channels"
		},
		"apps": {
			"$Resource": "{{ROOT}}/apps"
		},
		"multi": {
			"$Resource": "{{ROOT}}/multi"
		}
	},
	"$type": [
		"Resource",
		"Root"
	]
}
```

The API root object. This is the top-level of the API namespace.

### Location

This is the "Root" object in the API.

Canonical URL: ``{{ROOT}}``

* ``{{ROOT}}`` is the API root.

### Children

Name | URL | Description | Type
---- | ------------- | ----------- | ----
channels | ``{{ROOT}}/channels`` | The collection of channels on the platform instance | [``ChannelCollection``](#collection-types)
apps | ``{{ROOT}}/apps`` | All of the apps available on the platform instance | [``AppCollection``](#collection-types)
multi | ``{{ROOT}}/multi`` | Make multiple API requests at the same time. | [``Resource``](#resource)

The ``multi`` child does not really represent a child resource, but rather a callable method. See [POST Multiple Request Endpoint](#post-multiple-request-endpoint) for details.
