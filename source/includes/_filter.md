# Filter Expressions

Any collection supports being filtered: returning a subset of the results based on some input parameters.

Our filtering language is based on a heavily restricted JavaScript syntax. 

## Syntax

Allowed operators, functioning the same as those in JavaScript, and having the same precedence rules:
* ``V1 && V2``
* ``V1 || V2``
* ``V1 ? V2 : V3``
* ``!V1``
* ``~V1``
* ``+V1``
* ``-V1``
* ``V1 > V2``
* ``V1 < V2``
* ``V1 >= V2``
* ``V1 <= V2``
* ``V1 == V2``
* ``V1 === V2``
* ``V1 != V2``
* ``V1 !== V2``
* ``V1 + V2``
* ``V1 - V2``
* ``V1 * V2``
* ``V1 / V2``
* ``V1 % V2``
* ``V1 | V2``
* ``V1 ^ V2``
* ``V1 & V2``

Parenthesized expressions are allowed.

Objects and Arrays cannot be specified in the syntax allowed. They will be parsed, but if in a part of the expression that is actually evaluated, will result
in an error and the filter expression returning false (excluding any objects that triggered it), so:

``true || [ 1, 2, 3 ]``

will not trigger an error since the left side of the expression returns successfully, the right side is never evaluated.

(This is an implementation detail: we use (Esprima)[http://esprima.org] for parsing the expressions, which is a fully-featured JavaScript parser; the returned
syntax tree is evaluated to handle each expression.)

Allowed constructs from Esprima:

* ``ExpressionStatement``: this is the top-level statement parsed
* ``ConditionalExpression``: the ternary operator ``A ? B : C``
* ``LogicalExpression``: the ``&&`` and ``||`` operators
* ``UnaryExpression``: the ``!``, ``~``, unary ``+``, and unary ``-`` operators
* ``BinaryExpression``: the ``>``, ``>=``, ``<``, ``<=``, ``==``, ``===``, ``!=``, ``!==``, binary ``+``, binary ``-``, ``*``, ``/``, ``%``, ``|``, ``^``, and ``&`` operators.
* ``ArrowFunctionExpression``: Arrow functions, used for sub-queries: ``arg => Expression``
* ``CallExpression``: Function calls: ``Resource($.data.meta.createdBy)``, ``$.data.meta.createdBy.is($ => $.data.active)``
* ``Literal``: A JavaScript literal value: ``"String"``, ``0``, ``true``, ``null``
* ``MemberExpression``: Property lookups: ``$.foo``, ``a[0]``, ``b["c"]``
* ``Identifier``: Property labels: ``foo``, ``$``

## Arrow Functions

```javascript
$ => $.data.meta.active
obj => obj.data.meta.active
```

In the expression engine, an arrow function is used for when you need to check the value of some other object for it's properties for filtering purposes.

## Functions

### ``Object Resource(String | ResourcePointer)``

```javascript
Resource("https://example.digitalxe.com/api/1/apps/swt_addressBook/records/12345")
Resource($.data.meta.createdBy)
```

Fetch the specified resource. This is the only function available that is not called on some object property.

### ``Date Value.date()``

```javascript
$.data.meta.created.date()
"2015-12-01".date()
```

Given a value, try converting it into a date for comparison purposes. As a date, it can be used in boolean
comparisons to check for before / after, etc. Returns null if the value is not a date.

The value being converted to a date should be in a form accepted by JavaScript's ``Date`` object.

### ``String Value.toUpperCase()``

```javascript
$.data.data.title.toUpperCase()
"my string".toUpperCase()
```

Given a value, try turning it into an upper case string. Returns the empty string if the value cannot
be converted to a string.

This uses the JavaScript rules for converting a value to upper case.

### ``String Value.toLowerCase()``

```javascript
$.data.data.title.toLowerCase()
"MY STRING".toLowerCase()
```

Given a value, try turning it into an lower case string. Returns the empty string if the value cannot
be converted to a string.

This uses the JavaScript rules for converting a value to lower case.

### ``Boolean Value.startsWith(String)``

```javascript
$.data.data.title.startsWith("Demo")
```

Given a value, check to see if it begins with the specified string, returning true if it does.

### ``Boolean Value.endsWith(String)``

```javascript
$.data.data.title.endsWith("Demo")
```

Given a value, check to see if it ends with the specified string, returning true if it does.

### ``Boolean Value.contains(String)``

```javascript
$.data.data.title.contains("Demo")
```

Given a value, check to see if it contains the specified string, returning true if it does.

### ``String Value.substring(Number start [, Number end])``

```javascript
$.data.data.title.substring(3)
$.data.data.title.substring(0, 6)
```

Returns the portion of the string between ``start`` and ``end``.

This functions identically to the (JavaScript ``substring``)[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/substring] method.

### ``Boolean Object.is(Value)``

```javascript
$.data.meta.createdBy.is(Resource("https://example.digitalxe.com/api/1/apps/swt_addressBook/records/12345"))
$.data.meta.createdBy.is(Resource("https://example.digitalxe.com/api/1/apps/lms_accounts/records/12345"))
```

Checks to see if the specified objects are equal. This differs from a normal equality check in that, if the objects are resources, it checks to see if the resources
are the same instead of just the objects.

In the case above, the resources returned by the call to (``Resource``)[#object-resource-string-resourcepointer] both point at the same underlying object (``swt_addressBook``
and ``lms_accounts`` are both different views of people that share their underlying resources), so the equality check here returns true.

### ``Boolean Object.is(ArrowFunction)``

```javascript
$.children.instructor.is($ => $.data["First Name"] == "Demo")
```

Checks to see if the object matches the provided (Arrow Function)[#arrow-function].

If the objects in the array are (Resource Pointers)[#resource-pointer], then the argument to the arrow function will be the resource pointed at, unless there was
an error resolving the resource, in which case the original resource pointer will be passed.

### ``Boolean Array.has(Value | ArrowFunction)``

```javascript
$.children.courses.has(Resource("https://example.digitalxe.com/api/1/apps/lms_courses/records/12345"))
$.$type.has("AppRecord")
$.children.courses.has($ => $.data.active == true)
```

Loops over the array and checks to see if the provided argument matches in the same method as (``Object.is(Value)``)[#boolean-object-is-value] or (``Object.is(ArrowFunction)``)[#boolean-object-is-arrowfunction] would check.

