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
		},
		"s3-storage": {
			"$Resource": "{{CHANNEL}}/s3-storage"
		},
		"views": {
			"$Resource": "{{CHANNEL}}/views"
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

Name | Type | Description
---- | ---- | ----
``s3-storage`` | [S3 Storage descriptor](#s3-storage) | A description of the buckets made available from this channel along with information needed to access those buckets.
``apps`` | [AppCollection](#collections) | A collection of the apps that are in this channel.
``views`` | [View Parent](#view-parent) | The parent resource for all information about resource views. (This is here for completeness.)
``views/current`` | [View Collection](#view-collection) | A collection containing current resource view information over the past 8-9 hours.
``views/history`` | [View Collection](#view-collection) | A collection containing historic resource view information.

## S3 Storage

```json
{
	"buckets": {
		"https://s3-us-west-2.amazonaws.com/digitalxe-clients/example/video/channels/example/": {
			"name": "DigitalXE Videos",
			"type": "video",
			"region": "us-west-2",
			"bucket": "digitalxe-clients",
			"prefix": "example/video/channels/example/",
			"credentials": "Platform"
		}
	},
	"credentials": {
		"Platform": {
			"accessKeyId": "ASIA012345EXAMPLE",
			"secretAcccessKey": "ACCESSKEYEXAMPLE",
			"sessionToken": "fdsfdskjhvldsEXAMPLE",
			"expires": "2015-12-18T23:30:00Z"
		}
	}
}
```
		
The ``s3-storage`` child on a channel exposes information about Amazon S3 access.

### Properties

Name | Type | Description
--- | --- | ---
``buckets`` | Object | The list of S3 buckets that are known for this channel. This includes "Platform" buckets, which always exist. The object keys are full URL prefixes to each bucket.
``buckets[{{BucketURLPrefix}}]`` | Object | The description onf a bucket.
``buckets[{{BucketURLPrefix}}].name`` | String | The displayed name of this bucket.
``buckets[{{BucketURLPrefix}}].type`` | Enumeration | The bucket content type. One of 'video', 'audio', 'document', or 'all'.
``buckets[{{BucketURLPrefix}}].region`` | String | The Amazon region identifier that the bucket resides in
``buckets[{{BucketURLPrefix}}].bucket`` | String | The name of the S3 bucket
``buckets[{{BucketURLPrefix}}].prefix`` | String | The prefix of all items in the bucket. If this has a value, it always ends with a '/'.
``buckets[{{BucketURLPrefix}}].credentials`` | String | The name of the particular credential set used to access this bucket.
``credentials`` | Object | The list of known credential sets. This will always have a ``Platform`` entry, and may have a ``User`` entry as well.
``credentials.{{Type}}.accessKeyId`` | String | The Amazon "access key ID"
``credentials.{{Type}}.secretAccessKey`` | String | The Amazon "secret access key"
``credentials.{{Type}}.sessionToken`` | String | The Amazon "session token". This is only provided for temporary credentials (any generated for 'Platform'), so that those credentials can be restricted.
``credentials.{{Type}}.expires`` | Date | The date that the credentials expire, if they are temporary credentials.

### Example 

```javascript
// The URL of the file that we want to use S3 to access
var fileURL = "https://s3-us-west-2.amazonaws.com/digitalxe-clients/example/documents/channels/example/file.pdf";

// The resource ID that the file URL came from
var fileSource = "https://example.digitalxe.com/api/1/apps/example_files/records/12345";

// A regular expression for extracting the app ID from the URL
var extractAppID = /^https:\/\/\w+\.digitalxe\.com\/api\/1\/apps\/([^\/]+)\/;

var result = extractAppID.exec(fileSource);
var appID = result[1];
var channelKey = appID.split('_').shift(); // The channel key on an app is always the first segment of the app ID.

// Fetch(Resource, callback) -> Fill out this function yourself, but it fetches the URL in the 'Resource' argument
// and invokes 'callback' with the result.

Fetch("https://example.digitalxe.com/api/1/channels/by-key/" + channelKey + "/s3-storage", function(storageDetail) {
	/* 
	storageDetail looks like:
	var storageDetail = {
		buckets: {
			"https://s3-us-west-2.amazonaws.com/digitalxe-clients/example/documents/channels/example/": {
				name: "DigitalXE Documents",
				type: "document",
				region: "us-west-2",
				bucket: "digitalxe-clients",
				prefix: "example/documents/channels/example/",
				credentials: "Platform"
			}
		},
		credentials: {
			Platform: {
				accessKeyId: "AKIAFOOBAREXAMPLE",
				secretAccessKey: "fdsfsnfdvfhfwfw===EXAMPLE",
				sessionToken: "ibsjfhsdkjfcsncjdsnlieufhwieu==EXAMPLE",
				expires: "2015-12-22T12:34:56Z"
			}
		}
	}
	*/

	var whichBucket = "";
	for (var i in storageDetail.buckets) {
		if (fileURL.startsWith(i) && i.length > whichBucket.length) {
			whichBucket = i;
		}
	}
	var theBucket = storageDetail.buckets[whichBucket];
	var theEndpoint = "https://s3";
	if (theBucket.region != 'us-east-1') 
		theEndpoint += '-' + theBucket.region;
	theEndpoint += ".amazonaws.com/" + theBucket.bucket + "/";
	var theItemKey = whichBucket.substring(theEndpoint.length);
	var theCredentials = storageDetail.credentials[theBucket.credentials];

	// Now:
	// - theBucket contains the details of the S3 bucket (region, bucket name)
	// - theItemKey contains the key of the item in S3
	// - theCredentials contains the necessary authorization information to access the named item
});

```

Assuming you have a file object from the "example_files" app whose name was ``https://s3-us-west-2.amazonaws.com/digitalxe-clients/example/documents/channels/example/file.pdf``, you
would fetch the credentials from ``{{ROOT}}/channels/by-key/example/s3-storage`` ("example" is the channel key in the case of "example_files"); and then find the item in 
``buckets`` whose URL prefix matches the file path.)

See the code example to the right for some example code for getting this information.

<br style='clear: right;'>

## View Parent

This is a simple object that has 2 view collections as children: ``current`` and ``history``.

The difference between the two: ``current`` contains view data made over the previous 8-9 hours; ``history`` contains all older data.

Both children are [View Collection](#view-collection) objects.

## View Collection

This is a basic collection of details about record views. In addition to the normal [collection](#collections-collection) filtering rules 
using the ``expression`` parameter, you can also filter this collection with the following:

### Parameters

Name | Type | Default | Description
---|---|---|---
app | Resource ID | ``""`` | Filter the list of views by the app they came from. The resource here is an [``AppResource``](#app-appresource). Leave unspecified or pass nothing (in the query: ``app=``) to get all.
resource | Resource ID | ``""`` | Filter the list of views by the resource they're associated with. The resource here is an [``AppGroup``](#group-appgroup) or [``AppRecord``](#record-apprecord) object. Leave unspecified to get all objects.
user | Resource ID or String | ``""`` | Filter the list of views by the person who viewed them. This can also be one of the special values ``null`` to get all views by non-authenticated users or ``known`` to get all by authenticated users.
site | String | ``live`` | One of the values ``dev`` to include only those requests made against the development site (``example.digitalxe.com``), ``live`` to get those to the published site, or ``both`` to get all.
access | String | ``both`` | ``denied`` to get the items that were presented as previews, ``granted`` to get the items that were presented in their full form, or ``both`` if you don't care.
startTime | Date and time | ``""`` | The start time to query against. This is any date string considered valid by the V8 JavaScript engine's ``Date`` constructor.
endTime | Date and time | ``""`` | The end time to query against. This is any date string considered valid by the V8 JavaScript engine's ``Date`` constructor; if ``startTime`` is also specified, ``endTime`` must be greater than the value of ``startTime``.

## View objects

```json
{
	"id": "{{CHANNEL}}/views/1",
	"data": {
		"app": {
			"$Resource": "{{APP}}"
		},
		"resource": {
			"$Resource": "{{GROUP or RECORD}}"
		},
		"user": {
			"$Resource": "{{RECORD}}"
		},
		"dev": false,
		"accessGranted": true,
		"time": "2016-04-13T22:20:23Z"
	}
}
```

These are contained by a [View Collection](#view-collection), representing a single record view.

### Properties

Name | Type | Description
--- | --- | ---
``app`` | Resource | The app that the view object came from
``resource`` | Resource | The object that was viewed
``user`` | Resource | The user that viewed the object. This is ``null`` if the user was anonymous.
``dev`` | Boolean | Whether the view was done on the development site or the production site.
``accessGranted`` | Boolean | Whether the view retrieved the full object or just a preview of the object.
``time`` | Timestamp | The time of the record view.

