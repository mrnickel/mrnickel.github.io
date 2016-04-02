---
date: 2016-03-31T22:11:10-04:00
draft: true
title: More Project Updates
---

As I write more posts I've noticed more and more glaring holes in my site design / [StaticSiteGenerator](https://github.com/mrnickel/StaticSiteGenerator) solution. A few pain points if you will. In an effort to reflect on my projects every so often I figured I'd take a few notes on my blog / StaticSiteGenerator.

## What's the goal?

I believe it's important for developers to take a step back and reflect on what their solutions original goals were. This allows us to put things into perspective. Did we veer way off course? Did our solution even solve our original problems? What do we need going forward?

My goal for the StaticSiteGenerator was two fold.

1. Play with, and learn a new programming language (Go)
2. Get a basic solution up that would allow me to periodically write blog posts

The initial MVP (minimum viable product) certainly solved these 2 problems. I'm a big fan of releasing MVP's early and often, as the benefits (which are evident even with this small project), are that you find out what is actually required earlier than if you were to spend more time on the initial implementation, trying to solve each and every problem. The first implentation allowed me to start generating content and establishing a (small) social presence. But, as is also so often the case with MVP's, I was able to learn some things quickly, like my site's poor design.

### Bad design

First off, the design just wasn't working for me. The initial visit would show a user a giant wall of text. A little intimidating. What I decided to do instead was display only the first paragraph of each post. This was a pretty easy implementation from the Go side, however it revealed another hole in my site.

### Bad writing style

What I realized was that my posts didn't have a proper introduction. No real hook. This is just bad. Who's going to continue reading a post when they don't really know what it's about? Luckily I didn't have a lot of posts that needed to be addressed haha. As I was modifying my posts I realized another feature that should be implemented. The ability to preview posts.

### Can't easily preview posts

This was more of an annoyance during the writing process than anything else. It's always nice to see how the markdown will be generated into the template. To that end I implemented a `preview` function. What I wanted was to issue a `preview` command, specifying which post, then automatically launch my web browser to the page. This lead me to the final hole: the inability to launch the website locally.

### Built in webserver

In order for me to preview the post, I would have to start a local webserver. When I first started, I didn't want to go through all the trouble of setting up apache, so instead I would fire up the [built-in PHP webserver](http://php.net/manual/en/features.commandline.webserver.php). `php -S localhost:8080` from within the root directory, and visit http://localhost:8080/index.html. This was another minor annoyance, and I'm ultimately curious about writing webapps and microservices in Go. Like everyone else I used the standard libraries [net/http](https://golang.org/pkg/net/http/) package to serve up the site. It was amazingly easy to implement and I've had no issues with it thus far.

## Did I veer off course?

Because I'm a major proponent for realeasing MVP's I didn't have the opportunity to veer off course. Not that I wasn't tempted to. If you look at my list of [todos](https://github.com/mrnickel/StaticSiteGenerator) you'll see that I have a number of features that I want to implement. Each of which would be fun to implement. "Well, this is only a hoby project", I would tell myself "what would it hurt to spend some time implementing a spell checker?". And honestly, it wouldn't harm anything. I don't make any money from this site, and it would further my learning in Go (my firt project goal). However I believe that like anything else, practice makes perfect. And forcing yourself to adhere to various processes is also important to practice.

## Did my solution solve my original problem?

The problem that I wanted to solve was to learn Go. I had spent a lot of time reading other peoples blogs posts (which is what intrigued me so much about this language), but no real first hand usage. This gave me the opportunity to learn a bit about the language. Problem one solved.

My second problem that I needed to solve was to have a platform that would allow me to generate posts in order to establish a small social presence. Admitidely this could have been easily done by using a solution like [Hugo](http://gohugo.io). I could simply `brew update && brew install hugo`, and be off to the races. However, this would go counter to my first goal. As this is really a secondary goal I found it to be a fair tradeoff.

## What do I need going forward?

At this point I'm not too sure what I'll need. I have a few ideas of course, as is evidenced in my todo list. However which ones are most important? That I'm still not sure of. I won't really know until the project matures a bit. I'll need more content, more visitors, more learnings in order to determine which feature is the most important. Is there any way for me to speed up my learning? Absolutely there is. I could formally promote the project. Get more developers and writers using it. That would give me more diverse feedback about the project. My main goal with this project however is learning Go, and to that end I'll likely choose to implement a feature that would be interesting to solve.

## Conclusion

Overall I'm satisfied with where I've gotten with these projects. I've learned a little bit of Go, and I've got a website that proves it. The StaticSiteGenerator is evolving organically and is working quite well for my needs, although I still wouldn't recommend anyone else use it as it's not nearly up to par with the other solutions on the market. I've also started gathering some followers across the various social webs. I'll continue to write about the projects as the mature further.