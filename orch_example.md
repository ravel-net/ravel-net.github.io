---
layout: page
title: "Orchestration Example"
description: ""
---
{% include JB/setup %}

- start ravel with a toy topology

	> the toy topology (instance) is designed to accompany the application configuration (firewall and pga rules) for interesting orchestration scenario

{% highlight bash %}
cli-ravel$ sudo ./ravel.py --custom /home/mininet/cli-ravel/topo/toy_dtp.py --topo mytopo
{% endhighlight %}

- load the applications and orchestration protocol
{% highlight bash %}
# list all applications
cli-ravel$ apps 

# load applications
cli-ravel$ load kf
{% endhighlight %}

and now
