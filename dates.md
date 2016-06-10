---
layout: page
title: TTNinjs Dates
---

This is an attempt to document the interpretation of various date
fields in the ttninjs spec. Some fields are internal to TT and not
visible in metadata delivered to customers. Internal fields are
prefixed `_`.

### `versioncreated`

Is the time this version of the object in question was created.

* For photos this is the time of the photo.
* For news articles it is the time of release.
* For planning posts it is the time the planning post was created.

### `versionstored`

The time we added the object to our database.

* For photos where we act as a slave database, this is the time of archival in SDL.
* For news articles it is when we added it to our database.
* For planning it is when we added it to our database.

### `embargoed`

The time an object is embargoed until. Include a time.

### `date`

*Date part only* no time (see [datetime](#datetime)).

The date of an object. This is most often the same as
`versioncreated`, with notable exceptions:

* For photos this is the date the photo was taken.
* For news articles it is is the date the news story was released.
* For planning it is the *date of the planned event*.

### `datetime`

*Date and time*

The date and time of an object.

This field is exactly the same as [date](#date), but only exists when
there is a relevant time to communicate. For items where we have no
time, just a date (such as for old photography), this field is left
blank.

We want to avoid confusion about communicating 00:00 as time, when we
actually have no relevant time data.

### `enddate`

*Date part only*

Same as [date](#date) but for ending. 

### `enddatetime`

*Date and time*

Same as [datetime](#datetime) but for ending. 


### `_tstamp`

Internal field used for search result sorting. It is mapped
differently depending on type of data.

* `news` this field is set to `versioncreated` (time of news story).
* `planning` this field is set to `datetime` (time of planned event).
* `photos`:
  1. if `versioncreated` is `19700101` we believe it's unix time
     (epoch) 0 and not a correct date.
  2. if `versioncreated` is not okay:
     1. use `versionstored` if it is before last midnight. The
        midnight rule is to avoid old photos appear on first page.
     2. else `null` (we have no okay date)
 3. if `versioncreated` is okay:
    1. use `versioncreated` if it is before last midnight.
    2. else use `now`. This is so that photography for today that are
       indexed/recevied out-of-order, still will appear in the order we
       receive them.
