---
layout: page
title: Media API
---

## Text

```
GET https://tt.se/media/text/151006-senap-162464              # redirect to kundwebb
GET https://tt.se/media/text/151006-senap-162464.json          # ttninjs
GET https://tt.se/media/text/151006-senap-162464.xml           # ttnewsml
GET https://tt.se/media/text/151006-senap-162464.zip           # article and images packaged up

GET https://tt.se/media/text/151006-senap-162464-cutpaste.txt  # cut-paste txt variant
GET https://tt.se/media/text/151006-senap-162464-indesign.txt  # indesign document

GET https://tt.se/media/text/151006-senap-162464/a000_NormalPreview.jpg    # association a000 normal preview
GET https://tt.se/media/text/151006-senap-162464/a000_NormalThumbnail.jpg  # association a000 normal thumb
GET https://tt.se/media/text/151006-senap-162464/a000_CroppedThumbnail.jpg # association a000 cropped thumb
Image
```

## Image

```
GET https://tt.se/media/image/sdltaa20b4b                            # redirect to kundwebb
GET https://tt.se/media/image/sdltaa20b4b.json                       # ttninjs
GET https://tt.se/media/image/sdltaa20b4b.xml                        # ttnewsml
GET https://tt.se/media/image/sdltaa20b4b.zip                        # image and metadata packaged up
GET https://tt.se/media/image/sdltaa20b4b.jpg                        # hires jpg
GET https://tt.se/media/image/sdltaa20b4b_NormalHires.jpg            # hires jpg
GET https://tt.se/media/image/sdltaa20b4b_NormalPreview.jpg          # normal preview jpg
GET https://tt.se/media/image/sdltaa20b4b_NormalThumbnail.jpg        # normal thumb jpg
```

## Graphics

```
GET https://tt.se/media/graphic/gf177556                             # redirect to kundwebb
GET https://tt.se/media/graphic/gf177556.json                        # ttninjs
GET https://tt.se/media/graphic/gf177556.xml                         # ttnewsml
GET https://tt.se/media/graphic/gf177556.zip                         # graphic and metadata packaged up
GET https://tt.se/media/graphic/gf177556.pdf                         # hires color pdf
GET https://tt.se/media/graphic/gf177556_BlackAndWhiteHires.pdf      # hires b/w pdf
GET https://tt.se/media/graphic/gf177556.eps                         # hires color eps (if available)
GET https://tt.se/media/graphic/gf177556_BlackAndWhite.eps           # hires b/w eps (if available)
GET https://tt.se/media/graphic/gf177556_NormalPreview.jpg           # normal preview jpg
GET https://tt.se/media/graphic/gf177556_NormalThumbnail.jpg         # normal thumb jpg
GET https://tt.se/media/graphic/gf177556_CroppedThumbnail.jpg        # cropped thumb jpg
```

## Video

```
GET https://tt.se/media/video/sdltaa1f67a                            # redirect to kundwebb
GET https://tt.se/media/video/sdltaa1f67a.json                       # ttninjs
GET https://tt.se/media/video/sdltaa1f67a.xml                        # ttnewsml
GET https://tt.se/media/video/sdltaa1f67a_NormalThumbnail.jpg        # normal thumb jpg
GET https://tt.se/media/video/sdltaa1f67a_NormalPreview.mp4          # normal preview mp4
```

## Hires media

You need a
[TT subscription](https://support.tt.se/hc/sv/categories/200182508-Kontakta-oss)
agreement for hires media.

## Triggering an automatic delivery

1. Figure out the subscription agreement ID. Lookup your available
agreements with the [search API](search-api.html#lookup-your-productsagreements).
2. Generate an
   [api-key using the kundwebb](https://app.tt.se/mina-sidor/api-nycklar).
3. Create a
   [delivery channel using the kundwebb](https://app.tt.se/mina-sidor/kanaler). Notice
   the channel ID.

To deliver 151006-senap-162464 using a specific channel (which determins both formats and delivery method) do:

```
GET https://tt.se/media/text/151008-senap-162464?agr=40768&channel=user:ttit:ftp8ak=443bfbb6-cbc7-482c-b143-6dee6dc4ae8e
```

#### The parameters explained

```
?channel=<channelId>    # Use delivery channel. download channels still downloads.
```

```
?agr=<agreementId>      # When using a delivery channel, provide subscription agreement id to automatically "purchase" the item.
```

```
?ak=<apiKey>            # Use this api key to authenticate. With this, no login is needed.
```


## Zipped packages

When downloading a zip, we use a download channel to select the contents of the .zip

```
GET https://tt.se/media/text/151006-senap-162464.zip?channel=user:ttit:skrivbord  # channel deides zip contents
```
