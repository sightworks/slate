## File

```json
{
	"name": "https://example.digitalxe.com/path/to/a/file",
	"type": "image/png",
	"size": 4455
}
```

An object that describes a file.

### Fields

Name | Type | Description
---- | ---- | -----------
``name`` | String | The URL to the file
``type`` | String | The MIME type of the file
``size`` | Number | The size of the file

In most cases, ``name`` will be a URL residing on your instance (beginning with 'https://<your-customer-key>.digitalxe.com/'). If the app supports it, the file specified
may instead reside in an Amazon S3 bucket, with a URL matching:

``https://s3.amazonaws.com/{{bucket}}/{{key}}`` for the region "us-east-1"
``https://s3-{{region}}.amazonaws.com/{{bucket}}/{{key}}`` for all other region values

This may be an item residing in the ``digitalxe-clients`` bucket in Amazon's "us-west-2" region, in which case, the URL is something like:

``https://s3-us-west-2.amazonaws.com/digitalxe-clients/{{customerKey}}/{{type}}/channels/{{channel}}/path/to/a/file``

You can store any S3 object here, but the platform will need access to the file in question, by one of the following methods:

* Make sure we have an access key for the specified bucket & prefix in Channel Settings.
* Place it in the appropriate location within the ``digitalxe-clients`` bucket.
* Set it's permissions in Amazon to be globally readable.

For information about how to access this information, see the "Storage" section under [Channels](#channel-channelresource).

## Media File

```json
{
	"name": "https://example.digitalxe.com/path/to/a/file",
	"type": "audio/mp3",
	"size": 4455,
	"start": {
		"unit": "second",
		"count": 10
	},
	"end": {
		"unit": "second",
		"count": 40
	}
}
```

An object that describes a media file, with possible start and end points.

### Fields

Name | Type | Description
---- | ---- | -----------
``name`` | String | The URL to the file
``type`` | String | The MIME type of the file
``size`` | Number | The size of the file
``start`` | [Interval](#interval) | The start point of the file; leaving this null means the beginning of the file
``end`` | [Interval](#interval) | The end point of the file; leaving this null means the end of the file

## Interval

```json
{
	"count": 3,
	"unit": "minute"
}
```

An object that represents a time interval.

### Fields

Name | Type | Description
---- | ---- | -----------
``unit`` | Enumeration | The unit type represented by this object. One of ``second``, ``minute``, ``day``, ``month``, ``year``.
``count`` | Number | The number of ``unit`` items specified.

``unit`` does not include entries for 'hour' or 'week': to specify 1 hour, specify 60 minutes; for 1 week, specify 7 days.

## Resource Pointer

```json
{
	"$Resource": "{{RESOURCE}}"
}
```

A resource pointer is an object that contains a resource URL. They are constructed as to be easily noticed when using a resource.

The ``$Resource`` property contains the canonical URL of the resource.

## Resource Map

```json
{
	"name": {
		"$Resource": "{{RESOURCE}}"
	}
}
```

A resource map contains a set of named objects representing children of a given resource.

With a resource map, there are 2 ways to get to the resources provided:

* Take the value of the ``$Resource`` property in the object.
* Construct a URL using the URL of the current resource and appending '/' and the key from the resource map.

```
{
	"test-item": {
		"$Resource": "{{ROOT}}/test-item"
	}
}
```

> Resource map from {{ROOT}}/test

Using the resource map to the right, the ``test-item`` child can be accessed through either:

* ``{{ROOT}}/test-item`` - from the ``$Resource`` property
* ``{{ROOT}}/test/test-item`` - as a child of the original resource

## Response

```json
{
	"resource": "{{RESOURCE}}",
	"resources": {
		"{{RESOURCE}}": {
			"id": "{{RESOURCE}}",
			"data": null,
			"children": {
				"channels": {
					"$Resource": "{{RESOURCE}}/channels"
				}
			},
			"$type": [
				"Resource",
				"Root"
			]
		}
	}
}
```

This is an object containing the results of a resource request.

### Fields

Name | Type | Description
---- | ---- | -----------
resource | String | The canonical URL of the resource you requested. (This may not be the same as the one you requested, as many resources are available through multiple paths.)
resources | Object | An object whose keys consist of the canonical resource URLs that were retrieved and whose values are the resources themselves.
