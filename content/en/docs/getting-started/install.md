---
tags: ["getting-started", "installation"]
title: "Installation"
linkTitle: "Install"
date: 2017-01-05
weight: 4
description: >
  Learn about the different methods available to install Stevedore
---

Installing Stevedore is a straightforward process, and you have a few options to choose from. You can install using the provided installation script, which will take care of everything for you. Alternatively, you can download and install from a tarball or install from source if you prefer. This gives you more flexibility and control over the installation process.

## Using the install script

Installing Stevedore is easy thanks to the provided installation script. Simply run the following command to install Stevedore:
> **Note:** Before running the above command, make sure you have the 'curl' and 'sh' utilities installed on your machine.

```sh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/gostevedore/stevedore/main/scripts/install.sh)"
```

You can customize your installation passing options to the installation script. Those arguments can be provided to the script as positional arguments, as it is done with the `-h` in the following example.

```sh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/gostevedore/stevedore/main/scripts/install.sh)" -- -h

 Usage: -- [-f] [-h|?] [-b <binary-path>] [-d <releases-directory>] [-v <version>]

  -b: Specifies the path to the Stevedore binary file [default: /usr/local/bin/stevedore]
  -d: Specifies the directory to install the Stevedore release packages [default: /opt/stevedore]
  -f: Forces the installation of the binary
  -h: Shows the usage command
  -v: Defines the Stevedore version to install [default: the latest]
  -x: Sets the execution mode to debug
```

## Install from tarball

To install Stevedore using a tarball, simply download it directly from Github releases and follow these installation steps:

- Go to the Stevedore Github releases page and download the latest tarball

```sh
curl -fsSLO https://github.com/gostevedore/stevedore/releases/download/v0.11.0/stevedore_v0.11.0_Linux-x86_64.tar.gz
```

- Extract the contents of the tarball to a directory of your choice

```sh
tar xzfv stevedore_v0.11.0_Linux-x86_64.tar.gz
```

- Add the Stevedore executable to your system path or create a symlink

```sh
ln -sf ${PWD}/stevedore /usr/local/bin/stevedore
```

## Install from source

If you prefer to install Stevedore from source, you can clone the GitHub repository and build it manually. Here are the steps to follow:

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
