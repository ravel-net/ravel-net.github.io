---
layout: page
title: "Ravel Walkthrough"
description: ""
---
{% include JB/setup %}

* TOC
{:toc}


-------------------------

This walkthrough will demonstrate the Ravel CLI commands.  Throughout the tutorial, we will use the following prompt syntax:

* `$` to denote Linux commands typed into the shell prompt
* `ravel>` to denote commands typed into the Ravel CLI
* `app_name>` to denote commands typed into application _app_name_'s sub-shell


-------------------------

### Part 1: Startup Options

To display the help message describing Ravel's startup options:

    $ sudo ./ravel.py --help


#### Mininet

By default, Ravel will start Mininet and load the specified Mininet topology into a PostgreSQL database.

To specify a Mininet-style topology, use the parameter `--topo=TOPO`.  Ravel also accepts custom topologies in a Mininet format using the parameter `--custom=CUSTOM`.  For detailed documentation on creating custom topologies, refer to the [Mininet walkthrough](http://mininet.org/walkthrough/#custom-topologies).  For example, to start Mininet in the background with single switch and two hosts:

    $ sudo ./ravel.py --topo=single,2


#### PostgreSQL
To start Ravel without Mininet (e.g., to run database tests or if using a topology too large for mininet), use the `--onlydb` flag.  For example, to start the Ravel CLI and load the topology into the Postgres database without starting Mininet:

    $ sudo ./ravel.py --topo=single,2 --onlydb

To specify the database name and username, use the options `--db=DB` and `--user=USER`.  To force a password prompt for the database, use `--password`.  This is roughly equivalent to connecting to Postgres with `psql --dbname=DB --username=USER --password`.  The default database name and username can be set in _ravel.cfg_.

To reconnect to an existing database state, use the `--reconnect` flag.  _Note_: this assumes the specified topology is the same as the one already loaded in the database.


#### Verbosity

The default verbosity level is `info`.  Use the `--verbosity=LEVEL` parameter to specify one of: `error`, `warning`, `info`, `debug`.

    $ sudo ./rave.py --topo=single,3 --verbosity=debug



-------------------------

### Part 2: Ravel Commands
To list CLI commands:

    ravel> help

To exit the CLI:

    ravel> exit

To display the current configuration (i.e., database name, username, applications path):

    ravel> stat


#### SQL

Ravel provides a connection the database specified in the command-line options (or _ravel.cfg_).  To execute a SQL statement on the database, prefix the statement with the command `p`:

    ravel> p SELECT * FROM hosts;

To display real-time changes to a database table in a separate xterm window, use the `watch` command.  Specify the table name and optionally a limit on the number of rows:

    ravel> watch hosts 5

To refresh the database and truncate all tables except the topology, use `reinit`.  _Note_: this will not clear switch flow tables.


#### Mininet

Ravel also provides a connection to the Mininet instance started in the background (unless the `--onlydb` flag is used).  Prefix the Mininet command with `m`:

    ravel> m h1 ping h2

To drop into a Mininet sub-shell, type `m` with no additional parameters:

    ravel> m
    mininet> h1 ping h2


#### Flows

To add a flow between hosts, use `addflow`, with Mininet names as the hosts' names:

    ravel> addflow h1 h2

To delete a flow, use `delflow`, specifying either the hosts' names or flow ID (i.e., fid from the table cf).

    ravel> p SELECT * FROM rtm;
      fid    host1    host2
    -----  -------  -------
        1        1        2
    ravel> delflow 1


#### Performance

To report execution time for a command:

    ravel> time addflow h1 h2

To report detailed execution time:

    ravel> profile addflow h1 h2



-------------------------

### Part 3: Applications and Sub-shells

Ravel searches for available applications placed in the _apps_ directory.  To view discovered applications and their state:

    ravel> apps


#### Loading and Unloading Applications

To load one or more applications:

    ravel> load sample pga

To unload one or more applications:

    ravel> unload sample pga

#### Application Shells

Along with a SQL file containing application views, triggers, etc., an application can provide a Python file containing a sub-shell for monitoring and controlling the application.  For example, the sample application implements an `echo` command:

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

Or, type `help [app name] [app command` from the Ravel CLI.  (_Note_: an application must be loaded for its help to be accessible from the main CLI.)

    ravel> load sample
    ravel> help sample echo

To see a description of an application and its available commands, use:

    ravel> load sample
    ravel> help sample



-------------------------

### Part 4: Orchestration

Orchestration translates high-level application operations onto Ravel's base tables and coordinates the resulting updates.  When running multiple applications, it is recommended to use orchestration.

First, load the orchestration application:
(<span style="color:red">TODO:</span> orchestration app naming)

    ravel> load orch_auto

Next, specify a list of (loaded or unloaded) applications in descending order of priority using the `set` command.  Priorities are used to manage conflicts between proposed updates.  Updates from higher-priority applications will override updates from lower-priority ones.  To load the PGA application with the highest priority and routing with the lowest priority:

    ravel> orch_auto set pga merlin fw routing

_Note:_ any previous loaded applications that are not specified in the `set` command will be automatically unloaded.  Any unloaded applications that are specified in the `set` command will be automatically loaded.

#### Demo

<span style="color:red">TODO:</span> application descriptions (ie, firewall policy, pga policy, etc)

Now, let's see orchestration in action.  First, start the Ravel CLI with the toy topology in the _topo_ directory:

    $ sudo ./ravel.py --custom=topo/toy_dtp.py --topo mytopo

Load applications PGA, Merlin, Kinetic (FW), and Routing:

    ravel> load orch_auto
    ravel> orch_auto set pga merlin fw routing

Launch a separate xterm to watch for a policy violations:

    ravel> orch_auto watch

Try to add a flow between hosts _h1_ and _h2_ and observe the violation in the _watch_ terminal:

    ravel> addflow h1 h2
    ravel> orch_auto run

Confirm that the route is dropped using ping:

    ravel> m h1 ping h2

Next, try to add a flow in the reverse direction.  Observe no policy is violated and confirm the route is installed:

    ravel> addflow h2 h1
    ravel> orch_auto run
    ravel> m h1 ping h2

<span style="color:red">TODO:</span> add command select fid
Now, delete the flow and confirm the route is removed:

     ravel> delflow 2
     ravel> orch_auto run
     ravel> m h1 ping h2
