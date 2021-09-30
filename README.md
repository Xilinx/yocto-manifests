Xilinx Repo Manifests for the Yocto Project Build System
========================================================

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

**2.  Initialize a Repo client.**

Tell Repo where to find the manifest:

    $ ./repo init -u git://github.com/Xilinx/yocto-manifests.git -b 2021-experimental-dt-support

A successful initialization will end with a message stating that Repo is
initialized in your working directory. Your directory should now contain a
.repo directory where repo control files such as the manifest are stored but
you should not need to touch this directory.

To learn more about repo, look at http://source.android.com/source/version-control.html
***

**3.  Fetch all the repositories:**

    $ ./repo sync

Now go turn on the coffee machine as this may take 20 minutes depending on your
connection.

Staying Up to Date
------------------
To pick up the latest changes for all source repositories, run:

    $ ./repo sync
