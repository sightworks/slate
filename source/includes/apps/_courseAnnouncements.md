## Course Announcements

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
			"announcement": "",
			"parent_index": 0
		}
	},
	"children": {
		"parent": {
			"$Resource": "{{ROOT}}/apps/{{KEY}}_courses/records/{{record-id}}"
		},
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

An announcement on a course in the LMS.

### Location

Canonical URL:

``{{ROOT}}/apps/{{KEY}}_coursesAnnouncements/records/{{record-id}}``

### Fields

In addition to those specified by [``AppRecord``](#record-apprecord), an announcement includes:

Name | Type | Description
---- | ---- | -----------
``data.announcment`` | Text | The text of the announcement
``data.parent_index`` | Number | The order of this announcement on it's parent course

### Children

Name | URL | Description | Type
---- | ------------- | ----------- | ----
parent | ``{{ROOT}}/apps/{{KEY}}_courses/records/{{record-id}}`` | The parent course for this announcement | [``AppRecord``](#record-apprecord) of type [Course](#courses)
