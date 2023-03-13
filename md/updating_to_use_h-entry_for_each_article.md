---
date: 2023-03-13T06:49:16-04:00
draft: false
title: Updating to use H-Entry for each article
---

As I delve deeper into the [IndieWeb](https://indieweb.org/), I first took on the low hanging fruit of adding the [h-card](adding-h-ard-to-this-site.html) format to the footer.

With that low hanging fruit out of the way, I started looking into wrapping posts with the [h-entry](http://microformats.org/wiki/h-entry) classes.

The purpose to the **h-entry** card, as put forth by the [microformats.io](http://microformats.org/) site is:

> h-entry is a simple, open format for episodic or date-stamped content on the web. h-entry is often used with content intended to be syndicated, e.g. blog posts. h-entry is one of several open microformat standards suitable for embedding data in HTML.

Starting to add this made me realize that my blog post entries weren't _strictly_ accurate. I was using an `<article>` tag to contain the content of the blog post. The intended purpose as outlined in [this mdn doc is](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/article):

> The `<article>` HTML element represents a self-contained composition in a document, page, application, or site, which is intended to be independently distributable or reusable (e.g., in syndication)

My article wasn't truly _self contained_. The title portion of the article was instead in the `<header>` tag of this site. I needed to change up the markup and css for the site in order to make it adhere to the standards.

This led me down the rabbit hole of looking deeper into my markup.

What else wasn't quite idiomatic? Could I take this opportunity to clean up and minimize my HTML? Take this opportunity to perhaps learn a little more about the newer CSS properties available?

Why not!

## Index.tmpl Cleanup

Looking at the index page, I thought I could clean it up a little bit. It was already fairly clean, but I could sense some [divitis](https://csscreator.com/divitis) beginning to creep in.

```
<header>
  <div class="content">
	  <h1>Hello, my name is <span class="name">Ryan Nickel</span></h1>
	  <p>A personal blog focused on (primarily) software.</p>
  </div>
</header>
```

That extra `<div class="content">...</div>` was in there purely for styling the page. It added no semantic meaning to the markup. This is something that should be handled purely with CSS. 

This was pretty simple to do, especially considering all I needed to do was set some padding, which is so much easier now with `vw`. I can use some simple media queries to make this responsive as well. Simple.

The `<article>` tag was suffering from the same thing. One extra `<div>` in order to control the styles. The same `vw` CSS class can address this as well.

Finally, looking at the `<footer>` tag, this one changed quite a bit.

Again it had the same `<div>` issue, so I was able to easily remove that.

I then took the opportunity to properly style the various social media links to be a proper `<ul><li>...</li></ul>`. I was lazy the first time around and just wrote them out in a `<p>` tag, and modifying them was proving to be more arduous than I would prefer. Again this can all easily be styled via CSS and allow the markup to be more semantic.

```
footer ul {
  list-style-type: circle;
  display: flex;
  flex-direction: row;
  justify-content: center; 
  padding: 0;
}    

footer li {
  padding: 0px;
  margin-left: 20px;
}

footer li:first-child {
  list-style-type: none;
  margin-left: 0;
}
```

I was then able to modify the markup from

```
<p>
  <a rel="me" href="https://twitter.com/rnickel" class="url">Twitter</a> &#x26AC; <a rel="me" href="https://github.com/mrnickel" class="url">GitHub</a> &#x26AC; <a href="/about.html">About</a> &#x26AC; <a href="/rss.xml">RSS</a> &#x26AC; <a rel="me" href="https://techhub.social/@mrnickel">Mastodon</a>
</p>
```

to a much more readable

```
<ul>
	<li><a rel="me" href="https://twitter.com/rnickel" class="u-url">Twitter</a></li>
	<li><a rel="me" href="https://github.com/mrnickel" class="u-url">GitHub</a></li>
	<li><a href="/about.html">About</a></li>
	<li><a rel="me" href="https://indieweb.social/@mrnickel" class="u-url">Mastodon</a></li>
	<li><a href="/rss.xml">RSS</a></li>
</ul>   
```

## Post.tmpl Cleanup

This one was the more difficult template to clean up.

I want to ensure that the `<article>` tag was fully self-contained.

This means that my `<header>` tag needs to now be a child of the `<article>` tag.

To do this, I created a new `post.css` file that deviates somewhat from the `index.css` file. Although the overall styles are the same, the markup is different, which one could argue necessitates a new style sheet.

This allowed me to move the `<header>` tag within the `<article>` tag, thus fulfilling the requirement of having the `<article>` fully self contained. I was then able to move the actual copy of each article into `<main>` which one could also argue is more semantically correct, as it is the _main_ content of the document.

Finally I was able to add the appropriate microformat classes for the associated items:
- `p-name` to highlight the name of the article (within the header)
- `e-content`  to capture the body of the article
- `dt-published` I could add to the `<time>` element to capture the date in which the article was published
- `u-url` to allow any scraper to grab the permalink

And that was it.

## Conclusion

This was an interesting exercise for me. Getting into the nitty gritty of HTML and CSS isn't something I often do during my day-to-day. It was fun to jump into looking at the semantics and try to clean it up and toy around with CSS a little. I'm sure there are better ways to go about doing what I did, but my goal wasn't to do it perfectly but instead to just play around. That's half the fun of having your own website.