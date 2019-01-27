The `Debian` images are assembled using the [debos](https://github.com/go-debos/debos) utility, which uses the `Debian` package feed beneath. Stuff not available in official `Debian` packages will be built from sources or downloaded into the final image.

# INSTALL DEBOS (under Debian)

To install [debos](https://github.com/go-debos/debos) you can do the following steps:

```sh
$ sudo apt install golang git libglib2.0-dev libostree-dev qemu-system-x86 qemu-user-static debootstrap systemd-container xz-utils bmap-tools

$ export GOPATH=`pwd`/gocode
$ go get -u github.com/go-debos/debos/cmd/debos
```

# Weston

## Build an image and run it in the QEMU emulator.

First, make sure you have KVM installed:

```sh
$ sudo apt install qemu-kvm ovmf
```

Now that that’s done, let’s create the images, run:

```sh
$GOPATH/bin/debos -t suite:sid images/weston/debian-base.yaml
$GOPATH/bin/debos -t suite:sid images/weston/debian-qemu-uefi.yaml
```

Will create the following output:

- debian-base-sid-amd64.tar.gz, a tarball with the debian base filesystem.
- debian-qemu-uefi-sid-amd64.img, an image file for QEMU emulator.
- debian-qemu-uefi-sid-amd64.vdi, a VirtualBox image.

The final command runs the image using the QEMU emulator:

```sh
$ scripts/qemu-boot-efi debian-qemu-uefi-sid-amd64.img
```