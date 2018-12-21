Xilinx Repo Manifests for the Yocto Project Build System
=============================================
This repository provides Repo manifests to setup the Yocto build system for
supported Xilinx products.

The Yocto Project allows the creation of custom linux distributions for
embedded systems, including Xilinx-based systems.  It is a collection of git
repositories known as *layers* each of which provides *recipes* to build
software packages as well as configuration information.

Repo is a tool that enables the management of many git repositories given a
single *manifest* file.  Tell repo to fetch a manifest from this repository and
it will fetch the git repositories specified in the manifest and, by doing so,
setup a Yocto Project build environment for you!

Getting Started
---------------
**1.  Install Repo.**

Download the Repo script:

    $ curl https://storage.googleapis.com/git-repo-downloads/repo > repo

Make it executable:

    $ chmod a+x repo

Move it on to your system path:

    $ sudo mv repo /usr/local/bin/

If it is correctly installed, you should see a Usage message when invoked
with the help flag.

    $ repo --help

**2.  Initialize a Repo client.**

Create an empty directory to hold your working files:

    $ XILINX_SOURCES=/path/to/xilinx/sources
    $ mkdir -p $XILINX_SOURCES
    $ cd $XILINX_SOURCES

Tell Repo where to find the manifest:

    $ repo init -u git://gitenterprise.xilinx.com/Yocto/yocto-manifests.git

A successful initialization will end with a message stating that Repo is
initialized in your working directory. Your directory should now contain a
.repo directory where repo control files such as the manifest are stored but
you should not need to touch this directory.

To test out a known stable yocto version (example: fido), type:

    $ repo init -u git://gitenterprise.xilinx.com/Yocto/yocto-manifests.git -m meta-xilinx.xml -b fido
    $ repo sync

To get back to the latest master branch, type:

    $ repo init -u git://gitenterprise.xilinx.com/Yocto/yocto-manifests.git -m meta-xilinx.xml -b master
    $ repo sync

To learn more about repo, look at http://source.android.com/source/version-control.html
***

**3.  Fetch all the repositories:**

    $ repo sync

Now go turn on the coffee machine as this may take 20 minutes depending on your
connection.

Staying Up to Date
------------------
To pick up the latest changes for all source repositories, run:

    $ repo sync

Customize
---------
Sooner or later, you'll want to customize the Repo manifest to point at
different repositories and branches or pull in additional meta-layers.

Clone this repository (or fork it on gitenterprise):

    $ git clone git://gitenterprise.xilinx.com/Yocto/yocto-manifests.git

Make your changes (and contribute them back if they are generally useful), and
then re-initialize your repo client

    $ repo init -u <file:///path/to/your/git/repository.git>
