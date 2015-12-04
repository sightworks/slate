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

## App Group Meta Data (``AppGroupMetaData``)

```json
{
	"id": "{{{APP}}/meta/group",
	"data": {
		"$schema": "http://json-schema.org/draft-04/schema#"
	},
	"$type": [
		"Resource",
		"MetaDataObject",
		"AppGroupMetaData"
	]
}
```

This describes the structure of an [``AppGroup``](#group-appgroup) object.

The data (in the ``data`` property) is an object in [JSON Schema](http://tools.ietf.org/html/draft-zyp-json-schema-04) format.

### Location

``AppGroupMetaData`` objects are available as children of an [``AppMetaData``](#app-meta-data-appmetadata) object; they are also available
as children of an [``AppGroup``](#group-appgroup) object.

Canonical URL: ``{{ROOT}}/apps/{{app-id}}/meta/group``

* ``{{ROOT}}`` is the API root
* ``{{app-id}}`` is the ID of the app.

## App Record Meta Data (``AppRecordMetaData``)

```json
{
	"id": "{{{APP}}/meta/record",
	"data": {
		"$schema": "http://json-schema.org/draft-04/schema#"
	},
	"$type": [
		"Resource",
		"MetaDataObject",
		"AppRecordMetaData"
	]
}
```

This describes the structure of an [``AppRecord``](#group-appgroup) object.

The data (in the ``data`` property) is an object in [JSON Schema](http://tools.ietf.org/html/draft-zyp-json-schema-04) format.

### Location

``AppRecordMetaData`` objects are available as children of an [``AppMetaData``](#app-meta-data-appmetadata) object; they are also available
as children of an [``AppRecord``](#group-apprecord) object.

Canonical URL: ``{{ROOT}}/apps/{{app-id}}/meta/record``

* ``{{ROOT}}`` is the API root
* ``{{app-id}}`` is the ID of the app.

