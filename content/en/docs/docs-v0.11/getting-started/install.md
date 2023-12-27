---
tags: ["getting-started", "installation"]
title: "Installation"
linkTitle: "Install"
date: 2017-01-05
weight: 4
description: >
  Learn about the different methods available to install Stevedore
---

Installing Stevedore is a straightforward process, and you have a few options to choose from. You can install using the provided installation script, which will take care of everything for you. Alternatively, you can download and install from a tarball or install from the source if you prefer. This gives you more flexibility and control over the installation process.

## Using the install script

Installing Stevedore is easy thanks to the provided installation script. Simply run the following command to install Stevedore:
> **Note:** Before running the above command, make sure you have the 'curl' and 'sh' utilities installed on your machine.

```sh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/gostevedore/stevedore/main/scripts/install.sh)"
```

To customize your installation, you can pass options to the installation script. These arguments can be provided as positional arguments to the script.
The following example demonstrates how to set your desired location for the binary and the directory where a located the release files.

```sh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/gostevedore/stevedore/main/scripts/install.sh)" -- -b "${HOME}/tools/bin/stevedore" -d "${HOME}/opt/stevedore"
 Ensuring directory /home/user/opt/stevedore/v0.11.1 exist...
 Ensuring directory /home/user/tools/bin exist...
 Installing Stevedore v0.11.1 using the artefact stevedore_0.11.1_Linux_arm64.tar.gz...
 Downloading artefact from https://github.com/gostevedore/stevedore/releases/download/v0.11.1/stevedore_0.11.1_Linux_arm64.tar.gz
 Extracting artefact to /home/user/opt/stevedore/v0.11.1...

 Installation completed successfully! The binary is available in /home/user/tools/bin/stevedore.

Stevedore 0.11.1 Commit: 45f8d10 linux/arm64 BuildDate: 2023-06-24T14:35:33Z

 Cleanup directory /tmp/tmp.GbgojK...
```

## Install from tarball

To install Stevedore using a tarball, simply download it directly from GitHub releases and follow these installation steps:

- Go to the Stevedore Github releases page and download the latest tarball

```sh
curl -fsSLO https://github.com/gostevedore/stevedore/releases/download/v0.11.2/stevedore_v0.11.2_Linux-x86_64.tar.gz
```

- Extract the contents of the tarball to a directory of your choice

```sh
tar xzfv stevedore_v0.11.2_Linux-x86_64.tar.gz
```

- Add the Stevedore executable to your system path or create a symlink

```sh
ln -sf ${PWD}/stevedore /usr/local/bin/stevedore
```

## Install from source

If you prefer to install Stevedore from the source, you can clone the GitHub repository and build it manually. Here are the steps to follow:

- Clone the Stevedore repository using Git:

```sh
git clone https://github.com/gostevedore/stevedore.git
```

- Change the working directory to the cloned repository:

```sh
cd stevedore
```

- Build Stevedore using the Makefile

```sh
make build
```

- Add the Stevedore executable to your system path or create a symlink

```sh
cp dist/stevedore /usr/local/bin/stevedore
```

You can run `make tar` to generate a tarball within the `dist` folder.

### Use Goreleaser to create a snapshot

In case you want to use [GoReleaser](https://goreleaser.com/) to generate a snapshot you can run `make snapshot`.
