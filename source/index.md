---
title: API Reference

language_tabs:
  - json

toc_footers:
  - <a href='http://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Sightworks platform API doc header here

# Conventions

## Tokens

Various tokens are used below to describe URLs. All tokens are presented as ``{{token}}`` in the rest of this document.

All of them defined here in the Conventions section are presented as all uppercase: ``{{ROOT}}``.

Name | Description | ``$type`` field contents
---- | ----------- | ------------------------
{{ROOT}} | The top-level URL for the API: ``https://example.digitalxe.com/api/1``. | [``Root``](#root-root)
{{CHANNEL}} | The URL to a channel resource: ``{{ROOT}}/channels/{{channel-id}}`` | [``ChannelResource``](#channel-channelresource)
{{APP}} | The URL to an app resource: ``{{ROOT}}/apps/{{app-id}}`` | [``AppResource``](#app-appresource)
{{GROUP}} | The URL to a group resource: ``{{APP}}/groups/{{group-id}}`` | ``AppGroup``
{{RECORD}} | The URL to a record resource: ``{{APP}}/records/{{record-id}}`` | ``AppRecord``
{{RESOURCE}} | An arbitrary resource URL. | [``Resource``](#resource-resource)
{{KEY}} | A channel key. | N/A

Other tokens, like ``{{channel-id}}`` and ``{{group-id}}`` are described as they appear.

Within a given example code block, each token will carry the same meaning: if multiple objects of the same type are presented in any
code block, like multiple records or groups, each will have it's URL expanded out.
 
## Object Types

For all objects described as being of the [``Resource``](#resource-resource) type, their header includes the token that appears in their ``$type`` property. Others,
which are used primarily for exposition of the rest of the API, do not have this in their header.

# Authentication

TODO

# Basic Types

Name | Description | Example
---- | ----------- | -------
Resource ID | The canonical URL to a resource, as a JSON String | ``"https://example.digitalxe.com/api/1/channels/888da06d62339a2a72706ef2502ae5ed"``
Timestamp | A string containing an ISO-8601 format timestamp (more specifically: in JavaScript, ``JSON.stringify(aDateObject)`` where ``aDateObject`` is an instance of the ``Date`` type.) | ``"2015-11-16T08:45:21.000Z"``
Boolean | The literal values ``true`` or ``false`` | ``true``
String | A JSON string whose length is at most 255 | ``"A string"``
Text | A JSON string whose length is at most 16777215 (about 16Mbytes) | ``"This is some text"``
Enumeration | A JSON string whose value is one of the values specified in the enumeration set where this is used | ``"Text"``
Array of {{Type}} | An array that contains a uniform set of values of the specified type. | ``[ 1, 2, 3 ]``
String Map | An object whose values are all strings. | ``{"Key": "Value"}``
Number | A number | ``1``

# Objects

## Resource (``Resource``)

```json
{
	"$type": [ "Resource" ],
	"id": "{{RESOURCE}}",
	"data": "/* The contents of the resource goes here */",
	"children": {
		"{{name}}": {
			"$Resource": "{{RESOURCE}}/{{name}}"
		}
	}
}
```

A resource is the basic object in the API.

### Fields in a resource

Name | Type | Description
---- | ---- | -----------
$type | Array of String | The various type names associated with this resource object. All resources include 'Resource'; see the relevant sections on each resource to find out what other types might appear here.
id | String | The URL to this resource
data | Any | The contents of this resource.
children | Resource Map | Other resources available as children of this resource. The ``{{name}}`` of each item depends on the resource itself. Each resource object contains a list of it's children

The ``data`` property is not required and does not appear on certain resources (those whose ``$type`` property includes ``Collection``, primarily). The same is true of the ``children`` property. 

### Query Parameters for Resources

Name | Type | Default | Description
---- | ---- | ------- | -----------
depth | Number | 0 | How deep to traverse the resource graph to retrieve items. For a depth value greater than 0, each Resource Pointer object found in the result is inserted into the resulting data that is returned to you.

For ``depth``: Each object retrieved by this method is pulled as if it had no parameters specified for it.

### Children

Since ``Resource`` is the base type of all resources, it doesn't define any children. See the other object types for details about their child objects.

## Channel (``ChannelResource``)

```json
{
	"id": "{{CHANNEL}}",
	"data": {
		"title": "Channel Title",
		"key": "key"
	},
	"children": {
		"apps": {
			"$Resource": "{{CHANNEL}}/apps"
		}
	},
	"$type": [
		"Resource",
		"ChannelResource"
	]
}
```

This represents a channel in the platform.

### Location

``ChannelResource`` objects are available as children of a [``ChannelCollection``](#collection-types).

Canonical URL: ``{{ROOT}}/channels/{{channel-id}}``

* ``{{ROOT}}`` is the API root
* ``{{channel-id}}`` is the channel identifier.

Example:

``https://example.digitalxe.com/api/1/channels/7bb50dc958ecf596cbb43e0db584c6dc``

The URL to a ``ChannelResource`` object is also a value for the ``{{CHANNEL}}`` [token](#tokens) used elsewhere.

### Fields

Name | Type | Description
---- | ---- | -----------
``data.title`` | String | The name of the channel
``data.key`` | String | The 'key' for this channel, used as part of all App IDs that are bound to this channel.

### Children

Name | URL | Description | Type
---- | ------------- | ----------- | ----
apps | ``{{CHANNEL}}/apps`` | The collection of apps in this channel | [``AppCollection``](#collection-types)

## App (``AppResource``)

```json
{
	"id": "{{APP}}",
	"data": {
		"id": "{{APP}}",
		"title": "App Title"
	},
	"children": {
		"root": {
			"$Resource": "{{APP}}/root"
		},
		"records": {
			"$Resource": "{{APP}}/records"
		},
		"groups": {
			"$Resource": "{{APP}}/groups"
		},
		"meta": {
			"$Resource": "{{APP}}/meta"
		}
	},
	"$type": [
		"Resource",
		"AppResource"
	]
}
```

An App in the platform.

### Location

``AppResource`` objects are available as children of an [``AppCollection``](#collection-types).

Canonical URL: ``{{ROOT}}/apps/{{app-id}}``

* ``{{ROOT}}`` is the API root
* ``{{app-id}}`` is the app ID

Example URL:

``https://example.digitalxe.com/api/1/apps/swt_addressBook``

The URL to an ``AppResource`` object is also a value for the ``{{APP}}`` [token](#tokens) used elsewhere.

### Fields

Name | Type | Description
---- | ---- | -----------
``data.title`` | String | The name of the app

### Children

Name | URL | Description | Type
---- | ------------- | ----------- | ----
root | ``{{APP}}/root`` | The group in this app that holds 'ungrouped' records and top-level groups. | ``AppGroup``
records | ``{{APP}}/records`` | The collection of all records in this app. | ``AppRecordList``
groups | ``{{APP}}/groups`` | The collection of all groups in this app, excluding 'ungrouped'. | ``AppGroupList``
meta | ``{{APP}}/meta`` | The app metadata container. | ``AppMetaData``

## App Meta Data (``AppMetaData``)

```json
{
	"id": "{{APP}}/meta",
	"data": null,
	"children": {
		"group": {
			"$Resource": "{{APP}}/meta/group"
		},
		"record": {
			"$Resource": "{{APP}}/meta/record"
		}
	},
	"$type": [
		"Resource",
		"AppMetaData"
	]
}
```

Contains metadata about the app's records & groups, detailing the structure of a record.

### Location

``AppMetaData`` objects are available as children of an [``AppResource``](#app-appresource).

Canonical URL: ``{{ROOT}}/apps/{{app-id}}/meta``

* ``{{ROOT}}`` is the API root
* ``{{app-id}}`` is the ID of the app

### Children

Name | URL | Description | Type
---- | ------------- | ----------- | ----
group | ``{{APP}}/meta/group`` | Description of an ``AppGroup`` object within this app. | ``AppGroupMetaData``
record | ``{{APP}}/meta/record`` | Description of an ``AppRecord`` object within this app. | ``AppRecordMetaData``

<span class='warning'>These objects are not currently documented.</span>

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
	"$type": [ "Resource", "RecordLikeObject" ]
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
	"$type": [ "Resource", "RecordLikeObject", "AppGroup" ]
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

## Record (``AppRecord``)

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
			"url": "Fragment",
			"title": "Title",
			"keywords": "Keywords",
			"description": "Description"
		},
		"data": {},
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

A record within an app.

### Location

``AppRecord`` objects are available as children of an [``AppRecordList``](#collection-types) collection.

Canonical URL:

``{{ROOT}}/apps/{{app-id}}/records/{{record-id}}``

In the above:

* ``{{ROOT}}`` is the API root.
* ``{{app-id}}`` is the app that this record is from
* ``{{record-id}}`` is the ID of the record

Examples:

``https://example.digitalxe.com/api/1/apps/swt_addressBook/records/7bb50dc958ecf596cbb43e0db584c6dc``

### Fields

The fields in an ``AppRecord`` are dependent on the app they came from. Various apps within the system have their own documentation below.

### Children

Name | URL | Description | Type
---- | ------------- | ----------- | ----
groups | ``{{RECORD}}/groups`` | Groups that this record is in | [``AppGroupList``](#collection-types)
related | ``{{RECORD}}/related`` | Apps which have related records for this record | [``AppRelatedList``](#collection-types)

<span class='info'>Apps may also define their own children here, if the record has pointers to other objects in the system.</span>

## Root (``Root``)

```json
{
	"id": "{{ROOT}}",
	"data": null,
	"children": {
		"channels": {
			"$Resource": "{{ROOT}}/channels"
		},
		"apps": {
			"$Resource": "{{ROOT}}/apps"
		},
		"multi": {
			"$Resource": "{{ROOT}}/multi"
		}
	},
	"$type": [
		"Resource",
		"Root"
	]
}
```

The API root object. This is the top-level of the API namespace.

### Location

This is the "Root" object in the API.

Canonical URL: ``{{ROOT}}``

* ``{{ROOT}}`` is the API root.

### Children

Name | URL | Description | Type
---- | ------------- | ----------- | ----
channels | ``{{ROOT}}/channels`` | The collection of channels on the platform instance | [``ChannelCollection``](#collection-types)
apps | ``{{ROOT}}/apps`` | All of the apps available on the platform instance | [``AppCollection``](#collection-types)
multi | ``{{ROOT}}/multi`` | Make multiple API requests at the same time. | [``Resource``](#resource)

The ``multi`` child does not really represent a child resource, but rather a callable method. See [POST Multiple Request Endpoint](#post-multiple-request-endpoint) for details.

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

## File

```json
{
	"name": "https://example.digitalxe.com/path/to/a/file",
	"type": "image/png",
	"size": 4455
}
```

An object that describes a file.

### Fields

Name | Type | Description
---- | ---- | -----------
``name`` | String | The URL to the file
``type`` | String | The MIME type of the file
``size`` | Number | The size of the file

## Interval

```json
{
	"count": 3,
	"unit": "minute"
}
```

An object that represents a time interval.

### Fields

Name | Type | Description
---- | ---- | -----------
``unit`` | Enumeration | The unit type represented by this object. One of ``second``, ``minute``, ``day``, ``month``, ``year``.
``count`` | Number | The number of ``unit`` items specified.

``unit`` does not include entries for 'hour' or 'week': to specify 1 hour, specify 60 minutes; for 1 week, specify 7 days.

## Resource Pointer

```json
{
	"$Resource": "{{RESOURCE}}"
}
```

A resource pointer is an object that contains a resource URL. They are constructed as to be easily noticed when using a resource.

The ``$Resource`` property contains the canonical URL of the resource.

## Resource Map

```json
{
	"name": {
		"$Resource": "{{RESOURCE}}"
	}
}
```

A resource map contains a set of named objects representing children of a given resource.

With a resource map, there are 2 ways to get to the resources provided:

* Take the value of the ``$Resource`` property in the object.
* Construct a URL using the URL of the current resource and appending '/' and the key from the resource map.

```
{
	"test-item": {
		"$Resource": "{{ROOT}}/test-item"
	}
}
```

> Resource map from {{ROOT}}/test

Using the resource map to the right, the ``test-item`` child can be accessed through either:

* ``{{ROOT}}/test-item`` - from the ``$Resource`` property
* ``{{ROOT}}/test/test-item`` - as a child of the original resource

## Response

```json
{
	"resource": "{{RESOURCE}}",
	"resources": {
		"{{RESOURCE}}": {
			"id": "{{RESOURCE}}",
			"data": null,
			"children": {
				"channels": {
					"$Resource": "{{RESOURCE}}/channels"
				}
			},
			"$type": [
				"Resource",
				"Root"
			]
		}
	}
}
```

This is an object containing the results of a resource request.

### Fields

Name | Type | Description
---- | ---- | -----------
resource | String | The canonical URL of the resource you requested. (This may not be the same as the one you requested, as many resources are available through multiple paths.)
resources | Object | An object whose keys consist of the canonical resource URLs that were retrieved and whose values are the resources themselves.

# Apps

## Courses
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
			"url": "Fragment",
			"title": "Title",
			"keywords": "Keywords",
			"description": "Description"
		},
		"data": {
			"title": "Title",
			"summary": "Summary",
			"description": "Description",
			"image": {
				"name": "http://example.digitalxe.com/path/to/image",
				"size": 1823,
				"type": "image/png"
			},
			"icon": {
				"name": "http://example.digitalxe.com/path/to/image",
				"size": 1823,
				"type": "image/png"
			},
			"difficulty": "Basic",
			"estimatedTimeRequired": {
				"count": 5,
				"unit": "minute"
			},
			"isGraded": false,
			"courseOverviewVideoEmbedCode": "text",
			"enforceTheOrderOfActivitiesInThisCourse": true,
			"courseHasStartDate": false,
			"courseStartDate": null,
			"doNotIncludeInCourseCatalog": false,
			"thisCourseRequirtesOffsitePurchase": false,
			"externalCourseId": "",
			"disableOffsitePurchase": false,
			"purchaseButtonText": "",
			"disabledPurchaseButtonText": ""
		},
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

A course in the LMS.

### Location

Canonical URL:

``{{ROOT}}/apps/{{KEY}_courses/records/{{record-id}}``

### Fields

In addition to those specified by [``AppRecord``](#record-apprecord), a course includes:

Name | Type | Description
---- | ---- | -----------
``data.title`` | String | The course title
``data.summary`` | Text | The course summary (plaintext)
``data.description`` | Text | The course description (HTML)
``data.image`` | File | The course header image
``data.icon`` | File | The course icon
``data.difficulty`` | Enumeration | The difficulty level of the course; one of ``Basic``, ``Intermediate``, ``Advanced``.
``data.estimatedTimeRequired`` | [Interval](#interval) | The amount of time you think it will take to complete the course
``data.isGraded`` | Boolean | Whether this course has a grading scale associated with it
``data.courseOverviewVideoEmbedCode`` | Text | Video embed code for the course overview page (HTML)
``data.enforceTheOrderOfActivitiesInThisCourse`` | Boolean | Whether to force the user to go through this course in the order of activities or not.
``data.courseHasStartDate`` | Boolean | Whether this course has a start date
``data.courseStartDate`` | Timestamp | When the course begins
``data.doNotIncludeInCourseCatalog`` | Boolean | Whether to hide this from most views of courses on the site
``data.thisCourseRequiresOffsitePurchase`` | Boolean | Whether this course needs to be purchased or not before it can be taken.
``data.externalCourseId`` | String | The external course identifier for this course.
``data.disableOffsitePurchase`` | Boolean | Whether to disable the offsite purchase functionality (linking the course "Start" button to the "Buy a Course" page)
``data.purchaseButtonText`` | String | The title of the 'Buy a Course' button when displayed
``data.disabledPurchaseButtonText`` | String | The text to display in place of the 'Buy a Course' or "Start" buttons when the course cannot be purchased.

### Children


# Miscellaneous API methods

## POST Multiple Request Endpoint

``POST {{ROOT}}/multi``

Retrieve or update a number of resources simultaneously. This method does not perform a transaction (where, if for example, a set of POST or PUT requests are included, if one were to fail, none would happen), but
rather just performs each request.

### Request Body

```json
{
	"requests": [
		{
			"method": "HTTP-Method",
			"resource": "Resource-URL",
			"query": {
				"Query": "Parameter"
			},
			"data": {
				"POST": "Data goes here"
			}
		}
	]
}
```

Property | Type | Description
-------- | ---- | -----------
``requests`` | Array | Array of the requests to be made
``requests[N].method`` | String | The request method to invoke. This should be a valid HTTP request method
``requests[N].resource`` | String | The full URI to the resource that the request is for. This should be relative to the API base URL (in this documentation, ``https://example.digitalxe.com/api/1``.)
``requests[N].query`` | String Map | The query parameters to append.
``requests[N].data`` | Object | The data that would be provided in the POST body. Since the API only accepts JSON bodies for POST data, the data should be the object without being JSON encoded itself.

### Response Body

```json
{
	"result": [
		{
			"responseCode": 200,
			"result": "Response-Body"
		}
	]
}
```

The result contains a list of encapsulated responses.

Response:

Property | Type | Description
-------- | ---- | -----------
``result`` | Array | The responses, in the same order that the requests were in the original request.
``result[N].responseCode`` | Number | The HTTP response code that would have been returned for the request.
``result[N].headers`` | String Map | A map of HTTP headers that would have been returned, if any.
``result[N].result`` | Any | The response body of the request.

