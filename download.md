---
layout: page
title: "Download Ravel"
description: ""
---
{% include JB/setup %}

-------------------------

To use Ravel, download a virtual machine with all software pre-installed or download the code from GitHub and install from source.

* TOC
{:toc}

-------------------------

### Option 1: Pre-Packaged VM

1. Download one of the VM images with Ravel pre-installed:
  - Ubuntu 14.04 LTS (32-bit)
  - Ubuntu 14.04 LTS (64-bit)

2. Install virtualization software, such as [VirtualBox](https://www.virtualbox.org/wiki/VirtualBox) or [VMWare](https://my.vmware.com/en/web/vmware/downloads)
3. Extract the downloaded VM image `ravelvm.zip`
4. Create a new VM, using the extracted `ravelvm.vmdk` as the virtual disk

To set up a new virtual machine in VirtualBox:

1. From the menu bar, select _Machine_ --> _New_
2. Create a VM with a unique name, such as _ravelvm_.  Select type _Linux_ and _Ubuntu_, with the architecture of the image (32-bit or 64-bit) you chose
4. Set the memory to at least 512MB and click Next
5. Select the bullet _Use an existing virtual hard drive file_
6. Click the folder icon next to the drop down menu and navigate to the folder containing _ravelvm.vmdk_
7. Select _ravelvm.vmdk_ and press Open
8. Click Next to complete the setup


-------------------------

### Option 2: Install from Source

1. Check out a copy of Ravel's source code: <span style="color:red">TODO:</span>

    git clone REPO_LINK

2. Run the Ravel install script: <span style="color:red">TODO:</span>

    cd REPO_NAME
    util/install.sh [options]

Options for `install.sh` are:

* `-a`: install all required packages, including Mininet, Pox, and PostgreSQL
* `-m`: install only Mininet and Pox (ie, Mininet install with options `-kmnvp`)
* `-p`: install only PostgreSQL
* `-r`: install Python libraries needed for Ravel, configure PostgreSQL and extensions

#### Configure PostgreSQL Authentication

By default, PostgreSQL uses _peer_ authentication, in which the client's username authenticates a connection to the database.  Ravel requires either _trust_ (anyone can connect) or _md5_ (password-based) authentication.  If using _md5_, you must start Ravel with the `--password` flag to force a prompt for the password (eg, `sudo ./ravel.py --topo single,3 --password`).  Modify the file `/etc/postgresql/9.3/main/pg_hba.conf` and set _postgres_ and _all_ users to _trust_ or _md5_.  Alternatively, when running `install.sh` with `-a` or `-r`, you will be prompted to make this change automatically.


#### Optional: Configure _ovs-ofctl_

Ravel supports multiple protocols for database triggers to interact with the OpenFlow switches.  This allows changes in the database can be propogated to the network.  Supported protocols, configured in `ravel.cfg`, are: message queues (the default protocol), RPC, and `ovs-ofctl`.  If using the `ovs-ofctl` command, you must set passwordless sudo so database triggers can invoke the command:

1. Add the postgres user to sudoers

    sudo adduser postgres sudo

2. Allow passwordless sudo by editing /etc/sudoers (eg, using `sudo visudo`) and set the user specification to:

    sudo ALL=(ALL:ALL) NOPASSWORD:ALL

