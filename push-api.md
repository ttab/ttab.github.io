---
layout: page
title: Push API
---

The push API allows real-time access to feeds defined on the
[tt.se][tt] web site. The API is available using either
[WebSockets (Socket.IO)][wsapi] or [HTTP long poll][lpapi].

[tt]:http://beta.tt.se
[io]:http://socket.io
[wsapi]:#websocket-api
[lpapi]:#http-long-poll-api

* TOC
{:toc}

## WebSocket API

The WebSocket API is built on [socket.io][io]. The client emits
messages with a JSON payload containing an array of request parameters
and a callback which will be called when a response is received. In
addition, updates to subscribed feeds are pushed from server to client
with the subject `update`.

The main entry point is `http://beta.tt.se/punkt`. Clients will be
redirected to a login page unless they send a valid session cookie.

Example (client code):

`emit('getfeedmeta', [{name:'mypush'}], console.log);`

`socket.on('update', console.log)`

### getfeedmeta

Returns structure that describes what is part of the feed.

* `name` - name of feed like `"mypush"`.

### getfeed

Returns feed for given name.

Request

* `name` - name of feed. `"mypush"`.
* `from` - uri to start from, all newer entries from this will be
returned.

Response

* `hits` - array of hit
* `begin` - boolean set to true if the returned hits are from the beginning.

### getold

Returns older feed items

Request

* `name` - name of feed. `"mypush"`.
* `from` - uri to start from
* `amount` - amount of articles to get

Response

* `hits` - array of hit

### subscribe

Subscribes to updates to the given feed name. This command must be
reissued every 30 seconds to keep getting updates. We do this to
ensure that the client is driving whether we are keeping cached
entries for a user. If subscribe stops, the cache can be allowed
to expire.

* `name` - name of feed. `"mypush"`

### getitem

* `uri` - the uri of the item to fetch


## HTTP Long Poll API

All operations that are part of the [WebSocket API][wsapi] also have
corresponding HTTP endpoints:

 * `http://beta.tt.se/punkt/v1/getfeedmeta`
 * `http://beta.tt.se/punkt/v1/getfeed`
 * `http://beta.tt.se/punkt/v1/getold`
 * `http://beta.tt.se/punkt/v1/subscribe`
 * `http://beta.tt.se/punkt/v1/getitem`

Request parameters remain the same, and are passed as regular query
parameters. In addition, you must also provide a
[valid API Key][apikey] through the `ak` parameter. The response body
contains a JSON payload with the same contents as with the
[WebSocket API][wsapi].

Example:

`curl "https://beta.tt.se/punkt/v1/getfeedmeta?name=mypush&ak=<YOUR KEY HERE>"`

### update

In addition to the above endpoints, the HTTP Long Poll API provides an
endpoint for receiving updates to subscribed feeds:

 * `http://beta.tt.se/punkt/v1/update`

New feed items are pushed from server to client as they arrive.

#### Example

To subscribe to feed and receive continuous updates as new items arrive.

1. Create a [delivery channel][delchan] (leveranskanal) of type
   "Push-API" with a specic feed name.
2. Create an [api key][apikey].
3. "Star" some content (bevakningar)
   and [redirect their output][bevak] to the channel.
4. ll re-subscribe every 30 seconds and update to poll for new
   content.

```
    # every 30 secs do this
    https://beta.tt.se/punkt/v1/subscribe?name=<YOUR FEED NAME>&ak=<YOUR KEY HERE>

    # this call "hangs" until there is content available
    https://beta.tt.se/punkt/v1/update?name=<YOUR FEED NAME>&ak=<YOUR KEY HERE>
```

[apikey]:api.html#application-keys
[delchan]:https://beta.tt.se/mina-sidor/kanaler
[bevak]:https://beta.tt.se/bevakningar
