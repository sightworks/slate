## Person (``PeopleRecord``)

```json
{
    "id": "{{APP}}/records/{{ID}}",
    "data": {
        "meta": {
            "active": true,
            "created": "2015-11-25T00:40:14.000Z",
            "createdBy": {
                "$Resource": "{{ROOT}}/apps/swt_addressBook/records/57d1531fda6e4213b57482096efad00c"
            },
            "modified": "2015-11-25T00:40:14.000Z",
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
            "First Name": "First",
            "Last Name": "Last",
            "Nickname": "",
            "Title": "",
            "Photo": null,
            "Bio": "",
            "Prefix": "",
            "Suffix": "",
        },
        "role": {
            "source": {
                "$Resource": "{{RESOURCE}}"
            },
            "value": "Member"
        }
    },
    "children": {
        "groups": {
            "$Resource": "{{RECORD}}/groups"
        },
        "related": {
            "$Resource": "{{RECORD}}/related"
        },
        "meta": {
            "$Resource": "{{RECORD}}/meta"
        },
        "links": {
            "$Resource": "{{RECORD}}/links"
        },
        "views": {
            "$Resource": "{{RECORD}}/views"
        },
        "authTypes": {
            "$Resource": "{{RECORD}}/authTypes"
        }
    },
    "$linkTypes": [
        "AppRecord:swt_addressBook",
        "AppRecord:swt_addressBookPeople",
        "AppRecord:lms_accounts"
    ],
    "$type": [
        "Resource",
        "WritableResource",
        "RecordLikeObject",
        "LinkSource",
        "LinkTarget",
        "AppRecord",
        "PeopleRecord"
    ]
}
```

A Person record is a specialized version of a [``Record``](#record-apprecord) that has additions to handle the various account functionality within the system.

Person objects are shared objects in the system - the same person may have multiple different views of their account, accessed through the ``views`` collection.

### Location

``PeopleRecord`` objects are available as children of an [``PeopleRecordList``](#collection-types) collection.

Canonical URL:

``{{ROOT}}/apps/{{app-id}}/records/{{record-id}}``

In the above:

* ``{{ROOT}}`` is the API root.
* ``{{app-id}}`` is the app that this record is from
* ``{{record-id}}`` is the ID of the record

Examples:

``https://example.digitalxe.com/api/1/apps/swt_addressBook/records/7bb50dc958ecf596cbb43e0db584c6dc``

### Fields

Most of the fields are dependent upon the specific subtype of people that the record is for. People objects that are used to represent things like Platform accounts and LMS accounts also have:

Name | Type | Description
--- | --- | ---
``data.role`` | Object | An object representing the role. If the person object does not have a role assigned for the current app, this is ``null``.
``data.role.source`` | [Resource Pointer](#resource-pointer) | The role used for the value of the ``data.role.value`` property, from the [Role](#role-role) used.
``data.role.value`` | String | The access level granted to the selected person.

Updating the ``role`` property will assign the person to a new role or remove the user from the current role.

### Children

In addition to the children described by [``Record``](#record-apprecord), this has the following additional children:

Name | URL | Description | Type
--- | --- | --- | ---
``views`` | ``{{RECORD}}/views`` | Different views of the same person record. A person can be assigned to multiple roles and, as such, you need to be able to see how the views of data differ. | ``PersonViewCollection``
``authTypes`` | ``{{RECORD}}/authTypes`` | The authentication types associated with this user. | ``PersonAuthCollection``

## Person Authentication (``PersonAuth``)

```json
{
    "id": "{{RECORD}}/authTypes/{{authTypeId}}",
    "data": {
    },
    "children": {
        "type": {
            "$Resource": "{{ROOT}}/authTypes/{{authTypeId}}"
        }
    },
    "$type": [
        "Resource",
        "WritableResource",
        "PersonAuth"
    ]
}
```

This represents a specific type of authentication available for a [Person](#person-peoplerecord).

### Fields

The fields of the ``data`` object are described by the [Authentication Type](#authentication-type-authtype) object for the specified type.

### Children

Name | URL | Description | Type
--- | --- | --- | ---
``type`` | ``{{ROOT}}/authTypes/{{authTypeId}}`` | The authentication type that these credentials are for. | [``AuthType``](#authentication-type-authtype)
