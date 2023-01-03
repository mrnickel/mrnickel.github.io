---
date: 2023-01-02T20:26:18-05:00
draft: false
title: On Adding GitHub Actions To My StaticSiteGenerator
---

A number of years ago I started toying with [Go](https://go.dev). At the same time I was building this website you're reading right now. I wanted to join these two desires, so I thought I'd take advantage of the, new to me, static site hosting of github for my personal website.

I love learning new programming languages. I find that even if I don't use them in my day-to-day there's always a new paradigm that can be applied to whatever language I use as my daily driver.

My big takeaway from Go was to _always_ handle your errors, and to _always_ make your errors explicit. Handling errors appropriately is a difficult thing to do. Do you retry? How many times? Does it make sense to simply retry? In what context do you retry? Do you simply bubble the error up to the user and have them address the errors? How many different kinds of errors can be returned? Errors and edge cases are the Hard Part of programming, and handling these appropriately separates a Good Developer from a Bad Developer.

As an aside, when working with clients, I spend an inordinate amount of time trying to highlight and solution all of the possible edge cases. Somtimes they're edge cases that are incredibly unlikely to happen, however it's always good to highlight everything that could pontentially go wrong and how to deal with said issues.

I build the [StaticSiteGenerator](https://github.com/mrnickel/StaticSiteGenerator) as a way to play with Go, and to also tie that in with my attempt to statically host a site on [GitHub Pages](https://pages.github.com/).

I maintain enough servers daily, so the idea of maintaining another one for my personal site didn't appeal to me. Also, static site generators were all the rage, so why not jump on that band wagon?

Anyway, I built the [StaticSiteGenerator](https://github.com/mrnickel/StaticSiteGenerator), which has been meeting my needs. The application has been running for me just fine, and my last commit was in 2018. If it ain't broke don't fix it.

More recently I thought to myself, why not set up some CI/CD to automatically publish this software as a release to GitHub so I can easily pull it down to any of my machines to author a new post. No longer will I have to download the github repo, build it locally and set up aliases in my `.zprofile`. I can simply install the Go module and be on my way.

I originally thought all I'd have to do is create a github workflow to build and release it, however this is when I discovered that my app was a little outdated. Go had moved on from where I was. I had to quickly sort out their new module interfaces.

Thankfully it wasn't too arduous.

Previously I'd simply `go build` and take the generated binary and be off to the races.

Now it seems that go has these _modules_, which is Go's new way of managing Dependencies.

I added the `go.mod` file which is sort of like `npm`'s `package.json` file. Outlining all 3rd party dependencies, and I was back to building again.

## Publish Binary To GitHub Release

Setting up the GitHub Action to publish the SimpleSiteGenerator was pretty straight forwrd.

1. Add a `build.yml` file to the `.github/workflows` directory

## Build.yml

```
name: Go

on:
  push:
    branches:
      - "!*"
    tags:
      - "v*.*.*"

jobs:

  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3

    - name: Set up Go
      uses: actions/setup-go@v3
      with:
        go-version: 1.19

    - name: Run GoReleaser
      uses: goreleaser/goreleaser-action@v4
      with:
        args: release
      env:
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN}}
```

Fairly minimal. Whenever a tag is created in the format of **vX.X.X** the action will kick off, build the binary and upload it as a release.

Alright, now that we have a package that we can `go install`, I can get to running things locally.

## Install StaticSiteGenerator Locally

I want to do this in order to ensure that it will work remotely.

`go install github.com/mrnickel/StaticSiteGenerator@latest`
`echo alias ssg='StaticSiteGenerator >> ~/.zprofile`
`ssg`

This should now print out the actions of the StaticSiteGenerator.

# Conclusion

Lately I've been working primarily with TypeScript/JavaScript. Jumping back into Go is a nice breath of fresh air. It's so lightweight, doesn't require all sorts of build systems and bundlers. It **just works**.

Next up I think I'll look into adding this as a [HomeBrew](https://brew.sh/) package.