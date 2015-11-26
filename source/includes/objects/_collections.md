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
filter | String | ``null`` | A filter to be applied to the collection's result. (FIXME: Document this).

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
