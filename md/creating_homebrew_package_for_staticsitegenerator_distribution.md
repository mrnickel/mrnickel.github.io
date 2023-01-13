---
date: 2023-01-13T11:57:29-05:00
draft: true
title: Creating Homebrew Package For StaticSiteGenerator Distribution
---

In a previous post playing with [GitHub Actions](./on_adding_github_actions_to_my_staticsitegenerator.html) I thought it might be interseting to figure out how to distribute the binary via Homebrew.

While reading through the [Acceptable Formulae](https://docs.brew.sh/Acceptable-Formulae) documentation, my dreams of being accepted into the Homebrew-core packages were dashed. (end sarcasm)

The primary rule that would be broken by this software are:

> ## Niche (or self-submitted) stuff

Namely:

> be maintained (i.e. the last release wasn’t ages ago, it works without patching on all Homebrew-supported OS versions and has no outstanding, unpatched security vulnerabilities)

The software _has_ had an update in the past few weeks (at time of this writing), however previous to that was 4 years ago. Not exactly actively maintained.

> be known

Nobody knows this software exists except for myself

> be used

As far as I know, I'm the only user of it

> have a homepage

... not yet, however perhaps a quick github self hosted page that's essentially the readme file.

The next question to answer is how can I self host this formula? Seeing as I highly doubt Homebrew would accept my PR (and for good reason. Please continue your processes).

In Homebrew parlance, what I want to create is a [tap](https://docs.brew.sh/Taps). By default this relies on GitHub, which is perfect for this series of events.

I'm going to partially re-hash the official Homebrew documentation here for my own convenience:

1. Create a github repo **/mrnickel/homebrew-mrnickel**
    This is the convention that Homebrew uses in order to tap properly.
1. Create the **Go** formula using:
     `brew create https://github.com/mrnickel/StaticSiteGenerator/archive/refs/tags/{RELEASE_VERSION_HERE}.tar.gz --go`
	This will create a formula definition file that will be committed to the **mrnickel/homebrew-mrnickel** repo.
1. Edit the information as appropriate in the definition. Setting the **desc, homepage, url**
1. Copy the file from `$(brew --repository)/Library/Taps/homebrew/homebrew-core/Formula` directory,  to the **homebrew-mrnickel** git directory.
1. Commit and push to the **mrnickel/homebrew-mrnickel** repository
1. Run `brew tap mrnickel/mrnickel`
1. Run `brew install StaticSiteGenerator`

## Conclusion

After some trial and error I came up with a suitable [homebrew formula](https://github.com/mrnickel/homebrew-mrnickel/blob/main/staticsitegenerator.rb).

I've used Homebrew for many many years now. I've never fully understood how it works, just that it works. It was interesting to dive into it a bit deeper. They've done an excellent job with their documentation, and it's a well thought out package manager. I suppose that's why it's essentially the default package manager for MacOS.

I spent far too much time googling for ways to create the formula file, and I should have instead have just relied on the `brew help` and `brew create help` documentation as it's well represented. If only more software took as much pride in their docs as Homebrew does.

One thing I wish they would add would be a way to specify the path of the generated .rb file.

Now that I've set up this homebrew tap, I can add this to some machine provisioning scripts I've been meaning to write. Perhaps an article for another day. That along with some dotfiles that I should persist.