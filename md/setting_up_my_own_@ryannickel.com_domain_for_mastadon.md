---
date: 2023-04-18T15:51:13-04:00
draft: false
title: Setting up my own @ryannickel.com domain for Mastadon
---

If you're using Mastodon or other decentralized social networks, you might be looking for ways to improve your discoverability. One technique that's gained popularity is using the webfinger protocol to create a discoverable profile for your domain.

[Scott Hanselman](https://www.hanselman.com) wrote a [blog post](https://www.hanselman.com/blog/use-your-own-user-domain-for-mastodon-discoverability-with-the-webfinger-protocol-without-hosting-a-server
) about how to set this up, and it's a pretty easy process. Basically, you just need to create a webfinger file for your domain and add it to your site.

The webfinger file is a JSON object that contains information about you or your service, like your email address or URL. You can also include links to your social media profiles or a brief description of what you're all about.

In my case, I'm running this site on GitHub Pages, and the tricky part is getting the webfinger file accessible from the well-known URL on your domain. By default, GitHub Pages doesn't serve up files or directories that start with a `.`. But fear not, my friend! You can use the `_config.yml` file to override this behavior and include the `.well-known` directory in your site.

All you need to do is add the following line to your `_config.yml` file:

(for a full list of overrides see [Jekylls Configuration Options](https://jekyllrb.com/docs/configuration/options/) documentation)

```
include: ['.well-known']
```

That tells Jekyll to include the `.well-known` directory in your site's output, which means your webfinger file should now be accessible at https://yourdomain.com/.well-known/webfinger.

Once you've got your webfinger file set up, Mastodon and other decentralized services should be able to discover information about you and your domain by performing a webfinger lookup. This can help others find and connect with you, which is always a good thing!

So there you have it, folks. Adding a webfinger file to your GitHub Pages site is an easy way to improve your discoverability on decentralized social networks. Follow the steps outlined above and start making some new connections today!