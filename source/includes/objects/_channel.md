## Channel (``ChannelResource``)

```json
{
	"id": "{{CHANNEL}}",
	"data": {
		"title": "Channel Title",
		"key": "key"
	},
	"children": {
		"apps": {
			"$Resource": "{{CHANNEL}}/apps"
		}
	},
	"$type": [
		"Resource",
		"ChannelResource"
	]
}
```

This represents a channel in the platform.

### Location

``ChannelResource`` objects are available as children of a [``ChannelCollection``](#collection-types).

Canonical URL: ``{{ROOT}}/channels/{{channel-id}}``

* ``{{ROOT}}`` is the API root
* ``{{channel-id}}`` is the channel identifier.

Example:

``https://example.digitalxe.com/api/1/channels/7bb50dc958ecf596cbb43e0db584c6dc``

The URL to a ``ChannelResource`` object is also a value for the ``{{CHANNEL}}`` [token](#tokens) used elsewhere.

### Fields

Name | Type | Description
---- | ---- | -----------
``data.title`` | String | The name of the channel
``data.key`` | String | The 'key' for this channel, used as part of all App IDs that are bound to this channel.

### Children

Name | URL | Description | Type
---- | ------------- | ----------- | ----
apps | ``{{CHANNEL}}/apps`` | The collection of apps in this channel | [``AppCollection``](#collection-types)

