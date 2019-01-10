---
layout: page
title: API
navbar: 4
---

This page describes the various public APIs offered by TT Nyhetsbyrån.

* TOC
{:toc}

# Overview

| API              | Authentication  | Covers                                                     | Status     |
|------------------|-----------------|------------------------------------------------------------|------------|
| Media&nbsp;API   | OAuth2, API key | Access to metadata and digital assets. Trigger deliveries. | Active     |
| Content&nbsp;API | OAuth2          | Search, real time push, creating notifications.            | Active     |
| Deliveries       |                 | Automatic delivery of content                              | Active     |
| Search&nbsp;API  | API key         | Search                                                     | Deprecated |
| Push&nbsp;API    | API key         | Real time push                                             | Deprecated |


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


## Content API

*Content search, long poll, notifications*

The Content API replaces the old Search and Push APIs. It allows
clients to query our database for content, and listen for real time
updates (using long poll). The search API is largely compatible with
the old Search API to make migration easy.

The Content API also has some support for creating automatic mobile
and email notifications.

[Content API Documentation](https://api.tt.se/docs/)


## Deliveries

*FTP, SFTP*

Albeit deliveries may be pushing the definition of "API", the reality
is that several integrations use the Media API to on-demand trigger a
delivery, or simply have a continuous delivery of article metadata.

We use delivery channels to configure both formats and method of
delivery.

[Configure deliveries using kundwebb](https://app.tt.se/mina-sidor/kanaler)


## Search API (deprecated)

*HTTP searches*

To query our database for keywords and metadata, the search API
provides a request-response HTTP method. This is typically driven from
a "search" input field.

### No polling
{:.no_toc}

This API is not suitable for polling integrations. I.e. where the same
query is repeatedly run to discover new material. For this use, see
the Push API below.

[Search API Documentation](search-api.html)


## Push API (deprecated)

*Socket.IO, Long poll*

The push API is for supporting more "modern" push methods than
traditional upload protocols (like FTP). Significant for these methods
is a need to support somewhat non-dependable transports where a *list*
of latest x number of articles can be presented.

[Push API Documentation](push-api.html)

# Authentication

## OAuth2

The Media and Content APIs support OAuth2 bearer
authentication. Clients wishing to use the API need to contact TT
Nythetsbyrån to obtain a `client_id` and `client_secret`, which they
can use to generate access tokens, either using the regular
Authorization Code Grant Flow (for web apps), Implicit Grant Flow (for
client side apps) ot Password Credentials Flow (server-side apps).

### Endpoints

| authorization endpoint | https://tt.se/o/oauth2/auth  |
| token endpoint         | https://tt.se/o/oauth2/token |
| user endpoint          | https://tt.se/o/v2/user      |


### Example: generating a token using Password Credentials Flow

```bash
curl -XPOST -d username=<username> -d password=<password> -d client_id=<client_id> -d client_secret=<client_id> -d grant_type=password -d scope="roles" https://tt.se/o/oauth2/token
```

* `username` and `password` should be the regular TT login credentials.

* `client_id` is the (non-secret) client ID issued by TT.

* `client_secret` is the client secret issued by TT.

* `scope` need to be at least `roles` for the API to grant access.


The call will return an short-lived access token, which is valid
for 7 days, and a refresh token that can be used to generate new
access tokens, like this:


```bash
curl -XPOST -d client_id=<client_id> -d client_secret=<client_secret> -d grant_type=refresh_token -d refresh_token=<refresh_token> https://tt.se/o/oauth2/token
```


## OpenID Connect

We support OpenID Connect. The configuration is [here](https://tt.se/.well-known/openid-configuration).


## Application Keys

Access to our older API:s require keys that ties the requests to
specific users. Generate api-keys [using the
kundwebb](https://app.tt.se/mina-sidor/api-nycklar).


