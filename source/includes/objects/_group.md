## Group (``AppGroup``)

```json
{
	"id": "{{GROUP}}",
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
		"data": {
			"title": "Group title",
			"summary": "Group summary",
			"description": "Group description",
			"displayOrder": 1,
			"file": {
				"name": "https://example.digitalxe.com/path/to/a/file",
				"type": "image/png",
				"size": 1345
			}
		},
		"smartGroup": null
	},
	"children": {
		"parent": {
			"$Resource": "{{APP}}/groups/{{id}}"
		},
		"groups": {
			"$Resource": "{{GROUP}}/groups"
		},
		"records": {
			"$Resource": "{{GROUP}}/records"
		}
	},
	"$type": [ "Resource", "WritableResource", "RecordLikeObject", "AppGroup" ]
}
```

A group within an app.

### Location

``AppGroup`` objects are available as both:

* Children of an [``AppGroupList``](#collection-types) collection
* The ``root`` node of an [``AppResource``](#app-appresource) object

Canonical URL:

* For the ``root`` node:
  ``{{ROOT}}/apps/{{app-id}}/root``
* For any other group:
  ``{{ROOT}}/apps/{{app-id}}/groups/{{group-id}}``

In the above:

* ``{{ROOT}}`` is the API root.
* ``{{app-id}}`` is the app that this group is from
* ``{{group-id}}`` is the ID of the group

Examples:

``https://example.digitalxe.com/api/1/apps/swt_addressBook/root``
``https://example.digitalxe.com/api/1/apps/swt_addressBook/groups/7bb50dc958ecf596cbb43e0db584c6dc``

### Fields

In addition to the fields described in [``RecordLikeObject``](#groups-and-records-recordlikeobject), an ``AppGroup`` includes the following:

Name | Type | Description
---- | ---- | -----------
``data.title`` | String | The title of the group
``data.summary`` | Text | The summary text of the group; HTML
``data.description`` | Text | The description text of the group; HTP
``data.displayOrder`` | Number | The sort order within this object's parent group
``data.file`` | File | A file attached to this group.

<span class='warning'>This can change between apps! Check to see if the app has a group description available for it before assuming this is the layout of the group.</span>

### Children

Name | URL | Description | Type
---- | ------------- | ----------- | ----
parent | ``{{GROUP}}`` | The parent group of this group. | [``AppGroup``](#group-appgroup)
groups | ``{{GROUP}}/groups`` | Groups contained within this group | [``AppGroupList``](#collection-types)
records | ``{{GROUP}}/records`` | Records contained within this group | [``AppRecordList``](#collection-types)

The group identified by ``{{APP}}/root`` does not have a child node called ``parent`` as it is the top of the tree.

<span class='info'>Apps may also define their own children here, if the group has pointers to other objects in the system.</span>

### PUT Method

The group identified by ``{{APP}}/root`` is also technically a valid target for PUT, any attempts to update it will result in a ``400 Bad Request`` error being
returned.


