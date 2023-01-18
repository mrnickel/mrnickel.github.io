---
date: 2023-01-17T20:14:23-05:00
draft: false
title: On Removing Bootstrap Dependency
---

As part of my ongoing blog update, I'm working on removing my dependency on [Bootstrap](https://getbootstrap.com).

As a caveat, I am by no means a CSS expert. I know enough to get into trouble, and I plan on using this as an effort to better understand CSS, in specific the flex properties (which I _always_ forget).

As I was adding some [innocuous changes](adding_h-card_to_this_site,_and_scope_creep.html), it ended up breaking the layout of the site. This site was originally developed off of Bootstrap 3, and is (at the time of this writing) 2 major points behind the latest version.

While taking a quick peek at the upgrade path from Bootstrap 3 to 5, I decided it would be an interesting exercise to remove the dependency entirely.

The layout of the site is very basic and really doesn't need all of the overhead that Bootstrap brings long with it. I mean, how hard could it be?

In an effort to ensure consistent usability across all browsers, one should generally check browser compatability at [Can I Use](https://caniuse.com) for various CSS properties.

The primary CSS properties I use to manage this site's layout are:
 - [vh](https://caniuse.com/?search=vh)
 - [vw](https://caniuse.com/?search=vw)
 - [flex](https://caniuse.com/?search=flex)
 - [@media](https://caniuse.com/?search=%40media)

## VH - Vertical Height
Used to ensure that the footer of the site is always aligned at the bottom regardless of the height of the content.

If the height of the content happens to be very short, I still desire the footer to be properly aligned on the bottom.

Previously I used some CSS hacks that relied on having some known static heights set for the body's `margin-bottom` and the footer classes `height` properties.

These had to be the same. This was the major reason for my layout breaking while trying to add some h-card properties.

## VW - Width

Used to ensure that the content of the site only takes up a certain percentage of the browsers width.

## Flex

Primarily used for layout out the elements in the footer.

## @Media

Used for responsivenes. For something as simple as this blog I really have two formats: desktop and mobile.

## Discoveries

When I started building this site I was using Bootstrap with many of my other projects and it was an easy way for me to get started. I was comfortable with using it, and could put the site together quickly in order to focus on writing (for all the writing I've actually done).

Outside of the media property being supported way back since roughly 2010, all of the other properties listed above have been supported by the major browsers since roughly 2015. A full year before this blogs innogural [Hello World](hello_world.html) post. As browsers become more and more powerful, and implement better tooling, it becomes so much easier to simply roll your own CSS. 

## Typography

One of the other benefits to Bootstrap is having some sane typography defaults. Having each of the h1, h2, p tags etc be an elegant ratio to one another was something I didn't have to think about before. I went searching around the web for something that could teach me a little more about making this and came across [Type Scale](https://type-scale.com/). A tool that would allow me to play with a few toggles to achieve a ratio I found to be pleasing. Perfect!

## Responsiveness

The major reason for Bootstraps existance at the beggining was to make developers lives _much_ easier for dealing with responsiveness. This was the advent of users beginning the switch from primarily surfing the web on their computers to consuming via mobile devices. Add to that the discrepancies of various browsers and you have a recipe for unmanageable spaghetti CSS. Bootstrap allowed developers to do away with this completely and just use their CSS and move on with developing the product.

This isn't the case anymore, and rolling your own media queries is much more manageable. If I were building a more complex application or working with a team I'd still use something like Bootstrap or Tailwind in order to ensure easier onboarding and consistent language. Seeing as the goal with this update is to better understand CSS a bit more I'll see what I can do to implement this myself.

### Layout Updates

My first naieve layout had a single CSS selection for all devices. The header would take up `68vw` of the viewport, and the content would take up `45vw`. This looked acceptable on desktop. Quickly fell apart on mobile.

A quick `@media` definition solved that by setting the headers width to `90vw` and the content to `80vw`

The other item that quickly fell apart was the typography. Again it looked fine on desktop, but on mobile the header took up 90% of the visual height, not giving even a glimpse at the content.

I then used [Type Scale](https://type-scale.com) to give me some sane defaults for smaller resolutions.

That was all that was required in this realm. Thankfully this is such a basic site and basic content.

### Keep My Name Always Together

On various resolutions my name would be split across multiple lines. I wanted my name to always be on the same line regardless of the resolution. I wanted to control how and where the line break happened.

This one was as simple as adding `display: inline-block` to a `<span>` around **Ryan Nickel**

### Footer

The footer is the most complicated element in the site, which isn't really saying much as it's essentially a number of block elements aligned.

## Tools Used

- [Can I Use](https://caniuse.com/)
- [Type Scale](https://type-scale.com/)
- [Sanitize.css](https://csstools.github.io/sanitize.css/)
- [W3C Validator](https://validator.w3.org/)

## Conclusion

By removing the Bootstrap dependency I was able to greatly simplify the markup and CSS used. I was able to reduce the download size from 350kb to 177kb.

Would I recommend this route for everyone? If you're working on a passion project, then sure. However if you're working on a client product then absolutely not. Stick with something like Bootstrap or Tailwind.