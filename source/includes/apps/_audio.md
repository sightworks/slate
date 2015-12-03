## Audio Files

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
            "title": "Audio Test",
            "summary": "Summary",
            "description": "<p>Hello world</p>",
            "image": null,
            "transcript": "<P>Foo</p>",
            "file": {
                "name": "https://url.to.file/path.mp3",
                "type": "audio/mp3",
                "size": 12345,
                "start": {
                    "unit": "second",
                    "count": 5
                },
                "end": {
                    "unit": "second",
                    "count": 35
                }
            },
            "startBumperFile": null,
			"endBumperFile": null,
			"previewTranscript": "",
			"previewFile": null,
			"previewStartBumperFile": null,
			"previewEndBumperFile": null,
            "explicitContent": false
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
        "AppRecord:{{KEY}}_audio"
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

An audio file in the LMS.

### Location

Canonical URL:

``{{ROOT}}/apps/{{KEY}_audio/records/{{record-id}}``

### Fields

In addition to those specified by [``AppRecord``](#record-apprecord), an audio file includes:

Name | Type | Description
---- | ---- | -----------
``data.title`` | String | The audio file title
``data.summary`` | Text | The audio file summary (plaintext)
``data.description`` | Text | The audio file description (HTML)
``data.image`` | File | The audio file image (for list modes)
``data.file`` | [Media File](#media-file) | The audio file and start/end points
``data.transcript`` | Text | The audio file transcript (HTML)
``data.startBumperFile`` | [Media File](#media-file) | The audio file to play immediately before the primary audio file
``data.endBumperFile`` | [Media File](#media-file) | The audio file to play immediately after the primary audio file
``data.previewFile`` | [Media File](#media-file) | The audio file and start/end points, for the preview audio file
``data.previewTranscript`` | Text | The audio file transcript for the preview audio file (HTML)
``data.previewStartBumperFile`` | [Media File](#media-file) | The audio file to play immediately before the preview audio file
``data.previewEndBumperFile`` | [Media File](#media-file) | The audio file to play immediately after the preview audio file

