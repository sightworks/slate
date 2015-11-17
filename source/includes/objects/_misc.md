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
