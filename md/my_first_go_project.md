+++
date = 2016-02-18T08:46:24-05:00
draft = false
title = My first go project
+++



It's no secret that I'm currently enamoured with [Go](https://golang.org), as well as the projects it powers. ([Docker](https://www.docker.com) I'm looking at you). As such I recently started my first project with the lovely "new" language.

I thought I would get my feet wet by creating yet another [static site generator](https://github.com/mrnickel/StaticSiteGenerator). The very generator I use to build this site (because dogfooding is important!).

This post is primarily about getting myself acquainted with Go and writing my first little Go app.

Now, I come from a world of Java and (modern) PHP. A world where namespaces and packages abound - a world of classes. So as you can imagine, this transition made the leap to Go a little bit of a challenge. Don’t get me wrong, Go does have these attributes as well, but that's not what it stresses.

Instead, Go emphasizes simplicity.

As a proponent of simplicity, I decided to dive in head first!

I read over the documentation and got to work.

My [first version](https://github.com/mrnickel/StaticSiteGenerator/tree/0dca8507eed53acd1ca1bd8e92139678f79f7441) of the app utilized four different packages (not including main):

- [constants](https://github.com/mrnickel/StaticSiteGenerator/tree/0dca8507eed53acd1ca1bd8e92139678f79f7441/constants)
- [post](https://github.com/mrnickel/StaticSiteGenerator/tree/0dca8507eed53acd1ca1bd8e92139678f79f7441/post)
- [publish](https://github.com/mrnickel/StaticSiteGenerator/tree/0dca8507eed53acd1ca1bd8e92139678f79f7441/publish)
- [stats](https://github.com/mrnickel/StaticSiteGenerator/tree/0dca8507eed53acd1ca1bd8e92139678f79f7441/stats)

It worked, but that wasn't my primary goal. The ultimate goal was to educate myself with Go - to learn the right way to do things. From my past experience, in order to know how to do something correctly, you have to do a few wrongs things first.

Now that the project was functional, it was time to take a step back and review.

I sought feedback from the [golang subreddit](https://www.reddit.com/r/golang), and a few people were kind enough to give me some pointers.

## My first mistake: Too Many Packages

It became apparent that my head was very much in the Java world. "Namespace and package all the things", my brain thought.

As outlined by Andrew Gerrand in Google's official Go blog [Organizing Go Code](http://blog.golang.org/organizing-go-code), this is a big no-no:

>On the other hand, it is also easy to go overboard in splitting your code into small packages, in which case you will likely become bogged down in interface design, rather than just getting the job done.

This was me to a T. I had spent far too much time trying to "design" the interfaces than getting the job done (the job being to learn idiomatic Go).

My main concern was that I would pollute my other packages with private functions that had no business being there. However upon further analysis, I found this would be true with any sort of application.

The solution isn't packages, it's properly writing code (duh!)

## My second mistake: Too Complicated
With too many packages comes increased dependencies and therefore a web of complexities. This is counter to what Go stresses (simplicity!).
Peter Bourgon's post about [Go: Best Practices for Production Enviornments](http://peter.bourgon.org/go-in-production/), is a great read. I especially liked his section on [Repository Structure](http://peter.bourgon.org/go-in-production/#repository-structure):

> Our best practice is to keep things simple.

That really struck a chord with me.

This line even more so:

> Don’t create structure until you demonstrably need it.

"If it's good enough for SoundCloud", I thought, "it's good enough for me!" *1. So off I went, on a quest to simplify my application. My end result now has no packages (aside from main), and 3 files. It really doesn't get more straightforward than that.

It wasn't as easy as simply removing packages and tossing everything inside of the root directory. I had my reservations! Primarily, my need to keep the number of lines of code in a file as low as logically possible.

I typically try to keep the number of lines in a file as small as I can. To me, files with a large number of lines in it is a code smell. Obviously you should be able to refactor that, no? Well in Go that isn't necessarily the case. Go seems to lean towards writing code that revolves around adding functionality to structs; this rule isn't quite as set in stone.

The Go sources [burst](https://golang.org/src/net/http/request.go) [with](https://golang.org/src/net/http/server.go) [files](https://golang.org/src/net/http/transfer.go) [having](https://golang.org/src/net/http/transport.go) [large](https://golang.org/src/time/format.go) [lines of code](https://golang.org/src/time/time.go). It's definitely going to be some time before I get a feel for things, but I'm quite comfortable with the size of the post struct (the obvious workhorse for a project such as this).

There's only so much that one can learn in a small project, but schooling oneself is never the less the objective of this project. There's obviously much more I have left to grasp, but that's one of the best things about being a student of programming and life.

There's always more to learn; then perhaps if I’m fortunate, master.

If you have any feedback for me or want to chat further, hit me up on twitter [@rnickel](https://twitter.com/rnickel)

*1. This is a tidbit I plan on implementing in all my projects going forward, regardless of which language it's written in.
