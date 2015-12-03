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

An instructor record has no fields other than those specified by [``AppRecord``](#record-apprecord).

The person that is the instructor is listed as a child of this object (in the ``instructor`` field). The "lead instructor" on any course is the first entry in
the course's "courseInstructors" child collection.

If you need to create this object, specify an empty object for ``data``.

### Children

Name | URL | Description | Type
---- | ------------- | ----------- | ----
instructor | ``{{ROOT}}/apps/{{KEY}}_accounts/records/{{record-id}}`` | The instructor for this course | [``AppRecord``](#record-apprecord) of type [Account](#accounts)
parent | ``{{ROOT}}/apps/{{KEY}}_courses/records/{{record-id}}`` | The parent course for this instructor entry | [``AppRecord``](#record-apprecord) of type [Course](#courses)
