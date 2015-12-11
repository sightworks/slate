## External IDs

[Group](#group-appgroup) and [Record](#record-apprecord) objects have a property ``data.meta.external`` in their representation. This is meant to house an external identifier for the object in question.

The external ID can be in any format that you want to use, including objects.

An object stored as an external identifier has it's property names sorted alphabetically (take the list of property names, feed it to ``Array.sort()``), so that the object can be compared as a string if needed.

### Restrictions

An external ID must be less than 1024 bytes in length, as returned by taking the value in the ``external`` property and calling ``JSON.stringify()`` on it.

External IDs must be unique within the system (one external ID resolves to one resource in our system).

### Resolve an External ID

External IDs can be found at:

``{{ROOT}}/external?id={{id}}``

Where: ``{{id}}`` is the result of calling ``JSON.stringify()`` on the identifier.

If you want the resource associated with the ID:

``{{ROOT}}/external?id={{id}}&resource``

 