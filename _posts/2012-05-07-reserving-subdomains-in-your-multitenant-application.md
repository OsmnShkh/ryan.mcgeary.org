---
layout: post
title: Reserving Subdomains in Your Multi-Tenant Web Application
subdomains: [admin, api, assets, blog, calendar, demo, developer, developers, docs, files, ftp, git, lab, mail, manage, pages, sites, ssl, staging, status, support, www]
---
It's fairly common to segment the accounts in your multi-tenant application with
subdomains. [GitHub](http://github.com) gives you a subdomain that matches your
username
(e.g. [`rmm5t.github.io`](http://rmm5t.github.io)). [Freshbooks](https://mcgearygroup.freshbooks.com/refer/www
) gives you a subdomain that matches your company name
(e.g. [`busyconf.freshbooks.com`](http://busyconf.freshbooks.com)).  At
[BusyConf](http://busyconf.com), we give you a subdomain that matches your
conference name
(e.g. [`railsconf2012.busyconf.com`](http://railsconf2012.busyconf.com)).

Your customers usually get to choose their own subdomain during account sign-up,
and when developing a subdomain-based multi-tenant application, it easy to
forget to reserve some common subdomains for your own use and future
growth. There's nothing like trying to register a new service only to realize
that one of your customers already has the subdomain that you had hoped to use.

It's a good idea to make a list of subdomains that you don't want your
customers to use. Here's a list of subdomains that I like to reserve in my
multi-tenant applications:

`{{ page.subdomains | sort | join: ', ' }}`

What subdomains are missing from this list?

In terms of an `ActiveModel` validation in Rails, that looks something like this:

{% highlight ruby %}
RESERVED_SUBDOMAINS = %w(
  {{ page.subdomains | sort | join: ' ' }}
)
validates_exclusion_of :subdomain, :in => RESERVED_SUBDOMAINS,
                       :message => "Subdomain %{value} is reserved."
{% endhighlight %}
