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
        "SEO": {
	        "url": "",
	        "title": "",
	        "description": "",
	        "keywords": ""
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
            "explicitContent": false,
            "enableAccessRestriction": false,
            "hideWhenRestricted": false,
            "enableAccessForInstructors": false
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
``data.explicitContent`` | Boolean | If the audio file has explicit content.
``data.enableAccessRestriction`` | Boolean | Whether access restrictions are enabled on this content or not. If access restrictions are enabled, the system will either present a preview version of this object (if ``hideWhenRestricted`` is false), or hide the object entirely (if ``hideWhenRestricted`` is true).
``data.hideWhenRestricted`` | Boolean | Whether to hide this content entirely if the user doesn't have permission to see this item (whether by purchasing it, or by being a course instructor, etc.)
``data.enableAccessForInstructors`` | Boolean | Whether to grant permission to the instructors of certain courses to access this item. The list of courses is stored in the 'accessCourseInstructors' relationship.

### Relationships

Name | URL | Description | Item Type
--- | --- | --- | ---
tags | ``{{RECORD}}/related/tags`` | Tags for this audio file | [``AppRecordList``](#collection-types) with [Tag](#tags) objects.
audio | ``{{RECORD}}/related/audio`` | Related audio files | [``AppRecordList``](#collection-types) with [Audio File](#audio-files) objects.
courses | ``{{RECORD}}/related/courses`` | Related courses | [``AppRecordList``](#collection-types) with [Course](#courses) objects.
documents | ``{{RECORD}}/related/documents`` | Related documents | [``AppRecordList``](#collection-types) with [Document](#documents) objects.
videos | ``{{RECORD}}/related/videos`` | Related videos | [``AppRecordList``](#collection-types) with [Video](#videos) objects.
sponsors | ``{{RECORD}}/related/sponsors`` | Related sponsors | [``AppRecordList``](#collection-types) with [Sponsor](#sponros) objects.
accessCourseInstructors | ``{{RECORD}}/related/accessCourseInstructors`` | Courses whose instructors have access to this object | [``AppRecordList``](#collection-types) with [Course](#courses) objects.

