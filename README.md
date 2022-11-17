<h1 align="center">
  <!--
  <img src=".github/logo.png" alt="machinespawn" width="256" />
  <br />
  -->
  machinespawn
</h1>

<p align="center"><b>Quickly stand up <em>systemd-nspawn</em> containers</b>
<br />
Made with 💝 for <img src=".github/ubuntu.png" align="top" width="20" /> & <img src=".github/debian.png" align="top" width="20" />
</p>

# Introduction

A nifty and performant way to quickly create, stand up or manage [`systemd-nspawn`](https://www.freedesktop.org/software/systemd/man/systemd-nspawn.html) containers, `machinespawn` uses [`debootstrap`](https://wiki.debian.org/Debootstrap) and [`machinectl`](https://www.freedesktop.org/software/systemd/man/machinectl.html) and [`systemd-nspawn`](https://www.freedesktop.org/software/systemd/man/systemd-nspawn.html) to build run and remove minimal OS images. It is useful for that, and also as a stage to quickly build custom container or ISO images.

This project is currently a work-in-progress 🚧 and under active development so treat as alpha software and approach with caution 🛑 However `machinespawn` is already key part of the [Ubuntu Butterfly 🦋](https://github.com/butterfly-garden) [build process](https://github.com/butterfly-garden/image-build).

## Participate

We have a Discord for this project: [![Discord](https://img.shields.io/discord/712850672223125565?color=0C306A&label=WimpysWorld%20Discord&logo=Discord&logoColor=ffffff&style=flat-square)](https://discord.gg/sNmz3uw)

To see it in action, or to watch it becoming, you can watch these videos where I go from an idea to a working full-featured prototype of `machinespawn`.

[![machinespawn! 🐧 Dev tooling from concept to production 🧑‍💻](https://img.youtube.com/vi/-bQQ6QlXpJQ/0.jpg)](https://www.youtube.com/watch?v=-bQQ6QlXpJQ)

I live stream the development of `machinespawn` and other project on [Wimpy's World Twitch channel](https://twitch.tv/WimpysWorld).

## Requirements

This script relies on utilities, many of which are typically already installed on most Debian or Ubuntu systems. The following will ensure you have all you need:

```bash
sudo apt-get install debootstrap binutils iproute2 systemd-container wget
```

### Caching Proxy *(optional)*

If `apt-cacher-ng` is installed on the host `machinespawn` will automatically detect its presence and use it for container bootstrapping and executing commands inside the container with `run`. Install and configure `apt-cache-ng` as follows:

```bash
sudo apt-get install apt-cache-ng
```

Create `/etc/apt-cacher-ng/zz_debconf.conf` with the following in it:

```
PassThroughPattern: .*
```

Once the above `PassThroughPattern` is set, `apt-cacher-ng` will proxy but not cache objects stored on SSL/TLS repositories.

## Supported distros

Currently the following distros are supported:

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

### Architecture Support

Containers can be built for the host architecture or cross-bootstrapped for the following machine architectures:

* amd64
* i386
* armhf
* arm64

# Usage

`machinespawn` currently needs to be run as root via `sudo`

```bash
sudo machinespawn [command]
```

## Commands

### `bootstrap`

Build a machine from scratch.

The first build of a machine type and architecture may be quite heavy on time and resource usage, but intelligent caching, together with detection and use of local package caching proxies, should make this as efficient as possible and significantly reduce subsequent builds or re-builds.

The following bootstrap a container called 'bob' using Ubuntu 22.04 *(Jammy Jellyfish)* as the base.

```bash
sudo machinespawn ubuntu-22.04 bob
```

Here's an example that would bootstrap a Debian 11 container called 'fred':

```bash
sudo machinespawn debian-11 fred
```

### `list`

List the existing images (by default these will be found in `/var/lib/machines/`)

### `run`

Execute commands inside the container or start an interactive shell.

```bash
sudo machinespawn run bob /usr/bin/bash
```

### `clean-cache`

Remove cached content from `/var/cache/machinespawn`. This will release space taken by downloaded packages at the expense of needing to re-download if required.

### `pull-tar`

```bash
machinespawn pull-tar <URL> <Machine Name>
```

Downloads a .tar container image from the specified URL, and makes it available under the specified local machine name. The URL must be of type "http://" or "https://", and must refer to a .tar, .tar.gz, .tar.xz or .tar.bz2 archive file

```bash
sudo macheinespawn pull-tar  https://download.fedoraproject.org/pub/fedora/linux/releases/36/Cloud/x86_64/images/Fedora-Cloud-Base-36-1.5.x86_64.raw.xz FedoraCloudBase36
```

### `remove`

Completely remove a machine from `/var/lib/machines/`

## Reference

* [debbootstrap](https://wiki.debian.org/Debootstrap)
* [machinectl](https://www.freedesktop.org/software/systemd/man/machinectl.html)
* [systemd-nspawn](https://www.freedesktop.org/software/systemd/man/systemd-nspawn.html)

## TODOs

These important features are not *yet* implemented here.
 - [x] Publish in GitHub
 - [ ] Add support for other distros
   - container_pull_tar() should accept a distro argument or a tarball URL
 - [ ] Add support for starting/stopping
 - [ ] Add support for enabling/disabling
 - [ ] Add supporting for container booting
 - [ ] Add support for debootstrap variants
 - [ ] Check the required tools are installed
 - [ ] User provisioning
 - [ ] Home directory mounting
 - [ ] Bind sockets, audio, display servers and authority
