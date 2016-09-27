---
layout: page
title: body_html5 spec 1.2
---

This is a "loose" spec for the body_html5 part of [ttninjs][ttninjs].

Revision history
----------------

* 1.4 <footer> and <h5> for changes as of 2016-10-03
* 1.3 Clarify html-doc structure
* 1.2 alt-attribute in `<img>`, `div.byline` before `figcaption` (to be html5-valid)
* 1.1 Multiple `<article>`
* 1.0 Initial revision

Image hosting
-------------

TT is currently not set up to deliver the thumbnails or previews to
end users. The src/preview of `<img>` tags should be downloaded
locally and served off suitable platforms.

Entire HTML-documents as XML
----------------------------

`body_html` contains an entire HTML(5)-document including
`<html>`, `<head>` and `<body>`. It's an XML-polyglot so
whilst it adhere to HTML5, it is parseable using a strict
XML-parser (`img` tags are closed, etc).

```html
<html>
  <head></head>
  <body>
    <article>...</article>
    <article>...</article>
    ...
</html>
```

The intention is that the contents of `body_html5` can be
saved to a file and viewed without styling and also picked
apart using an XSL-transform or a
[jQuery alike tool](https://github.com/algesten/zu).

Multiple article
----------------

Some body_html5 contain multiple `<article>` elements. This is
currently the case for "notispaket" and similar products. Such
articles are not related (more than grouped under say "bostad"),
i.e. they have different subjects. The structure of each such article
follows the below example.

body_html5
----------

```html
<!-- body_html5 can contain one or many <article> elements -->
<article>

  <!-- An article can have multiple occurences of <section> with
       article text.
       data-charcount is the number of characters in this section
       excluding <figurecaption>. -->
  <section data-charcount="2377">

    <!-- Each <section> starts with a title.-->
    <h1>Elephant baby stray on the road, cause minor chaos</h1>

    <!-- Minor data commonly found at the beginning of articles. -->
    <div class="dat">
      <span class="vignette">Elephant baby</span>
      <span class="source">TT</span>
    </div>

    <!-- <h4> is a preamble which can have internal <p>. -->
    <h4>
      <p>A baby elephant strayed on to a busy intersection in the wee
      morning hours.</p>
      <p>Motorists slowed down which caused minor delays.</p>
    </h4>

    <!-- The main body text. -->
    <div class="bodytext">
      <p>Our correspondent in Mysore reports that a baby elephant
        strayed on to the road in the wee morning hours. As motorists
        slowed down, a minor traffic chaos ensued.</p>

      <!-- Body text can have internal <h2> headers -->
      <h2>No one hurt</h2>
      <p>No one got injured in the incident as local authorities
        quickly came to help the poor animal back into the safety of
        the woods.</p>

      <!-- Quotes are marked with <blockquote> -->
      <blockquote>The elephant was causing a scene being out of
        control. I'm glad it's back in the woods.</blockquote>

      <p>Some bystanders commented that the calf was "causing a
      scene".</p>

      <!-- h5 is used for direct questions, typically alternated with
           blockquote -->
      <h5>Are you afraid of elephants?</h5>
      <blockquote>A baby this size is not much to fear, but a
          lone tusker is another story.</blockquote>

    </div>

    <!-- Byline for this text part. -->
    <div class="byline">Martin/TT</div>

    <!-- There can be multiple <figure> elements -->
    <figure>
      <!-- The src is always a square cropped thumbnail, which is
           accessible without logging in.
        data-assoc-ref   is a key in the assocations collection
                         in ttninjs.
        data-preview-src is a url to an uncropped preview which
                         requires login to reach. -->
      <img src="<cropped thumbnail path>"
           data-assoc-ref="a000"
           alt="Baby elephant causing minor chaos"
           data-preview-src="<preview path>"/>

      <!-- Image byline -->
      <div class="byline">Karl/TT</div>

      <!-- Image caption -->
      <figcaption>Baby elephant causing minor chaos</figcaption>

    </figure>

    <!-- Specifically for material that has a broadcast time, such
    as articles about TV-programs. That information is in
    a footer.
    <footer class="broadcastinfo">
      <p>Channel 5, 20/3 at 08:00</p>
    </footer>

  </section>

  <!-- A section containing one or several notable quotes. -->
  <section class="quotes" data-charcount="87" >
    <!-- Optional headline for the quotes. -->
    <h1>Important quotes</h1>
    <blockquote>I got really late for work. It was
    horrid. (Motorist)</blockquote>
  </section>

  <!-- Facts & figures related to the article -->
  <aside class="facts" data-charcount="95">
    <!-- Optional headline for the facts. -->
    <h1>Elephant incidents</h1>
    <p>Elephant traffic incidents 2014: 311</p>
    <p>Geographic region: Mostly south india</p>
  </aside>

  <!-- A loosely related side note to the article -->
  <aside class="notes" data-charcount="142">
    <!-- Optional headline for the notes. -->
    <h1>Good to know</h1>
    <p>Chances of seeing elephants next to the road are quite high in
      south india.</p>
    <p>Never stop your vehicle or lower your windows. Elephants can be
    dangerous.</p>
  </aside>

</article>
```

[ttninjs]: https://raw.githubusercontent.com/ttab/ttspec/master/ttninjs-schema_1.0.json
