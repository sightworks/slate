## Groups and Records (``RecordLikeObject``)

```json
{
	"id": "{{RESOURCE}}",
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
		"SEO": {
			"url": "Fragment",
			"title": "Title",
			"keywords": "Keywords",
			"description": "Description"
		},
		"data": {}
	},
	"children": {},
	"$type": [ "Resource", "WritableResource", "RecordLikeObject" ]
}
```

This structure is used for any object that behaves similarly to a record.

### Location

``RecordLikeObject`` objects are not directly found. Their subtypes, [``AppRecord``](#record-apprecord) and [``AppGroup``](#group-appgroup),
are found in various places; see their documentation for details.

### Fields

Name | Type | Description
---- | ---- | -----------
``data.meta.active`` | Boolean | Whether the record is active or archived.
``data.meta.created`` | Timestamp | The time the record was created.
``data.meta.createdBy`` | [Resource Pointer](#resource-pointer) to a [Person](#person-record) [record](#record-apprecord). | The person who created this record.
``data.meta.modified`` | Timestamp | The time the record was last modified.
``data.meta.modifiedBy`` | [Resource Pointer](#resource-pointer) to a [Person](#person-record) [record](#record-apprecord). | The person who last modified this record.
``data.meta.publishing.active`` | Timestamp | The timestamp that the ``data.meta.active`` flag should be toggled to ``true``.
``data.meta.publishing.archive`` | Timestamp | The timestamp that the ``data.meta.active`` flag should be toggled to ``false``.
``data.SEO.url`` | String | If the record provides SEO data, the fragment used on the website for accessing this object.
``data.SEO.title`` | String | The title to use on pages that represent this record in the HTML &lt;title&gt; tag.
``data.SEO.keywords`` | Text | The keywords to use in the HTML &lt;meta name="keywords"&gt; tag.
``data.SEO.description`` | Text | The keywords to use in the HTML &lt;meta name="description"&gt; tag.

### Children

The children of a record-like object are determined dynamically by the type of record being displayed. 

See the Children sections in [``AppRecord``](#record-apprecord) and [``AppGroup``](#group-appgroup) for others that are always here.

### PUT Method

When updating these resources, you are required to specify the full body of the resource (as returned in the resource's ``data`` property). If you don't, then
you will get back an error response of ``400 Bad Request`` with a code ``INVALID_BODY``.

In the ``meta`` property, ``created``, ``createdBy``, ``modified``, and ``modifiedBy`` are used when updating the record. If they do not have the same values as the
system currently recognizes, you will get a ``409 Conflict`` error, with a code ``CONFLICT`` and ``error.detail.conflictField`` indicating the field which has a different
value.


