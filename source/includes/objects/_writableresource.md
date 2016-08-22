## Writable Resource (``WritableResource``)

```json
{
	"$type": [ "Resource", "WritableResource" ],
	"id": "{{RESOURCE}}",
	"data": "/* The contents of the resource goes here */",
	"children": {
		"{{name}}": {
			"$Resource": "{{RESOURCE}}/{{name}}"
		}
	}
}
```

A writable resource is a resource that can be modified through the API.

### Fields

The fields for a writable resource are the same as those for a normal resource.

### Query Parameters for Resources

The query parameters for a writable resource are the same as those for a normal resource.

### Children

Since ``Resource`` is the base type of all resources, it doesn't define any children. See the other object types for details about their child objects.

### PUT ``{{RESOURCE}}``

```
PUT {{RESOURCE}} HTTP/1.1
Content-Type: application/json

/* The full representation of the resource's "data" property here, updated as necessary */
```

Invoking this method will update the representation of the resource.

See [Errors](#errors) below for details of errors that might come back.

### POST ``{{RESOURCE}}``

```
POST {{RESOURCE}} HTTP/1.1
Content-Type: application/json

/* Only the updated fields on the resource */
```

Invoking this method will also update the representation of the resource, in the same manner as PUT.

This will merge the values in the object provided into the representation of the resource and update it accordingly. The errors here are the same as for PUT, since behind the scenes, it uses the same
mechanisms for updating the resource.
