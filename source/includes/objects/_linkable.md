## Link Target (``LinkTarget``)

```json
{
	"id": "{{RESOURCE}}",
	"$type": [ "Resource", "AppRecord", "LinkTarget" ],
	"$linkTypes": [ "AppRecord:lms_courses" ],
	"data": {},
	"children": {}
}
```

A link target is an object that can be added to collections or otherwise set as a child of an object declared as a [Link Source](#link-source-linksource).

Name | Type | Description
---- | ---- | ----
``$linkTypes`` | Array of Strings | Tags that describe where this object can be used.

## Link Source (``LinkSource``)

```
{
	"id": "{{RESOURCE}}",
	"$type": [ "Resource", "AppRecord", "LinkSource" ],
	"data": {},
	"children": {
		"links": {
			"$Resource": "{{RESOURCE}}/links"
		}
	}
}
```

A link source is an object that can have, as children, objects declared as [Link Targets](#link-target-linktarget).

### Children

Name | URL | Description | Type
--- | --- | --- | ---
links | ``{{RESOURCE}}/links`` | A set of the various properties that can be linked on this object. | [Link Resource](#link-resource-linkresource)

## Link Resource (``LinkResource``)

```
{
	"id": "{{RESOURCE}}",
	"$type": [ "Resource", "WritableResource", "LinkResource" ],
	"children": {},
	"data": {
		"{{NAME}}": {
			"type": "LinkType",
			"target": {
				"$Resource": "{{RESOURCE2}}"
			}
		}
	}
}
```

A link resource object describes the possible connections that a [Link Source](#link-source-linksource) object can make.

It's ``data`` property might contain multiple links, in which case there will be multiple ``{{NAME}}`` entries there.

Name | Type | Description
--- | --- | ---
``data.{{NAME}}.type`` | String | A link target type (a value that should appear in the ``$linkTypes`` field of a [Link Target](#link-target-linktarget).
``data.{{NAME}}.target`` | [Resource Pointer](#resource-pointer) or ``null`` | The resource currently being pointed at by this link, or ``null`` if it doesn't point at one.

### Update Links

FIXME: This is not yet implemented.

#### Request

```
PUT {{RESOURCE}} HTTP/1.1
Content-Type: application/json

{
	"{{NAME}}": {
		"target": {
			"$Resource": "{{RESOURCE2}}"
		}
	}
}
```

Name | Type | Description
--- | --- | ---
{{NAME}}.target | [Resource Pointer](#resource-pointer) or ``null`` | The resource to change the link called ``{{NAME}}`` to point at.

#### Response

NOT IMPLEMENTED
