Data Format
===========

Each ghost records the activity of a single user in a single page.
A ghost lists the sequence of user interactions, in chronological order.

After recording, a single ghost recorder is expected to be received.
For replay, several ghost records are sent together, wrapped in an extra array.

For conciseness, records are represented as nested arrays rather than objects
with named properties. Fields are ordered from the most common (the version of
the data format) to the most specific (the relative time of the recording).

To avoid repeating values within ghost records from one user activity to the
next, an intermediate array is inserted which holds the common values followed
with an array for each user activity that starts with these values. Multiple
levels of nesting are allowed.

Fields
------

  0. General Custom - any, free field for private use,
                      e.g. 0 to leave the field empty
  1. Major Version - integer, fixed to 0 in this version.
                     Positive integers are reserved for this specification
                     while negative integers are free for private use.
  2. Minor Version - integer, fixed to 2 in this version.
                     Positive integers are reserved for this specification
                     while negative integers are free for private use.
  3. Patch Version - integer, fixed to 0 in this version.
                     Positive integers are reserved for this specification
                     while negative integers are free for private use.
  4. Ancestor CSS Selector - string, CSS selector for an ancestor element of
                             recorded user activity, e.g. "#header" or "body"
  5. Parent XPath - string, XPath, relative to first element matching the above
                    Ancestor CSS Selector, for the element closest to recorded
                    user activity, e.g. "table/tbody/tr[3]/td[2]". The ancestor
                    element itself may be selected using the XPath ".".
  6. Activity Type - integer, see the list in the section below.
                     positive integers are reserved for this specification
                     while negative integers are free for private use,
                     e.g. use -1 for a custom event.
  7. Relative Top - integer, top position of user activity within parent
                    element in basis points (one hundredth of percent)
  8. Relative Left - integer, left position of user activity within parent
                     element in basis points (one hundredth of percent)
  9. Relative Time - integer, time relative to the start of the recording,
                      in milliseconds
  10. Activity Details - any, additional details on the activity which may be
                         used for more accurate playback. For example, a string
                         matching the text before the position of the cursor.
                         May be omitted when last in the array and not used.
  11. Specific Custom - any, free field for private use, may be omitted from
                        the end of the array when not used.

Activity Types
--------------

    0 - move mouse
    1 - click
    2 - scroll
    3 - resize window
    4 - type text (text input, text area)
    5 - select a value (check box, radio button, input)
    6 - focus a form control
    7 - blur a form control

Examples
--------

This example shows a single ghost record with two levels of nested arrays.

      [
        0, // private use
        0,2,0, // records using version 0.1.0 of the data format
        "#header ul.bio", // ancestor element is a list
        [
          "li[9]/p", // first paragraph in 9th list item of the list
          0, // mouse move
          [
            5000, // 50% top
            7500, // 75% left
            500 // 0.5s after start of recording
          ],
          [
            2000, // 20% top
            5000, // 50% left
            1000 // 1s after start of recording
          ]
        ],
        [
          "li[9]/p/a[2]", // second link anchor
          [
            0, // mouse move
            3000, // 30% top
            4000, // 40% left
            2000 // 2s after start of recording
          ],
          [
            1, // click
            3000, // 30% top
            4000, // 40% left
            2500 // 2.5s after start of recording
          ]
        ]
      ]

Next section
------------

[Extensions](extensions.md)
