---
layout: page
title: TT NewsML spec
---

TT:s main content format is [ttninjs](https://raw.githubusercontent.com/ttab/ttspec/master/ttninjs-schema_1.0.json).
The XML variant is a [IPTC NewsML G2 2.20](https://iptc.org/standards/newsml-g2/) compliant translation of ttninjs
where each field somehow relates back to the original ttninjs.

Revision history
----------------

* 1.6 added `<bylines>`
* 1.5 added `<contentMetaExtProperty>` for `date`, `datetime`, `enddate` and `enddatetime`.
* 1.4 added `<revisions>` section.
* 1.3 fixed literal in `<contentMetaExtProperty>` and use of `<remoteContentExtProperty>`
* 1.2 `<altId>` to `<contentMetaExtProperty>`. `<groupRef>` on the same level as `<itemRef>`
* 1.1 updated with `<newsMessage>` and `<packageItem>`
* 1.0 initial revision

XML outline
-----------

All TT`<newsItem>`s are wrapped in a `<newsMessage>` envelopes – also single ones. This is because most TT produced texts have associated images, and the only way to represent that structure in NewsML G2 is by providing multiple `<newsItem>` – one for the text and one for each image.

### Finding the relevant newsItem

To navigate the xml-structure and find the main `<newsItem>`. Do the following:

1. Look in `<packageItem>` for `<groupSet>` descendant `<group>` with attribute `role="role:main"`.
2. In that group find an `<itemRef>`
3. The URI for that itemRef is in attribute `residref="[uri here]"`
4. That URI is used to match a `<newsItem guid="[match uri here]">` in the `<itemSet>`.

Pseudocode with XPATH.

```
uri = /itemSet/packageItem/groupSet/group[@role="role:main"]/itemRef/@residref
mainitem = /itemSet/newsItem[@guid=$uri]
```

#### Text with images

```xml
<newsMessage>
  <header>...</header>
  <itemSet>
    <packageItem>...[describes relationship of newsItem below]...</packageItem>
    <newsItem guid="[text uri]">...</newsItem>
    <newsItem guid="[image1 uri]">...</newsItem>
    <newsItem guid="[image2 uri]">...</newsItem>
    ...
  </itemSet>
</newsMessage>
```

#### Video, graphics, etc

For items without associated content, we still provide the `<packageItem>` since a consumer of our XML should be able to have one way of parsing the content regardless of whether it's single or multiple `<newsItem>`s.

```xml
<newsMessage>
  <header>...</header>
  <itemSet>
    <packageItem>...[provided for consistency]...</packageItem>
    <newsItem guid="[image/video/graphics uri]">...</newsItem>
  </itemSet>
</newsMessage>
```


The XML explained
-----------------

```xml
<newsMessage>
  <!--  spec at http://spec.tt.se/  -->
  <header>
    <!-- time this xml was sent/generated -->
    <sent>2015-05-19T14:36:11+02:00</sent>
    <!-- always tt.se -->
    <sender>tt.se</sender>
    <!-- always the same. signifies that all items must be considered
         together as a package. -->
    <signal qcode="nmsig:atomic"/>
  </header>
  <itemSet>

    <!-- for <newsItem> that have associated content, like news texts
         with inline/associated images, the <packageItem> conveyes the
         relationship between the main (text) <newsItem> and the
         associated (image) <newsItem> -->
    <!-- notice that the entire <packageItem> is omitted for content
         that has no associated content (like images) -->
    <!-- guid is the main items uri with a "-pack" suffix, however
         this should not be "parsed" or interpreted. -->
    <!-- xml:lang set to whatever ttninjs.language is set to and may
        be omitted. -->
    <packageItem guid="http://tt.se/media/text/150410-elephantuv2-85160-pack"
                 version="1" standard="NewsML-G2"
                 standardversion="2.20" conformance="power" xml:lang="sv">
      <!-- see <newsItem> below -->
      <catalogRef
          href="http://www.iptc.org/std/catalog/catalog.IPTC-G2-Standards_24.xml"/>
      <!-- see <newsItem> below -->
      <catalogRef href="http://tt.se/spec/catalog/catalog.tt-g2.1_0.xml"/>
      <!-- same <itemMeta> as in the main <newsItem>, see definition below. -->
      <itemMeta>
        <itemClass qcode="ninat:text"/>
        <provider qcode="nprov:TT"/>
        <versionCreated>2015-05-19T08:35:48+02:00</versionCreated>
        <pubStatus qcode="stat:usable"/>
      </itemMeta>
      <groupSet root="root">
        <group id="root" role="group:main">
          <!-- uri reference to the main <newsItem> (i.e. the text),
               which can be found as <newsItem guid="<uri>" below -->
          <itemRef residref="http://tt.se/media/text/150410-elephantuv2-85160"/>
          <!-- tells us that the whole "media" group is structurally
               part of the main text. -->
          <groupRef idref="media"/>
        </group>
        <!-- group of media beloning to the main text -->
        <group id="media" role="group:package" mode="pgrmod:bag">
          <!-- image 1. uri reference to the first associated image -->
          <itemRef residref="http://tt.se/media/image/07CB2F5B68...BED5B008E49">
            <!-- the association id found in the original
                 ttninjs.associations JSON object. -->
            <altId type="tt:associd">a000</altId>
          </itemRef>
          <!-- image 2 -->
          <itemRef residref="http://tt.se/media/image/EA5981F5BAF...D7930B23E">
            <altId type="tt:associd">a001</altId>
          </itemRef>
          ...
        </group>
      </groupSet>
    </packageItem>

    <!-- guid is always ttninjs.uri -->
    <!-- when doing news texts with associated images the first
         <newsItem> will be the text followed by more <newsItem> for
         the images. -->
    <newsItem guid="http://tt.se/media/text/150410-elephantuv2-85160"
              version="1"
              standard="NewsML-G2"
              standardversion="2.20"
              conformance="power"
              xml:lang="sv">

      <!-- catalog for ninat:, nprov:, irel:, cpnat:, drol:, etc... -->
      <catalogRef
          href="http://www.iptc.org/std/catalog/catalog.IPTC-G2-Standards_24.xml"/>
      <!-- catalogRef for tt:, ttstat:, ttext:, etc... -->
      <catalogRef href="http://tt.se/spec/catalog/catalog.tt-g2.1_0.xml"/>

      <rightsInfo>
        <copyrightHolder uri="http://tt.se/copyright">
          <!-- value is ttninjs.copyrightholder -->
          <name>TT Nyhetsbyrån</name>
        </copyrightHolder>
        <!-- value is ttninjs.copyrightnotice -->
        <copyrightNotice>
          (c) Copyright 2015 TT Nyhetsbyrån, all rights reserved
        </copyrightNotice>
      </rightsInfo>

      <itemMeta>

        <!-- ttninjs.type prefixed with ninat: unless ttninjs.type is
             'planning' in which case the qcode is 'plinat:newscoverage'
             MANDATORY
        -->
        <itemClass qcode="ninat:text"/>

        <!-- as defined http://cv.iptc.org/newscodes/newsprovider/
             MANDATORY -->
        <provider qcode="nprov:TT"/>

        <!-- value is ttninjs.versioncreated MANDATORY -->
        <versionCreated>2015-04-10T16:25:32-05:00</versionCreated>

        <!-- value is ttninjs.embargoed optional -->
        <embargoed>2015-04-12T12:00:00Z</embargoed>

        <!-- value is ttninjs.pubstatus prefixed with stat: unless we use
             the special TT states "replaced" and "commissioned" in which
             case the qcode is prefixed tt: MANDATORY for main <newsItem> -->
        <pubStatus qcode="stat:usable"/>

        <!-- value is ttninjs.description_usage optional -->
        <edNote>
          Removed some spelling mistakes and updated the images.
        </edNote>

        <!-- for each uri in ttninjs.replacing array there is a <link>
             element where the href is the uri. optional -->
        <link rel="irel:previousVersion"
              href="http://tt.se/media/text/20150410:150410-elephant-85140"/>
        <link rel="irel:previousVersion"
              href="http://tt.se/media/text/20150410:150410-elephantuv1-85142"/>

      </itemMeta>

      <contentMeta>

        <!-- value is ttninjs.urgency MANDATORY if news text -->
        <urgency>4</urgency>

        <!-- same as <versionCreated>. MANDATORY for main <newsItem> -->
        <contentCreated>2015-04-10T16:25:32-05:00</contentCreated>

        <!-- one <creator> for each in ttninjs.bylines optional -->
        <creator literal="Martin Algesten"/>

        <!-- one <contributor> for each signature in
             ttninjs.signatures optional -->
        <contributor literal="mag"/>
        <contributor literal="jln"/>

        <!-- tag is ttninjs.language. optional -->
        <language tag="sv"/>

        <!-- for each value in ttninjs.subject with uri
             http://tt.se/spec/keyword/1.0 one <keyword>. these are ad
             lib keywords. optional -->
        <keyword>elephant</keyword>
        <keyword>intersection</keyword>

        <!-- for each value in ttninjs.subject with subref uri one
             <subject>. each qcode is prefixed medtop: these are
             controlled subject categorisations. optional -->
        <subject type="cpnat:abstract" qcode="medtop:01000000"
                 typeuri="http://tt.se/spec/subref/1.0">
          <name>Konst, kultur och nöje</name>
        </subject>
        <subject type="cpnat:abstract" qcode="medtop:08000000">
          <name>Mänskligt</name>
        </subject>

        <!-- for each value in ttninjs.product one <subject> with uri
             http://tt.se/spec/product/1.0 MANDATORY -->
        <subject type="tt:product" literal="TTUTR"
                 typeuri="http://tt.se/spec/product/1.0">
          <name>Utrikes</name>
        </subject>

        <!-- for each value in ttninjs.person one <subject> with uri
             http://tt.se/spec/person/1.0 optional -->
        <subject type="cpnat:person" typeuri="http://tt.se/spec/person/1.0">
          <name>Martin Algesten</name>
        </subject>

        <!-- for each value in ttninjs.organisation one <subject> with uri
             http://tt.se/spec/organisation/1.0 optional -->
        <subject type="cpnat:organisation"
                 typeuri="http://tt.se/spec/organisation/1.0">
          <name>Acme Panda Ltd</name>
        </subject>

        <!-- for each value in ttninjs.place one <subject> with uri
             http://tt.se/spec/place/1.0 optional -->
          <!-- if we have a geo area the attribute literal is an
               identifier that can be found in <assert literal="...">
               with the geo area below. if no geo area the literal is
               omitted. -->
        <subject type="cpnat:place" typeuri="http://tt.se/spec/place/1.0"
                literal="plc_helsingfors">
          <name>Helsingfors</name>
        </subject>

        <!-- for each value in ttninjs.object one <subject> with uri
             http://tt.se/spec/object/1.0 optional -->
        <subject type="cpnat:object" typeuri="http://tt.se/spec/object/1.0">
          <name>Nytorgsfontänen</name>
        </subject>

        <!-- for each value in ttninjs.event one <subject> with uri
             http://tt.se/spec/event/1.0 optional -->
        <subject type="cpnat:event" typeuri="http://tt.se/spec/event/1.0">
          <name>Påsk</name>
        </subject>

        <!-- value is ttninjs.slug optional -->
        <slugline>elephantUV2</slugline>

        <!-- value is ttninjs.headline. MANDATORY -->
        <headline>Elephant baby stray on the road, cause minor chaos</headline>

        <!-- value is ttninjs.byline. MANDATORY for type picture -->
        <creditline>Martin Algesten / TT</creditline>

        <!-- value is ttninjs.description_text optional -->
        <description role="drol:caption">A baby elephant strayed on to a
        busy intersection in the wee morning hours.</description>

        <!-- value is ttninjs.profile MANDATORY for main <newsItem> -->
        <contentMetaExtProperty type="ttext:profile" literal="PUBL"/>

        <!-- value is ttninjs.job optional -->
        <contentMetaExtProperty type="ttext:job" literal="150410"/>

        <!-- value is ttninjs.webprio optional -->
        <contentMetaExtProperty type="ttext:webprio" literal="4"/>

        <!-- value is ttninjs.commissioncode optional -->
        <contentMetaExtProperty
        type="ttext:commissioncode" literal="4412"/>

        <!-- value is ttninjs.commissionedby optional -->
        <contentMetaExtProperty
        type="ttext:commissionedby" literal="5123"/>

        <!-- value is ttninjs.charcount optional -->
        <contentMetaExtProperty
        type="ttext:charcount" literal="741"/>

        <!-- value is ttninjs.originaltransmissionreference optional -->
        <contentMetaExtProperty
        type="ttext:originaltransmissionreference" literal="85160"/>

        <!-- value is ttninjs.week optional -->
        <contentMetaExtProperty type="ttext:week" literal="21"/>

        <!-- value is ttninjs.date optional. See http://spec.tt.se/dates.html -->
        <contentMetaExtProperty type="ttext:date" literal="2015-12-28"/>

        <!-- value is ttninjs.datetime optional. See http://spec.tt.se/dates.html  -->
        <contentMetaExtProperty type="ttext:datetime" literal="2015-12-28T15:56:12+02:00"/>

        <!-- value is ttninjs.enddate optional. See http://spec.tt.se/dates.html  -->
        <contentMetaExtProperty type="ttext:enddate" literal="2015-12-29"/>

        <!-- value is ttninjs.enddatetime optional. See http://spec.tt.se/dates.html  -->
        <contentMetaExtProperty type="ttext:enddatetime" literal="2015-12-29T15:56:15+02:00"/>

        <!-- revision history of document. see http://spec.tt.se/revisions.html -->
        <revisions xmlns="http://tt.se/spec/newsml">
          <revisionItem reluri="http://tt.se/media/text/150410-elephant-85140">
            <slug>elephant</slug>
          </revisionItem>
          <revisionItem reluri="http://tt.se/media/text/150410-elephantuv-85142">
            <slug>elephantUV</slug>
          </revisionItem>
          <revisionItem reluri="http://tt.se/media/text/150410-elephantuv2-85160">
            <slug>elephantUV2</slug>
            <replacing reluri="http://tt.se/media/text/150410-elephant-85140"/>
            <replacing reluri="http://tt.se/media/text/150410-elephantuv-85142"/>
          </revisionItem>
        </revisions>

        <!-- bylines for document. follows the structure of ttninjs. -->
        <bylines xmlns="http://tt.se/spec/newsml">
            <bylineItem>
                <byline>Martin Algesten/TT</byline>
                <firstname>Martin</firstname>
                <lastname>Algesten</lastname>
                <role>Photographer</role>
                <email>martin.algesten@acme.com</email>
                <jobtitle>Editor in chief</jobtitle>
                <phone>+46555123456</phone>
                <initials>mag</initials>
                <affiliation>TT</affiliation>
            </bylineItem>
        </bylines>

      </contentMeta>

      <!-- When a <subject type="cpnat:place"> above has an associated
           geo area, the actual details for that geo area is found
           here. The attribute literal="" ties the two together. -->
      <assert literal="plc_helsingfors">
        <type qcode="cpnat:geoArea"/>
        <name>Helsingfors</name>
        <geoAreaDetails>
          <position latitude="60.1708" longitude="24.9375"/>
        </geoAreaDetails>
      </assert>

      <contentSet>

        <inlineXML contenttype="text/html">
          <!-- value is ttninjs.body_html5. content is not xhtml but
               rather html5 ensured to be xml compliant.
               See http://tt.se/spec/body_html5 -->
          <html>
            <head>
              <title>Elephant baby stray on the road, cause minor chaos</title>
            </head>
            <body>
              ...
              <!-- each image in the text is wrapped in <figure> -->
              <figure>
                <!-- src="" points to a square cropped thumbnail
                     suitable for overviews.  notice that access to
                     the image requires a tt.se login, and TT is
                     (currently) not hosting these images publicly on
                     behalf of our clients. -->
                <!-- data-assoc-ref is a reference to the
                     association-key in the ttninjs.associations
                     (JSON) object. this key can correlated to an
                     image in the <packageItem> part -->
                <!-- data-uri is the image uri which can be found in
                     the guid attribute of the corresponding image
                     <newsItem guid=""> -->
                <!-- data-preview-src is a link to a larger preview of
                     the image.  again this is behind a tt.se login
                     and is not hosted publicly. -->
                <img src="https://tt.se/.../6207/a002_CroppedThumbnail.jpg"
                     data-assoc-ref="a002"
                     data-uri="http://tt.se/media/image/6699D...4D30EA158"
                     data-preview-src="https://.../6207/a002_NormalPreview.jpg"/>
                <!-- image caption to go underneath the image -->
                <figcaption>Bild 3: Sara Zetterström, bloggare.</figcaption>
                <!-- photographer byline. to be displayed in relation
                     to the image -->
                <div class="byline">Martin Algesten/TT</div>
              </figure>
              ...
            </body>
          </html>
        </inlineXML>

        <!-- <remoteContent> is only used for alternative renderings
             of the same content, like in various renditions of
             images/video/graphics. -->

        <!--
            @rendition is set to ttninjs/rendition/usage
            Hires = rnd:highRes
            Preview = rnd:preview
            Thumbnail = rnd:thumbnail -->
        <!--
            @href is ttninjs/rendition/href -->
        <!--
            @size is ttninjs/rendition/sizeinbytes -->
        <!--
            @contenttype is ttninjs/rendition/mimetype -->
        <!--
            @height/width is corresponding ttninjs height/width in
            pixels -->
        <remoteContent rendition="rnd:highRes"
                       href="https://app.tt.se/media/.../a000_NormalHires.jpg"
                       size="65789"
                       contenttype="image/jpeg"
                       width="640" height="427">
          <!-- ttrend:variant is the value of
               ttninjs/rendition/variant specified here
               https://github.com/ttab/ttspec/blob/master/variantusage.js#L19
               -->
          <remoteContentExtProperty
          type="ttrend:variant" literal="Normal"/>
          <!-- ttrend:usage is the intended usage of the rendition,
               similar concept to rnd:highRes, rnd:preview. possible
               values are specified here:
               https://github.com/ttab/ttspec/blob/master/variantusage.js#L20
               -->
          <remoteContentExtProperty
          type="ttrend:usage" literal="Preview"/>
        </remoteContent>


        <!-- Remote content element for a video -->
        <!--
            @rendition is set to ttninjs/rendition/usage
            Hidef = ttrend:hiDef
            Hires = ttrend:hiRes
            Preview = ttrend:preview
            Thumbnail = ttrend:thumbnail -->
        <!--
            @href is ttninjs/rendition/href -->
        <!--
            @size is ttninjs/rendition/sizeinbytes -->
        <!--
            @contenttype is ttninjs/rendition/mimetype -->
        <!--
            @height/width is corresponding ttninjs height/width in pixels -->
        <!--
            @duration is is ttninjs/rendition/duration time in seconds. -->
        <!--
            @durationunit is always "timeunit:seconds" -->
        <remoteContent rendition="ttrend:rndhidef"
                       href="https://app.tt.se/media/.../a000_NormalHires.mp4"
                       size="6578941"
                       contenttype="video/mp4"
                       duration="140"
                       durationunit="timeunit:seconds"
                       width="720" height="576">
          <!-- ttrend:variant is the value of
               ttninjs/rendition/variant specified here
               https://github.com/ttab/ttspec/blob/master/variantusage.js#L19
               -->
          <remoteContentExtProperty
          type="ttrend:variant" literal="Normal"/>
          <!-- ttrend:usage is the intended usage of the rendition,
               similar concept to rnd:highRes, rnd:preview. possible
               values are specified here:
               https://github.com/ttab/ttspec/blob/master/variantusage.js#L20
               -->
          <remoteContentExtProperty
          type="ttrend:usage" literal="Preview"/>
        </remoteContent>

      </contentSet>
    </newsItem>
  </itemSet>
</newsMessage>
```
