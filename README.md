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

The manifest files provided on this branch, and the purpose of each, are
listed below:

* **default.xml** - PetaLinux Distro manifest.

> **Note:** This manifest is intended to be used only with the PetaLinux
> distro. PetaLinux reaches end of life (EOL) with the 2026.2 release.

## Getting Started

> **Prerequisites:**
> 1. See the [Preparing the Build Host](https://docs.yoctoproject.org/scarthgap/singleindex.html#preparing-the-build-host)
>    documentation.
> 2. Configure your Git settings before you run the Repo commands.
>    ```
>    $ git config --global user.email "you@example.com"
>    $ git config --global user.name "Your Name"
>    ```
> 3. Make sure the build host shell is bash, not csh or dash.
> 4. A basic understanding of the Yocto Project build system is assumed. Please
>    consult the [Yocto Project documentation](https://docs.yoctoproject.org/)
>    for further information.

1. Download and install the Repo tool (if it was not installed in a previous
   step). If you have Repo installed through a package manager, remove it
   first, as it is likely out of date and will cause issues.
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
   2. Clone the Yocto Project meta layer sources using the manifest as shown
      below, where `<release_version>` is the release branch name (for
      example, `rel-v2026.2-scarthgap`). A successful initialization ends with
      a message stating that Repo is initialized in your working directory.
      Your directory should now contain a `.repo` directory where Repo control
      files such as the manifest are stored, but you should not need to touch
      this directory. To learn more about Repo, see
      [Repo command reference](https://source.android.com/docs/setup/reference/repo).
      ```
      $ repo init -u https://github.com/Xilinx/yocto-manifests.git -b <release_version>
      ```

3. Fetch all the repositories.
   ```
   $ repo sync
   ```

4. Optionally, start a development branch from the revision specified in the
   manifest.
   ```
   $ repo start <branch_name> --all
   ```

5. Set up the Yocto Project build environment by sourcing the `setupsdk`
   script.
   ```
   $ source setupsdk
   ```

6. Set the hardware `MACHINE` configuration variable in the
   `<proj-dir>/build/conf/local.conf` file for the target board or QEMU.
   ```
   MACHINE = "<target_machine_name>"
   ```
   For the list of available target machines, see the meta layer README files:
   * [meta-amd-adaptive-socs-bsp README](https://github.com/Xilinx/meta-amd-adaptive-socs/blob/rel-v2026.2-scarthgap/meta-amd-adaptive-socs-bsp/README.asoc.bsp.md)
   * [meta-xilinx-tools README](https://github.com/Xilinx/meta-xilinx-tools/blob/rel-v2026.2-scarthgap/README.xsct.bsp.md)
   * [meta-kria README](https://github.com/Xilinx/meta-kria/blob/rel-v2026.2-scarthgap/README.kria.bsp.md)

7. If the build directory is on NFS storage, add a `TMPDIR` path on local
   storage to `build/conf/local.conf` as shown below. By default, `TMPDIR` is
   set to `build/tmp`.
   ```
   TMPDIR = "/tmp/$USER/yocto/<release_version>/build"
   ```

8. Add the WIC image type to the default target image in
   `build/conf/local.conf` as shown below.
   ```
   IMAGE_FSTYPES += "wic"
   WKS_FILES = "xilinx-default-sd.wks"
   ```

9. Build the `qemu-helper-native` package to set up QEMU network tap devices.
   ```
   $ bitbake qemu-helper-native
   ```

10. Manually configure tap interfaces for your build system by running
    `runqemu-gen-tapdevs` as root. Once the tap interfaces are successfully
    created, you can list them with the `ip link` command.
    ```
    $ sudo <proj-dir>/sources/poky/scripts/runqemu-gen-tapdevs $(id -g $USER) 4
    ```

11. Build the target image.
    ```
    $ bitbake petalinux-image-minimal
    ```

12. Boot the target image in QEMU.
    > **Note:** Use the `slirp` option if you do not have sudo permissions or
    > tap devices are not enabled on your build host.

    * Without slirp:
      ```
      $ runqemu nographic
      ```
    * With slirp:
      ```
      $ runqemu nographic slirp
      ```

13. To boot on hardware, flash the WIC image to an SD card using `dd` or
    balenaEtcher. The WIC image is located at
    `${TMPDIR}/deploy/images/${MACHINE}/<target-image>-${MACHINE}.rootfs.wic`.
    For a fast and easy way to flash the image, see
    [Flashing Images Using bmaptool](https://docs.yoctoproject.org/scarthgap/singleindex.html#flashing-images-using-bmaptool).

## Staying Up to Date

To pick up the latest changes for all source repositories, run:
```
$ repo sync
```

## Additional Documentation

For more information, see the
[meta-xilinx README](https://github.com/Xilinx/meta-xilinx/blob/rel-v2026.2-scarthgap/README.md).
