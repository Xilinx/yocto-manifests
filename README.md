# AMD Repo Manifests for the OpenBMC Yocto Project Build System

This repository provides Repo manifests to setup the OpenBMC Yocto build system
for supported AMD products.

The Yocto Project allows the creation of custom linux distributions for
embedded systems, including AMD based systems. It is a collection of git
repositories known as *layers* each of which provides *recipes* to build
software packages as well as configuration information.

Repo is a tool that enables the management of many git repositories given a
single *manifest* file.  Tell repo to fetch a manifest from this repository and
it will fetch the git repositories specified in the manifest and, by doing so,
setup a Yocto Project build environment for you!

## Manifest Files
---
There are multiple manifest files, the purpose of each one is listed
below:

* **default-obmc.xml** - External items.  Used to setup the
                OpenBMC build environment for AMD released components.

## Getting Started
---
For instructions on setting up the build host, initializing the repo client,
and building and booting the OpenBMC images, see
[README.building.md](https://github.com/Xilinx/openbmc/blob/amd-wrynose/meta-amd-adaptive-socs-openbmc/README.building.md).
