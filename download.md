---
layout: page
title: "Download"
description: "Download and Install"
group: navigation
---
{% include JB/setup %}

<!-- ------------------------- -->

To try Ravel, download a virtual machine with all software pre-installed or download the code from GitHub and install from source.

* TOC
{:toc}

-------------------------

### Option 1: Pre-Packaged VM

1. Download the VM images with Ravel pre-installed:
    - [Ubuntu 18.04 LTS (64-bit), Ravel v0.2.1](https://github.com/ravel-net/ravel/releases/download/v0.2.1/ravelvm-0.2.1-amd64.zip)

2. Install virtualization software, such as [VirtualBox](https://www.virtualbox.org/wiki/VirtualBox) or [VMWare](https://my.vmware.com/en/web/vmware/downloads)
3. Extract the downloaded zip file containing the VM image.
4. Import appliance from the VM image.

To set up a new virtual machine in VirtualBox:

1. From the menu bar, select _Machine_ --> _New_
2. Create a VM with a unique name, such as _ravelvm_.  Select type _Linux_ and _Ubuntu_, with the architecture of the image (32-bit or 64-bit) you chose
4. Set the memory to at least 512MB and click Next
5. Select the bullet _Use an existing virtual hard drive file_
6. Click the folder icon next to the drop down menu and navigate to the folder containing _ravelvm32.vmdk_ (or _ravelvm64.vmdk_)
7. Select _ravelvm32.vmdk_ (or _ravelvm64.vmdk_) and press Open
8. Click Next to complete the setup

Log in to the VM with the username `ravel` and password `ravel`.  The sudo password is `ravel`.  To keep the VM size small, the Ubuntu installation does not have a GUI installed.  To install Unity, run `sudo apt-get install ubuntu-desktop`.

-------------------------

### Option 2: Install from Source

1. Check out a copy of Ravel's source code:

    `git clone http://github.com/ravel-net/ravel`   

2. Run the Ravel install script:

    `cd ravel`   
    `git checkout v0.2.1`   
    `util/install.sh [options]`

3. Update _ravel.cfg_ with the __*absolute*__ path to Pox.  If installing Pox with _install.sh_, this will be Ravel's parent directory.

Options for `install.sh` are:

* `-a`: install all required packages, including Mininet, Pox, and PostgreSQL
* `-m`: install only Mininet and Pox (i.e., Mininet install with options `-kmnvp`)
* `-p`: install only PostgreSQL
* `-r`: install Python libraries required by Ravel, configure PostgreSQL account and extensions


#### Configure PostgreSQL

By default, PostgreSQL uses _peer_ authentication, in which the client's username authenticates a connection to the database.  Ravel requires either _trust_ (any user can connect) or _md5_ (password-based) authentication.  If using _md5_, you will need to start Ravel with the `--password` flag to force a prompt for the password (e.g., `sudo ./ravel.py --topo single,3 --password`).

To set the authentication method, edit _/etc/postgresql/9.3/main/pg_hba.conf_ and set _postgres_ and _all_ users to _trust_ or _md5_.  Alternatively, when running `install.sh` with `-a` or `-r`, you will be prompted to make this change automatically.

_install.sh_ will create a PostgreSQL user `ravel` and database `ravel`.  To connect directly to this PostgreSQL database, use:

{% highlight bash %}
psql -Uravel
\c ravel
{% endhighlight %}

#### Optional: Configure _ovs-ofctl_

Ravel supports multiple protocols for database triggers to interact with the OpenFlow switches.  This allows changes in the database to be propagated to the network.  Supported protocols, configured in _ravel.cfg_, are: message queues (the default protocol), RPC, and _ovs-ofctl_.  If using the _ovs-ofctl_ command, you must set passwordless sudo so database triggers can run commands using sudo:

1. Add the postgres user to sudoers

    sudo adduser postgres sudo

2. Allow passwordless sudo by editing /etc/sudoers (e.g., using `sudo visudo`) and set the user specification to:

    postgres ALL=(ALL) NOPASSWD:ALL


-------------------------

### VM Archive

* Ravel v0.2.1
  - [Ubuntu 18.04 LTS (64-bit), Ravel v0.2.1](https://github.com/ravel-net/ravel/releases/download/v0.2.1/ravelvm-0.2.1-amd64.zip)

* Ravel v0.2
  - [Ubuntu 14.04 LTS (32-bit), Ravel v0.2](http://downloads.ravel-net.org/ravelvm-0.2-i386.zip)
  - [Ubuntu 14.04 LTS (64-bit), Ravel v0.2](http://downloads.ravel-net.org/ravelvm-0.2-amd64.zip)

* Ravel v0.1
  - [Ubuntu 14.04 LTS (32-bit), Ravel v0.1](http://downloads.ravel-net.org/ravelvm-0.1-i386.zip)
  - [Ubuntu 14.04 LTS (64-bit), Ravel v0.1](http://downloads.ravel-net.org/ravelvm-0.1-amd64.zip)
