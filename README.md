# Zero OS

[![Zero-OS Automatic Builder](https://github.com/legion-core/Zero-OS/actions/workflows/zero-os-automatic-builder.yml/badge.svg)](https://github.com/legion-core/Zero-OS/actions/workflows/zero-os-automatic-builder.yml)

[![Zero-OS Scheduled Builder](https://github.com/legion-core/Zero-OS/actions/workflows/zero-os-scheduled-build.yml/badge.svg)](https://github.com/legion-core/Zero-OS/actions/workflows/zero-os-scheduled-build.yml)

[![Zero-OS Watchdog](https://github.com/legion-core/Zero-OS/actions/workflows/zero-os-build-watchdog.yml/badge.svg)](https://github.com/legion-core/Zero-OS/actions/workflows/zero-os-build-watchdog.yml)

Zero OS is a custom Fedora Atomic image you can use as a starting point for your own OS or custom image (you are expected to extend/layer stuff on Zero OS, as it is headless by default).

I made this because setting up Fedora after every install gets old fast. (It is also used as upstream for legion-os & solaris-os.)

It’s basically Fedora with some of the boring stuff already done. PipeWire, media codecs, and common drivers are already there, so you don’t have to install that junk yourself every time. (Uses rpm-fusion + rpm-fusion-tainted to do so.)

Zero OS is built using [BlueBuild](https://blue-build.org), and it is based on Fedora’s [fedora-bootc](https://docs.fedoraproject.org/en-US/bootc/) image. If you want to build your own image like this, check out the [BlueBuild docs](https://blue-build.org/how-to/setup/).

---

## ▸ Installation

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

Zero OS uses the latest Fedora version by default. A GTS version (one Fedora version behind branch) is not planned at the moment, but can be made in the future if needed.

---

## ▸ Features

* Fedora Atomic base: provides stability & OS rollbacks
* PipeWire stack preinstalled (with ALSA & JACK support)
* Media codecs (from rpm-fusion)
* Common hardware drivers (Mesa + Vulkan stack)
* Easy to extend (only contains needed/most-used packages, no bloat by design)

---

## ▸ ISO

Offline ISOs are currently WIP.

To make an ISO yourself, you can use BlueBuild’s guide (it’s easy to follow):
[Generate an offline ISO using BlueBuild’s ISO guide](https://blue-build.org/how-to/generate-iso/)

---

## ▸ Verification

If you want to verify the cosign signature of the image, use this command:
(Images are signed with [Sigstore](https://www.sigstore.dev/) and [cosign](https://github.com/sigstore/cosign).)

```bash
cosign verify --key cosign.pub ghcr.io/legion-core/zero-os
```

If this passes, the image came from this repo. (If it fails, don’t use the image — it’s likely tampered with.)

---

## ▸ Credits

* [cron-job.org](https://cron-job.org) – used to automate build workflow (more reliable cron implementation)
* [BlueBuild](https://blue-build.org) – project’s base; without it, Zero OS likely wouldn’t exist
* [Fedora](https://getfedora.org) – uses Fedora’s fedora-bootc image as a base

---

## ▸ FAQ

### 1. why did you choose 'fedora-bootc' as base instead of recommended ublue images or bluebuild base images ?

A. 'fedora-bootc' provides you with less bloat ootb; even free codecs are not a part of the image by default. This streamlines the build process + keeps the image cleaner.

---

### 2. Why is X package not a part of Zero OS ?

A. Zero OS by default aims to be lightweight & headless. If you feel X package provides functionality or a use case that will benefit everyone, you are free to make an issue or PR, but there is no guarantee that it will be accepted.

---

### 3. Why does Zero OS ship X package instead of Y ?

A. Zero OS usually follows freedesktop standards or the most popular package for that use case.

---

### 4. Does Zero OS follow Upstream Fedora Desktop Defaults ?

A. Not always. For example, Zero OS uses PPD (not finalized yet) instead of upstream tuned, as PPD is a freedesktop standard. Similarly, Zero OS doesn’t ship nano as default; default is vim, etc.

---

### 5. How can I contribute to Zero OS ? and why do my builds fail in fork ?

A.

* There are multiple ways to contribute to Zero OS:

  1. Open an issue and attach fix as an attachment (only for minor/QoL fixes)
  2. Fork, commit, pull request

* But note:

  * All build/workflow builds will fail because your fork lacks the cosign key (this is to protect integrity/security of Zero OS project)

* Fix for this:

  * Make a new branch
  * Rename the OS/image to your liking
  * Sign image using cosign
  * Check if builds pass
  * If builds pass, edits were likely structurally correct and the main branch won’t fail either
  * Remove this branch and make a PR to main branch

---

### 6. Are older Intel/AMD GPU’s (legacy) & Nvidia GPUs (pre-turing, i.e. older than GTX 16xx) supported ? will they be supported ?

A.

* Zero OS only ships latest AMD/Intel drivers; i.e. legacy GPUs are not supported.
* Nvidia GPUs that don’t support nvidia-open drivers are not supported ootb.

But Zero OS has a planned Zero OS ISO:

* This ISO will not contain any GPU drivers.
* User will be able to layer/install any version of drivers they want (including legacy drivers).

NOTE:

* There won’t be any official support for legacy GPUs.
* It’s up to the user to manage those drivers.
* Zero OS doesn’t guarantee stability with legacy drivers.

---

### 7. Does Zero OS ship 32bit packages/libs to support 32bit applications (e.g. Steam Client) ?

A. No. Zero OS only ships 64bit packages by default, but users can layer/install those packages/libs manually, as they are available in repos.

---

### 8. Does Zero OS include printing support ? are HP/older printers supported by default ?

A. Zero OS ships cups + system-config-printer by default (with avahi for WiFi printer detection), but hplip or older printer drivers are not included, as they are user-specific and would bloat Zero OS if all were added.

---

### 9. what are extra repos included by Zero OS by default ?

A. Zero OS by default includes extra 3rd-party repos which are not part of Fedora by default:

1. rpm-fusion (complete stack)
2. rpm-fusion-tainted (for nvidia-open drivers)
3. VS Code repo (official Microsoft repo; added as VS Code Flatpak is a bit hacky)
4. Brave Browser repo (official)
5. Vivaldi Browser repo (official)
6. starship COPR (official COPR to install starship on Fedora)

---

### 10. Is local package layering supported ?

A. Zero OS doesn’t recommend package layering, and `rpm-ostree install` is considered deprecated (only use as a last resort). Users are recommended to make a custom Zero OS image using BlueBuild if they want to add packages to Zero OS.

For normal packages, users can install using: linuxbrew/homebrew, flatpak, toolbox, distrobox, podman containers — all of these are already shipped in Zero OS by default.
