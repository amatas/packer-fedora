
# Packer templates for Fedora

### Overview

This repository contains templates for Fedora that can create Vagrant boxes
using Packer.

## Current Boxes

* [Fedora 23 (64-bit)](https://atlas.hashicorp.com/inclusivedesign/boxes/fedora23)
* [Fedora 22 (64-bit)](https://atlas.hashicorp.com/inclusivedesign/boxes/fedora22)

## Building the Vagrant boxes

To build all the boxes, you will need VirtualBox and VMware
installed.

A GNU Make `Makefile` drives the process via the following targets:

    make        # Build all the box types (VirtualBox, VMware & Parallels)
    make test   # Run tests against all the boxes
    make list   # Print out individual targets
    make clean  # Clean up build detritus

### Proxy Settings

The templates respect the following network proxy environment variables
and forward them on to the virtual machine environment during the box creation
process, should you be using a proxy:

* http_proxy
* https_proxy
* ftp_proxy
* rsync_proxy
* no_proxy
 
### Tests

The tests are written in [Serverspec](http://serverspec.org) and require the
`vagrant-serverspec` plugin to be installed with:

    vagrant plugin install vagrant-serverspec

The `Makefile` has individual targets for each box type with the prefix
`test-*` should you wish to run tests individually for each box.

    make test-virtualbox/fedora22
    
Similarly there are targets with the prefix `ssh-*` for registering a
newly-built box with vagrant and for logging in using just one command to
do exploratory testing.  For example, to do exploratory testing
on the VirtualBox training environmnet, run the following command:

    make ssh-virtualbox/fedora22

Upon logout `make ssh-*` will automatically de-register the box as well.

### Makefile.local override

You can create a `Makefile.local` file alongside the `Makefile` to override
some of the default settings.  The variables can that can be currently
used are:

* CM
* CM_VERSION
* HEADLESS
* \<iso_path\>
* UPDATE

`Makefile.local` is most commonly used to override the default configuration
management tool, for example with Chef:

    # Makefile.local
    CM := chef

Changing the value of the `CM` variable changes the target suffixes for
the output of `make list` accordingly.

Possible values for the CM variable are:

* `nocm` - No configuration management tool
* `chef` - Install Chef
* `puppet` - Install Puppet
* `salt`  - Install Salt

You can also specify a variable `CM_VERSION`, if supported by the
configuration management tool, to override the default of `latest`.
The value of `CM_VERSION` should have the form `x.y` or `x.y.z`,
such as `CM_VERSION := 11.12.4`

The variable `UPDATE` can be used to perform OS patch management.  The
default is to not apply OS updates by default.  When `UPDATE := true`,
the latest OS updates will be applied.

The variable `HEADLESS` can be set to run Packer in headless mode.
Set `HEADLESS := true`, the default is false.

The variable `PACKER` can be used to set the path to the packer binary.
The default is `packer`.

The variable `ISO_PATH` can be used to set the path to a directory with
OS install images.  This override is commonly used to speed up Packer
builds by pointing at pre-downloaded ISOs instead of using the default
download Internet URLs.

### Acknowledgments

[SmartyStreets](http://www.smartystreets.com) is providing basebox hosting for the box-cutter project.

![Powered By SmartyStreets](https://smartystreets.com/resources/images/smartystreets-flat.png)
