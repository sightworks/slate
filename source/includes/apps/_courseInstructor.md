## Course Instructor

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
			"parent_index": 0
		}
	},
	"children": {
		"instructor": {
			"$Resource": "{{ROOT}}/apps/{{KEY}}_accounts/records/{{record-id}}"
		},
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

An instructor on a course in the LMS.

### Location

Canonical URL:

``{{ROOT}}/apps/{{KEY}}_coursesInstructors/records/{{record-id}}``

### Fields

In addition to those specified by [``AppRecord``](#record-apprecord), an instructor record includes:

Name | Type | Description
---- | ---- | -----------
``data.parent_index`` | Number | The order of this instructor on it's parent course

The instructor record itself can be found in the Children section. The lead instructor is the one on this course that has the smallest value
in the ``parent_index`` field.

### Children

Name | URL | Description | Type
---- | ------------- | ----------- | ----
instructor | ``{{ROOT}}/apps/{{KEY}}_accounts/records/{{record-id}}`` | The instructor for this course | [``AppRecord``](#record-apprecord) of type [Account](#accounts)
parent | ``{{ROOT}}/apps/{{KEY}}_courses/records/{{record-id}}`` | The parent course for this instructor entry | [``AppRecord``](#record-apprecord) of type [Course](#courses)
