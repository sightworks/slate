## Resource (``Resource``)

```json
{
	"$type": [ "Resource" ],
	"id": "{{RESOURCE}}",
	"data": "/* The contents of the resource goes here */",
	"children": {
		"{{name}}": {
			"$Resource": "{{RESOURCE}}/{{name}}"
		}
	}
}
```

A resource is the basic object in the API.

### Fields in a resource

Name | Type | Description
---- | ---- | -----------
$type | Array of String | The various type names associated with this resource object. All resources include 'Resource'; see the relevant sections on each resource to find out what other types might appear here.
id | String | The URL to this resource
data | Any | The contents of this resource.
children | Resource Map | Other resources available as children of this resource. The ``{{name}}`` of each item depends on the resource itself. Each resource object contains a list of it's children

The ``data`` property is not required and does not appear on certain resources (those whose ``$type`` property includes ``Collection``, primarily). The same is true of the ``children`` property. 

### Query Parameters for Resources

Name | Type | Default | Description
---- | ---- | ------- | -----------
depth | Number | 0 | How deep to traverse the resource graph to retrieve items. For a depth value greater than 0, each Resource Pointer object found in the result is inserted into the resulting data that is returned to you.

For ``depth``: Each object retrieved by this method is pulled as if it had no parameters specified for it.

### Children

Since ``Resource`` is the base type of all resources, it doesn't define any children. See the other object types for details about their child objects.
