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

## Collections (``Collection``)

```json
{
	"$type": [ "Resource", "Collection" ],
	"id": "{{RESOURCE}}",
	"collection": {
		"length": 1,
		"items": [
			{ "$Resource": "{{RESOURCE}}/{{id}}" }
		]
	}
}
```

Collections are a resource which contains an ordered set of other resources.

### Fields

Name | Type | Description
---- | ---- | -----------
$type | Array of String | The type of object. Collections always include 'Collection' in this set.
id | String | The URL to this resource
collection.length | Number | The number of entities in this collection
collection.items | Array of [Resource Pointers](#resource-pointer) | The entities in the collection

### Parameters that can be used with a collection

Name | Type | Default | Description
---- | ---- | ------- | -----------
start | Number | 0 | The starting position in the collection to return items from
limit | Number | 10 | The maximum number of items in this collection to return
filter | String | ``null`` | A filter to be applied to the collection's result. (FIXME: Document this).

### Collection Types

Name | Description | Content Type
---- | ----------- | ------------
``ChannelCollection`` | A list of channels. | [``ChannelResource``](#channel-channelresource)
``AppCollection`` | A list of apps. | [``AppResource``](#app-appresource)
``AppRecordList`` | A list of records within an app | [``AppRecord``](#record-apprecord)
``AppGroupList`` | A list of groups within an app | [``AppGroup``](#group-appgroup)
``AppRelatedList`` | A list of apps which have related records for a given record | [``AppRecordList``](#collection-types)

