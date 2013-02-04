Extensions
----------

When defining an extension, take advantage of the fields of the records
which are partially or completely free for private use.

Custom fields can be used to store any type of value, including objects
and arrays to group multiple values; to allow this, arrays in custom
fields are not treated like the start of a nested record.

The General Custom field at the start of each record can be used for properties
common to one or several ghosts; given its position, it can be shared
without being repeated, by using nested records starting with one of
the following fields.

The Specific Custom field at the end of each record on the contrary is better
used for properties specific to one single user activity, for example
complementary details for the rendering of the activity not included in the
Activity Details field.

Extensions that preserve backward compatibility may still use the values of
the Major/Minor/Patch Version of the corresponding data format. A player is
expected to ignore user activity types with unknown negative values.
Additional types of events can be recorded using an extension of the format
by assigning a different negative value to each one. In custom user activities,
both the Activity Details field and the Specific Custom field can be used to
store custom information.

When the extension introduces backward-incompatible modifications, such as
fields removed or added or with different ranges of values expected, a negative
version number shall be used instead. This version number may still be based
on semantic versioning, treating the Major/Minor/Patch versions in absolute
values for this purpose.

For example, based on the version 2,1,0 of the data format, you may define
a version -2,-2,0 that stores the width and height of the browser window in an
array in the General Custom field, and then a version -3,0,0 that breaks the
compatibility with the previous extension  by storing an object with "width"
and "height" properties instead of an array.

