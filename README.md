<div align="center">
 <img width="120" alt="AzuOS: If it runs on the Web, it runs on AzuOS" src="https://raw.githubusercontent.com/AzuSystem/azuos-legacy/refs/heads/main/assets/logo.svg">
	<h1>AzuOS</h1>
	<p>AzuOS is an Debian-based Linux Distro focused on its unreal design</p>
	<a href='https://azusystem.github.io/azuos'>Visit our Website ↗</a>
</div>

## 🤔 What is this repository for?
This repository hosts the debian live-build configuration and files for building an AzuOS system.

## 💻 Build instructions

### 🧰 Prerequisites
To build the live-build config, you need to have a host system running **Debian** (or derivatives).

_NOTE: Ubuntu is NOT recommended as a host system due to their rather different repositories + live-build binary_

The following packages are ALSO required for building:
- git
- live-build
- debootstrap

You can install these by running the following command with _administrative_ permissions:

```bash
apt upgrade && apt update
apt install git live-build # The pkg should automatically ship w/ debootstrap
```

### 🧬 Cloning repository
Next, clone this repository by running the following in the Arch system:

```bash
git clone https://github.com/AzuSystem/live-build.git
```

### 🔨 Build system
Move the newly cloned repository folder inside a new folder where you will be performing the building. You can't run `lb` ( live-build ) commands directly inside the config folder hence this preperation step.

```bash
mkdir builddir; mv live-build builddir; cd builddir
```

In a less concise / compacted format:
```bash
mkdir builddir
mv live-build builddir
cd builddir
```

Then afterwards, simply run
```bash
sudo lb clean --purge
rm binary bootstrap chroot common source # There are a few issues with the build process so this is a temporary solution
sudo lb config \
    --distribution trixie \
    --architectures amd64 \
    --archive-areas "main contrib non-free non-free-firmware" \
    --debian-installer none \
    --binary-images iso-hybrid \
    --bootappend-live "boot=live components hostname=azuos quiet splash" \
    --image-name azuos-amd64.hybrid \
    --iso-application "AzuOS" \
    --iso-volume "AZUOS" \
    --iso-publisher "AzuSystem" \
    --iso-preparer "AzuSystem"
sudo lb build
```

The outputted image should be located directly inside of `builddir/`.
