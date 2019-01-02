---
date: 2019-01-02T14:41:54-08:00
draft: false
title: Migrating from 1Password to KeePassXC
---

I'm a big proponent of password managers, and I've been a heavy user of 1Password for a long time now. Several years ago, I purchased it for Mac, Windows, iOS and Android.

Fast forward to today and they've updated to a subscription model, and they host the passwords online.

I completely understand their need to continue to generate revenue from existing customers in order to improve the product and as a software developer myself I sympathize with them. However I don't want to add yet another monthly subscription to my wallet.

On a security note, I'm not so inclined to host my passwords on a 3rd party platform. I feel it's too big of a honey pot. I prefer to keep my important files offline. (I also don't support services like Dropbox for anything important). Especially seeing as services like LastPass [have already been breached](https://www.pcworld.com/article/2936621/the-lastpass-security-breach-what-you-need-to-know-do-and-watch-out-for.html). 

While my current version of 1Password continues to work just fine across my devices, they're not actively improving the software, so I've decided to try to find an open source alternative which hopefully I can help contribute to.

After some research, I stumbled across [KeePassXC](https://keepassxc.org/) as recommended by [EFF](https://ssd.eff.org/en/module/how-use-keepassxc) and thought I'd give it a try.

The first hurdle I wanted to jump was getting my 1Password content into KeePassXC.

To do that:

1. Open 1Password, log in, and select the vault (if on Mac).
1. File > Export > All Items. Enter your password.
1. Change the File Format to **CSV**. Leave everything else the same and press **Save**
1. Open KeePassXC
1. Select **Import from CSV** and select the file you exported from 1Password
1. Create a password to unlock and select **OK**
1. In the top section, select **Consider '\' an escape character**
1. Set the following columns:
    
    Group: **Not present**

    Title: **Column 3**

    Username: **Column 6**

    Password: **Column 2**

    URL: **Column 5**

    Notes: **Column 1**

    Last Modified: **Not present**

    Created: **Not present**

1. Press **OK**

If your 1Password is like mine (littered with temporary passwords), you're definitely going to want to go through and clean up a bunch of entries after the import.

I'm going to try to use this as my main password manager for the time being, but one shortcoming I've come across so far is that it's difficult to enter credit card information for safe keeping.

KeePass has a template plugin system that can handle this, so hopefully KeePassXC can implement something similar.

Another good article worth mentioning to better acquaint you with the ins and outs of the software is posted by [Sam Schlinkert](http://samschlinkert.com/) [Getting Started With KeePassXC](https://sts10.github.io/2017/06/27/keepassxc-setup-guide.html)