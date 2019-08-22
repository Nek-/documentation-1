---
outputFileName: index.html
---

# Uninstalling Event Store

This document describes how to uninstall Event Store. The instructions depend on which installation method you used. For different installation methods, refer to [Getting started - Step 1](~/getting-started/index.md). These instructions cover how to uninstall Event Store, but not how to remove dependencies such as the .NET framework or Mono.

### [Windows](#tab/tabid-1)

### Chocolatey

If you installed Event Store with Chocolatey, you can uninstall with:

```powershell
choco uninstall eventstore-oss
```

This removes the `eventstore-oss` Chocolatey package.

### Binary or built from source

If you installed Event Store by [downloading a binary](https://eventstore.org/downloads/), you can remove it by:

* Deleting the `EventStore-OSS-Win-*` directory.
* Removing the directory from your PATH.

### [Linux](#tab/tabid-2)

### Pre-built packages

If you installed one of the [pre-built packages for Debian based systems](https://packagecloud.io/EventStore/EventStore-OSS), you can remove it with:

```shell
sudo apt-get purge eventstore-oss
```

This removes Event Store completely, including any user settings.

### Binary or built from source

If you built Event Store from source, remove it by deleting the directory containing the source and build, and manually removing any environment variables.

### [macOS](#tab/tabid-macos)

<!--NOTE: the following needs to be tested. I am basing it off some googling -->

If you installed Event Store using Homebrew, you can remove it with one of the following:

```shell
brew cask uninstall eventstore
```

### Binary or built from source

If you built Event Store from source, remove it by deleting the directory containing the source and build, and manually removing any environment variables.

