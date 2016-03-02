---
layout: page
title: "Orchestration Example"
description: ""
---
{% include JB/setup %}

- start ravel with a toy topology

	> the toy topology (instance) is designed to accompany the application configuration (firewall and pga rules) for interesting orchestration scenario

{% highlight bash %}
sudo ./ravel.py --custom /home/mininet/cli-ravel/topo/toy_dtp.py --topo mytopo
{% endhighlight %}

- load the applications and orchestration protocol
{% highlight bash %}
# list all available applications
apps 

# load the applications
load merlin kf pga orch
# watch the application views
watch tm, cf, PGA_violation, FW_violation, FW_policy_acl
{% endhighlight %}

- scenario 1

{% highlight bash %}
addflow h1 h2
# observe violation
orch run
# route dropped
addflow h2 h1
orch run
# observe that FW_policy_acl changed, allowing inbound traffic from h1 to h2 now since an outbound traffic from h2 to h1 was initiated

addflow h1 h2
# observe cf table: PGA violation, FW no violation
orch run
# route from h1 to h2 is successful installed, tm table FW tag is set to 1 by PGA

# stop traffic from h2 to h1
delflow 2
# trigger change in FW_policy_acl, disallowing traffic to h1 to h2
orch run
# flow from h1 to h2 is dropped as well
{% endhighlight %}
