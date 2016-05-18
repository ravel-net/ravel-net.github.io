---
layout: page
title: "Walkthrough"
description: "Ravel Walkthrough"
group: navigation
---
{% include JB/setup %}

* TOC
{:toc}


-------------------------

This walkthrough will demonstrate the Ravel CLI commands.  Throughout the tutorial, we will use the following prompt syntax:

* `$` to denote Linux commands typed into the shell prompt
* `ravel>` to denote commands typed into the Ravel CLI
* `app_name>` to denote commands typed into the sub-shell of application _app_name_


-------------------------

### Part 1: Startup Options

To display the help message describing Ravel's startup options:

    $ sudo ./ravel.py --help


#### Mininet

By default, Ravel will start Mininet and load the specified Mininet topology into a PostgreSQL database.  To specify a Mininet-style topology, use the parameter `--topo=TOPO`.  The topology parameter is required.  Ravel also accepts custom topologies in a Mininet format using the parameter `--custom=CUSTOM`.  For detailed documentation on creating custom topologies, refer to the [Mininet walkthrough](http://mininet.org/walkthrough/#custom-topologies).

For example, to start Mininet in the background with single switch and three hosts:

    $ sudo ./ravel.py --topo=single,3


#### PostgreSQL
To start Ravel without Mininet (e.g., to run database scalability tests), use the `--onlydb` flag:

    $ sudo ./ravel.py --topo=single,3 --onlydb

To specify the database name and username, use the options `--db=DB` and `--user=USER`.  To force a password prompt for the database, use `--password`.  This is equivalent to connecting to Postgres with `psql --dbname=DB --username=USER --password`.  The default database name and username can be set in _ravel.cfg_.

To reconnect to an existing database state, use the `--reconnect` flag.  _Note_: this assumes the specified topology is the same as the one already loaded in the database.


#### Verbosity

The default verbosity level is `info`.  Use the `--verbosity=LEVEL` parameter to specify one of: `error`, `warning`, `info`, `debug`.

    $ sudo ./ravel.py --topo=single,3 --verbosity=debug



-------------------------

### Part 2: Ravel Commands
To list CLI commands:

    ravel> help

To exit the CLI:

    ravel> exit

To display the current configuration (i.e., database name, username, applications path):

    ravel> stat

Ravel searches for available applications placed in the _apps_ directory.  To view discovered applications and their status:

    ravel> apps


#### SQL

Ravel provides a connection the database specified in the command-line options (or _ravel.cfg_).  To execute a SQL statement on the database, prefix the statement with the command `p`:

    ravel> p SELECT * FROM hosts;

To display real-time changes to a database table in a separate xterm window, use the `watch` command.  Specify one or table names separated by spaces:

    ravel> watch cf rm

To refresh the database by truncating all tables except the topology (_note_: this will not clear switch flow tables):

    ravel> reinit


#### Mininet

Ravel also provides a connection to the Mininet instance started in the background (unless the `--onlydb` flag is used).  Prefix the Mininet command with `m`:

    ravel> m h1 ping h2

To drop into a Mininet sub-shell, type `m` with no additional parameters:

    ravel> m
    mn> h1 ping h2


#### Performance

To report execution time for a command:

    ravel> time [command]

To report detailed execution time (if the command profiles individual operations):

    ravel> profile [command]

(_Note_: the profile command only monitors operations when running Ravel with Mininet, i.e., starting Ravel without the `--only-db` parameter.)


-------------------------

### Part 3: Orchestration

In Ravel, application are loaded using the `orch` (orchestration) command.  Orchestration translates high-level application operations onto Ravel's base tables and coordinates the resulting updates for multiple applications running simultaneously.

To load one or more applications, specify a list of applications in ascending order of priority using the `load` command:

    ravel> orch load routing fw

Here, orchestration will assign `fw` the highest priority.  Priorities are used to manage conflicts in updates proposed by different applications. Updates from higher-priority applications will override updates from lower-priority ones.  _Note:_ the `load` command requires a total ordering of applications.  When running the command a second time, any applications that are not listed in the second `load` call will be unloaded automatically.

Under orchestration, each application can propose a change.  To commit a change and check for conflicts with other running applications, use `orch run`.  To automatically commit changes, use `orch auto on`.  To disable, use `orch auto off`.  For example, to propose adding a flow:

    ravel> orch load routing fw
    ravel> rt addflow h1 h2
    ravel> orch run

This is the same as:

    ravel> orch load routing fw
    ravel> orch auto on
    ravel> rt addflow h1 h2

To report execution or profiled times for flow modification commands, use `orch auto on`:

    ravel> orch load routing fw
    ravel> orch auto on
    ravel> profile rt addflow h1 h2
    ravel> time rt addflow h1 h3

-------------------------

### Part 4: Applications and Sub-shells

#### Application Shells

Along with a SQL file containing application tables, views, and rules, an application can provide a Python file containing a sub-shell for monitoring and controlling the application.  For example, the sample application implements an `echo` command:

    ravel> orch load sample
    ravel> sample echo Hello World

To drop into an application's sub-shell, type the application's name (or shortcut) with no additional parameters:

    ravel> sample
    sample> echo Hello World


#### Watching Application Components

To watch real-time changes to all of an application's components (i.e., tables, views) use:

    ravel> sample watch


#### Help Commands
To print help for an application's commands, open it's shell for normal use of the help command:

    ravel> sample
    sample> help echo

Or, type `help [app name] [app command]` from the Ravel CLI.  (_Note_: an application must be loaded for its help to be accessible from the main CLI.)

    ravel> orch load sample
    ravel> help sample echo

To see a description of an application and its available commands, use:

    ravel> orch load sample
    ravel> help sample


#### Application: Routing

To add a flow between hosts, load the `routing` application and use the `addflow` command with Mininet names as the hosts' names:

    ravel> orch load routing
    ravel> orch auto on
    ravel> rt addflow h1 h2
    ravel> m h1 ping h2

To set the firewall attribute for a flow, specifying that it should be routed through a firewall, if possible, append a 1 to the `addflow` command:

    ravel> rt addflow h1 h2 1


To delete a flow, use `delflow`, specifying either the hosts' names or flow ID (i.e., fid from the table rm).

    ravel> p SELECT * FROM rm;
      fid    host1    host2
    -----  -------  -------
        1        1        2
    ravel> rt delflow 1
    

#### Application: Firewall

The firewall application implements a stateful firewall.  To add a (src,dst) pair to the whitelist, using Mininet hostnames:

    ravel> orch load fw
    ravel> fw addflow h1 h2

To add a host to the whitelist, to allow a host to initiate an outbound connection:

    ravel> fw addhost h1

To remove a host or flow from the whitelist:

    ravel> fw delflow h1 h2
    ravel> fw delhost h1



-------------------------

### Part 5: Orchestration Demo

Now, let's see orchestration in action by combining the routing and firewall applications.  First, start the Ravel CLI with the toy topology in the _topo_ directory:

    $ sudo ./ravel.py --custom=topo/toy_dtp.py --topo mytopo

Load the routing and firewall applications, assigning higher priority to the firewall application:

    ravel> orch load routing fw

Next, add a flow and a host to the whitelist:

    ravel> fw addhost h4
    ravel> fw addhost h2
    ravel> fw addflow h4 h3

Launch a watch window to observe insertions to the configuration table, firewall policy table, and firewall violation table:

    ravel> watch rm cf fw_violation fw_policy_acl

Add a flow that is in the whitelist of approved flows, and specify that the flow should be routed through a firewall by appending a 1 to the `addflow` command:

    ravel> rt addflow h4 h3 1
    ravel> orch run

Observe that the flow is installed in the configuration table (`cf`) and the the hosts can ping each other:

    ravel> m h4 ping h3

Now, try adding a flow that is not in the approved flow or host whitelist, by proposing a flow with an external host as the source:

    ravel> rt addflow h1 h2 1

Observe a new row is inserted into the firewall violation table.  Next, commit the change:

    ravel> orch run

Observe that the firewall application repairs the violation by removing the proposed flow from the reachability table `rm`.
