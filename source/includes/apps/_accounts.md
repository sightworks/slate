## Accounts

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
		"SEO": {
			"url": "Person-Name",
			"title": "Person Name",
			"description": null,
			"keywords": null
		},
		"data": {
			"First Name": "Person",
			"Last Name": "Name",
			"{{KEY}}_instructor": false,
			"Prefix": "",
			"Suffix": "",
			"Nickname": "",
			"Title": "",
			"Photo": "",
			"Bio": "",
			"{{KEY}}_notificationMethod": "Email",
			"{{KET}}_EmailAddress": "person.name@example.com"
		}
	},
	"children": {
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

An account in the LMS.

### Location

Canonical URL:

``{{ROOT}}/apps/{{KEY}}_accounts/records/{{record-id}}``

### Fields

In addition to those specified by [``AppRecord``](#record-apprecord), an account includes:

Name | Type | Description
---- | ---- | -----------
``data["First Name"]`` | String | The first name of the person
``data["Last Name"]`` | String | The last name of the person
``data.{{KEY}}_instructor`` | Boolean | Whether the person is an instructor. (This only makes them appear in our backend for instructor selection)
``data.Prefix`` | String | The prefix for the person's name ('Mr.', 'Ms.', etc)
``data.Suffix`` | String | A suffix for the person's name ('Jr.', 'Sr.', etc)
``data.Nickname`` | String | A nickname for the person (displayed in preference to First and Last Name)
``data.Title`` | String | The person's business title ('Director', 'CEO', etc)
``data.Photo`` | File | The person's photo
``data.Bio`` | Text | The person's bio (HTML)
``data.{{KEY}}_notificationMethod`` | Enumeration | How the user wants to get notifications ('None', 'Email')
``data.{{KEY}}_EmailAddress`` | String | The email address of the user if they aren't a local user in our system. Required if notificationMethod is 'Email' and they aren't a local user.
    
### Relationships

Name | URL | Description | Item Type
---- | --- | ----------- | ---------
courses | ``{{RECORD}}/related/courses`` | Courses that this user is considered to have purchased, for the purposes of the "thisCourseRequiresOffsitePurchase" flag on a Course. | [``AppRecordList``](#collection-types) with [Course](#courses) objects
products | ``{{RECORD}}/related/products`` | Products that this user is considered to have purchased. | [``AppRecordList``](#collection-types) with [Product](#products) objects.
tags | ``{{RECORD}}/related/tags`` | Tags associated with this user | [``AppRecordList``](#collection-types) with [Tag](#tags) objects.
bookmarkCourses | ``{{RECORD}}/related/bookmarkCourses`` | Courses that this user has bookmarked / favorited | [``AppRecordList``](#collection-types) with [Course](#courses) objects.
bookmarkAudio | ``{{RECORD}}/related/bookmarkAudio`` | Audio files that this user has bookmarked / favorited | [``AppRecordList``](#collection-types) with [Audio](#audio) objects.
bookmarkVideo | ``{{RECORD}}/related/bookmarkVideo`` | Videos that this user has bookmarked / favorited | [``AppRecordList``](#collection-types) with [Video](#video) objects.
bookmarkDocuments | ``{{RECORD}}/related/bookmarkDocuments`` | Documents that this user has bookmarked / favorited | [``AppRecordList``](#collection-types) with [Document](#documents) objects.
