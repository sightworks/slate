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
			"title": "Product Name"
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
			"type": "CourseRecord",
			"parent_index": 0
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
``data.type`` | Enumeration | The type of entry. One of "AllCourses", "CourseGroup", "CourseRecord"
``data.parent_index`` | Number | The order of this item in the set of items attached to the product.

### Children

Name | URL | Description | Type
--- | --- | --- | ---
course | ``{{ROOT}}/apps/{{KEY}}_courses/records/{{ID}}`` | The course that is attached to this product item, when ``data.type`` is 'CourseRecord'. | [``AppRecord``](#record-apprecord) from [Courses](#courses)
courseGroup | ``{{ROOT}}/apps/{{KEY}}_courses/groups/{{ID}}`` | The group of courses that is attached to this product item, when ``data.type`` is 'CourseGroup' | [``AppGroup``](#group-appgroup) from [Courses](#courses)
parent | ``{{ROOT}}/apps/{{KEY}}_products/records/{{ID}}`` | The product that this item is attached to. | [``AppRecord``](#record-apprecord) from [Products](#products)
