# AMD Repo Manifests for the Yocto Project Build System

This repository provides Repo manifests to setup the Yocto build system for
supported AMD products.

The Yocto Project allows the creation of custom linux distributions for
embedded systems, including AMD based systems.  It is a collection of git
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

* **default.xml** - PetaLinux manifest

* **default-edf.xml** - Embedded Development Framework Project manifest

## Getting Started

> **Pre-requisites:**
> 1. See [Preparing Build Host](https://docs.yoctoproject.org/5.0.8/singleindex.html#preparing-the-build-host) documentation.
> 2. Configure the git settings before you run the repo commands.
>     ```
>     $ git config --global user.email "you@example.com"
>     $ git config --global user.name "Your Name"
>     ```
> 3. Make sure build host shell is bash not csh or dash.
> 4. A basic understanding of the Yocto build system is assumed - please consult the documentation for further information https://docs.yoctoproject.org/
>

---
1. Download and install Repo tool (if it wasn't installed in a previous step).
   Note that if you have repo installed through a package manager that should be
   removed first as it is likely out of date and will cause issues.

   ```
   $ curl https://storage.googleapis.com/git-repo-downloads/repo > repo
   $ chmod a+x repo
   $ mv repo ~/bin/
   $ PATH=~/bin:$PATH
   $ repo --help
   ```
2. Initialize a Repo client.
   1. Create the project directory.
      ```
      $ mkdir -p yocto/<release_version>
      $ cd yocto/<release_version>
      ```
   2. Clone the Yocto meta layer source using yocto manifest as show below. A
      successful initialization will end with a message stating that Repo is
      initialized in your working directory. Your directory should now contain a
      .repo directory where repo control files such as the manifest are stored
      but you should not need to touch this directory.
      To learn more about repo, look at https://source.android.com/setup/develop/repo
      > **Note:**
      > If you are using default-edf.xml then follow https://github.com/Xilinx/meta-amd-edf/blob/rel-v2025.2/README.build.md
      ```
      $ repo init -u https://github.com/Xilinx/yocto-manifests.git -b <release_version>
      ```

3. Fetch all the repositories.
   ```
   $ repo sync
   ```

4. Start a branch with for development starting from the revision specified in
   the manifest. This is an optional step.
   ```
   $ repo start <branch_name> --all
   ```

5. Setup the Yocto OE Init scripts by sourcing `setupsdk` script.
   ```
   $ source setupsdk
   ```

6. Set hardware `MACHINE` configuration variable in <proj-dir>/build/conf/local.conf
   file for a specific target which can boot and run the in the board or QEMU.
```
MACHINE = "<target_machine_name>"
```
* For list of available target machines see meta layer README files.

  * [meta-amd-adaptive-socs-bsp README](https://github.com/Xilinx/meta-amd-adaptive-socs/blob/master/meta-amd-adaptive-socs-bsp/README.asoc.bsp.md)
  * [meta-xilinx-tools README](https://github.com/Xilinx/meta-xilinx-tools/tree/master/README.xsct.bsp.md)
  * [meta-kria README](https://github.com/Xilinx/meta-xilinx/tree/master/meta-xilinx-bsp#amd-xilinx-evaluation-boards-bsp-machines-files)

7. For NFS build host system modify the build/conf/local.conf and add TMPDIR
   path as shown below. On local storage $TMPDIR will be set to build/tmp
```
TMPDIR = "/tmp/$USER/yocto/release_version/build"
```

8. Modify the build/conf/local.conf file to add wic image to default target
   image as shown below.
```
IMAGE_FSTYPES += "wic"
WKS_FILES = "xilinx-default-sd.wks"
```

9.  Build the qemu-helper-native package to setup QEMU network tap devices.
```
$ bitbake qemu-helper-native
```

10.  Manually configure a tap interface for your build system. As root run
   <path-to>/sources/poky/scripts/runqemu-gen-tapdevs, which should generate a
   list of tap devices. Once tap interfaces are successfully create you should
   be able to see all the interfaces by running ifconfig command.

```
$ sudo ./<path-to-layer>/poky/scripts/runqemu-gen-tapdevs $(id -g $USER) 4
```

11. Building the target image.
```
$ bitbake petalinux-image-minimal
```

12. Simulate and Boot target image using QEMU.
> **Note:** Use `slirp` option if you don't have sudo permissions and tap devices
  are enabled on your build host.

* Without slirp
```
$ runqemu nographic
```

* With slirp
```
$ runqemu nographic slirp
```

13. Booting from HW, Use dd command or balena etcher to flash the wic image file
    to SD card. WIC image will be build/tmp/deploy/${MACHINE}/target-image-machine-datetimestamp.rootfs.wic,
    See [Flashing Images Using bmaptool](https://docs.yoctoproject.org/singleindex.html#flashing-images-using-bmaptool)
    for fast and easy way to flash the image.

## Staying Up to Date

To pick up the latest changes for all source repositories, run:
```
$ repo sync
```

## Additional Documentation

For more information see https://github.com/Xilinx/meta-xilinx/blob/master/README.md
