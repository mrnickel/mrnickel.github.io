---
date: 2023-01-16T21:34:45-05:00
draft: false
title: Adding H-Card To This Site, and Scope Creep
---

I was recently inspired by [@tbeseda@indieweb.social](https://indieweb.social/@tbeseda) to add some [h-card](http://microformats.org/wiki/h-card) formatting to my site.

The requirement was to simply add the appropriate classes to the footer of the site. Simple. But as so often happens with software, the scope crept.

## Creep 1: Research a New Proxy Service

I needed to send the page through a parser, but do so without publishing it to the server.

In the past I had used [ngrok](https://ngrok.com/), however now it seems like it's clamped down and requires an account in order to run it. I don't have an ngrok account, and didn't really feel like signing up for yet another account. Not my cup-o-tea.

I then looked at [localtunnel](https://github.com/localtunnel/localtunnel) which looked promising. A simple `npx` command to assign the running port and you're off to the races. However, this service has a confirmation page (with an ad to support their service) which requires a user to confirm when accessing. This won't work for a scraper such as [https://microformats.io/](https://microformats.io/).

I then found [localhost.run](https://localhost.run/) to serve up the site running on my local machine, then parsed it through [https://microformats.io/](https://microformats.io/), which leverages the existing SSH tools already found on my machine. This was _exactly_ what I was looking for. Brilliant usage of SSH to provide me what I was looking for.

## Creep 2: Update The SataticSiteGenerator to Rebuild All Pages

This was when I realized a deficiency in my [StaticSiteGenerator](https://github.com/mrnickel/StaticSiteGenerator): The inability to regenerate each page with the updated template.

This was a fun little exercise in revisiting some of my older code. The last git commit for this project was on Oct 31, 2018. Four years of dust sitting on this particular codebase. And four years since I've even looked at any Go code.

I could have left the older statically generated HTML pages as the older format. A way to see how the site progresses over the years, however I can't stomach inconsistencies, so I needed to write the appropriate procedure to clean everything up. Besides, that's what git is all about right? Preserving histories.

## Creep 3: Remove Bootstrap 3 Dependency

When adding the bio photo to the footer, it threw off the styles of the site and required fixing css.

It's currently 2 full point versions behind the Bootstrap framework, which would likely mean almost an entire re-write anyway. So I'm going to take the opportunity to make the site even lighter and use simple CSS. It's a simple site anyway. No sense in adding extra load to download CSS that I only use 1% of. (post on this to forthcoming)

## Creep 4: Add More Microformats

Further inspired by [The IndieWeb](https://indieweb.org/), there are a few more simple microformats that fit in nicely. Namely [h-feed](http://microformats.org/wiki/h-feed), and [h-entry](http://microformats.org/wiki/h-entry).

## Conclusion

It always seems to happen that a seeming innocuous change snowballs into more and more feature requests. In this case the desire to simply add a basic h-card implementation to the footer turned into updating a dependency, updating some deprecated dependencies (in this case removing the dependency entirely)

It's always interesting to see how software takes on a life of it's own as it's used more and more.