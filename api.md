---
layout: page
title: API
navbar: 1
---

## Application Keys

Access to our API:s require keys that ties the requests to specific
users. Generate api-keys
[using the kundwebb](https://beta.tt.se/mina-sidor/api-nycklar).

# Overview

Our API:s are divided into four parts, two request-response, two server-push.

![API Segments](https://cloud.githubusercontent.com/assets/227204/18877858/edefba5e-84ce-11e6-9514-197e33154686.png)


# Request-Response

Request response style API:s require an HTTP request to either fetch
data or trigger some behaviour.


## Media API

*HTTP downloads, Trigger deliveries*

The media API is for downloading content such as metadata, images,
articles and also zips with bundled metadata and articles. Such
bundles are defined using delivery channels.

A channel specifies both formats and method of delivery. Such
deliveries can be either download or any push delivery such as
FTP. Hence the media API both for downloading and on-demand triggering
deliveries.

[Media API Documentation](media-api.html)

## Search API

*HTTP searches*

To query our database for keywords and metadata, the search API
provides a request-response HTTP method. This is typically driven from
a "search" input field.

#### No polling

This API is not suitable for polling integrations. I.e. where the same
query is repeatedly run to discover new material. For this use, see
the Push API below.

[Search API Documentation](search-api.html)



# Server Push

API:s that pushes our data out as it becomes available.

## Deliveries

*FTP, SFTP, S3, HTTP(S)*

Albeit deliveries may be pushing the definition of "API", the reality
is that several integrations use the Media API to on-demand trigger a
delivery, or simply have a continuous delivery of article metadata.

We use delivery channels to configure both formats and method of
delivery.

[Configure deliveries using kundwebb](https://beta.tt.se/mina-sidor/kanaler)

## Push API

*Socket.IO, Long poll*

The push API is for supporting more "modern" push methods than
traditional upload protocols (like FTP). Significant for these methods
is a need to support somewhat non-dependable transports where a *list*
of latest x number of articles can be presented.

*Details coming*
