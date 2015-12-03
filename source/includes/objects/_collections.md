## Collections (``Collection``)

```json
{
	"$type": [ "Resource", "Collection" ],
	"id": "{{RESOURCE}}",
	"collection": {
		"length": 1,
		"items": [
			{ "$Resource": "{{RESOURCE}}/{{id}}" }
		]
	}
}
```

Collections are a resource which contains an ordered set of other resources.

### Fields

Name | Type | Description
---- | ---- | -----------
$type | Array of String | The type of object. Collections always include 'Collection' in this set.
id | String | The URL to this resource
collection.length | Number | The number of entities in this collection
collection.items | Array of [Resource Pointers](#resource-pointer) | The entities in the collection

### Parameters that can be used with a collection

Name | Type | Default | Description
---- | ---- | ------- | -----------
start | Number | 0 | The starting position in the collection to return items from
limit | Number | 10 | The maximum number of items in this collection to return
filter | String | ``true`` | A [filter expression](#expressions) to be applied to the collection's result.

The filter expression is an expression that, when invoked against a resource, returns a value that can be coerced to a boolean (``true`` or ``false``); only objects
in the collection for which the expression returns something that becomes ``true`` are included in the output.

Filter examples:

* ``$.data.meta.active``<br>
  In an ``AppGroupList`` or ``AppRecordList``, retrieve only the objects which are active.
* ``$.children.instructor.is(Resource("https://example.digitalxe.com/api/1/apps/swt_addressBook/records/12345"))``<br>
  In an ``AppRecordList`` from the [Course Instructor](#course-instructor) app, retrieve only those records whose
  person object is the specified one.
* ``Resource($.children.related).children.courses.has(Resource("https://example.digitalxe.com/api/1/apps/lms_courses/records/12345")) && $.data.meta.created.date() > "2015-11-01".date()``<br>
  Find all objects that have a related course with ID ``https://example.digitalxe.com/api/1/apps/lms_courses/records/12345`` and were created after November 1, 2015.
* ``($.children.member.is(Resource("https://example.digitalxe.com/api/1/apps/swt_addressBook/records/12345")) || $.children.member.is(Resource("https://example.digitalxe.com/api/1/apps/swt_addressBook/records/12346"))) && $.data.meta.created.date() > "2015-11-01".date()``<br>
  Find all objects that have a ``member`` resource that is either ``https://example.digitalxe.com/api/1/apps/swt_addressBook/records/12345`` or
  ``https://example.digitalxe.com/api/1/apps/swt_addressBook/records/12345``, and that were created after November 1, 2015.
  
### Collection Types

Name | Description | Content Type
---- | ----------- | ------------
``ChannelCollection`` | A list of channels. | [``ChannelResource``](#channel-channelresource)
``AppCollection`` | A list of apps. | [``AppResource``](#app-appresource)
``AppRecordList`` | A list of records within an app | [``AppRecord``](#record-apprecord)
``AppGroupList`` | A list of groups within an app | [``AppGroup``](#group-appgroup)
``AppRelatedList`` | A list of apps which have related records for a given record | [``AppRecordList``](#collection-types)
``RoleCollection`` | A list of the roles available in the system. | [``Role``](#role-role)
``AuthTypeCollection`` | A list of the authentication types in the system. | [``AuthType``](#authentication-type-authtype)
``PeopleRecordList`` | A list of ``PeopleRecord`` objects within an app. This is a specialization of ``AppRecordList``. | [ ``PeopleRecord``](#person-peoplerecord)
``PersonViewCollection`` | A list of equivalent ``PeopleRecord`` objects for the current object. Each object represents the same person but presents a possibly different field list and roles. | [``PeopleRecord``](#person-peoplerecord)
``PersonAuthCollection`` | A list of the authentication types available for the current user. | [``PersonAuth``](#person-authentication-personauth)
