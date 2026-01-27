# Zero OS   [![bluebuild build badge](https://github.com/legion-core/zero-os/actions/workflows/bluebuild-build-update.yml/badge.svg)](https://github.com/legion-core/zero-os/actions/workflows/bluebuild-build-update.yml)

Zero OS is a base Fedora Atomic image you can use as a starting point for your own OS or custom image ( you are expected to extend/layer stuff on zero-os as it is headless by default )

I made this because setting up Fedora after every install gets old fast. ( it is also used as upstream for legion-os & solaris-os )

It’s basically Fedora with some of the boring stuff already done. PipeWire, media codecs, and common drivers are already there, so you don’t have to install that junk yourself every time. ( uses rpm-fusion + tainted to do so )

Zero OS is build using BlueBuild , it is based on 'fedora-bootc' image , If you want to build your own image like this, check out the [BlueBuild docs](https://blue-build.org/how-to/setup/).

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

Zero OS uses latest fedora Version by default , a GTS version ( one fedora version behind branch ) is not planned at the moment but can be made in future if needed.
---

## Features

* Fedora Atomic base : Provieds Stability & OS-rollbacks
* PipeWire Stack Preinstalled ( with alsa , jack support )
* Media codecs ( from rpm-fusion )
* Common hardware drivers ( mesa + vulkan stack )
* Easy to extend ( only contains needed/most used packages , no bloat by design )

---

## ISO

Offline ISO's are currently WIP , to make a iso yourself you can use BlueBuild's GUIDE ( its easy to follow ) :
Generate an offline ISO using [BlueBuild’s ISO guide](https://blue-build.org/how-to/generate-iso/).

---

## Verification
if you want to vefify cosign signatuee of image use this command :
( Images are signed with [Sigstore](https://www.sigstore.dev/) and [cosign](https://github.com/sigstore/cosign). )

```bash
cosign verify --key cosign.pub ghcr.io/legion-core/zero-os
```

If this passes, the image came from this repo. ( if it fails dont use the image its likely to be tampered with )

---

## Credits

* [cron-job.org](https://cron-job.org) : used to automate build workflow ( more reliable then github's build in cron implementaion + free ) .
* [BlueBuild](https://blue-build.org)  : project's base , without it Zero OS likely wouldn't exist .
* [Fedora](https://getfedora.org)      : uses fedora's 'fedora-bootc' image as a base

## FAQ

1. why did you choose 'fedora-bootc' as base instead of recommended ublue images or bluebuild base images ?
A. 'fedora-bootc' provides you with less bloat ootb , even free codecs are not a part of image by default , this streamlines the build process + keeps image cleaner .

2. Why is X package not a part of Zero OS ?
A. Zero OS by default aims to be lightweigth $ headless , if you feel X package is provides functionality or usecase that will benefit everyone you are free to make a issue or pr , but there is no gurantee that it will be accepted.

3. Why does Zero OS ship X package insted of Y ?
A. Zero OS usually follows freedesktop standards or the most popular package for that usecase.

4. Does Zero OS follow Upsteam Fedora Desktop Defaults ?
A. Not always , for example Zero OS used PPD ( not finalized yet ) instead of upstream tuned as ppd is a freedesktop standard , similarly Zero OS dosent ship nano as default is vim etc.

5. How can I contribute to Zero OS ? and why do my builds fail in fork ?
A. there are multiple ways to contribute to Zero OS ;
   1.> Open a Issue and attach fix as a attachment ( only for minor/QoL fix )
   2.> Fork , commit , pull request : but thing to note is all build/workflow build will fail because your fork lacks the cosign key , this is to protect integrity/Security of Zero OS project.
       fix for this ; a. make a new branch , rename the os/image to your liking , sign image using cosign , and check if builds pass.
                      b. if the builds pass then its likely that edits were structurally correct and the main branck wont fail either.
                      c. you can now remove this branch and make a pr to main branch.

6. Are older Intel/AMD gpu's (legacy) & Nvidia GPU ( pre turing i.e older then gtx 16xx ) supported ? will they we supported ?
A. Zero OS only ships latest AMD/Intel Drivers i.e legacy gpu's are not supported , nvidia gpu's that dont support nvidia-open drivers are not supported ootb.
   but Zero OS has a planned zero-os iso , this iso will not contain any gpu drivers i.e user will be able to layer/install any version of drivers user want ( including legacy drivers )
   NOTE : there wont be any official support for legacy gpu's , its upto user to manage those drivers and Zero OS dosent gurantee stability with legacy Drivers

7. Does Zero OS ship 32bit packages/libs to support 32bit applications ( eg. Steam Client ) ?
A. No Zero OS only ship 64bit packages by default , but user can layer/install those packages/libs manually as they are avaliable in repos.

8. Does Zero OS include printing support ? are hp/older printers supported by default ?
A. Zero OS ships cups+system-config-printer by default ( with avahi for wifi printer detection ) , but hplip or older printer drivers are not included as they are user specific and bloat Zero OS if I add all of them.

9. what are extra repos included by Zero OS by default ?
A. Zero OS by default included extra 3rd party repos which are not part of fedora by default :
   1. rpm-fusion ( complete stack )
   2. rpm-fusion-tainted ( for nvidia-open drivers )
   3. VS code repo ( official vscode repo by microsoft , added as vscode flatpak is a bit hacky , so it gives user ability to install vscode on a system level )
   4. Brave Browser Repo ( official Brave repo )
   5. Vivaldi Browser Repo ( official Vivaldi repo )
   6. starship copr ( official copr to install strship on fedora )

10. Is local  package layering supported ?
A. Zero OS dosent recommend package layering , ans 'rpm-ostree install' is considered depricated i.e only use as a last resort , users are recommended to make a custom Zero OS image using BlueBuild if they wanna add packages to Zero OS .
   for normal packages can be installed using : linuxbrew/homebrew , flatpak , toolbox , distrobox , podman contrainers  ; all of these are alredy shipped in Zero OS by default.  
