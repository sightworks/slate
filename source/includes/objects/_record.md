## Record (``AppRecord``)

```json
{
	"id": "{{RECORD}}",
	"data": {
		"meta": {
			"active": true,
			"created": "2015-11-16T08:45:21.000Z",
			"createdBy": {
				"$Resource": "{{ROOT}}/apps/swt_addressBook/records/{{person-id}}"
			},
			"modified": "2015-11-16T08:45:21.000Z",
			"modifiedBy": {
				"$Resource": "{{ROOT}}/apps/swt_addressBook/records/{{person-id}}"
			},
			"publishing": {
				"active": "2015-11-16T07:00:00.000Z",
				"archive": null
			}
		},
		"SEO": {
			"url": "Fragment",
			"title": "Title",
			"keywords": "Keywords",
			"description": "Description"
		},
		"data": {},
	},
	"children": {
		"groups": {
			"$Resource": "{{RECORD}}/groups"
		},
		"related": {
			"$Resource": "{{RECORD}}/related"
		},
		"meta": {
			"$Resource": "{{RECORD}}/meta"
		}
	},
	"$type": [ "Resource", "WritableResource", "RecordLikeObject", "AppRecord" ]
}
```

A record within an app.

### Location

``AppRecord`` objects are available as children of an [``AppRecordList``](#collection-types) collection.

Canonical URL:

``{{ROOT}}/apps/{{app-id}}/records/{{record-id}}``

In the above:

* ``{{ROOT}}`` is the API root.
* ``{{app-id}}`` is the app that this record is from
* ``{{record-id}}`` is the ID of the record

Examples:

``https://example.digitalxe.com/api/1/apps/swt_addressBook/records/7bb50dc958ecf596cbb43e0db584c6dc``

### Fields

The fields in an ``AppRecord`` are dependent on the app they came from; they all include the fields described in [``RecordLikeObject``](#groups-and-records-recordlikeobject). Various apps within the system have their own documentation below.

### Children

Name | URL | Description | Type
---- | ------------- | ----------- | ----
groups | ``{{RECORD}}/groups`` | Groups that this record is in | [``AppGroupList``](#collection-types)
related | ``{{RECORD}}/related`` | Apps which have related records for this record | [``AppRelatedList``](#collection-types)
meta | ``{{RECORD}}/meta`` | Metadata about the record's structure | [``AppRecordMetaData``](#app-record-meta-data-apprecordmetadata)

The metadata provided as a child of a record will not necessarily be equivalent to the metadata provided for a record at ``{{ROOT}}/apps/{{app-id}}/meta/record`` - 
if the record structure changes based on a field that can only be set at creation, the metadata provided here will be for the final form of the object as it exists here. 
The metadata provided at ``{{ROOT}}/apps/{{app-id}}/meta/record`` will still confirm that the record structure matches what is defined, but attempting to validate it 
will allow you to change those fields that cannot be changed; in the metadata here, those fields will be provided as an enumeration of length 1 (meaning the only legal 
value will be the one in the field when it is retrieved).

<span class='info'>Apps may also define their own children here, if the record has pointers to other objects in the system.</span>

### Relationships

In any specific record type, record lists accessible beneath ``related`` will be noted in a Relationships section.

