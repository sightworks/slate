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
	        "timeOfEntry": "2015-12-03T23:23:50.000Z",
	        "autocomplete": null,
	        "statement": "Began",
	        "objectType": "Course",
	        "successfulCompletion": false,
	        "scoresAvailableForThisObject": false,
	        "pointsEarned": null,
	        "pointsPossible": null,
	        "gradeDescription": null,
	        "objectDetail": null,
	        "tc_type": "",
	        "tc_timestamp": null,
	        "tc_agentType": null,
	        "tc_agentId": null,
	        "tc_registrationId": null,
	        "tc_stateId": null,
	        "tc_activityId": null,
	        "tc_documentType": null,
			"rewardCount": null,
			"rewardFile": null,
			"eventType": null,
			"eventValue": null
		}
    },
    "children": {
	    "member": {
		    "$Resource": "{{ROOT}}/apps/{{KEY}}_accounts/records/{{person-id}}"
		},
		"course": {
			"$Resource": "{{ROOOT}}/apps/{{KEY}}_courses/records/{{course-id}}"
		},
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

Each of these are represented as "{{``statement``}} {{``objectType``}}"; the order is in the sequence you would typically encounter them.

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

* ``precedingEntry``: If this is the 'Ended' statement from a 'Began' / 'Ended' pair, the statement here is the 'Began' statement associated with this.
* ``parentEntry``: The "parent" of this statement:
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
``data.timeOfEntry`` | Date | The time the entry was made. This may be the same as the date of creation (but doesn't have to be).
``data.statement`` | Enumeration | The statement being made. Currently: Began, Ended, Progress, Earned, Event, TinCan
``data.objectType`` | Enumeration | The object that is the target of this item. Currently: Course, Activity, Reward, Other
``data.autocomplete`` | Date | If this is set, and ``statement`` is "Began", the time to automatically construct the "Ended" record to go with this.
``data.successfulCompletion`` | Boolean | If the statement is 'Ended', whether this history entry represents a success or failure.
``data.scoresAvailableForThisObject`` | Boolean | If the statement is 'Ended', whether this history entry has scoring information available.
``data.pointsEarned`` | Number | If the statement is 'Ended' and ``scoresAvailableForThisObject`` is true, the number of points earned.
``data.pointsPossible`` | Number | If the statement is 'Ended' and ``scoresAvailableForThisObject`` is true, the number of points that could have been earned.
``data.gradeDescription`` | String | If the statement is 'Ended' and ``scoresAvailableForThisObject`` is true, the text from the [Grade](#course-grading) ``name`` property that represents the grade earned.
``data.objectDetail`` | Object | Details that depend on the type of statement being made. (FIXME: Document the structure of these.)
``data.tc_*`` | String | If the statement type is 'TinCan', various pieces of data representing the makeup of any object represented in the [Tin Can API](https://tincanapi.com/).
``data.rewardCount`` | Number | If the object type is "Reward", and the reward in ``children.reward`` is of a type that grants multiple items (like a Points reward), the count associated.
``data.rewardFile`` | File | If the object type is "Reward", and the reward in ``children.reward`` generates a file, the file associated.
``data.eventType`` | String | If the statement is 'Event', a named event being recorded.
``data.eventValue`` | String | If the statement is 'Event', the associated value for the event.

### Children

Name | URL | Description | Type
---- | ------------- | ----------- | ----
parentEntry | ``{{ROOT}}/apps/{{KEY}}_history/records/{{record-id}}`` | The "parent" history entry for the selected item. | [``AppRecord``](#record-apprecord) of type History
precedingEntry | ``{{ROOT}}/apps/{{KEY}}_history/records/{{record-id}}`` | The "preceding" history entry for the selected item. | [``AppRecord``](#record-apprecord) of type History
member | ``{{ROOT}}/apps/{{KEY}}_accounts/records/{{record-id}}`` | The member that this history entry is for. | [``AppRecord``](#record-apprecord) of type [Account](#accounts)
course | ``{{ROOT}}/apps/{{KEY}}_courses/records/{{record-id}}`` | In a statement with an object type of 'Course' or 'Activity', the course that the statement references. | [``AppRecord``](#record-apprecord) of type [Course](#courses)
activity | ``{{ROOT}}/apps/{{KEY}}_activities/records/{{record-id}}`` | In a statement with an object type of 'Activity', the activity that the statement references. | [``AppRecord``](#record-apprecord) of type [Activity](#activities)
reward | ``{{ROOT}}/apps/{{KEY}}_rewards/records/{{record-id}}`` | In a statement with an object type of 'Reward', the reward. | [``AppRecord``](#record-apprecord) of type Reward
rule | ``{{ROOT}}/apps/{{KEY}}_rewardEngine/records/{{record-id}}`` | In a statement with an object of type 'Reward', the rule that describes how the reward is granted. | [``AppRecord``](#record-apprecord) of type Rule

### Examples

* Fetch the list of courses that a user has began and completed:<br>
  ``{{ROOT}}/apps/{{KEY}}_history/records`` with 'depth' of 2 and 'filter':<br>
  ``$.meta.active && ($.data.statement == "Began" || $.data.statement == "Ended") && $.data.objectType == "Course" && $.children.member.is(Resource("{{ROOT}}/apps/{{KEY}}_accounts/records/{{id}}"))``<br><br>
  Use ``depth=2`` to ask the system for the course information so it can be referenced as well.
  
* Fetch the list of courses that a user has completed:<br>
  ``{{ROOT}}/apps/{{KEY}}_history/records`` with 'depth' of 2 and 'filter':<br>
  ``$.meta.active && $.data.statement == "Ended" && $.data.objectType == "Course" && $.children.member.is(Resource("{{ROOT}}/apps/{{KEY}}_accounts/records/{{id}}"))``<br><br>
  Again, ``depth=2`` will cause the system to go into the history items (at depth 1) and then fetch the courses (at depth 2).<br>
  The associated "Began Course" records will also be pulled (as they exist in the ``precedingEntry`` child pointer).