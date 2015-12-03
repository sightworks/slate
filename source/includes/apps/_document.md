## Documents

```json
{
    "id": "{{RECORD}}",
    "data": {
        "meta": {
            "active": true,
            "created": "2015-12-03T23:23:50.000Z",
            "createdBy": {
                "$Resource": "{{ROOT}}/apps/swt_addressBook/records/{{person-id}}",
            },
            "modified": "2015-12-03T23:23:50.000Z",
            "modifiedBy": null,
            "publishing": {
                "active": null,
                "archive": null
            }
        },
        "data": {
            "title": "Document Test",
            "summary": "Summary",
            "description": "<p>Hello world</p>",
            "image": null,
            "file": {
                "name": "https://url.to.file/path.pdf",
                "type": "application/pdf",
                "size": 12345
            },
            "body": "",
            "previewFile": null,
            "previewBody": ""
        }
    },
    "children": {
        "groups": {
            "$Resource": "{{RECORD}}/groups",
        },
        "related": {
            "$Resource": "{{RECORD}}/related"
        },
        "meta": {
            "$Resource": "{{RECORD}}/meta"
        },
        "links": {
            "$Resource": "{{RECORD}}/links"
        }
    },
    "$linkTypes": [
        "AppRecord:{{KEY}}_documents"
    ],
    "$type": [
        "Resource",
        "WritableResource",
        "RecordLikeObject",
        "LinkSource",
        "LinkTarget",
        "AppRecord"
    ]
}
```

An document in the LMS.

### Location

Canonical URL:

``{{ROOT}}/apps/{{KEY}_documents/records/{{record-id}}``

### Fields

In addition to those specified by [``AppRecord``](#record-apprecord), a document includes:

Name | Type | Description
---- | ---- | -----------
``data.title`` | String | The document title
``data.summary`` | Text | The document summary (plaintext)
``data.description`` | Text | The document description (HTML)
``data.image`` | File | The document image (for list modes)
``data.file`` | File | A downloadable representation of the document
``data.body`` | Text | The document body, as HTML
``data.previewFile`` | File | A downloadable representation of the document preview
``data.previewBody`` | Text | The document preview, as HTML


