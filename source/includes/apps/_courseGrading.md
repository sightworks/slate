## Course Grading

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
			"name": "A+",
			"minimum": 97,
			"maximum": 100,
			"thisIsAPassingGrade": true,
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

A grade on a course in the LMS.

### Location

Canonical URL:

``{{ROOT}}/apps/{{KEY}}_coursesGrading/records/{{record-id}}``

### Fields

In addition to those specified by [``AppRecord``](#record-apprecord), a grade includes:

Name | Type | Description
---- | ---- | -----------
``data.name`` | String | The name of this grade
``data.minimum`` | Number | The minimum score (percentage) to have earned this grade
``data.maximum`` | Number | The maximum score (percentage) to have earned this grade
``data.thisIsAPassingGrade`` | Boolean | Whether this grade constitutes a "pass" or not
``data.parent_index`` | Number | The order of this grade on it's parent course

The grading scale for a course should not contain overlapping entries, and the grading scale will be extended to fill the entire span
between 0 and 100%.

* At the top end of the grading scale: the highest entry will have it's range extended to 100%.
* At the bottom end of the grading scale: If the lowest entry is a passing grade, then an entry for the bottom portion of the scale will be
  treated as existing with ``name`` being 'Fail', ``minimum`` being 0, ``maximum`` being 1 smaller than the lowest grade given, and ``thisIsAPassingGrade``
  being considered ``false``; if the lowest entry is a failing grade, then it's ``minimum`` value will be treated as 0.  

Scores are inclusive (the example entry covers all scores from 97% to 100%); all scores are rounded to the nearest
percentage value.

### Children

Name | URL | Description | Type
---- | ------------- | ----------- | ----
parent | ``{{ROOT}}/apps/{{KEY}}_courses/records/{{record-id}}`` | The parent course for this grade | [``AppRecord``](#record-apprecord) of type [Course](#courses)
