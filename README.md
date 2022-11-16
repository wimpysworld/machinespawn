# machinespawn

A nifty and performant way to quickly stand up or manage `systemd_nspawn containers`

`machinespawn`uses `debbootstrap` and `machinectl` and `systemd_nspawn` to build run and remove minimal OS images
It is useful for that, and also as a stage to quickly build custom container or ISO images.

It is currently a work in progress under heavy and rapid development, but  is already key part
of the [Butterfly](https://github.com/butterfly-garden) build process.

## Supported OS images

Currently the following OS Releases are supported:

* Debian
  *  8 (jessie)
  *  9 (stretch)
  *  10 (buster)
  *  11 (bullseye)
  *  12 (bookworm)
* Ubuntu
  *  16.04 (xenial)
  *  18.04 (bionic)
  *  20.04 (focal)
  *  22.04 (jammy)
  *  22.10 (kinetic)

## Architecture Support
## TODOs

These important features are not *yet* implemented here.
 - [ ] Publish in GitHub
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


## Actions

### bootstrap

Build a machine from scratch.

The first build of a machine type and architecture may be quite heavy on time and resource usage, but intelligent cacheing,
together with detection and use of local package caching proxies, should make this as efficient as possible and significantly reduce subsequent builds or re-builds

`sudo machinespawn ubuntu-jammy bread`

will build and bootstrap a container called `bread` using a base *ubuntu 22.04 (jammy)* image
### run

Spawn the container

`sudo machinespawn run bread`

### clean-cache

remove cached content from `/var/cache/machinespawn`

### pull-tar

pull a root file system tar-file to import a base system image


### remove

completely remove a machine


```shell
Usage: machinespawn [command]
```
