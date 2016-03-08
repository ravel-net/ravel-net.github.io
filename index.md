---
layout: page
title: Ravel&#58; A Database-Defined Network
tagline: Supporting tagline
---
{% include JB/setup %}

---  

Ravel is a software-defined networking (SDN) controller that uses a standard SQL database to represent the network.  _Why a database?_ SDN fundamentally revolves around data representation--representation of the network topology and forwarding, as well as the higher-level abstractions useful to applications.

In Ravel, the entire network control infrastructure is implemented within a SQL database.  Abstractions of the network take the form of _SQL views_ expressed by SQL queries that can be instantiated and extended on the fly.  To allow multiple simultaneous abstractions to collectively drive control, Ravel automatically _orchestrates_ the abstractions to merge multiple views into a coherent forwarding behavior.

For more information, read through our [Publications]({{site.url}}publications).


## Get Started ##

Download the [Ravel VM]({{site.url}}download#option-1-pre-packaged-vm) or [install from source]({{site.url}}download#option-2-install-from-source) from our GitHub repo (<span style="color:red">TODO:</span> Repo link).  Then try the [Walkthrough]({{site.url}}walkthrough).


## Contribute ##

Develop your own applications!  Read our [developer guide]({{site.url}}develop) or browse through the [API](api/annotated.html).
