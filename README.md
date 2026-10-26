# AMD Repo Manifests for the Yocto Project Build System

This repository provides Repo manifests to set up the Yocto Project build
system for supported AMD products.

The Yocto Project allows the creation of custom Linux distributions for
embedded systems, including AMD-based systems. It is a collection of Git
repositories known as *layers*, each of which provides *recipes* to build
software packages as well as configuration information.

Repo is a tool that enables the management of many Git repositories given a
single *manifest* file. Tell Repo to fetch a manifest from this repository and
it will fetch the Git repositories specified in the manifest and, by doing so,
set up a Yocto Project build environment for you.

## Manifest Files

There are multiple manifest files. The purpose of each one is listed below:

* **default.xml** - Symbolic link to `default-edf.xml`, used when no manifest
  file is specified with `repo init -m`.

* **default-edf.xml** - AMD Embedded Development Framework (EDF) manifest.

## Getting Started

For instructions on setting up the build host, fetching sources, and building
and running images, see the
[AMD EDF documentation](https://edf.docs.amd.com/en/latest/osdev/operating-system-integration-and-development.html).

## Additional Documentation

For more information, see the
[meta-xilinx README](https://github.com/Xilinx/meta-xilinx/blob/master/README.md).
