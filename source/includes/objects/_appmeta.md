## App Meta Data (``AppMetaData``)

```json
{
	"id": "{{APP}}/meta",
	"data": null,
	"children": {
		"group": {
			"$Resource": "{{APP}}/meta/group"
		},
		"record": {
			"$Resource": "{{APP}}/meta/record"
		}
	},
	"$type": [
		"Resource",
		"AppMetaData"
	]
}
```

Contains metadata about the app's records & groups, detailing the structure of a record.

### Location

``AppMetaData`` objects are available as children of an [``AppResource``](#app-appresource).

Canonical URL: ``{{ROOT}}/apps/{{app-id}}/meta``

* ``{{ROOT}}`` is the API root
* ``{{app-id}}`` is the ID of the app

### Children

Name | URL | Description | Type
---- | ------------- | ----------- | ----
group | ``{{APP}}/meta/group`` | Description of an ``AppGroup`` object within this app. | ``AppGroupMetaData``
record | ``{{APP}}/meta/record`` | Description of an ``AppRecord`` object within this app. | ``AppRecordMetaData``

<span class='warning'>These objects are not currently documented.</span>
