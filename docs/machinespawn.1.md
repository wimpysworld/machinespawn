---
author: Martin Wimpress
date: November 16, 2022
footer: machinespawn
header: machinespawn Manual
section: 1
title: MACHINESPAWN
---


# Machinespawn

Quickly stand up <em>systemd_nspawn</em> containers

## Introduction

A nifty and performant way to quickly create, stand up or manage [`systemd_nspawn`](https://www.freedesktop.org/software/systemd/man/systemd-nspawn.html) containers,
`machinespawn` uses [`debootstrap`](https://wiki.debian.org/Debootstrap) and [`machinectl`](https://www.freedesktop.org/software/systemd/man/machinectl.html) and [`systemd_nspawn`](https://www.freedesktop.org/software/systemd/man/systemd-nspawn.html) to build run and remove minimal OS images.
It is useful for that, and also as a stage to quickly build custom container or ISO images.


## Warning :warning:
It is currently a  work-in-progress 🚧  under heavy and rapid development so treat as alpha software and approach with caution 🛑. However is already key part
of the [Ubuntu Butterfly](https://github.com/butterfly-garden) [build process](https://github.com:butterfly-garden/image-build).

## Requirements

This script relies on utilities, many of which are typically already installed on most Debian or Ubuntu systems.
The following will ensure you have all you need:

    sudo apt-get install binutils iproute2 systemd-container wget unzip
    # optionally also
    # open-infrastructure-container-tools


Specifically it requires:

* bash
* [debootstrap](https://wiki.debian.org/Debootstrap)
* ip
* systemd-nspawn
* [machinectl](https://www.freedesktop.org/software/systemd/man/machinectl.html)
* wget (depending on version of machinctl installed)

## Supported OS images

Currently the following OS Releases are supported:

* Debian Releases
  *  8 (jessie)
  *  9 (stretch)
  *  10 (buster)
  *  11 (bullseye)
  *  12 (bookworm)
* Ubuntu Releases
  *  16.04 (xenial)
  *  18.04 (bionic)
  *  20.04 (focal)
  *  22.04 (jammy)
  *  22.10 (kinetic)

## Architecture Support

Containers can be built for the host architecture or cross-bootstrapped for the following machine architectures:

* amd64
* i386
* armhf
* arm64


## Usage

`machinespawn` currently needs to be run as root via `sudo`

    Usage: sudo machinespawn [command]

## Commands

### bootstrap

Build a machine from scratch.

The first build of a machine type and architecture may be quite heavy on time and resource usage, but intelligent cacheing,
together with detection and use of local package caching proxies, should make this as efficient as possible and significantly reduce subsequent builds or re-builds

    sudo machinespawn ubuntu-jammy bread

will build and bootstrap a container called `bread` using a base **ubuntu 22.04 (jammy)** image

### list

List the existing images (by default these will be found in `/var/lib/machines/`)

### run

Spawn the container

    sudo machinespawn run bread

Without parameters you will be dropped into a root `bash` shell

### clean-cache

Remove cached content from `/var/cache/machinespawn`. This will release space taken by downloaded packages at the expense of needing to re-download if required.

### pull-tar

    machinespawn pull-tar  <URL> <Machine Name>

Downloads a .tar container image from the specified URL, and makes it available under the specified local machine name. The URL must be of type "http://" or "https://", and must refer to a .tar, .tar.gz, .tar.xz or .tar.bz2 archive file


    sudo macheinespawn pull-tar  https://download.fedoraproject.org/pub/fedora/linux/releases/36/Cloud/x86_64/images/Fedora-Cloud-Base-36-1.5.x86_64.raw.xz FedoraCloudBase36

### remove

completely remove a machine from `/var/lib/machines/`


There is a channel for this project at [Discord](https://discord.gg/sNmz3uw)


## Reference

* [debootstrap](https://wiki.debian.org/Debootstrap)
* [machinectl](https://www.freedesktop.org/software/systemd/man/machinectl.html)
* [systemd-nspawn](https://www.freedesktop.org/software/systemd/man/systemd-nspawn.html) [homepage](https://open-infrastructure.net/software/compute-tools)
