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


## Getting Started
---
1. See the [Preparing Build Host](https://docs.yoctoproject.org/singleindex.html#preparing-the-build-host)
   documentation to install essential host packages on your build host. The
   following command installs the host packages based on an Ubuntu distribution.
```
$ sudo apt install gawk wget git diffstat unzip texinfo gcc build-essential chrpath socat cpio python3 python3-pip python3-pexpect xz-utils debianutils iputils-ping python3-git python3-jinja2 libegl1-mesa libsdl1.2-dev xterm python3-subunit mesa-common-dev zstd liblz4-tool chrpath diffstat lz4
$ sudo locale-gen en_US.UTF-8
```

2.  Install Repo tool.

* Download the Repo script.
```
$ curl https://storage.googleapis.com/git-repo-downloads/repo > repo
```

* Make it executable.
```
$ chmod a+x repo
```

* Move it on to your system path.
```
$ sudo mv repo /usr/local/bin/
```

If it is correctly installed, you should see a Usage message when invoked
with the help flag.
```
$ repo --help
```
3. Initialize a Repo client.

* Create an empty directory to hold your working files.
```
$ mkdir -p yocto/<release_version>
$ cd yocto/<release_version>
```

* Clone the Yocto meta layer source using yocto manifest as show below.
```
$ repo init -u git://gitenterprise.xilinx.com/Yocto/yocto-manifests.git -b <release_version>
```
A successful initialization will end with a message stating that Repo is
initialized in your working directory. Your directory should now contain a
.repo directory where repo control files such as the manifest are stored but
you should not need to touch this directory.

To learn more about repo, look at https://source.android.com/setup/develop/repo

4. Fetch all the repositories.
```
$ repo sync
```

5. Start a branch with for development starting from the revision specified in
   the manifest. This is an optional step.
```
$ repo start <branch_name> --all
```

6. Setup the Yocto OE Init scripts by sourcing `setupsdk` script.
```
$ source setupsdk
```

7. Set hardware `MACHINE` configuration variable in <proj-dir>/build/conf/local.conf
   file for a specific target which can boot and run the in the board or QEMU.
```
MACHINE = "<target_machine_name>"
```
* For list of available target machines see meta layer README files.

 * [meta-xilinx-bsp README](https://github.com/Xilinx/meta-xilinx/tree/master/meta-xilinx-bsp#amd-xilinx-evaluation-boards-bsp-machines-files)
 * [meta-kria README](https://github.com/Xilinx/meta-xilinx/tree/master/meta-xilinx-bsp#amd-xilinx-evaluation-boards-bsp-machines-files)

8. For NFS build host system modify the build/conf/local.conf and add TMPDIR
   path as shown below. On local storage $TMPDIR will be set to build/tmp
```
TMPDIR = "/tmp/$USER/yocto/release_version/build"
```

9. Modify the build/conf/local.conf file to add wic image to default target
   image as shown below.
```
IMAGE_FSTYPES += "wic"
WKS_FILES = "xilinx-default-sd.wks"
```

10. Build the qemu-helper-native package to setup QEMU network tap devices.
```
$ bitbake qemu-helper-native
```

11. Manually configure a tap interface for your build system. As root run
   <path-to>/sources/poky/scripts/runqemu-gen-tapdevs, which should generate a
   list of tap devices. Once tap interfaces are successfully create you should
   be able to see all the interfaces by running ifconfig command.

```
$ sudo ./<path-to-layer>/poky/scripts/runqemu-gen-tapdevs $(id -u $USER) $(id -g $USER) 4 tmp/sysroots-components/x86_64/qemu-helper-native/usr/bin
```

12. Building the target image.
```
$ bitbake petalinux-image-minimal
```

13. Simulate and Boot target image using QEMU.
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

14. Booting from HW, Use dd command or balena etcher to flash the wic image file
    to SD card. WIC image will be build/tmp/deploy/${MACHINE}/target-image-machine-datetimestamp.rootfs.wic,
    See [Flashing Images Using bmaptool](https://docs.yoctoproject.org/singleindex.html#flashing-images-using-bmaptool)
    for fast and easy way to flash the image.

## Staying Up to Date

To pick up the latest changes for all source repositories, run:
```
$ repo sync
```

## Customize

Sooner or later, you'll want to customize the repo manifest to point at different repositories and branches or pull in additional meta-layers.

* Clone this repository (or fork it on gitenterprise):
```
$ git clone git://gitenterprise.xilinx.com/Yocto/yocto-manifests.git
```

* Make your changes (and contribute them back if they are generally useful), and then re-initialize your repo client.
```
$ repo init -u <file:///path/to/your/git/repository.git>
```

## Additional Documentation

For more information see https://github.com/Xilinx/meta-xilinx/blob/master/README.md
