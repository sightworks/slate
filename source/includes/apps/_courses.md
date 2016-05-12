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
			"disabledPurchaseButtonText": "",
			"courseHasEndDate": false,
			"courseEndDate": null
		}
	},
	"children": {
		"activityRoot": {
			"$Resource": "{{ROOT}}/apps/{{KEY}}_activities/records/{{record-id}}"
		},
		"coursesAnnouncements": {
			"$Resource": "{{RECORD}}/coursesAnnouncements"
		},
		"coursesComments": {
			"$Resource": "{{RECORD}}/coursesComments"
		},
		"coursesDownloads": {
			"$Resource": "{{RECORD}}/coursesDownloads"
		},
		"coursesGrading": {
			"$Resource": "{{RECORD}}/coursesGrading"
		},
		"coursesInstructors": {
			"$Resource": "{{RECORD}}/coursesInstructors"
		},
		"coursesPrerequisites": {
			"$Resource": "{{RECORD}}/coursesPrerequisites"
		},
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
``data.courseStartDate`` | Timestamp | When the course begins (people cannot start the course before this date)
``data.courseHasEndDate`` | Boolean | Whether this course has an end date
``data.courseEndDate`` | Timestamp | When the course ends (people cannot start the course after this date)
``data.doNotIncludeInCourseCatalog`` | Boolean | Whether to hide this from most views of courses on the site
``data.thisCourseRequiresOffsitePurchase`` | Boolean | Whether this course needs to be purchased or not before it can be taken.
``data.externalCourseId`` | String | The external course identifier for this course.
``data.disableOffsitePurchase`` | Boolean | Whether to disable the offsite purchase functionality (linking the course "Start" button to the "Buy a Course" page)
``data.purchaseButtonText`` | String | The title of the 'Buy a Course' button when displayed
``data.disabledPurchaseButtonText`` | String | The text to display in place of the 'Buy a Course' or "Start" buttons when the course cannot be purchased.
``data.enableAccessRestriction`` | Boolean | Whether access restrictions are enabled on this content or not. If access restrictions are enabled, the system will either present a preview version of this object (if ``hideWhenRestricted`` is false), or hide the object entirely (if ``hideWhenRestricted`` is true).
``data.hideWhenRestricted`` | Boolean | Whether to hide this content entirely if the user doesn't have permission to see this item (whether by purchasing it, or by being a course instructor, etc.)
``data.enableAccessForInstructors`` | Boolean | Whether to grant permission to the instructors of certain courses to access this item. The list of courses is stored in the 'accessCourseInstructors' relationship. The instructors of this course always have access to this content, regardless of the value of this flag or any other restrictions placed on this content.

### Children

Name | URL | Description | Type
---- | ------------- | ----------- | ----
activityRoot | ``{{ROOT}}/apps/{{KEY}}_activities/records/{{record-id}}`` | The top-level activity section for this course | [``AppRecord``](#record-apprecord) of type [Activity](#activities)
coursesAnnouncements | ``{{RECORD}}/coursesAnnouncements`` | Announcements made on this course | [``AppRecordList``](#collection-types) with [Course Announcement](#course-announcements) objects
coursesDownloads | ``{{RECORD}}/coursesDownloads`` | Downloads made availble on this course | [``AppRecordList``](#collection-types) with [Course Download](#course-downloads) objects
coursesGrading | ``{{RECORD}}/coursesGrading`` | Grading scale for this course | [``AppRecordList``](#collection-types) with [Course Grade](#course-grading) objects
coursesInstructors | ``{{RECORD}}/coursesInstructors`` | Instructors for this course | [``AppRecordList``](#collection-types) with [Course Instructor](#course-instructors) objects. The first item in the set is considered the "lead" instructor.
coursesPrerequisites | ``{{RECORD}}/coursesPrerequisites`` | Prerequisite lists for this course | [``AppRecordList``](#collection-types) with [Course Prerequisite](#course-prerequisites) objects.
coursesContributors | ``{{RECORD}}/coursesContributors`` | Contributors and their titles for this course | [``AppRecordList``](#collection-types) with [Contributor](#contributors) objects.

### Relationships

Name | URL | Description | Item Type
---- | --- | ----------- | ---------
sponsors | ``{{RECORD}}/related/sponsors`` | Sponsors for this course | [``AppRecordList``](#collection-types) with [Sponsor](#sponsors) objects
tags | ``{{RECORD}}/related/tags`` | Tags for this course | [``AppRecordList``](#collection-types) with [Tag](#tags) objects
audio | ``{{RECORD}}/related/audio`` | Related audio files | [``AppRecordList``](#collection-types) with [Audio File](#audio-files) objects.
courses | ``{{RECORD}}/related/courses`` | Related courses | [``AppRecordList``](#collection-types) with [Course](#courses) objects.
documents | ``{{RECORD}}/related/documents`` | Related documents | [``AppRecordList``](#collection-types) with [Document](#documents) objects.
videos | ``{{RECORD}}/related/videos`` | Related videos | [``AppRecordList``](#collection-types) with [Video](#videos) objects.
accessCourseInstructors | ``{{RECORD}}/related/accessCourseInstructors`` | Courses whose instructors have access to this object | [``AppRecordList``](#collection-types) with [Course](#courses) objects.
