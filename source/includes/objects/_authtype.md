## Authentication Type (``AuthType``)

```json
{
	"id": "{{ROOT}}/authTypes/com.sightworks.platform.authentication.PasswordAuth",
	"data": {
		"title": "Password",
		"info": {
			"operations": {
				"create": true,
				"setToken": true,
				"resetToken": true,
				"changeInformation": true,
				"remove": true
			},
			"fields": {
				"username": {
					"type": "email",
					"name": "Email Address"
				}
			}
		}
	},
	"children": {
	},
	"$type": [
		"Resource",
		"AuthType"
	]
}
```

Authentication types tell the system how people might authenticate to the system.

### Location

Authentication type objects exist as:

``{{ROOT}}/authTypes/{{authTypeId}}``

### Fields

Name | Type | Description
--- | --- | ---
``data.title`` | String | The name of the authentication type.
``data.info.operations.create`` | Boolean | Whether people can be assigned this authentication type, not having had it previously.
``data.info.operations.setToken`` | Boolean | Whether a token (password, private key, etc.) can be set for this authentication type explicitly.
``data.info.operations.resetToken`` | Boolean | Whether a request can be made to generate a new authentication token for this authentication type.
``data.info.operations.changeInformation`` | Boolean | Whether the data associated with this authentication type can be updated after it has been created.
``data.info.operations.remove`` | Boolean | Whether this authentication type can be removed.
``data.fields.{{name}}.type`` | Enumeration | The type of field represented by ``{{name}}`` in an authentication data object. One of ``string``, ``email``.
``data.fields.{{name}}.name`` | String | The user-friendly title of this field.

The various flags under ``data.info.operations`` control operations on a [``PersonAuth``](#person-authentication-personauth) or [``PersonAuthCollection``](#person-authentication-list-personauthcollection) regarding this authentication type:

- ``create``: Allows the ``PUT`` method to be invoked at ``{{RECORD}}/authTypes/{{authTypeId}}`` when a ``GET`` request would otherwise return an HTTP 404 Not Found error.
- ``setToken``: Allows the ``POST`` method to be invoked with a body containing a new token at ``{{RECORD}}/authTypes/{{authTypeId}}``.
- ``resetToken``: Allows the ``POST`` method to be invoked with a body requesting a new token at ``{{RECORD}}/authTypes/{{authTypeId}}``.
- ``changeInformation``: Allows the ``PUT`` method to be invoked at ``{{RECORD}}/authTypes/{{authTypeId}}`` when a ``GET`` request would return success.
- ``remove``: Allows the ``DELETE`` method to be invoked at ``{{RECORD}}/authTypes/{{authTypeId}}``.

In all cases above, ``{{RECORD}} is a [``PeopleRecord``](#person-peoplerecord) object in an app that has the ``role`` endpoint exposed.

The fields under ``data.fields`` are the readable contents of the authentication info. If the ``type`` property of one of the fields is ``email``, then the content passed in needs to be a well-formed email address, otherwise it can be any string. No property can be specified as ``null``.
