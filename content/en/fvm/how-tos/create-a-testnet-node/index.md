---
title: "Create a testnet node"
description: "Developers can spin up a Filecoin node on a testnet to troubleshoot their apps and infrastructure before pushing everything to the Filecoin mainnet. This quick guide shows you how to spin up a Filecoin node and sync to a testnet."
lead: "Developers can spin up a Filecoin node on a testnet to troubleshoot their apps and infrastructure before pushing everything to the Filecoin mainnet. This quick guide shows you how to spin up a Filecoin node and sync to a testnet."
menu:
    fvm:
        parent: "fvm-how-tos"
---

{{< beta-warning >}}

## Prerequisities

We're going to use the Lotus implementation of a Filecoin node in this guide. To run a Lotus node, your computer must have the following:

- MacOS or Linux installed. Windows is not supported.
- 8-core CPU and 8 GiB RAM.
- Enough space to store the current testnet chain. Depending on which testnet you connect to, this chain could grow in size by ~10 GiB per day.

## Dependencies

The dependencies for Lotus depend on which operating system you are running.

### Linux dependencies

You will need the following software installed to install and run Lotus.

#### System-specific

Building Lotus requires some system dependencies, usually provided by your distribution.

Arch:

```shell
sudo pacman -Syu opencl-icd-loader gcc git bzr jq pkg-config opencl-icd-loader opencl-headers opencl-nvidia hwloc
```

Ubuntu/Debian:

```shell
sudo apt install mesa-opencl-icd ocl-icd-opencl-dev gcc git bzr jq pkg-config curl clang build-essential hwloc libhwloc-dev wget -y && sudo apt upgrade -y
```

Fedora:

```shell
sudo dnf -y install gcc make git bzr jq pkgconfig mesa-libOpenCL mesa-libOpenCL-devel opencl-headers ocl-icd ocl-icd-devel clang llvm wget hwloc hwloc-devel
```

OpenSUSE:

```shell
sudo zypper in gcc git jq make libOpenCL1 opencl-headers ocl-icd-devel clang llvm hwloc && sudo ln -s /usr/lib64/libOpenCL.so.1 /usr/lib64/libOpenCL.so
```

Amazon Linux 2:

```shell
sudo yum install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm; sudo yum install -y git gcc bzr jq pkgconfig clang llvm mesa-libGL-devel opencl-headers ocl-icd ocl-icd-devel hwloc-devel
```

#### Rustup

Lotus needs [rustup](https://rustup.rs). The easiest way to install it is:

```shell
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

#### Go

To build Lotus, you need a working installation of [Go 1.18.1 or higher](https://golang.org/dl/):

```shell
wget -c https://golang.org/dl/go1.18.1.linux-amd64.tar.gz -O - | sudo tar -xz -C /usr/local
```

{{< alert >}}
You'll need to add `/usr/local/go/bin` to your path. For most Unix operating systems, you can run something like:

```shell
echo 'export PATH=$PATH:/usr/local/go/bin' >> ~/.bashrc && source ~/.bashrc
```

See the [official Golang installation instructions](https://golang.org/doc/install) if you get stuck.
{{< /alert >}}

All the dependencies should now be installed. Head down to the [Build and install section]({{< relref "#build-and-install" >}}).

### MacOS dependencies

You must have XCode and Homebrew installed to build Lotus from source.

#### XCode Command Line Tools

Lotus requires that X-Code CLI tools be installed before building the Lotus binaries.

1. Check if you already have the XCode Command Line Tools installed via the CLI, run:

    ```shell
    xcode-select -p
    ```

    ```plaintext
    /Library/Developer/CommandLineTools
    ```

    If this command returns a path, then you have Xcode already installed! You can [move on to installing dependencies with Homebrew](#homebrew). If the above command doesn't return a path, install Xcode:

    ```shell
    xcode-select --install
    ```

Next up is installing Lotus' dependencies using Homebrew.

#### Homebrew

We recommend that macOS users use [Homebrew](https://brew.sh) to install each of the necessary packages.

1. Use the command `brew install` to install the following packages:

   ```shell
   brew install go bzr jq pkg-config hwloc coreutils
   ```

Next up is cloning the Lotus repository and building the executables.

#### Rust

Rustup is an installer for the systems programming language Rust. Run the installer and follow the onscreen prompts. The default installation option should be chosen unless you are familiar with customization:

```shell
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

## Build and install

Once all the dependencies are installed, you can build and install Lotus. This process is the same for both Linux and MacOS.

1. Clone the repository:

   ```shell
   git clone https://github.com/filecoin-project/lotus.git
   cd lotus/
   ```

1. Switch to the testnet branch you want to build for:

    ```shell
    git checkout ntwk/wallaby
    ```

1. Build and install Lotus:

   ```shell
   make clean wallabynet
   sudo make install
   ```

1. Once the installation is finished, use the command down below to ensure Lotus is installed successfully for the right network.

   ```shell
   lotus --version
   ```

   ```plaintext
   lotus version 1.17.2+wallaby+git.8db6a939c
   ```

You should now have a working Filecoin node running on a testnet.
