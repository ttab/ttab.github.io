---
layout: page
title: Revisions section in ttninjs
---

## Motivation

TT publishes news stories using what internally is referred to as
"takes". A take is a part of a news story, as it develops, and is in
itself not a complete article for web publication or print. Eventually
a "full story" will follow that replaces all individual "bits".

A full news story from TT is often a result of a chain of
replacements. Each replacement can replace one or many of the previous
broadcasts. Each have their own unique ID (no version increment).

Takes and news stories from TT are replaced using new IDs, not
versioned.

## Pointers forward-backward

The simplest way of following replacement chains are using `replacing`
and `replacedby`. The first field, `replacing` is an array of `uri`
this document replaces. Replaced by is a single `uri` string of the
document that has replaced this document.

## Revision history

Additionally there is a `revisions` section that, for each document,
holds the entire revision history for all things up until the current
document (not updated in future replacements).

### First line is first version

A new chain of documents also holds itself in the `revisions` section.

```coffee
uri: "http://tt.se/media/text/1"
slug: "usa-val"
...
revisions: [

  # first version also has an entry in the history
  {uri:"http://tt.se/media/text/1", slug:"usa-val"}

]
```

### Each replacement is added

For each new version, one row in the array is added refering to the
previous.

```coffee
uri: "http://tt.se/media/text/2"
slug: "usa-valUV1"
replacing: ["http://tt.se/media/text/1"]
...
"revisions": [

  # previous version entry
  {uri:"http://tt.se/media/text/1", slug:"usa-val"}

  # new version entry
  {uri:"http://tt.se/media/text/2", slug:"usa-valetUV1",
   replacing:["http://tt.se/media/text/1"]}

]
```

### Merged documents copies all revisions

If we replace two (or more) documents, each with their own revision
chains, we copy all revision history together, in order.


#### First document

```coffee
uri: "http://tt.se/media/text/2"
slug: "usa-valUV1"
replacing: ["http://tt.se/media/text/1"]
...
"revisions": [

  {uri:"http://tt.se/media/text/1", slug:"usa-val"}
  {uri:"http://tt.se/media/text/2", slug:"usa-valetUV1",
   replacing:["http://tt.se/media/text/1"]}

]
```

#### Second document

```coffee
uri: "http://tt.se/media/text/12"
slug: "usa-clinton-UV1"
replacing: ["http://tt.se/media/text/11"]
...
"revisions": [

  {uri:"http://tt.se/media/text/11", slug:"usa-clinton"}
  {uri:"http://tt.se/media/text/12", slug:"usa-clintonUV1",
   replacing:["http://tt.se/media/text/11"]}

]
```

#### Merged document

The order in `replacing` is preserved in how previous revision history
is copied.

```coffee
uri: "http://tt.se/media/text/42"
slug: "usa-altogether"
replacing: ["http://tt.se/media/text/2", "http://tt.se/media/text/12"]
...
"revisions": [

  # Entire first document revisions
  {uri:"http://tt.se/media/text/1", slug:"usa-val"}
  {uri:"http://tt.se/media/text/2", slug:"usa-valetUV1",
   replacing:["http://tt.se/media/text/1"]}

  # Entire second document revisions
  {uri:"http://tt.se/media/text/11", slug:"usa-clinton"}
  {uri:"http://tt.se/media/text/12", slug:"usa-clintonUV1",
   replacing:["http://tt.se/media/text/11"]}

  # New entry
  {uri:"http://tt.se/media/text/42", slug:"usa-altogether",
   replacing:["http://tt.se/media/text/2", "http://tt.se/media/text/12"]}

]
```
