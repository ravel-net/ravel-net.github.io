---
layout: page
title: "Ravel Developer Documentation"
description: ""
---
{% include JB/setup %}

* TOC
{:toc}


-------------------------

### Ravel Overview
Ravel allows multiple applications to execute simultaneously and collectively drive network control.  This is accomplished through _orchestration_.  A Ravel user sets priorities on the orchestrated applications using the ordering passed to the `orch` application.  (For more details, see the orchestration demo in the [Walkthrough]({{site.url}}walkthrough#part-4-orchestration).)

Applications can _propose_ updates (through an insertion, deletion, or update to one of its views), which are then checked against all other orchestrated applications' policies.  If the proposed update violates the constraints of another application, a higher priority application can overwrite the update.

#### Ravel Base Tables

Ravel uses a flat representation of the network and exposes the topology and forwarding tables as tables.  Tables that may be of interest to applications are described below.

    # topology
    tp(sid integer, nid integer, ishost integer, isactive integer, bw integer)

    # hosts
    hosts (hid integer, ip varchar, mac varchar, name varchar)

    # switches
    switches (sid integer, dpid varchar, ip varchar, mac varchar, name varchar)

    # nodes (hosts and switches)
    nodes (id integer, name varchar)

    # configuration (forwarding) table
    cf (fid integer, pid integer, sid integer, nid integer)

    # traffic (reachability) matrix
    tm (fid integer, src integer, dst integer, vol integer, FW integer, LB integer)

    

### Developing Applications
Applications in Ravel can consist of two components: an implementation in SQL and a sub-shell implemented in Python.  Application sub-shells provide commands to monitor and control the application from the Ravel CLI.  Name the SQL file `[appname].sql` and the Python file `[appname].py`, and place them in the `apps/` directory.  The CLI will search for SQL and Python files in this directory.

For an overview of the CLI and application superclasses, see the [API](api/annotated.html).


#### SQL Component
An application's SQL component can create tables, views on Ravel base tables, 
or add triggers to Ravel base tables.  To interact with orchestration, an application must define its constraints and a protocol for reconciling conflicts.  To add constraints, create a violation table in the form `appname_violation`.  To create a repair protocol, add a rule in the form `appname_repair`.

For example, suppose we want to implement a bandwidth monitor that limits flows to a particular rate (see `apps/merlin.sql` for the full implementation).  We can create a table to store the rate for each flow:

{% highlight sql %}
CREATE TABLE bw_policy (
    fid      integer,
    rate     integer,
    PRIMARY KEY(fid)
);
{% endhighlight %}

And then a violation table that finds flows that violate the constraint `fid < rate`:

{% highlight sql %}
CREATE VIEW bw_violation AS (
    SELECT tm.fid, rate AS req, vol AS asgn
	FROM tm, bw_policy
	WHERE tm.fid = bw_policy.fid AND rate > vol
);
{% endhighlight %}

If the view contains more than zero rows, a conflict has occurred.  To repair a conflict, we can reset the flow rate:

{% highlight sql %}
CREATE RULE bw_repair AS
    ON DELETE TO bw_violation
    DO INSTEAD
        UPDATE tm SET vol=OLD.req WHERE fid=OLD.fid;

{% endhighlight %}

#### Python Sub-Shell
To interact with an application, the Ravel CLI can load a shell with commands for monitoring and controlling the application's behavior.  This is an application _sub-shell_ launched from the Ravel CLI by typing the application's name or shortcut after it has been loaded.  To create a sub-shell, create a Python file and extend the class [AppConsole](api/classravel_1_1app_1_1AppConsole.html) and add the following variables:

* `shortcut`: defines a shortcut for the application from the Ravel CLI
* `description`: a short description of the application
* `console`: defines the class inheriting [AppConsole](api/classravel_1_1app_1_1AppConsole.html)

For example:

{% highlight python %}
from ravel.app import AppConsole

class MyConsole(AppConsole):
        do_echo(self, line):
                "Echo input"
                print "MyConsole says:", line

shortcut = "my"
description = "my demo console"
console = MyConsole

{% endhighlight %}

The [AppConsole](api/classravel_1_1app_1_1AppConsole.html) class contains the properties:

* `self.db`: a reference to [ravel.db.RavelDb](api/classravel_1_1db_1_1RavelDb.html)
* `self.env`: a reference to [ravel.env.Environment](api/classravel_1_1env_1_1Environment.html), the CLI's executing environment
* `self.components`: a list of [ravel.app.AppComponent](api/classravel_1_1app_1_1AppComponent.html), the application's SQL components (ie, tables, views, rules)

For more information about the application superclasses and their properties, see the [API](api/annotated.html).
