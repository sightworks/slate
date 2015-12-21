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
s3-storage | ``{{CHANNEL}}/s3-storage`` | Information about Amazon S3 storage for files in this channel | ``GenericResource``

### Storage

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

Properties:

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

