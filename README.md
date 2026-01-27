# Zero OS   [![bluebuild build badge](https://github.com/legion-core/zero-os/actions/workflows/bluebuild-build-update.yml/badge.svg)](https://github.com/legion-core/zero-os/actions/workflows/bluebuild-build-update.yml)

Zero OS is a base Fedora Atomic image that can be used as a starting point for building your own operating system or custom image.

It aims to stay close to a stock Fedora experience, but with some of the common setup already done — things like PipeWire, media codecs, and basic drivers are included so you don’t have to install them manually after every install.

See the [BlueBuild docs](https://blue-build.org/how-to/setup/) for quick setup instructions for creating your own repository based on this template.

After setup, it is recommended you update this README to describe your custom image.

---

## Installation

> [!WARNING]
> [This is an experimental feature](https://www.fedoraproject.org/wiki/Changes/OstreeNativeContainerStable), try at your own discretion.

To rebase an existing atomic Fedora installation to the latest build:

* First rebase to the unsigned image, to get the proper signing keys and policies installed:

  ```bash
  rpm-ostree rebase ostree-unverified-registry:ghcr.io/legion-core/zero-os:latest
  ```

* Reboot to complete the rebase:

  ```bash
  systemctl reboot
  ```

* Then rebase to the signed image, like so:

  ```bash
  rpm-ostree rebase ostree-image-signed:docker://ghcr.io/legion-core/zero-os:latest
  ```

* Reboot again to complete the installation:

  ```bash
  systemctl reboot
  ```

The `latest` tag will automatically point to the latest build. That build will still always use the Fedora version specified in `recipe.yml`, so you won't get accidentally updated to the next major version.

---

## Features

* Base Fedora Atomic image
* PipeWire pre-installed
* Media codecs included
* Common hardware drivers included
* Built and published automatically using GitHub Actions
* Signed container images
* Designed to be extended and customized

---

## ISO

You can generate an offline ISO using the instructions available [here](https://blue-build.org/how-to/generate-iso/).

---

## Verification

These images are signed with [Sigstore](https://www.sigstore.dev/)'s [cosign](https://github.com/sigstore/cosign). You can verify the signature by downloading the `cosign.pub` file from this repo and running the following command:

```bash
cosign verify --key cosign.pub ghcr.io/legion-core/zero-os
```

---

## Credits

* [cron-job.org](https://cron-job.org)
* [BlueBuild](https://blue-build.org)
* [Fedora](https://getfedora.org)
