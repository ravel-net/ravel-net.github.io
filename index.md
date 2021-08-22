---
layout: page
title: Ravel&#58; A Database-Defined Network
description: "Ravel&#58; A Database-Defined Network"
---
{% include JB/setup %}


Ravel is a software-defined networking (SDN) controller that uses a standard SQL database to represent the network.  _Why a database?_ SDN fundamentally revolves around data representation--representation of the network topology and forwarding, as well as the higher-level abstractions useful to applications.

In Ravel, the entire network control infrastructure is implemented within a SQL database.  Abstractions of the network take the form of _SQL views_ expressed by SQL queries that can be instantiated and extended on the fly.  To allow multiple simultaneous abstractions to collectively drive control, Ravel automatically _orchestrates_ the abstractions to merge multiple views into a coherent forwarding behavior.

<!-- For more information, read through our [Publications]({{site.url}}/publications). -->

This project is supported by the National Science Foundation [Award CNS-1909450](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1909450&HistoricalAwards=false) and [CNS 1657285](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1657285&HistoricalAwards=false).

<br/>

## News ##

* Sarasate video: coming soon
* Ravel video walkthrough is up: [[walkthrough]](videos/walkthrough.mp4)

#### 2021 ####

* November, 2021: _Faure: A Partial Approach to Network Analysis_, [HotNets 2021](https://conferences.sigcomm.org/hotnets/2021/)
* August, 2021: Demo _Sarasate: A Strong Representation System for Networking Policies_, [SIGCOMM 2021 demo](https://conferences.sigcomm.org/sigcomm/2021/cf-posters.html) [[Extended abstract](http://anduowang.github.io/docs/sigcomm2021demo.pdf)]

#### 2020 ####

* March, 2020: _A Case of Knowledge-driven Policy Management: Bringing Discipline to Internet Routing_, [SOSR 2020](https://conferences.sigcomm.org/sosr/2020/index.html) [[poster](http://anduowang.github.io/docs/sosr20posters-paper4.pdf)]

#### 2019 ####

* June, 2019: _A Logical Approach to Representing and Reasoning About Interdomain Routing Policies_ [DATALOG 2.0 2019](https://sites.sju.edu/plw/datalog2/)
* June, 2019: _Internet Routing and Non-monotonic Reasoning_ [LPNMR 2019](https://sites.sju.edu/plw/lpnmr-2019/) [[paper](http://anduowang.github.io/docs/lpnmr19.pdf)]
* April, 2019: _Enabling Policy Innovation in Interdomain Routing: A Software-Defined Approach_ [SOSR 2019](https://conferences.sigcomm.org/sosr/2019/) [[paper](http://anduowang.github.io/docs/p94.pdf)] [[slide](http://anduowang.github.io/docs/p94-sosr19.pdf)]

#### 2018 ####

* June: [Ravel v0.2.1](https://github.com/ravel-net/ravel/releases/tag/v0.2.1) released
* April, 2018: Poster _A Semantic Approach to Modularizing SDN Software_, [NSDI 18 demo](https://www.usenix.org/conference/nsdi18/glance)
* March, 2018: _Database Criteria for Network Policy Chain_, [SDN-NFV Security 18](https://www.cs.clemson.edu/nss/sdnfvsec2018/program.html)

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
