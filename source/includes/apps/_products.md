## Products

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
		"data": {
			"title": "Product Name",
			"summary": "Product summary",
			"image": {
				"name": "http://example.digitalxe.com/path/to/image",
				"size": 1823,
				"type": "image/png"
			},
			"description": "Description HTML",
			"price": "Price detail",
			"buyButtonText": "Text for Buy Now button",
			"buyButtonUrl": "Link for Buy Now button",
			"availableForPurchase": true,
			"availableStart": null,
			"availableEnd": null,
			"notAvailableText": "",
			"hasBanner": false,
			"bannerBackgroundColor": "",
			"bannerTextColor": "",
			"bannerText": "",
			"bannerTextSize": "100%",
			"bannerImage": null,
			"bannerImageAlign": "left"
		}
	},
	"children": {
		"groups": {
			"$Resource": "{{RECORD}}/groups"
		},
		"productsItems": {
			"$Resource": "{{RECORD}}/productsItems"
		}
	},
	"$type": [ "Resource", "RecordLikeObject", "AppRecord" ]
}
```

A product in the LMS.

### Location

Canonical URL:

``{{ROOT}}/apps/{{KEY}}_products/records/{{record-id}}``

### Fields

In addition to those specified by [``AppRecord``](#record-apprecord), a product includes:

Name | Type | Description
---- | ---- | -----------
``data.title`` | String | The name of the product
``data.summary`` | String | The summary of the product (deprecated)
``data.image`` | File | The product image (deprecated)
``data.description`` | String | The product description (HTML, deprecated)
``data.price`` | String | Pricing information
``data.buyButtonText`` | String | The text to display on the "Buy" button
``data.buyButtonUrl`` | String | The link for the "Buy" button
``data.availableForPurchase`` | Boolean | Whether the product is available for purchase or not.
``data.availableStart`` | Date ('YYYY-MM-DD') | If the product is available for purchase, the date the buy button becomes active
``data.availableEnd`` | Date ('YYYY-MM-DD') | If the product is available for purchase, the date the buy button becomes inactive
``data.notAvailableText`` | String | Text to display to the user if ``availableForPurchase`` is false.
``data.hasBanner`` | Boolean | Whether this product has a promotional banner appearing on objects that use it
``data.bannerBackgroundColor`` | String (CSS color) | The background color to use for the banner
``data.bannerTextColor`` | String (CSS color) | The foreground color to use for the banner
``data.bannerText`` | String | The text to display in the banner (HTML)
``data.bannerTextSize`` | Enumeration | The size of text to use. Values are '50%', '75%', '100%', '125%', '150%', '200%'.
``data.bannerImage`` | File | The image to display in the banner
``data.bannerImageAlign`` | Enumeration | The side of the banner to display the image; 'left' or 'right'.

### Children

Name | URL | Description | Type
--- | --- | --- | ---
productsItems | ``{{RECORD}}/productsItems`` | The items that make up this product. | [``AppRecordList``](#collection-types) with [Product Item](#product-item) objects

## Product Items

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
		"data": {
			"type": "CourseRecord"
		}
	},
	"children": {
		"groups": {
			"$Resource": "{{RECORD}}/groups"
		},
		"parent": {
			"$Resource": "{{ROOT}}/apps/{{KEY}}_products/records/{{ID}}"
		},
		"course": {
			"$Resource": "{{ROOT}}/apps/{{KEY}}_courses/records/{{ID}}"
		}
	}
}
```

### Location

Canonical URL:

``{{ROOT}}/apps/{{KEY}}_productsItems/records/{{record-id}}``

### Fields

In addition to those specified by [``AppRecord``](#record-apprecord), a product item includes:

Name | Type | Description
--- | --- | ---
``data.type`` | Enumeration | The type of entry. One of "AllCourses", "CourseGroup", "CourseRecord", "AllVideos", "VideoGroup", "VideoRecord", "AllAudio", "AudioGroup", "AudioRecord", "AllDocuments", "DocumentGroup", "DocumentRecord".

### Children

Name | URL | Description | Type
--- | --- | --- | ---
course | ``{{ROOT}}/apps/{{KEY}}_courses/records/{{ID}}`` | The course that is attached to this product item, when ``data.type`` is 'CourseRecord'. | [``AppRecord``](#record-apprecord) from [Courses](#courses)
courseGroup | ``{{ROOT}}/apps/{{KEY}}_courses/groups/{{ID}}`` | The group of courses that is attached to this product item, when ``data.type`` is 'CourseGroup' | [``AppGroup``](#group-appgroup) from [Courses](#courses)
audio | ``{{ROOT}}/apps/{{KEY}}_audio/records/{{ID}}`` | The audio file that is attached to this product item, when ``data.type`` is 'AudioRecord'. | [``AppRecord``](#record-apprecord) from [Audio Files](#audio-files)
audioGroup | ``{{ROOT}}/apps/{{KEY}}_audio/groups/{{ID}}`` | The group of audio files that is attached to this product item, when ``data.type`` is 'AudioGroup' | [``AppGroup``](#group-appgroup) from [Audio Files](#audio-files)
video | ``{{ROOT}}/apps/{{KEY}}_videos/records/{{ID}}`` | The video that is attached to this product item, when ``data.type`` is 'VideoRecord'. | [``AppRecord``](#record-apprecord) from [Videos](#videos)
videoGroup | ``{{ROOT}}/apps/{{KEY}}_videos/groups/{{ID}}`` | The group of videos that is attached to this product item, when ``data.type`` is 'VideoGroup' | [``AppGroup``](#group-appgroup) from [Videos](#videos)
document | ``{{ROOT}}/apps/{{KEY}}_documents/records/{{ID}}`` | The document that is attached to this product item, when ``data.type`` is 'DocumentRecord'. | [``AppRecord``](#record-apprecord) from [Documents](#documents)
documentGroup | ``{{ROOT}}/apps/{{KEY}}_documents/groups/{{ID}}`` | The group of documents that is attached to this product item, when ``data.type`` is 'DocumentGroup' | [``AppGroup``](#group-appgroup) from [Documents](#documents)
parent | ``{{ROOT}}/apps/{{KEY}}_products/records/{{ID}}`` | The product that this item is attached to. | [``AppRecord``](#record-apprecord) from [Products](#products)
