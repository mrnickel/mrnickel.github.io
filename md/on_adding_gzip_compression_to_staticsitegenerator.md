---
date: 2023-01-04T10:42:53-05:00
draft: false
title: On Adding GZip Compression To StaticSiteGenerator
---

In an attempt to be a Good Web Citizen, I thought I'd try to add some GZip compression to the responses served up by the [StaticSiteGenerator](https://github.com/mrnickel/StaticSiteGenerator).

What kind of performance increases will I get from this? Likely very little as my site is incredibly basic.

Who's asking for this update? Absolutely no one.

Why am I doing it? Curiosity.

## Question 1: Performance Increase?

I'm assuming there will be little to none. The site is essentially 99% text based. The other 1% would be the few images served up.

- Non GZipped: 169kb transfered
- GZipped: 54.8kb transferred

Roughly a third less transferred to the client. I guess my assumption above was incorrect!

## Question 2: How Complicated Would This Be?

Go is a very well thought out language purpose built for creating web applications. My assumption here is that it would be very straight forward.

As I'm by no means an effective Go programmer, and my experience is very limited with the language, I did what any developer would do: Google it.

I found a few examples that relied on 3rd party packages to solve the problem, but as I always try to limit my reliance on others packages, I continued until I found something that relied solely on the [Go stdlib](https://pkg.go.dev/std). I found [this example](https://gist.github.com/bryfry/09a650eb8aac0fb76c24) by [bryfry](https://github.com/bryfry), plugged it in and it worked instantly.

# Conclusion

This was a fun little experiment. It was refreshing to dip my toes back into Go again, and dust off the knowledge a bit.

Go's middleware design pattern is incredibly effective allowing for an elegant plug-n-play solution.

I was suitably impressed with the performance gains of adding just a few lines of code. A third of the file size is enough to ensure that I definitely use this on all my other projects.

I also always find it satisfying to work towards being a Good Web Citizen. No sense in adding more unnecessary bandwidth to the internet.