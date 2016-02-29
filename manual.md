---
layout: page
title: "Manual"
description: ""
---
{% include JB/setup %}

* TOC
{:toc}

## start the CLI ##

{% highlight bash %}
sudo ./ravel.py
{% endhighlight %}

The parameters for this script are:

{% highlight bash %}
--remote          start mininet with a remote controller
--db=DB           specify a database name (default: mininet)
--user=USER   specify the database username (default: mininet)
--password       if not using trust authentication, prompt for the database password
--custom=DIR   for mininet: specify a file containing a custom topology
--topo=TOPO    for mininet: specify a mininet topology
{% endhighlight %}

for example:

{% highlight bash %}
sudo ./ravel.py --topo single,3
{% endhighlight %}
