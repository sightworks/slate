## Writable Collection (``WritableCollection``)

```json
{
	"$type": [ "Resource", "Collection", "WritableCollection" ],
	"id": "{{RESOURCE}}",
	"collection": {
		"length": 1,
		"items": [
			{ "$Resource": "{{RESOURCE}}/{{id}}" }
		]
	}
}
```

A collection that can have new objects added to it. There are no particular differences in the appearance of this collection to a base [Collection](#collections-collection).

### Add object to the collection

#### Request
```
POST {{RESOURCE}} HTTP/1.1
Content-Type: application/json

{
	"object": {}
}
```

Posting an object will result in a new object being created and added to the collection. The contents of the ``object`` property should be the
of the same structure as the ``data`` property on a resource of the same type, and will be validated according to the same rules. For more details,
see the sections on the relevant object types.

#### Response

```
HTTP/1.1 201 Created
Location: {{RESOURCE}}

{
	"created": "{{RESOURCE}}",
	"collection": {{RESOURCE2}}",
	"resources": {
		"{{RESOURCE}}": {
			/* The resource you created */
		},
		"{{RESOURCE2}}": {
			/* The collection object with the new item in it */
		}
	}
}
```

The response body for an object creation request will be a JSON object with the following properties:

Name | Type | Description
---- | ---- | -----------
``created`` | Resource ID | The URL to the resource that was created. This will be the same value as is in the Location header on the response.
``collection`` | Resource ID | The URL of the collection that you requested the resource to be added to.
``resources.{{RESOURCE}}`` | [Resource](#resource-resource) | The resource that was created.
``resources.{{RESOURCE2}}`` | [Collection](#collections-collection) | The collection that you requested the resource be added to.

#### Errors

- [INVALID_BODY](#invalid-body) when:
  - No properties exist in the outermost object.
  - Unexpected properties exist in the outermost object. (The only property that should exist to add an object is ``object``.)
  - The object being created is not well formed. (See the validation rules on the relevant object types for details.)
- [UNKNOWN_ERROR](#unknown-error) when:
  - Another error is encountered on the server.

## Changeable Collection (``ChangeableCollection``)

```json
{
	"$type": [ "Resource", "Collection", "WritableCollection", "ChangeableCollection" ],
	"id": "{{RESOURCE}}",
	"collection": {
		"length": 1,
		"types": [ "Type:subtype", ... ],
		"items": [
			{ "$Resource": "{{RESOURCE}}/{{id}}" }
		]
	}
}
```

This is much the same as a [Writable Collection](#writable-collection-writablecollection), with the addition of ``collection.types``, 
which describes objects that can be [linked](#link-target-linktarget) here.

### Change the list of objects in the collection

#### Request

```
POST {{RESOURCE}} HTTP/1.1
Content-Type: application/json

{
	"append": [
		{
			"$Resource": "{{RESOURCE2}}"
		}
	],
	"remove": [
		{
			"$Resource": "{{RESOURCE3}}"
		}
	}
}
```

Fields in the request:

Name | Type | Description
---- | ---- | -----------
append | Array of [Resource Pointer](#resource-pointer) | A list of resources to add to this collection
remove | Array of [Resource Pointer](#resource-pointer) | A list of resources to remove from this collection

For the various fields:

- ``append``:<br>
  The object list to append must be all objects that do not exist as part of this collection.<br>
  The objects must have a ``type`` of [``LinkTarget``](#link-target-linktarget).<br>
  The objects must have one of the types in the ``types`` property on the collection in it's ``$linkTypes`` property.<br>
  Each object in this list will be added to the collection.
- ``remove``:<br>
  The object list to remove must be all objects that are in this collection, and not provided in the same update by ``append``.<br>
  Each object in this list will be removed from the collection.

One or both of ``append`` and ``remove`` can be provided.

#### Response

On success, the response is the same as retrieving the collection, with all of the changes specified applied.

#### Errors

- [INVALID_BODY](#invalid-body) when:
  - No properties exist in the outermost object.
  - Unexpected properties exist in the outermost object. (Only 'append' and 'remove' should appear here.)
  - If ``append`` or ``remove`` is not an array, does not contain [Resource Pointer](#resource-pointer) objects, or those resource pointers do not resolve to [Resource](#resources-resource) objects.
  - If both ``append`` and ``remove`` are specified, if any items in ``append`` also appear in ``remove``.
  - If any of the objects being added using ``append`` are already in this collection.
  - If any of the objects being added using ``append`` are not of a suitable type.
  - If any of the objects being removed using ``remove`` are not in this collection.
- [UNKNOWN_ERROR](#unknown-error) when:
  - Another error is encountered on the server.


## Sortable Collection (``SortableCollection``)

```json
{
	"$type": [ "Resource", "Collection", "WritableCollection", "ChangeableCollection", "SortableCollection" ],
	"id": "{{RESOURCE}}",
	"collection": {
		"length": 1,
		"types": [ "Type:subtype", ... ],
		"items": [
			{ "$Resource": "{{RESOURCE}}/{{id}}" }
		]
	}
}
```

A sortable collection appears the same as a [Changeable Collection](#changeable-collection-changeablecollection).

### Change the position of an object in the collection

#### Request

```
POST {{RESOURCE}} HTTP/1.1
Content-Type: application/json

{
	"position": {
		"node": {
			"$Resource": "{{RESOURCE2}}"
		},
		"at": "before",
		"ref": {
			"$Resource": "{{RESOURCE3}}"
		}
	}
}
```

Name | Type | Description
---- | ---- | -----------
position.node | [Resource Pointer](#resource-pointer) | The item in the collection being moved.
position.at | Enumeration | Where to move the item to in the collection. One of ``before``, ``after``, ``start``, ``end``.
position.ref | [Resource Pointer](#resource-pointer) | When ``position.at`` is ``before`` or ``after``, the item that is to be the next or previous item in the collection.

#### Response

On success, the response is the same as retrieving the collection, with the node specified by ``position.node`` in it's new location.

#### Errors

- [INVALID_BODY](#invalid-body) when:
  - No properties exist in the outermost object.
  - Unexpected properties exist in the outermost object. (Only ``position`` should appear here.)
  - ``position.node`` is not a valid resource pointer.
  - ``position.ref`` is not a valid resource pointer, if ``position.at`` is ``before`` or ``after``.
  - ``position.at`` is not one of ``before``, ``after``, ``start``, ``end``.
  - The resources specified by ``position.node`` or ``position.ref`` (if specified) are not part of this collection.
- [UNKNOWN_ERROR](#unknown-error) when:
  - Another error is encountered on the server.

### Add an object at a specified location within the collection

#### Request

```
POST {{RESOURCE}} HTTP/1.1
Content-Type: application/json

{
	"object": {},
	"position": {
		"at": "before",
		"ref": {
			"$Resource": "{{RESOURCE2}}"
		}
	}
}
```

Name | Type | Description
---- | ---- | -----------
object | Object | The description of the resource to add.
position.at | Enumeration | Where to move the item to in the collection. One of ``before``, ``after``, ``start``, ``end``.
position.ref | [Resource Pointer](#resource-pointer) | When ``position.at`` is ``before`` or ``after``, the item that is to be the next or previous item in the collection.

This is equivalent to adding an object to the collection (as in [Writable Collection](#writable-collection-writablecollection)), and then positioning it where you want it, except that if positioning fails, the record is not created.

#### Response

The response body is the same for adding an object to a collection by [Writable Collection](#writable-collection-writablecollection)'s method to add an object.

#### Errors

- [INVALID_BODY](#invalid-body) when:
  - No properties exist in the outermost object.
  - Unexpected properties exist in the outermost object. (Only ``position`` and ``object`` should appear here.)
  - ``position.node`` is specified.
  - ``position.ref`` is not a valid resource pointer, if ``position.at`` is ``before`` or ``after``.
  - ``position.at`` is not one of ``before``, ``after``, ``start``, ``end``.
  - The resource specified by``position.ref`` is not part of this collection.
  - The object being created is not well formed. (See the validation rules on the relevant object types for details.)
- [UNKNOWN_ERROR](#unknown-error) when:
  - Another error is encountered on the server.

### Directly order the collection

#### Request

```
POST {{RESOURCE}} HTTP/1.1
Content-Type: application/json

{
	"order": [
		{ "$Resource": "{{RESOURCE1}}" }
	],
	"append": [
		{ "$Resource": "{{RESOURCE2}}" }
	],
	"remove": [
		{ "$Resource": "{{RESOURCE2}}" }
	]
}
```

Set the order of the items in this collection, potentially adding and removing items at the same time.

Name | Type | Description
---- | ---- | ----
order |	Array of [Resource Pointers](#resource-pointer) | The new order to be applied to the collection.
append | Optional array of [Resource Pointers](#resource-pointer) | Items to add to the collection
remove | Optional array of [Resource Pointers](#resource-pointer) | Items to remove from the collection

The items in ``remove`` must be items currently in the collection; those in ``append`` must be items not in the collection.

The items in ``order`` must be all of the items that will be in the final collection after processing ``append`` and ``remove``.

#### Response

The response is the same as that from [Writable Collection](#writable-collection-writablecollection)'s method to add and remove objects from a collection.

#### Errors

- [INVALID_BODY](#invalid-body) when:
  - Unexpected properties exist in the outermost object. (Only 'order', append' and 'remove' should appear here.)
  - For ``order``, ``append`` and ``remove``: if they are not an array, do not contain [Resource Pointer](#resource-pointer) objects, or those resource pointers do not resolve to [Resource](#resources-resource) objects.
  - If both ``append`` and ``remove`` are specified, if any items in ``append`` also appear in ``remove``.
  - If any of the objects being added using ``append`` are already in this collection.
  - If any of the objects being added using ``append`` are not of a suitable type.
  - If any of the objects being removed using ``remove`` are not in this collection.
  - If ``order`` contains items that are not in the collection after ``append`` and ``remove`` have been processed.
- [UNKNOWN_ERROR](#unknown-error) when:
  - Another error is encountered on the server.
