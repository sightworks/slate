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
            "method": "POST",
            "resource": "https://wvc.digitalxe.com/api/1/apps/lms_products/records",
            "data": {
                "object": {
                    "meta": {
                        "active": true,
                        "publishing": {
                            "active": null,
                            "archive": null
                        }
                    },
                    "data": {
                        "title": "Product"
                    }
                }
            }
        },
        {
            "method": "POST",
            "resourceAt": {
                "index": 0,
                "expression": "$.result.resources[$.result.created].children.productsItems"
            },
            "data": {
                "object": {
                    "meta": {
                        "active": true,
                        "publishing": {
                            "active": null,
                            "archive": null
                        }
                    },
                    "data": {
                        "type": "CourseRecord"
                    }
                }
            }
        },
        {
            "method": "PUT",
            "resourceAt": {
                "index": 1,
                "expression": "$.result.resources[$.result.created].children.links"
            },
            "data": {
                "course": {
                    "target": {
                        "$Resource": "https://wvc.digitalxe.com/api/1/apps/lms_courses/records/1456e26cc564eeff0700a2d9e5d0e2d5"
                    }
                },
                "courseGroup": {
                    "target": null
                },
                "parent": {
                    "target": {
                        "$Expression": {
                            "index": 0,
                            "expression": "Resource($.result.created)"
                        }
                    }
                }
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
``requests[N].resourceAt`` | Expression Descriptor | An expression to be evaluated to retrieve the resource URL
``requests[N].query`` | String Map | The query parameters to append.
``requests[N].data`` | Object | The data that would be provided in the POST body. Since the API only accepts JSON bodies for POST data, the data should be the object without being JSON encoded itself.

An expression descriptor:

Property | Type | Description
--- | --- | ---
``expression`` | String | An [expression](#expressions) that selects the value to be used.
``index`` | Number | The position in the result set to use as the '$' object for evaluating the expression

Only one of ``resource`` and ``resourceAt`` can be specified for each request. If ``resourceAt`` is specified, the expression provided
in the ``resourceAt.expression`` property needs to result in one of a String (being a resource URL), a Resource, or a Resource Pointer being
returned.

The ``data`` property can also have, within it's content, expressions to be evaluated. These are specified using an object with a single
property ``$Expression`` containing an expression descriptor.

### Response Body

```json
{
    "result": [
        {
            "responseCode": 201,
            "headers": {
                "Location": "https://wvc.digitalxe.com/api/1/apps/lms_products/records/95c2f5ce60653ce9b72086112f5835a6"
            },
            "result": {
                "created": "https://wvc.digitalxe.com/api/1/apps/lms_products/records/95c2f5ce60653ce9b72086112f5835a6",
                "collection": "https://wvc.digitalxe.com/api/1/apps/lms_products/records",
                "resources": {
                    "https://wvc.digitalxe.com/api/1/apps/lms_products/records/95c2f5ce60653ce9b72086112f5835a6": {
                        "id": "https://wvc.digitalxe.com/api/1/apps/lms_products/records/95c2f5ce60653ce9b72086112f5835a6",
                        "data": {
                            "meta": {
                                "active": true,
                                "created": "2015-12-02T23:03:18.000Z",
                                "createdBy": {
                                    "$Resource": "https://wvc.digitalxe.com/api/1/apps/swt_addressBook/records/57d1531fda6e4213b57482096efad00c"
                                },
                                "modified": "2015-12-02T23:03:19.000Z",
                                "modifiedBy": null,
                                "publishing": {
                                    "active": null,
                                    "archive": null
                                }
                            },
                            "data": {
                                "title": "Product"
                            }
                        },
                        "children": {
                            "groups": {
                                "$Resource": "https://wvc.digitalxe.com/api/1/apps/lms_products/records/95c2f5ce60653ce9b72086112f5835a6/groups"
                            },
                            "related": {
                                "$Resource": "https://wvc.digitalxe.com/api/1/apps/lms_products/records/95c2f5ce60653ce9b72086112f5835a6/related"
                            },
                            "meta": {
                                "$Resource": "https://wvc.digitalxe.com/api/1/apps/lms_products/records/95c2f5ce60653ce9b72086112f5835a6/meta"
                            },
                            "productsItems": {
                                "$Resource": "https://wvc.digitalxe.com/api/1/apps/lms_products/records/95c2f5ce60653ce9b72086112f5835a6/productsItems"
                            },
                            "links": {
                                "$Resource": "https://wvc.digitalxe.com/api/1/apps/lms_products/records/95c2f5ce60653ce9b72086112f5835a6/links"
                            }
                        },
                        "$linkTypes": [
                            "AppRecord:lms_products"
                        ]
                    },
                    "https://wvc.digitalxe.com/api/1/apps/lms_products/records": {
                        "id": "https://wvc.digitalxe.com/api/1/apps/lms_products/records",
                        "collection": {
                            "length": 1,
                            "items": [
                                {
                                    "$Resource": "https://wvc.digitalxe.com/api/1/apps/lms_products/records/95c2f5ce60653ce9b72086112f5835a6"
                                }
                            ]
                        }
                    }
                }
            }
        },
        {
            "responseCode": 201,
            "headers": {
                "Location": "https://wvc.digitalxe.com/api/1/apps/lms_productsItems/records/6acdbc64517d5182b9515bbce55f880a"
            },
            "result": {
                "created": "https://wvc.digitalxe.com/api/1/apps/lms_productsItems/records/6acdbc64517d5182b9515bbce55f880a",
                "collection": "https://wvc.digitalxe.com/api/1/apps/lms_products/records/95c2f5ce60653ce9b72086112f5835a6/productsItems",
                "resources": {
                    "https://wvc.digitalxe.com/api/1/apps/lms_productsItems/records/6acdbc64517d5182b9515bbce55f880a": {
                        "id": "https://wvc.digitalxe.com/api/1/apps/lms_productsItems/records/6acdbc64517d5182b9515bbce55f880a",
                        "data": {
                            "meta": {
                                "active": true,
                                "created": "2015-12-02T23:03:19.000Z",
                                "createdBy": {
                                    "$Resource": "https://wvc.digitalxe.com/api/1/apps/swt_addressBook/records/57d1531fda6e4213b57482096efad00c"
                                },
                                "modified": "2015-12-02T23:03:19.000Z",
                                "modifiedBy": null,
                                "publishing": {
                                    "active": null,
                                    "archive": null
                                }
                            },
                            "data": {
                                "type": "CourseRecord"
                            }
                        },
                        "children": {
                            "groups": {
                                "$Resource": "https://wvc.digitalxe.com/api/1/apps/lms_productsItems/records/6acdbc64517d5182b9515bbce55f880a/groups"
                            },
                            "related": {
                                "$Resource": "https://wvc.digitalxe.com/api/1/apps/lms_productsItems/records/6acdbc64517d5182b9515bbce55f880a/related"
                            },
                            "meta": {
                                "$Resource": "https://wvc.digitalxe.com/api/1/apps/lms_productsItems/records/6acdbc64517d5182b9515bbce55f880a/meta"
                            },
                            "parent": {
                                "$Resource": "https://wvc.digitalxe.com/api/1/apps/lms_products/records/95c2f5ce60653ce9b72086112f5835a6"
                            },
                            "links": {
                                "$Resource": "https://wvc.digitalxe.com/api/1/apps/lms_productsItems/records/6acdbc64517d5182b9515bbce55f880a/links"
                            }
                        },
                        "$linkTypes": [
                            "AppRecord:lms_productsItems"
                        ]
                    },
                    "https://wvc.digitalxe.com/api/1/apps/lms_products/records/95c2f5ce60653ce9b72086112f5835a6/productsItems": {
                        "id": "https://wvc.digitalxe.com/api/1/apps/lms_products/records/95c2f5ce60653ce9b72086112f5835a6/productsItems",
                        "collection": {
                            "length": 1,
                            "items": [
                                {
                                    "$Resource": "https://wvc.digitalxe.com/api/1/apps/lms_productsItems/records/6acdbc64517d5182b9515bbce55f880a"
                                }
                            ],
                            "types": [
                                "AppRecord:lms_productsItems"
                            ]
                        }
                    }
                }
            }
        },
        {
            "responseCode": 200,
            "result": {
                "resource": "https://wvc.digitalxe.com/api/1/apps/lms_productsItems/records/6acdbc64517d5182b9515bbce55f880a/links",
                "resources": {
                    "https://wvc.digitalxe.com/api/1/apps/lms_productsItems/records/6acdbc64517d5182b9515bbce55f880a/links": {
                        "id": "https://wvc.digitalxe.com/api/1/apps/lms_productsItems/records/6acdbc64517d5182b9515bbce55f880a/links",
                        "data": {
                            "courseGroup": {
                                "type": "AppGroup:lms_courses",
                                "target": null
                            },
                            "course": {
                                "type": "AppRecord:lms_courses",
                                "target": {
                                    "$Resource": "https://wvc.digitalxe.com/api/1/apps/lms_courses/records/1456e26cc564eeff0700a2d9e5d0e2d5"
                                }
                            },
                            "parent": {
                                "type": "AppRecord:lms_products",
                                "target": {
                                    "$Resource": "https://wvc.digitalxe.com/api/1/apps/lms_products/records/95c2f5ce60653ce9b72086112f5835a6"
                                }
                            }
                        },
                        "children": {},
                        "$type": [
                            "Resource",
                            "WritableResource",
                            "LinkResource"
                        ]
                    }
                }
            }
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
``result[N].headers`` | String Map | A map of HTTP headers that would have been returned, if any. The property does not exist if no headers would have been returned.
``result[N].result`` | Any | The response body of the request.

