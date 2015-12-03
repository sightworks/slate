## Videos

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
            "title": "Video Test",
            "summary": "Summary",
            "description": "<p>Hello world</p>",
            "image": null,
            "transcript": "<P>Foo</p>",
            "file": {
                "name": "https://url.to.file/path.mov",
                "type": "video/quicktime",
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
        "AppRecord:{{KEY}}_video"
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

A video file in the LMS.

### Location

Canonical URL:

``{{ROOT}}/apps/{{KEY}_videos/records/{{record-id}}``

### Fields

In addition to those specified by [``AppRecord``](#record-apprecord), a video includes:

Name | Type | Description
---- | ---- | -----------
``data.title`` | String | The video title
``data.summary`` | Text | The video summary (plaintext)
``data.description`` | Text | The video description (HTML)
``data.image`` | File | The video image (for list modes)
``data.file`` | [Media File](#media-file) | The video's file and start/end points
``data.transcript`` | Text | The video transcript (HTML)
``data.startBumperFile`` | [Media File](#media-file) | The video to play immediately before the primary video
``data.endBumperFile`` | [Media File](#media-file) | The video to play immediately after the primary video
``data.previewFile`` | [Media File](#media-file) | The video's file and start/end points, for the preview video
``data.previewTranscript`` | Text | The video transcript for the preview video (HTML)
``data.previewStartBumperFile`` | [Media File](#media-file) | The video to play immediately before the preview video
``data.previewEndBumperFile`` | [Media File](#media-file) | The video to play immediately after the preview video

