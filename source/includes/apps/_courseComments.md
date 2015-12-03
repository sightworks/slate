## Course Comments

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
			"time": "2015-11-16T08:45:21.000Z",
			"comment": "This is awesome!"
		}
	},
	"children": {
		"parent": {
			"$Resource": "{{ROOT}}/apps/{{KEY}}_courses/records/{{record-id}}"
		},
		"author": {
			"$Resource": "{{ROOT}}/apps/{{KEY}}_accounts/records/{{record-id}}"
		},
		"inReplyTo": {
			"$Resource": "{{ROOT}}/apps/{{KEY}}_comments/records/{{record-id}}"
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

An comment on a course in the LMS.

### Location

Canonical URL:

``{{ROOT}}/apps/{{KEY}}_coursesComments/records/{{record-id}}``

### Fields

In addition to those specified by [``AppRecord``](#record-apprecord), a comment includes:

Name | Type | Description
---- | ---- | -----------
``data.time`` | Timestamp | The time of the comment
``data.comment`` | Text | The text of the comment

### Children

Name | URL | Description | Type
---- | ------------- | ----------- | ----
parent | ``{{ROOT}}/apps/{{KEY}}_courses/records/{{record-id}}`` | The parent course for this comment | [``AppRecord``](#record-apprecord) of type [Course](#courses)
author | ``{{ROOT}}/apps/{{KEY}}_accounts/records/{{record-id}}`` | The author for this comment | [``AppRecord``](#record-apprecord) of type [Account](#accounts)
inReplyTo | ``{{ROOT}}/apps/{{KEY}}_coursesComments/records/{{record-id}}`` | The comment that this is in reply to | [``AppRecord``](#record-apprecord) of type [Course Comment](#course-comments)

