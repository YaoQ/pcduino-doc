# Install Docker on Ubuntu

Docker is supported on these Ubuntu operating systems:

* Ubuntu Wily 15.10
* Ubuntu Trusty 14.04 (LTS)
* Ubuntu Precise 12.04 (LTS)

This page instructs you to install using Docker-managed release packages and installation mechanisms. Using these packages ensures you get the latest release of Docker. If you wish to install using Ubuntu-managed packages, consult your Ubuntu documentation.

> Note: Ubuntu Utopic 14.10 and 15.04 exist in Docker’s `APT` repository but are no longer officially supported.

## Prerequisites
Docker requires a 64-bit installation regardless of your Ubuntu version. Additionally, your kernel must be 3.10 at minimum. The latest 3.10 minor version or a newer maintained version are also acceptable.

Kernels older than 3.10 lack some of the features required to run Docker containers. These older versions are known to have bugs which cause data loss and frequently panic under certain conditions.

To check your current kernel version, open a terminal and use uname -r to display your kernel version:
```bash
$ uname -r
3.11.0-15-generic
```
> Note: If you previously installed Docker using APT, make sure you update your APT sources to the new Docker repository.

## Update your apt sources
Docker’s `APT` repository contains Docker 1.7.1 and higher. To set APT to use packages from the new repository:

1. If you haven’t already done so, log into your Ubuntu instance as a privileged user.
2. Open a terminal window.
3. Update package information, ensure that APT works with the https method, and that CA certificates are installed.
```bash
 $ sudo apt-get update
 $ sudo apt-get install apt-transport-https ca-certificates
 ```
4. Add the new GPG key.
```bash
$ sudo apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
```
5. Open the `/etc/apt/sources.list.d/docker.list` file in your favorite editor.  
If the file doesn’t exist, create it.

6. Remove any existing entries.
7. Add an entry for your Ubuntu operating system. The possible entries are:

  * On Ubuntu Precise 12.04 (LTS)  
`deb https://apt.dockerproject.org/repo ubuntu-precise main`

  * On Ubuntu Trusty 14.04 (LTS)  
`deb https://apt.dockerproject.org/repo ubuntu-trusty main`

  * Ubuntu Wily 15.10  
`deb https://apt.dockerproject.org/repo ubuntu-wily main`
> Note: Docker does not provide packages for all architectures. To install docker on a multi-architecture system, add an [arch=...] clause to the entry. Refer to the Debian Multiarch wiki for details.

8. Save and close the `/etc/apt/sources.list.d/docker.list` file.

9. Update the APT package index.  
`$ sudo apt-get update`  
10. Purge the old repo if it exists.  
`$ sudo apt-get purge lxc-docker`
11. Verify that APT is pulling from the right repository.  
`$ apt-cache policy docker-engine`  
From now on when you run `apt-get upgrade`, `APT` pulls from the new repository.

## Prerequisites by Ubuntu Version
* Ubuntu Wily 15.10
* Ubuntu Vivid 15.04
* Ubuntu Trusty 14.04 (LTS)

For Ubuntu Trusty, Vivid, and Wily, it’s recommended to install the `linux-image-extra` kernel package. The `linux-image-extra` package allows you use the aufs storage driver.

To install the `linux-image-extra` package for your kernel version:

1. Open a terminal on your Ubuntu host.
2. Update your package manager.  
`$ sudo apt-get update`
3. Install the recommended package.
`$ sudo apt-get install linux-image-extra-$(uname -r)`
4. Go ahead and install Docker.

If you are installing on Ubuntu 14.04 or 12.04, `apparmor` is required. You can install it using: `apt-get install apparmor`

## Install
Make sure you have installed the prerequisites for your Ubuntu version.

Then, install Docker using the following:

1. Log into your Ubuntu installation as a user with `sudo` privileges.
2. Update your `APT` package index.
```
$ sudo apt-get update
```
3. Install Docker.
```
$ sudo apt-get install docker-engine
```
4. Start the docker daemon.
```
$ sudo service docker start
```
6. Verify `docker` is installed correctly.
```
$ sudo docker run hello-world
```

This command downloads a test image and runs it in a container. When the container runs, it prints an informational message. Then, it exits.

## Ref
1. https://docs.docker.com/engine/installation/linux/ubuntulinux/
