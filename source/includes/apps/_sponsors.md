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
courses | ``{{RECORD}}/related/courses`` | The courses that this sponsor is attached to. | [``AppRecordList``](#collection-types) with [Course](#courses) objects

