---
layout: page
title: Degrees of Separation
navbar: 1
---

# UNDER REVISION, NOT FINALIZED!

## Summary

This document describes a (TT specific) conceptual system of how
closely material is related. The given exampel is how a news text
relates to a news image.

* Level 1 - *embedded*. The images are embedded in the text; they are
  delivered together. Only happens as a consequence of an editor
  actively associating the two.
* Level 2 - *linked*. The text links to an image. Not delivered
  together, however an editor has actively associated the two.
* Level 3 - *by job*. The text is implicitly refering to an image. Not
  delivered together. Happens when the text and the image share a job
  id. I.e. TT editorial process means the two have been created as
  part of the same news event.
* Level 4 - *meta*. The text is implicitly refering to an image by
  means of sharing keywords or other metadata pertaining to the same
  news event. Not delivered together. Such links can be created by
  editors manually grouping material.



## Level 1 - embedded

The images are embedded in the text; they are delivered together. Only
happens as a consequence of an editor actively associating the two.

In ttninjs under `body_html5` we find the images as `<figure>` in the
text, maybe in situ, maybe last.

```html
<figure>
    <img src="https://beta.tt.se/media/text/161208-larslerin-380203/a001_CroppedThumbnail.jpg"
         alt="I första programmet diskuterar Lars Lerin föräldraskap med skådespelaren Helena Bergström. Pressbild."
         data-assoc-ref="a001"
         data-uri="http://tt.se/media/image/8DADBB3B24C74988970E98F8A070E5B8"
         data-preview-src="https://beta.tt.se/media/text/161208-larslerin-380203/a001_NormalPreview.jpg"
     />
    <div class="byline">SVT</div>
    <figcaption>I första programmet diskuterar Lars Lerin föräldraskap med skådespelaren Helena Bergström. Pressbild.</figcaption>
</figure>
```

Under ttninjs `associations` we find:

* an image with a `uri` of the original photo.
* a `representationtype` of `incomplete` since we do not have the full
  original photo medata.
* `renditions` section with `href` back to where we can obtain the
  image file.

```json
{
  "uri": "http://tt.se/media/text/161208-larslerin-380203",
  "type": "text",
  "mimetype": "text/html",
  "representationtype": "complete",
  "associations": {
    "a001": {
      "byline": "SVT",
      "description_text": "I första programmet diskuterar Lars Lerin föräldraskap med skådespelaren Helena Bergström. Pressbild.",
      "mimetype": "image/jpeg",
      "representationtype": "incomplete",
      "type": "picture",
      "uri": "http://tt.se/media/image/8DADBB3B24C74988970E98F8A070E5B8",
      "renditions": {
        "r00": {
          "sizeinbytes": 78979,
          "mimetype": "image/jpeg",
          "href": "https://beta.tt.se/media/text/161208-larslerin-380203/a001_CroppedThumbnail.jpg",
          ...
```

## Level 2 - linked

The text links to an image. Not delivered together, however an editor
has actively associated the two.

In ttninjs under `associations`:

* a `representationtype` set to `associated`.
* no renditions.

N.B. In contrast to level 1, this level is not part of `body_html5`.

```json
{
  "uri": "http://tt.se/media/text/161208-larslerin-380203",
  "type": "text",
  "mimetype": "text/html",
  "representationtype": "complete",
  "associations": {
    "a001": {
      "byline": "SVT",
      "description_text": "I första programmet diskuterar Lars Lerin föräldraskap med skådespelaren Helena Bergström. Pressbild.",
      "mimetype": "image/jpeg",
      "representationtype": "associated",
      "type": "picture",
      "uri": "http://tt.se/media/image/8DADBB3B24C74988970E98F8A070E5B8",
    }
  }
  ...
```

## Level 3 - by job

The text is implicitly refering to an image. Not delivered
together. Happens when the text and the image share a job id. I.e. TT
editorial process means the two have been created as part of the same
news event.

In ttninjs we find no explicit mention of the image (or text). The
only link is a shared `job` field value.

The text:

```json
{
  "uri": "http://tt.se/media/text/161208-larslerin-380203",
  "type": "text",
  "mimetype": "text/html",
  "representationtype": "complete",
  "job": "209796",
  ...
```

The image:

```json
{
  "uri": "http://tt.se/media/image/8DADBB3B24C74988970E98F8A070E5B8",
  "type": "picture",
  "mimetype": "image/jpeg",
  "representationtype": "complete",
  "job": "209796",
  ...
```

## Level 4 - meta

The text is implicitly refering to an image by means of sharing
keywords or other metadata pertaining to the same news event. Not
delivered together. Such links can be created by editors manually
grouping material.

In ttninjs we find no explicit mention of the image (or text). The
only link would be to match up metadata such as a combination of a
date and specific keywords. Below example `versioncreated` of
`2016-12-08` and `person` value `Lars Lerin`.


The text:

```json
{
  "uri": "http://tt.se/media/text/161208-larslerin-380203",
  "type": "text",
  "mimetype": "text/html",
  "representationtype": "complete",
  "versioncreated": "2016-12-08T13:02:13+01:00",
  "person": [
    {
      "name": "Lars Lerin",
      "scheme": "http://tt.se/spec/person/1.0"
    },
  ...
```

The image:

```json
{
  "uri": "http://tt.se/media/image/8DADBB3B24C74988970E98F8A070E5B8",
  "type": "picture",
  "mimetype": "image/jpeg",
  "representationtype": "complete",
  "versioncreated": "2016-12-08T10:42:11+01:00",
  "person": [
    {
      "name": "Lars Lerin",
      "scheme": "http://tt.se/spec/person/1.0"
    },
  ...
```
