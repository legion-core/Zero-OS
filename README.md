# Zero OS   [![bluebuild build badge](https://github.com/legion-core/zero-os/actions/workflows/bluebuild-build-update.yml/badge.svg)](https://github.com/legion-core/zero-os/actions/workflows/bluebuild-build-update.yml)

Zero OS is a base Fedora Atomic image you can use as a starting point for your own OS or custom image.

I made this because setting up Fedora after every install gets old fast.

It’s basically Fedora with some of the boring stuff already done. PipeWire, media codecs, and common drivers are already there, so you don’t have to install that junk yourself every time.

If you want to build your own image from this, check out the [BlueBuild docs](https://blue-build.org/how-to/setup/).

Once you’re set up, you’ll probably want to tweak this README to describe what *your* image is about.

---

## Installation

> [!WARNING]
> [This is an experimental Fedora feature](https://www.fedoraproject.org/wiki/Changes/OstreeNativeContainerStable)
> Use at your own discretion.

To switch an existing Fedora Atomic system to Zero OS:

* Rebase to the unsigned image:

  ```bash
  rpm-ostree rebase ostree-unverified-registry:ghcr.io/legion-core/zero-os:latest
  ```

* Reboot:

  ```bash
  systemctl reboot
  ```

* Rebase to the signed image:

  ```bash
  rpm-ostree rebase ostree-image-signed:docker://ghcr.io/legion-core/zero-os:latest
  ```

* Reboot again:

  ```bash
  systemctl reboot
  ```

After that, you should be running Zero OS.

The `latest` tag always points to the newest build. The Fedora version comes from `recipe.yml`, so you won’t move to a new major release unless you change it there.

---

## Features

* Fedora Atomic base
* PipeWire
* Media codecs
* Common hardware drivers
* Automatic builds (GitHub Actions)
* Signed images
* Easy to extend

---

## ISO

You can generate an offline ISO using [BlueBuild’s ISO guide](https://blue-build.org/how-to/generate-iso/).

---

## Verification

Images are signed with [Sigstore](https://www.sigstore.dev/) and [cosign](https://github.com/sigstore/cosign).

```bash
cosign verify --key cosign.pub ghcr.io/legion-core/zero-os
```

If this passes, the image came from this repo.

---

## Credits

* [cron-job.org](https://cron-job.org)
* [BlueBuild](https://blue-build.org)
* [Fedora](https://getfedora.org)
