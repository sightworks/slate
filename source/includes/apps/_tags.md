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

When constructing a tag, the portion of the URL generated for the ``record-id`` is constructed by:

* Collapsing all white space into a single space character
* Trimming leading and trailing white space
* Converting to lower case
* Removing all non-alphanumeric characters [0-9, a-z]
* Camel-casing the remaining portion.
* Truncating to 32 characters.

If a new tag is created that would have the same ``record-id`` portion as an existing tag, 
the system will append "-{number}" to the ``record-id`` portion to make it unique.

For a tag "Small Animal", the resulting URL would be:

``{{ROOT}}/apps/{{KEY}}_tags/records/smallAnimal``

For "Shelter Medicine & Surgery-General", the resulting URL is:

``{{ROOT}}/apps/{{KEY}}_tags/records/shelterMedicineSurgerygeneral``

Tag text is not considered unique in the system. (Creating a new tag with the same text as an existing tag is allowed.)

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


