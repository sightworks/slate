## Sponsors

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
			"title": "Awesome Sponsor",
			"logo": {
				"file": "https://example.digitalxe.com/sponsors/awesome-logo.gif",
				"size": 1234,
				"type": "image/gif"
			},
			"link": "http://awesome.example.com/",
			"openInNewWindow": true
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

A sponsor in the LMS.

### Location

Canonical URL:

``{{ROOT}}/apps/{{KEY}}_sponsors/records/{{record-id}}``

### Fields

In addition to those specified by [``AppRecord``](#record-apprecord), a sponsor includes:

Name | Type | Description
---- | ---- | -----------
``data.title`` | String | The name of the sponsor
``data.logo`` | File | The sponsor's logo
``data.link`` | String | The URL of the sponsor's website (where the link should go when this sponsor is displayed)
``data.openInNewWindow`` | Boolean | Whether to open the link in the same window or a new window

### Relationships

Name | URL | Description | Item Type
---- | --- | ----------- | ---------
audio | ``{{RECORD}}/related/audio`` | Related audio files | [``AppRecordList``](#collection-types) with [Audio File](#audio-files) objects.
courses | ``{{RECORD}}/related/courses`` | Related courses | [``AppRecordList``](#collection-types) with [Course](#courses) objects.
documents | ``{{RECORD}}/related/documents`` | Related documents | [``AppRecordList``](#collection-types) with [Document](#documents) objects.
videos | ``{{RECORD}}/related/videos`` | Related videos | [``AppRecordList``](#collection-types) with [Video](#videos) objects.

