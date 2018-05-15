---
date: 2018-05-15T09:53:49-04:00
draft: true
title: Docker and Charles
---

Ah, [Charles](https://www.charlesproxy.com/). Every mobile developers' best friend. When logging fails and you just can't figure it out, it's time to go deep into the system and see the raw data.

This is a post I've been meaning to write for a long time.

It's my go-to tool when I'm debugging network calls in my Swift code.

My preferred development process is to run services locally via `docker-compose up`. This ensures I do not step on anyone's toes (amongst other benefits on which I'll write later)

You may have noticed, however, that starting up Charles causes your docker container to shut down! Oh no! That's a dev to do?!

Why does this happen? I wish I could tell you that I knew enough about the inner workings of Docker to give you an idea as to why this happens, however, I do not.

I do however have a solution that works for me. Simply go into your docker menu > proxies > enable **Manual Proxy configuration**. I leave all other fields blank (default values). Now Apply & Restart, start your Docker containers and all is well with the world!

Hope this helps someone (likely mostly myself) in the future.