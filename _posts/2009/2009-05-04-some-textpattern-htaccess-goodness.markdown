---
title: "Some TextPattern .htaccess Goodness"
layout: post
category: development
redirect_from:
  - /2009/05/04/some-textpattern-htaccess-goodness/
---

One of my key things for giving a site the best SEO I can is to reduce the number of URLs that point to the same content.

I've made a note of the little `.htaccess` niceties I've used in the setting up sites.

## No-www

My personal preference is to remove www from URLs, which is a simple rule seen all over the internet.

{% highlight apache %}
RewriteCond %{HTTP_HOST} ^www\\.(.*) [NC]
RewriteRule ^(.*)[\\/]?$ http://%1/$1 [R=301,NC,L]
{% endhighlight %}

If you prefer, have the 'www' I'm no fascist, it's more of a tidiness thing. But pick one.

## Removing the trailing slash

When using a CMS such as textpattern, that uses URL rewriting, you can often get the same url with a trailing slash, I've found a way to remove that. Note the two lines that single out the `mint` (analytics) and `rpc` (connection for desktop blogging software) directories, if these aren't in there the respective service will not function. This works for any directory, just copy the line and replace the name.

{% highlight apache %}
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_URI} !rpc/(.*)$
RewriteCond %{REQUEST_URI} !mint/(.*)$
RewriteCond %{REQUEST_URI} (.*)[\\/]$
RewriteRule ^(.*)[\\/]$ http://%{HTTP_HOST}/$1 [R=301,L]
{% endhighlight %}

This also means Mint and Google Analytics do not record two separate URLs, one with a trailing slash and one without.

### Standard redirects

When importing the old articles I realised there was a couple of out of date pieces so I simply removed the articles and added standard redirects to the latest information.

{% highlight apache %}
Redirect 301 /writing/setting-up-a-ruby-on-rails-development-mac http://andycroll.com/writing/the-big-reinstall-setting-up-rails-on-leopard
Redirect 301 /writing/setup-a-ruby-on-rails-development-mac-on-os-x-leopard http://andycroll.com/writing/the-big-reinstall-setting-up-rails-on-leopard
{% endhighlight %}