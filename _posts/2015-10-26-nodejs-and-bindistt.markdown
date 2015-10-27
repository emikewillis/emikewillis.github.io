---
layout: post
title:  "Node.js, Gentoo, and USE flags"
date:   2015-10-26 18:17:18
categories: gentoo
---

In the process of setting up this [Jekyll][jekyll]-powered blog with Github pages, I stumbled my way into a USE flag conflict of sorts. 

The Node.js ebuild (net-libs/nodejs) requires that OpenSSL (dev-libs/openssl) be installed without the bindist USE flag enabled. Additionally, OpenSSH (net-misc/openssh) requires that its bindist flag be the same as OpenSSL's. Some searching lead me to [this][forumpost2] thread on the Gentoo forums, which reveals that the generic stage3 tarball from which most Gentoo installations arise ships with the bindist flag enabled globally.

Following the lead of a post in [this other thread][forumpost1], I set my USE flags in /etc/portage/make.conf like so:

{% highlight ini %}

USE="-bindist"

{% endhighlight %}

And ran the following emerge command to rebuild everything affected by the USE flag change: 

{% highlight bash %}
emerge -auDN --with-bdeps=y @world
{% endhighlight %}

After letting around 8 packages re-install, I was able to install Node.js and begin using Jekyll.

# Sidenotes

* This issue may also be fixable by disabling bindist for OpenSSL and OpenSSH only, instead of globally. I did not try doing that, however.

* The bindist USE flag enables certain features that are patent encumbered. These features may or may not be important to you, especially in OpenSSL and OpenSSH. The 2nd and 3rd posts in the [first forum thread][forumpost2] I linked elaborate on this a bit, as well as list which OpenSSL features are affected by the USE flag.


[jekyll]: http://jekyllrb.com/
[forumpost1]: https://forums-lb.gentoo.org/viewtopic-t-1020062.html?sid=10829fdcb814a382f4374370f07d53d5
[forumpost2]: https://forums.gentoo.org/viewtopic-t-985044-highlight-bind+openssl.html
