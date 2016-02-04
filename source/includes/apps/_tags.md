## Tags

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
		"data": {
			"tag": "Tag",
			"hiddenTag": false
		}
	},
	"children": {
		"groups": {
			"$Resource": "{{RECORD}}/groups"
		},
		"related": {
			"$Resource": "{{RECORD}}/related"
		}
	},
	"$type": [ "Resource", "RecordLikeObject", "AppRecord" ]
}
```

A tag in the LMS.

### Location

Canonical URL:

``{{ROOT}}/apps/{{KEY}}_tags/records/{{record-id}}``

### Fields

In addition to those specified by [``AppRecord``](#record-apprecord), a tag includes:

Name | Type | Description
---- | ---- | -----------
``data.tag`` | String | The label associated with the tag.
``data.hiddenTag`` | Boolean | Whether this tag is exposed to the public or not.

### Relationships

Name | URL | Description | Item Type
---- | --- | ----------- | ---------
accounts | ``{{RECORD}}/related/accounts`` | The people that this tag is attached to. | [``PeopleRecordList``](#collection-types) with [Account](#accounts) objects
audio | ``{{RECORD}}/related/audio`` | The audio files that this tag is attached to. | [``AppRecordList``](#collection-types) with [Audio File](#audio) objects
courses | ``{{RECORD}}/related/courses`` | The courses that this tag is attached to. | [``AppRecordList``](#collection-types) with [Course](#courses) objects
documents | ``{{RECORD}}/related/documents`` | The documents that this tag is attached to. | [``AppRecordList``](#collection-types) with [Document](#documents) objects
videos | ``{{RECORD}}/related/videos`` | The videos that this tag is attached to. | [``AppRecordList``](#collection-types) with [Video](#videos) objects


