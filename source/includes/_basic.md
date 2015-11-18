# Basic Types

Name | Description | Example
---- | ----------- | -------
Resource ID | The canonical URL to a resource, as a JSON String | ``"https://example.digitalxe.com/api/1/channels/888da06d62339a2a72706ef2502ae5ed"``
Timestamp | A string of the form: "``YYYY-MM-DD``T``HH:MM:SS.SSS``Z", as would be returned by ``JSON.stringify(new Date())`` | ``"2015-11-16T08:45:21.000Z"``
Boolean | The literal values ``true`` or ``false`` | ``true``
String | A JSON string whose length is at most 255 | ``"A string"``
Text | A JSON string whose length is at most 16777215 (about 16Mbytes) | ``"This is some text"``
Enumeration | A JSON string whose value is one of the values specified in the enumeration set where this is used | ``"Text"``
Array of {{Type}} | An array that contains a uniform set of values of the specified type. | ``[ 1, 2, 3 ]``
String Map | An object whose values are all strings. | ``{"Key": "Value"}``
Number | A number | ``1``

