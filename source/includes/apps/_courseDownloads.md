## Course Downloads

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
			"title": "An awesome download",
			"file": {
				"name": "https://example.digitalxe.com/awesome.pdf",
				"type": "application/pdf",
				"size": 34322
			}
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

An download on a course in the LMS.

### Location

Canonical URL:

``{{ROOT}}/apps/{{KEY}}_coursesDownloads/records/{{record-id}}``

### Fields

In addition to those specified by [``AppRecord``](#record-apprecord), a download includes:

Name | Type | Description
---- | ---- | -----------
``data.title`` | String | The title of the download
``data.file`` | File | The file to download

### Children

Name | URL | Description | Type
---- | ------------- | ----------- | ----
parent | ``{{ROOT}}/apps/{{KEY}}_courses/records/{{record-id}}`` | The parent course for this download | [``AppRecord``](#record-apprecord) of type [Course](#courses)
