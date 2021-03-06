## Course Prerequisites

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

An prerequisite list on a course in the LMS.

### Location

Canonical URL:

``{{ROOT}}/apps/{{KEY}}_coursesPrerequisites/records/{{record-id}}``

### Fields

A prerequisite record has no fields other than those specified by [``AppRecord``](#record-apprecord).

The actual prerequisite courses are stored in the ``courses`` relationship on this record.

To create a new prerequisite list, specify an empty object for the ``data`` property.

### Children

Name | URL | Description | Type
---- | ------------- | ----------- | ----
parent | ``{{ROOT}}/apps/{{KEY}}_courses/records/{{record-id}}`` | The parent course for this prerequisite entry | [``AppRecord``](#record-apprecord) of type [Course](#courses)

### Relationships

Name | URL | Description | Item Type
---- | --- | ----------- | ---------
courses | ``{{RECORD}}/related/courses`` | The list of prerequisite courses | [``AppRecordList``](#collection-types) with [Course](#courses) objects

