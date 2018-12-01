---
title: 'SYSTART - Oxalis SDK'
...

Binary image download {#_binary_image_download}
=====================

You can find binary images under <http://www.ebs-systart.com/oxalis>

Building an image from the NXP QoriQ reference SDK (Yocto based) {#_building_an_image_from_the_nxp_qoriq_reference_sdk_yocto_based}
================================================================

The following instructions assume you are running Linux and are well
versed in using a terminal. On any other operating system we recommend
installing VirtualBox from <https://www.virtualbox.org> or any other
virtualization system that lets you run a Linux distribution.

NOTE

:   Quoting the NXP SDK Documentation: "Yocto Project supports typical
    Linux distributions: Ubuntu, Fedora, CentOS, Debian, OpenSUSE, etc.
    More Linux distributions are continually being verified. This SDK
    has been verified on following Linux distributions: Ubuntu 14.04,
    CentOS-7.1.1503, Debian 8.2, Fedora 22 and OpenSUSE 13.2" You can
    use other distros, notably a more current LTS Ubuntu, but then you
    get Warnings to ignore and patches to apply. It will not work out of
    the box.

Install and update the NXP Reference SDK {#_install_and_update_the_nxp_reference_sdk}
----------------------------------------

The SDK is only available for download with an NXP.com account, which is
free. (Detailed descriptions are on nxp.com)

1.  Download, unpack and install the NXP-QoriQ Linux SDK v2.0 source

2.  Download and install the NXP-QoriQ Linux SDK v2.0 17.03 Upgrade

3.  Change into your SDK directory using the linux console.

4.  Run `. ./fsl-setup-env -m ls1012frdm`

5.  Accept the EULA, you should now have been transferred into a
    directory called `build_ls1012afrdm`.

6.  Verify your installation state by running `bitbake fsl-image-core`
    (This will take a long time, depending on your system)

If bitbake runs through without major errors, your installation of the
Yocto SDK has been successful.

NOTE

:   The image that resulted from this test-build carries no Oxalis
    specific customizations but might be bootable on your board
    (not supported).

Pull in the source code for Oxalis {#_pull_in_the_source_code_for_oxalis}
----------------------------------

Yocto is essentially a complete source tree of Linux and all its tools,
that can be changed and configured using a layering approach. SYSTART
provides a BSP in the shape of a Yocto layer.

NOTE

:   If you want to understand what is happening in detail, refer to the
    Yocto documentation.

Get the BSP Layer {#_get_the_bsp_layer}
-----------------

Execute the following:

    cd <sdk-install-dir>/
    mkdir systart
    cd systart
    git clone https://github.com/ebs-systart/meta-systart-oxalis
    source meta-systart-oxalis/scripts/oxalis-setup-env

You should now have been switched into a new build directory

    <sdk-install-dir>/build_oxalisls1012a

(this is called the `<build-dir>` in the following sections).

From here `bitbake` should be able to build these images:

-   `oxalis-image-kernelitb`: Kernel, U-Boot and minimal Linux
    environment (BusyBox) ramdisk in U-Boot loadable format

-   `oxalis-image-core`: RootFS image with core components

-   `oxalis-image-full`: RootFS with full content of the yocto
    distribution, including GUI (very large, extreme build times,
    not supported).

    NOTE

    :   The `bitbake` process should take less time than before if you
        selected the core or kernel image.

The best starting point is `oxalis-image-kernelitb` because of its
compactness, its simplicity and the fact that our deployment guide
assumes you have built it. Often you might want to build a complete
distribution on a root filesystem on a larger storage medium than the
64MB onboard flash, then the `-core` and `-full` rootfs images are what
you need.

After the build you can find the images in:

    <sdk-install-dir>/<build-dir>/tmp/deploy/images/ls1012aoxalis/

Deploying the kernelitb ramdisk image to Oxalis {#_deploying_the_kernelitb_ramdisk_image_to_oxalis}
===============================================

NOTE

:   This section assumes you have successfully completed our "Getting
    started" guide and can get to the U-Boot console on Oxalis.

This is a two step process:

1.  Load the Linux image into Oxalis’s RAM

2.  Then, either

    -   Transfer it into onboard flash memory for automatic booting

    -   or boot it on-the-fly

The onboard flash memory is 64MB in size. You should keep your Kernel
and Ramdisk image below 55MB in size for unproblematic operation. (First
5 MB are reserved for bootloader and initialization data.)

Via SD Card, USB Stick or SATA {#_via_sd_card_usb_stick_or_sata}
------------------------------

1.  Format the storage medium with fat32

2.  copy and rename the kernel itb image to `kernel.itb` and place it in
    the root folder of the storage medium

3.  Ensure that the image has been fully written. Use "Safe remove" on
    Windows and the `sync` command on the linux console

4.  Power down your Oxalis

5.  Insert the medium in the appropriate slot

6.  Power the Oxalis up again

7.  Load the Kernel ramdisk into RAM:

    -   SDard: `fatload mmc 0 0x96000000 kernel.itb`

    -   USB Stick: `usb start; fatload usb 0 0x96000000 kernel.itb`

8.  If the loading was successful, you can now

    -   flash it to QSPI using `sf write 0x500000 0x96000000 $filesize`

    -   or boot directly, by running `pfe stop; bootm 0x96000000` to
        boot the kernel with the ramdisk

9.  Login with `root` and press enter. No password is required

    NOTE

    :   Why the `pfe stop` command? The packet forwarding engine (PFE)
        is part of the hardware accelerated ethernet subsystem of the
        LS1012A. It needs to be shut down before Kernel boot, so the
        Kernel code can reinitialize PFE for itself.

Configure Autoboot via QSPI {#_configure_autoboot_via_qspi}
---------------------------

Congratulations! {#_congratulations}
================

You are now running a full Linux Kernel with a minimal Linux environment
called BusyBox on your Oxalis! From here it is possible to add your own
applications, either by loading them via the network or by adding your
own yocto layer (complex).

-   The official BusyBox page is here: <https://busybox.net>

-   Wikipedia has a very nice and concise explanation of what BusyBox
    actually is: <https://en.wikipedia.org/wiki/BusyBox>
