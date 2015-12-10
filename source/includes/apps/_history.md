## History

```json
{
    "id": "{{RECORD}}",
    "data": {
        "meta": {
            "active": true,
            "created": "2015-12-03T23:23:50.000Z",
            "createdBy": {
                "$Resource": "{{ROOT}}/apps/swt_addressBook/records/{{person-id}}",
            },
            "modified": "2015-12-03T23:23:50.000Z",
            "modifiedBy": null,
            "publishing": {
                "active": null,
                "archive": null
            }
        },
        "data": {
	        "member": {
		        "$Resource": "{{ROOT}}/apps/{{KEY}}_accounts/records/{{person-id}}"
		    },
	        "timeOfEntry": "2015-12-03T23:23:50.000Z",
	        "sequence": {
		        "parent": null,
		        "previous": null
		    },
		    "objectDetail": null,
		    "statement": {
			    "type": "Began",
			    "autocomplete": null
			},
			"objectType": {
				"type": "Course",
				"course": {
					"$Resource": "{{ROOT}}/apps/{{KEY}}_courses/records/{{course-id}}"
				}
			}
		}
    },
    "children": {
        "groups": {
            "$Resource": "{{RECORD}}/groups",
        },
        "related": {
            "$Resource": "{{RECORD}}/related"
        },
        "meta": {
            "$Resource": "{{RECORD}}/meta"
        },
        "links": {
            "$Resource": "{{RECORD}}/links"
        }
    },
    "$linkTypes": [
        "AppRecord:{{KEY}}_history"
    ],
    "$type": [
        "Resource",
        "WritableResource",
        "RecordLikeObject",
        "LinkSource",
        "LinkTarget",
        "AppRecord"
    ]
}
```

A history entry in the LMS. These represent the full state of a user over time.

### Usage by the System

History entries are used by the system for storing all state associated with a user.

The current state associated with a user can be determined by looking for all history entries which have their ``meta.active`` property set to true; 
for the most part, history entries with ``meta.active`` being false are ignored. They are used, however, in cases such as retaking a test - if the
test activity says that the test can only be taken 5 times, then the code will check for the previous history entries before allowing a retake of the test.

#### Statements

Each of these are represented as "{{``statement.type``}} {{``objectType.type``}}"; the order is in the sequence you would typically encounter them.

* "Event Other": Some event was recorded for the user.
* "Began Course": The user began the specified course
* "Began Activity": The user began the specified activity
* "Progress Activity": The user made progress on the specified activity.
* "TinCan Activity": A Tin Can object was stored in the user's history for this activity.
* "Ended Activity": The user completed the specified activity.
* "Ended Course": The user completed the specified course.
* "Earned Reward": The user earned a reward.

#### Organization

Each statement has 2 pointers to already-existing statements:

* ``sequence.previous``: If this is the 'Ended' statement from a 'Began' / 'Ended' pair, the statement here is the 'Began' statement associated with this.
* ``sequence.parent``: The "parent" of this statement:
  * "Began Activity" and "Ended Activity" have a "Began Course" as their parent.
  * "Progress Activity" and "TinCan Activity" have a "Began Activity" as their parent.
  * "Earned Reward" can have any other statement as it's parent, depending on the reason the reward was grantd.

### Location

Canonical URL:

``{{ROOT}}/apps/{{KEY}_history/records/{{record-id}}``

### Fields

In addition to those specified by [``AppRecord``](#record-apprecord), a history entry includes:

Name | Type | Description
---- | ---- | -----------
``data.member`` | Resource Pointer | The member that the statement is for.
``data.timeOfEntry`` | Date | The time the entry was made. This may be the same as the date of creation (but doesn't have to be).
``data.sequence.parent`` | Resource Pointer | The parent history entry for this item
``data.sequence.previous`` | Resource Pointer | The previous history entry under this parent
``data.objectDetail`` | Object | Details that depend on the type of statement being made. (FIXME: Document the structure of these.)
``data.statement.type`` | Enumeration | The type of statement; one of 'Began', 'Ended', 'Progress', 'Earned', 'Event', 'TinCan'.
``data.statement.autocomplete`` | Date | When ``statement.type`` is 'Began', the time to process the remaining state for this entry and construct an 'Ended' record for it.
``data.statement.success`` | Boolean | When ``statement.type`` is 'Ended', if this statement represents a successful completion of the object in ``objectType``.
``data.statement.score`` | Object | When ``statement.type`` is 'Ended' or 'Progress', the object representing scoring information, or null if no scores are avaialble.
``data.statement.score.earned`` | Number | The number of points earned for this object
``data.statement.score.possible`` | Number | The number of points possible for this object
``data.statement.grade`` | String | When ``statement.type`` is 'Ended', the text describing the grade earned.
``data.statement.count`` | Number | When ``statement.type`` is 'Earned', the number of items earned.
``data.statement.file`` | File | When ``statement.type`` is 'Earned', an associated file that was earned.
``data.statement.eventType`` | String | When ``statement.type`` is 'Event', the label describing the event.
``data.statement.eventValue`` | String | When ``statement.type`` is 'Event', the value associated with the event label.
``data.statement.tincan`` | Tin Can Data Object | When ``statement.type`` is 'TinCan', various pieces of data representing the makeup of any object represented in the [Tin Can API](https://tincanapi.com/).
``data.objectType.type`` | Enumeration | The type of object; one of 'Activity', 'Course', 'Reward', 'Other'.
``data.objectType.activity`` | Resource Pointer | When ``objectType.type`` is 'Activity', the [Activity](#activities) object that the history entry is for.
``data.objectType.course`` | Resource Pointer | When ``objectType.type`` is 'Course' or 'Activity', the [Course](#courses) that the history entry is for.
``data.objectType.reward`` | Resource Pointer | When ``objectType.type`` is 'Reward', the Reward that the history entry is for.
``data.objectType.rule`` | Resource Pointer | When ``objectType.type`` is 'Reward', the Rule that caused the history entry to be created.

### Examples

* Fetch the list of courses that a user has began and completed:<br>
  ``{{ROOT}}/apps/{{KEY}}_history/records`` with 'depth' of 2 and 'filter':<br>
  ``$.data.meta.active && ($.data.data.statement.type == "Began" || $.data.data.statement.type == "Ended") && $.data.data.objectType == "Course" && $.data.data.member.is(Resource("{{ROOT}}/apps/{{KEY}}_accounts/records/{{id}}"))``<br><br>
  Use ``depth=2`` to ask the system for the course information so it can be referenced as well.
  
* Fetch the list of courses that a user has completed:<br>
  ``{{ROOT}}/apps/{{KEY}}_history/records`` with 'depth' of 2 and 'filter':<br>
  ``$.data.meta.active && $.data.data.statement.type == "Ended" && $.data.data.objectType.type == "Course" && $.data.data.member.is(Resource("{{ROOT}}/apps/{{KEY}}_accounts/records/{{id}}"))``<br><br>
  Again, ``depth=2`` will cause the system to go into the history items (at depth 1) and then fetch the courses (at depth 2).<br>
  The associated "Began Course" records will also be pulled (as they exist in the ``data.sequence.previous`` pointer).