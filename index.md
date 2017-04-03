---
layout: page
title: Ravel&#58; A Database-Defined Network
description: "Ravel&#58; A Database-Defined Network"
---
{% include JB/setup %}


Ravel is a software-defined networking (SDN) controller that uses a standard SQL database to represent the network.  _Why a database?_ SDN fundamentally revolves around data representation--representation of the network topology and forwarding, as well as the higher-level abstractions useful to applications.

In Ravel, the entire network control infrastructure is implemented within a SQL database.  Abstractions of the network take the form of _SQL views_ expressed by SQL queries that can be instantiated and extended on the fly.  To allow multiple simultaneous abstractions to collectively drive control, Ravel automatically _orchestrates_ the abstractions to merge multiple views into a coherent forwarding behavior.

For more information, read through our [Publications]({{site.url}}/publications).

<br/>

## News ##

* Ravel video walkthrough is up: [[walkthrough]](videos/walkthrough.mp4)

#### 2017 ####
* April: Poster _Automating SDN Composition: A Database Perspective_ presented at [SOSR '17](http://conferences.sigcomm.org/sosr/2017/) [[extended abstract]](docs/sosr17extendedabstract.pdf) [[poster]](docs/sosr17poster.pdf)
* March: Short position paper _Reflections on Data Integration for SDN_ presented at [SDN-NFV Security '17](https://www.cs.clemson.edu/nss/sdnfvsec2017/) [[paper]](docs/sdnnfv17.pdf) [[slides]](docs/sdnnfv17-slides.pdf)


#### 2016 ####

* September: [Ravel v0.2](https://github.com/ravel-net/ravel/releases/tag/v0.2) released
* March: Short paper _Ravel: A Database-Defined Network_ presented at [SOSR '16](http://conferences.sigcomm.org/sosr/2016/) [[paper]](docs/sosr16.pdf) [[slides]](docs/SOSR16slide2.pdf) [[demo]](videos/sosr_demo.mp4)
* March: [Ravel v0.1](https://github.com/ravel-net/ravel/releases/tag/v0.1) released


<br/>

## Get Started ##

Download the [Ravel VM]({{site.url}}/download#option-1-pre-packaged-vm) or [install from source]({{site.url}}/download#option-2-install-from-source) from our [GitHub repository](http://github.com/ravel-net/ravel).  Then try the [Walkthrough]({{site.url}}/walkthrough).

Or develop your own applications.  Read our [Developer Guide]({{site.url}}/manual) and browse through the [API](api/annotated.html).
