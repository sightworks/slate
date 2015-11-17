## App (``AppResource``)

```json
{
	"id": "{{APP}}",
	"data": {
		"id": "{{APP}}",
		"title": "App Title"
	},
	"children": {
		"root": {
			"$Resource": "{{APP}}/root"
		},
		"records": {
			"$Resource": "{{APP}}/records"
		},
		"groups": {
			"$Resource": "{{APP}}/groups"
		},
		"meta": {
			"$Resource": "{{APP}}/meta"
		}
	},
	"$type": [
		"Resource",
		"AppResource"
	]
}
```

An App in the platform.

### Location

``AppResource`` objects are available as children of an [``AppCollection``](#collection-types).

Canonical URL: ``{{ROOT}}/apps/{{app-id}}``

* ``{{ROOT}}`` is the API root
* ``{{app-id}}`` is the app ID

Example URL:

``https://example.digitalxe.com/api/1/apps/swt_addressBook``

The URL to an ``AppResource`` object is also a value for the ``{{APP}}`` [token](#tokens) used elsewhere.

### Fields

Name | Type | Description
---- | ---- | -----------
``data.title`` | String | The name of the app

### Children

Name | URL | Description | Type
---- | ------------- | ----------- | ----
root | ``{{APP}}/root`` | The group in this app that holds 'ungrouped' records and top-level groups. | ``AppGroup``
records | ``{{APP}}/records`` | The collection of all records in this app. | ``AppRecordList``
groups | ``{{APP}}/groups`` | The collection of all groups in this app, excluding 'ungrouped'. | ``AppGroupList``
meta | ``{{APP}}/meta`` | The app metadata container. | ``AppMetaData``

