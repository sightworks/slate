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
			"tag": "Tag"
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

### Relationships

Name | URL | Description | Item Type
---- | --- | ----------- | ---------
courses | ``{{RECORD}}/related/courses`` | The courses that this tag is attached to. | [``AppRecordList``](#collection-types) with [Course](#courses) objects


